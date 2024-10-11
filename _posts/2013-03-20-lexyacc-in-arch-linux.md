---
layout: post
title: "Lex/YACC in Arch Linux"
title_id: "Setup Lex/YACC in Arch Linux in Fedora"
description: ""
categories: [technology, how-to]
tags: [flex, arch linux, compiler, lex, flex static, fedora]
---

In our sixth semester of B-Tech we have a course on Compiler Design. For that two tools are being used: Lex which is a
lexical analyzer and Yacc is a parser generator. While compiling the code for the lexical analyzer created by lex and the
code for parse created by yacc we require few libraries to include some important function templates into the code. the
flags used along with gcc for this purpose are `-ll` and  `-lfl`.

After installing Lex/Yacc in my Fedora system few months back, I got an error stating that definition for yywrap is not
included. After searching for some time I got the solution for it.

There can be methods to solve it :

- Just include the function yywrap in the lex file in the function definition part(third part of the code).Compiling can be
done as `gcc lex.yy.c y.tab.c`

- Install **flex-static** library to use the gcc flag -ll. After this just compile it like `gcc lex.yy.c y.tab.c -ll`

But later I changed to Arch Linux, after 1 year of using Fedora.

But in Arch when I tryed to compile the lex/yacc file initially the same problem arised. I thought I could solve it the
same way as before. All the researches I had done was in vain. I couldn't find a package similar to flex-static anywhere.
At that point of time I used the first option, which easily solved my problem.
