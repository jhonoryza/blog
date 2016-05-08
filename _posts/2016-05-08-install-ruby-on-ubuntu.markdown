---
layout: post
title:  "install ruby on ubuntu"
date:   2016-05-08 18:36:16 +0000
categories: linux
---

I found that this is the easy and simple way to install ruby on your machine
If you’re using Ubuntu 14.04 (Trusty) or newer then you can add the package repository like this:

``` shell
$ sudo apt-get install software-properties-common
$ sudo apt-add-repository ppa:brightbox/ruby-ng
$ sudo apt-get update
```

Or if you’re on Ubuntu 12.04 (Precise) or older

``` shell
$ sudo apt-get install python-software-properties
$ sudo apt-add-repository ppa:brightbox/ruby-ng
$ sudo apt-get update
```

Installing the packages
Each version of Ruby has its own packages - just install the packages for the versions you’d like to use.

So to install Ruby 2.3

``` shell
$ sudo apt-get install ruby2.3 ruby2.3-dev
```

If you like blogging you can install jekyll

``` shell
$ sudo gem install jekyll -V
```