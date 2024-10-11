---
layout: post
title: "What is Buffer Overflow Attack"
title_id: "Computer Security: Introduction to Buffer Overflow Attacks"
description: ""
category: [technology, old_blog]
tags: [bufferoverflow, attack, security, introduction]
---

Have anyone you heard the statement - *Water water everywhere. But not a drop to drink*.

You will understand this, if you start looking for a tutorial to learn about buffer overflow. Everyone of it would
tell the same set of instructions, but nothing works. They all are missing out the main and important points. With
this post I am trying to make up for that problem. Here I will be giving all the required background research about
buffer overflow and the attack which uses this exploit.

When we pour more water into a glass, it overflows. The overflown water can really be a mess. The same thing happens
in the case of a buffer overflow in a program also. When the size of the data to be stored is more than the size of the
buffer allocated for the variable, the data is copied to the adjacent memory locations. By the way a buffer is a term
given for the continues memory locations allocated for a variable. this overflow may lead to overwriting of the adjacent
stack pointers such as ESP or EIP. This kind of overwriting of stack pointers can lead to serious problems like escalating
ones privilege, or may be crashing the whole system. Even when people always talk about(me included :P) styles of
programming and other related things but programmers are never taught or they think about SECURE CODDING which is equally
important but ironically given the least importance(May only be true among beginners and intermediate people. This is since
expert people works in production environment and its very crucial in that situation to follow security measures).

Buffer overflow attack mainly aims at those executable files which has a setuid bit enabled. This setuid bit gives the
executable a property that it will inherit all the privileges of its owner rather than the user which runs the executable.
So if somehow a shell is spawned while running this process the shell will run with the privileges of root user if the owner
of the executable was root. Thus explicitly, the assumptions that we take while using the below mentioned attack on an
executable are:

- Owner of the executable is root

- Setuid bit is enabled

Next two main concepts that we have to give importance are NOP Sled and the payload script named as a shell script.

Its continuation can be read [here](/technology/2014/07/25/buffer-overflow-attack/)
