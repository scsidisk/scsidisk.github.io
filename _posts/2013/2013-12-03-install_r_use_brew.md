---
layout: post
title: "Installing R on OS X"
date: 2013-12-03
author: scsidisk
categories: Development
tags: R, MacOSX, Brew
---

This is a quick introduction to installing R on OS X Mountain Lion.

But first, what is R?

R is a system for statistical computation and graphics. It provides, among other things, a programming language, high level graphics, interfaces to other languages and debugging facilities.

### 1. Installation

You can download a package for OS X, or go with Homebrew – my route. For the Mountain Lion, you’ll need to install XQuartz and a Fortran compiler first.

Install XQuartz. Download it from here.

Install Fortran & R:

```bash
# updated 30 Sept 2013
$ brew update
$ brew tap homebrew/science
$ brew install gfortran r
```

Homebrew should be done compiling and installing within 10 minutes.

### 2. Test

To test the installation, run r in terminal, you should get something like this:

```
dvdsmpsn-mac:~ david$ r

R version 3.0.1 (2013-05-16) -- "Good Sport"
Copyright (C) 2013 The R Foundation for Statistical Computing
Platform: x86_64-apple-darwin12.4.0 (64-bit)

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under certain conditions.
Type 'license()' or 'licence()' for distribution details.

  Natural language support but running in an English locale

R is a collaborative project with many contributors.
Type 'contributors()' for more information and
'citation()' on how to cite R or R packages in publications.

Type 'demo()' for some demos, 'help()' for on-line help, or
'help.start()' for an HTML browser interface to help.
Type 'q()' to quit R.

>
```

You can then start some baby steps such as basic addition and creating functions

```r
> x <- 4+5
> x
[1] 9
>
> addup <- function(a, b=10)
+ {
+     return (a+b)
+ }
> addup(4,5)
[1] 9
```

When you’re done, quit the R console:

```r
> q()
```