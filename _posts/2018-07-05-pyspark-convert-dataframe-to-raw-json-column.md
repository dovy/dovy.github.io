---
layout: post
title:  "Convert a Spark (python/pyspark) values into a single raw_json column"
date:   2018-05-09 08:00:00
categories: Spark
tags: [Spark, Python, ETL]
imagefeature: abstract-art-blur-373543.jpg
featured: false
comments: true

---

I recently was tasked with creating a full json representation of each row, as it's own individual column. This is how I did it.

<!--more-->

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
newDF = df.withColumn('raw_kvp', kvp_udf(df.columns)(*df.columns))\
    .select(
        "*",
        f.udf(lambda x: json.dumps(dict(zip(x[::2],x[1::2]))), StringType())(f.col('raw_kvp'))\
        .alias('raw_json')
    ).drop('raw_kvp')
# newDF2.printSchema()
#newDF.select('raw_json').show(1, truncate=False)
```

What does this do? It takes each column per row, and creates a KVP. From that KVP it converts it into a dictionary, then dumps a json representaiton of that dictionary, while dropping the key-value-pair column.

Long story short, this should save you time if you're looking for the same type of thing.  :)





```
