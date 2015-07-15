---
layout:     post
title:      Debian packaging
date:       2015-07-16 01:34:18
author:     Sriram Sundarraj
summary:    Quick debian packaging guide. 
categories: tuorial
thumbnail:  debian
tags:
 - debian
 - packaging
 - ubuntu
 - tutorial
---


# Simple Debian Packaging in 10 Minutes

Recently, I made a simple debian package. I noticed that there weren't any simple tutorials for making a package. I had to go through many sources to make a small package. That's the motivation behind this post.

This is not comprehensive by any means. It's just a quick guide to making simple, small debian packages.

## 1. Tools you will need

You will need the `dpkg` package. If you are using a debian system (such as Ubuntu) this should be installed already.
If you're using OS X you can install it from homebrew (`brew install dpkg`). 

## 2. Basic structure

Essentially, a debian file holds data regarding your package and files that your package needs. It copies the files that are required onto the target system. It may execute some commands before and after installation.

First off, you will need to create a folder which is going to be packaged. We will call this the `PACKAGE` folder. It contains all your package files and data. 
Inside this, you should create a folder named `DEBIAN`. This folder will contain all the meta data pertaining to your package and all the install scripts.

You directory strcture should look something like this.

    .PACKAGE/
    |
    |--DEBIAN/


## 3. Control file

Control file contains the most vital information about the package such as package name, maintainer. version number, description, etc. 
A sample control file is provided below. It contains a few of the fields that a control file can have. For more information regarding the fields, you can check the [debian website](https://www.debian.org/doc/debian-policy/ch-controlfields.html).

```
Package: MyPackage
Depends: python-dev
Version: 0.0.1
Section: tutorial
Architecture: all
Essential: no
Maintainer: me@email.com
Description: This is a debian packaging tutorial.
```

The control file should be named `control` and placed in the `PACKAGE/DEBIAN` directory. 
Your directory structure will look like this.


    .PACKAGE/
    |
    |--DEBIAN/
       |
       |-control


## 4. Adding files to your package

Now to your code. You should treat the `PACKAGE` directory as the root (i.e. `/`) of the file system on which the package is going to be installed.
Meaning, if you put a file in `PACKAGE/var/www/html/`, the file will appear in `/var/www/html/` when the package is installed. 

Since the installation is done with super user permissions, files can be placed anywhere in the file system.

Temporary files should be placed in the `/tmp` directory. 

Your directory structure may look like this.


    .PACKAGE/
    |
    |--var/
    |  |
    |  |--www/
    |     |
    |     |--html/
    |        |
    |        |--hello_world.html
    |        |--my_blog.html
    |
    |--DEBIAN/
       |
       |-control



## 5. Post and pre install scripts
   
You may wish to run certain scripts before or after the files in the `PACKAGE` directory are copied to the target file system.
These are `preinst` and `postinst` files. 

Post and pre install files should start with a shebang (`#!/bin/bash`). Pre install scripts are run before the package is installed. Post install scripts are run after the package's files are copied.

An example `postinst` file:

```
#!/bin/bash
cd /tmp/my_package
python setup.py install  # install the package
echo "MyPackage has been installed"
```

The `postinst` file and `preinst` file should have permissions between `555` and `755`.
These files also have to be put in the `PACKAGE/DEBIAN` directory.

Your package structure may look like this.

```
    .PACKAGE/
    |
    |--var/
    |  |
    |  |--www/
    |     |
    |     |--html/
    |        |
    |        |--hello_world.html
    |        |--my_blog.html
    |
    |--DEBIAN/
       |
       |-control
       |-postinst
       |-preinst

```

## 6. Running the package command.

Navigate to the parent directory of `PACKAGE` and execute the `dpkg-deb` command.

```
dpkg-deb --build PACKAGE
```

Your package should have been built as a `.deb` file.



# Conclusion

I hope this helped you in getting a quick understanding of how to make simple debian packages. If you spot any mistakes or corrections, please feel free to write to me.



