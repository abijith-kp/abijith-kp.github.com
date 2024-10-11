---
layout: post
title: "How to use Cython compiler??"
title_id: "Introduction: Use cython to make python modules"
description: ""
categories: [technology, python, how-to]
tags: [python, cython, compiler, optimize, c, languages]
---

Introduction
-------------

There are two terms which could be interchangeably confused: *Cython* and *CPython*.

*Cython* is a compiled language which can be regarded as a superset of Python. The compiler translates the input
Cython/Python code to a portable C code. Which in-turn could be compiled with a GCC compiler to get an executable.
Cython helps to make a python program run faster. Whereas *CPython* implies that its the C language implementation
of python interpreter. There are other versions of python as well like Jython(Java implementation),
IronPython(C# implementation) etc.

Say for example, let us consider a [sample program](https://gist.github.com/abijith-kp/ae260a89da5a9b67457b).
Refer to the description given for more information.

When the sample.py is run, the running time is recored as given below:

> 0.004858970642089844, 0.004592180252075195, 0.004554033279418945, 0.00444793701171875

For working with Cython initially we have to convert the python/Cython code into C language. It can be done using
the Cython compiler.

{% highlight bash %}
$ cython --embed sample.py
{% endhighlight %}

Output will be a C source file named sample.c

**NOTE**:

If the option `--embed[=<method_name>]` is not given along with cython command, a main function is not included in
the resulting code

After the compilation step, we have two ways to proceed:

- Make a shared library and use it as a python extension module(used as a library).

- Make a binary file which could be executed a highier speed(if it has an input output style code)

Make a module
-------------

{% highlight bash %}
$ gcc -shared -fPIC -Os -I /usr/include/python2.7 -o sample.so sample.c
{% endhighlight %}

Note the use of .so extension for the output shared library. The file sample.so can be imported as a regular
python module by the name sample. If a python program by the same name is present in the same directory, then
the python file will only get imported.
Recorded running time for module imported from shared library:

> 0.015142202377319336, 0.014052867889404297, 0.012368202209472656, 0.01092982292175293

Recorded running time for module imported from python script:

> 0.00722503662109375, 0.00661015510559082, 0.006145954132080078, 0.0056989192962646484

Make an executable
------------------

{% highlight bash %}
$ gcc -I /usr/include/python2.7 -o sample sample.c -lpython2.7
{% endhighlight %}

This is just a regular gcc compilation with the header file location mentioned using -I flag.

After compiling with cython and running the resultant C program, the recored times for five trials are as follows:

> 0.002379894256591797, 0.0022690296173095703, 0.0022478103637695312, 0.002251863479614258

The running time has doubled after running using the cython generated code.

Other flags such as -lpthread, -Os, -lm, -lutil, -ldl can be used as optional as per your requirement.

| Command          | |     Purpose                            |
|------------------|-|---------------------------------------:|
|   -Os            | |   Optimize size                        |
|   -lpython2.7    | |   Include python2.7 libraries          |
|   -lpthread      | |   Include pthread libraries            |
|   -lm            | |                                        |
|   -lutil         | |                                        |
|   -ldl           | |                                        |


All the library dependency can be seen using ldd command

{% highlight bash %}
$ ldd sample
  linux-vdso.so.1 (0x00007fff95ffe000)
  libpython2.7.so.1.0 => /usr/lib/libpython2.7.so.1.0 (0x00007f64805e4000)
  libpthread.so.0 => /usr/lib/libpthread.so.0 (0x00007f64803c6000)
  libm.so.6 =so> /usr/lib/libm.so.6 (0x00007f64800c2000)
  libutil.so.1 => /usr/lib/libutil.so.1 (0x00007f647febf000)
  libdl.so.2 => /usr/lib/libdl.so.2 (0x00007f647febf000007f647fcbb000)
  libc.so.6 => /usr/lib/libc.so.6 (0x00007f647f90d000)
  /lib64/ld-linux-x86-64.so.2 (0x00007f64809ad000)
{% endhighlight %}

FAQ:
----
*Quest*: Why do I get this import error? "ImportError: ./sample.so: cannot dynamically load executable" \\
  *Ans*: You are trying to import a file which is not a shared library. Use -shared flag along with gcc

*Quest*: What is the error init function not defined? "ImportError: dynamic module does not define init function (initsample_test)" \\
  *Ans*: Names of the files, ie the shared library and both input codes(C and python/cython) has to be
         same with only the extensions changed.

Interesting Links:
------------------

[Stackoverflow](http://stackoverflow.com/questions/5105482/compile-main-python-program-using-cython) - A stackoverflow question similar to our topic.

[Presentation](http://www.behnel.de/cython200910/talk.html) by Dr.Stefan Behnel.

[Tutorial](http://www.cprogramming.com/tutorial/shared-libraries-linux-gcc.html) on shared libraries.

**NOTE**:

This article do not contain on how to wite a Cython code. This [link](http://docs.cython.org/src/userguide/index.html)
will help you learn some Cython code.
