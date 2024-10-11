---
layout: post
title: "Awk - Simple but powerful"
description: ""
category: old_blog
tags: [awk,brian,kernighan,text,processing,simple,powerful,tool]
---

Everything happened today while I was trying to visualize a set of items and its
frequencies. Usually, till date, I had converted it to CSVs and then loaded into
excel(in my office system!!). Everyone could easily interpret the values in the dataset this way.
But when I needed to analyse this data alone, it became a little inconvenient for
me. Thus I thought of writing a script which would help me to visualize this
set of data in ASCII format. I know that there are many tools out there which
would really help me do it through command prompt. But here I was in a situation
where I would not be able to install anything new. Thus AWK came to the
rescue...

Before going further into what I had done, I wanted to take some time to see
what I had understood regarding the language from one of its creators.

As Brian Kernighan, said in one of his [lectures](https://www.youtube.com/watch?v=Sg4U4r_AgJU), which I accidentally saw last
week, writing a language for a specific task can help in solving that problem
very well than writing a general purpose language every time which will include
all the unwanted and complex constructs. These undesirable features in a general
purpose language will only slow down the process of problem solving rather than
helping to solve it efficiently.

AWK has solved the problem of text processing very efficiently, with many of the
commonly used program patterns coded into the language itself. Thus we don't
have to go behind solving all the problems that is in front of us and just focus on
the core things that are needed.

The need to parse the files line by line and execute some kind of common code
pattern is very usual in text processing tasks. This is very neatly implemented
in the language. The programmer just has to write only what the repeating code
pattern is.

Each line can be broken down into columns - this by default uses space as a
field separator. The field separators can also be changed.

As Mr.Kernighan said, if there is only one class that you can teach him data
structures, then teach him associated arrays. This data structure has been used
in AWK. It helps in maintaining a status of everything that we had done during
our processing stage, and this status list can be utilized during the final
consolidating stage.

Now coming back to the point what I was talking about... \\
Below given is the script that I had written to take the input as a list of two
member set in a file, where the first member is the count and the second
element is the identifier for each item. The script will print out the
identifier and the horizontal bar graph for the entire list of items. I had to do
a normalization for the values in count field, since the range of values may not
be that predictable.

{% highlight bash %}
awk 'BEGIN \
     { min = -1 } \
     { if (min == -1) { min = $1} else if (min > $1) {min = $1}; \
       if (max < $1) max = $1; normal_count[$2] = $1; \
       sum += $1 \
     } \
     END \
     { diff = max - min; \
       for (i in normal_count) { normal_count[i] = (normal_count[i] - min)*100/diff; printf "%3d ", i; \
         for ( j=0; j<normal_count[i]; j++ ) \
         { printf "="; } print ""; \
         } \
     }' input | less
{% endhighlight %}

**NOTE1:** Thank you [Ashwin](https://www.facebook.com/ashwin.prasad.54) for reading the first draft
of this post.

**NOTE2:** Mostly all the points mentioned are taken directly from the lecture
mentioned above.

**NOTE3:** I may have to rewrite the article.
