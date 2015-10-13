---
layout:     post
title:      Command line Python tools
author:     Sriram Sundarraj
date:       2015-08-17 02:34:18
summary:    My first blog post.
categories: python
thumbnail: jekylli
tags:
 - about
---

If you're already accustomed to writing Python scripts you may be wondering how to create command line modules. 
Command line modules are scripts that can be run directly from the command line without explicitly using the Python interpreter.
That is, executing ` $ youtube-dl <url>` instead of `$ python youtube-dl.py <url>`.  

In this post, I'm assuming that you know how to create regular Python modules using `setuptools`. 

## 1. The scaffolding

Usually, Python modules have the following directory structure:

```
my_module
|
|--my_module/
|  |
|  |--foo.py
|  |--bar.py
|  |--__init__.py
|
|--README.md
|--...
|--other files
|--...
|--setup.py
```

## 2. Your code

The `__init__.py` file if the `my_module` directory should have a function (or should import a function) that needs to be invoked when the command is in the command line.
Let us suppose that this function is named `console_main`.

## 3. Using setuptools to make it a module

Your `setup.py` file will have a `setup` function (one that is imported from `setuptools`). To make your regular Python module into a command line module, you just have to add one line to it.

```

setup(name="Sriram's Blog Module",
      ......
      # Line to be added:
      entry_points = {
          'command-name':['my_module:console_main'],
      },
      .....
      )
```

## 4. Running it

Open a terminal, setup the module by running `python setup.py install`. Then, type `command-name` and you'll see the `console_main`
function being executed.

## 5. Bonus: Argument parsers

Coming soon. I just discovered a few interesting libraries that do argument parsing in Python. I'll get back to you guys on this with more research.

