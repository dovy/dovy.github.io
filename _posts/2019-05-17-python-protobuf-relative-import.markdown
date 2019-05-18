---
layout: post
title:  "Storing Google Protobuf in a Relative Path for Python"
date:   2015-04-25 11:59:49
categories: WordPress
tags: [PHP, Authentication, Bypass, Code, Hack]
imagefeature: escape.jpg
featured: false
comments: true
overlay_opacity: 
overlay_color: 
---

On how I have stuggled with this for SO long. Google Protocol buffers are fantastic, but they're a pain in the neck with 
python! Unless you install them in a special way they won't import, or will they!

<!--more-->

## Why you should care

Sometimes you want to install packages in a relative manner. Say in a lambda or cloud function, or what have you. If you
do a full pip install every script now has full access to that version. Sometimes using a virtual environment can be too tough.
What's the answer? Realative importing.

```bash
pip install -r requirements.txt -t ./packages
```

But with Protobuffers they're special. Google didn't just install them in a standard method. So now we have to do some magic
here to get things to work.

## The Magic
Now all you have to do is creat a file. Call it anything you want. I called mine proto_import.py. Now place this code inside:
```python
relative_path = '/packages'
import sys, types, os;has_mfs = sys.version_info > (3, 5);dir_path = os.path.dirname(os.path.realpath(__file__)) + relative_path;p = os.path.join(dir_path, *('google',));importlib = has_mfs and __import__('importlib.util');has_mfs and __import__('importlib.machinery');m = has_mfs and sys.modules.setdefault('google', importlib.util.module_from_spec(importlib.machinery.PathFinder.find_spec('google', [os.path.dirname(p)])));m = m or sys.modules.setdefault('google', types.ModuleType('google'));mp = (m or []) and m.__dict__.setdefault('__path__',[]);(p not in mp) and mp.append(p)
```

Change the `relative_path` variable to the relative location of your pip install, import the file to any file using protobuffers, and voila!

## Where this came from
When you install Protobuf google installs a protobuf-X.X.X-pyX.X-nspkg.pth file. This contains a variation of the above code, 
and ONLY WORKS in the default `site-packages` folder. If anyone has a more elegant way to pick up a .pth file I am all ears. 
Otherwise, this is a great soltion for me. I hope it helps someone else.
