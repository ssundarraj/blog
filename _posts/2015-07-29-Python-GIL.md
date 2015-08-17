---
layout:     post
title:      The Python GIL
author:     Sriram Sundarraj
date:       2015-07-29 02:34:18
summary:    The hard facts about the Python GIL.  
categories: python
thumbnail: python
tags:
 - python
 - threading
 - GIL
---

This post aims to give you the hard facts about the Python GIL.

## What is GIL

GIL stands for Global Interpreter Lock. It is used in Python to make sure that only one thread can execute at a time. 
This means that, even in a multi-core machine, two Python threads can't execute simultaneously. The effects of this are huge for concurrent Python applications. It greatly limits thread performance.

## Effect on Performance

As David Beazly pointed out at PyCon 2010:

```
def countdown(n):
    while n > 0:
        n -=1

# Non-threaded
COUNT = 100000000 # 100 million
countdown(COUNT)


## Threaded approach
t1 = Thread(target=countdown,args=(COUNT//2,))
t2 = Thread(target=countdown,args=(COUNT//2,))
t1.start(); t2.start()
t1.join(); t2.join()
```

The threaded approach is 1.2x slower than the threaded approach on a quad core machine. 

This wouldn't be expected in other languages such as C++, since C++ does not have a GIL.
The reason for this behaviour is that the context switches that happen as a result of threading take up significant time.



## Does this affect all functions?

If the function that needs to be run concurrently is IO bound, then the effect of the GIL on performance is far less. It's so much lesser that as a rule of thumb, IO bound threads can safely run concurrently on Python. 

This is because the thread would spend more time waiting in an IO queue rather than in the ready queue. Hence, the number of context switches would be lesser and the effect of the wouldn't be pronounced. 


## What can be done with CPU bound tasks?

Each Python process has it's own GIL. This basically means that if we have multiple Python processes, the threads belonging to each process can run on only one core. Each process can run on it's own core, and hence we can achieve true concurrency!

```
import multiprocessing
p1 = multiprocessing.Process(target=countdown, (args=COUNT//2,))
p2 = multiprocessing.Process(target=countdown, (args=COUNT//2,))
p1.start(); p2.start()
p1.join(); p2.join()
```

## Conclusion

* Threads can only run on one processor.
* Use threads for IO bound tasks.
* Use multiprocessing for CPU bound tasks.




