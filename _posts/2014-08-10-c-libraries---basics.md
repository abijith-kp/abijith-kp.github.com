---
layout: post
title: "C Libraries - Basics"
title_id: "How to create and add C library"
description: ""
categories: [technology, how-to]
tags: [C, library, gcc, learning, create, beginner, intermediate]
---

Agenda:

- Which all libraries are installed?

- How to use that library?

This is a very beginner level how-to article on using a C libraries available in
your system. I felt like writing this post after I had a difficulty with
dealing with these things. You should know that this is not a complete guide,
but could serve the needs of a beginner to understand the what actually is the
basic requirement.

Lets start...

For using a library in your program, first we need to get two information ready. One
the location of the header file. ie the `.h` file and secondly the shared library
file with the extension `.so`. \\
The default directory where all the headers are stored is `/usr/include/`. And
the corresponding .so library file is stored inside `/usr/lib`, `/usr/lib32`,
`/usr/lib64` directories.

As the first step let's see how to include a header files. Let the
header files to be included be in the directory /path/to/my_module and their names be
`header_file1.h, header_file2.h`.

So in order to include header_file1.h, there are two ways to do it.

* Using -I option while compiling:\\
-I flag is used to give the directory in which the header file is placed. Here
the required argument is `/path/to/my_module`.
{% highlight bash %}
gcc -I /path/to/my_module sample.c -o sample
{% endhighlight %}

* Changing the format of #include pre-processor statement:\\
If my_module directory is placed inside the standard include directory, the
pre-processor include statement can be written as:
{% highlight c %}
#include <my_module/header_file1.h>
{% endhighlight %}

With this change your code is ready to get linked. In the next step we need
to tell the compiler which would the shared library that is to be linked with your code.

The library file will have a name in the form of: liblibrary_name.so[.version].
While compiling a code with a not-standard header file included, then a flag is
to be given at compile time to GCC.

ie. Here, let the target file is `libmy_module.so`. Therefore the flag to be used will
be -lmy_module. The name can be derived as: \\
`l` for telling that it is a library (easy to remember)\\
`my_module` from the name of the shared object

As another example, if the flag used is `-labc`, then the library file that will
be used by the compiler would be libabc.so.

Along with gcc the location where the shared object is saved can be mentioned
using `-L` option. You can ignore this if it was the default location.
{% highlight bash %}
gcc -L /path/to/libmy_module.so -lmy_module sample.c
{% endhighlight %}

With this I guess one would be able to include any custom header files in your C
program and make it better!!!.

Reference:
----------

[swarthmore.edu](http://www.cs.swarthmore.edu/~newhall/unixhelp/howto_C_libraries.html)
