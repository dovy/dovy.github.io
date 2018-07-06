---
layout: post
title:  "Convert a Spark dataframe into a JSON string, row by row"
date:   2018-05-09 08:00:00
categories: Spark
tags: [Spark, Python, ETL]
imagefeature: abstract-art-blur-373543.jpg
featured: false
comments: true

---

Not all schemas are created equal. Sometimes, no matter how much you massage the structure, you want to make sure and future-proof your work. That's what this is all about. Taking the original data from a dataframe, and making a JSON representation of it in a single column. That way you can be sure and maintain all of your data long term.

<!--more-->

Let's say you have a complex schema and you're planning to adjust it a bit. You'll rename here, sum there... But what if you mess up? Wouldn't it be nice to have an original copy stored in the data so for future iterations you can come back and save yourself from ETL misery?

This little utility, takes an entire spark dataframe, converts it to a key-value pair rep of every column, and then converts that to a dict, which gets boiled down to a json string.

This block of code is really plug and play, and will work for any spark dataframe (python). It takes your rows, and converts each row into a json representation stored as a column named raw_json. Give it a try!

```python
# Create raw_json column
import json
import pyspark.sql.functions as f

def kvp(cols, *args):
    # Create KVP of column and row
    a = cols
    b = map(str, args)
    c = a + b
    c[::2] = a
    c[1::2] = b
    return c

kvp_udf = lambda cols: f.udf(lambda *args: kvp(cols, *args), ArrayType(StringType()))
newDF = df.withColumn('raw_kvp', kvp_udf(df.columns)(*df.columns)).select(
    "*",
    f.udf(
        lambda x: json.dumps(
            dict(zip(x[::2],x[1::2]))
        ), 
        StringType())(
            f.col('raw_kvp')
        ).alias('raw_json')
    ).drop('raw_kvp')
# newDF2.printSchema()
#newDF.select('raw_json').show(1, truncate=False)
```

Long story short, this will save you time if you're looking for the same type of thing, because I could not find a complete solution for this anywhere.  :)
