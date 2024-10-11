---
layout: post
title: "At the Finals"
title_id: "Experiences at the finals of InCTF '13"
description: ""
category: [experience, old_blog]
tags: [security, inctf, bufferoverflow, learning, amrita, competition]
---

I will continue with what happened after [this](/2014/07/26/hacking-at-inctf/)

When the organizers of the event called us for announcing the winners, they requested that one person from each team to talk
about our experience at [InCTF](www.inctf.in). I was the person who went from my team. Actually I had many thing to say but
when I got there I didn't talk much. Out of 11-13 teams participated we were able to make it to the fifth position. The first
thought that came to me when I write this post is that this event had given me a good experience in a totally different field
which altogether changed my interests.

Final round of InCTF was conducted at Amrita College, Amritapuri Campus for two days, on June 1, June 2.  First of all it was
a great experience reaching there. I took the tickets and was waiting for my friends in front of the train. But they got into
the train before calling me and the train started. When the train was at a distance that I could not catch I called them up and
said that I didn't get and made them jump back to the platform :P. Then we had to catch a bus to Kayamkulam and we reached very late, on the last bus to the college.

Next day was a practice session where we were given a vulnerable Ubuntu image. First we had to bypass the login and change the
root password. As we were newbies in this area we were only able to bypass this login. But inside we had to start few custom
made services and exploit its vulnerabilities. There were three services that we had to start. The source code of the vulnerable
services were also provided. It would basically be written in either of [Python](tag list), [C](tag list), and [C++](tag list).
We could actually understand what will be the work flow of the program. But even with our basic understanding that we should
use a [buffer-overflow attack](link to post) to retrieve whatever data we need, we could not put that into practice. This was
when I really felt that our seniors could have helped us a bit more. I don't want to put blame on them because they were the
people who intimated us that there is a competition like this is being conducted. I am really thankful to them for it.

After the first round we three team members did run behind the organizers to give give us some tips on how and what to do.
They were very helpful and gave us tips on how to crack this competition. I think we utilized all the chance that we got to
talk to the organizers especially [Bithin](http://bithin.blogspot.in/), [Seshagiri Prabhu](http://seshagiriprabhu.wordpress.com),
[Aravind S Raj](http://arvindsraj.wordpress.com/). More than the competition we had a friendly conversation and exchanged our
views on various topics not like professionals but as people who want to learn new things.

That night we decided that we read some related materials. But the the situation was against us. No range to get Internet
connection... tiredness due to travel... everything came together :(. Even then we sat for some time just talking on what
to do the next day. From the inspiration from our seniors and the fact that they were the winners last time, we were looking
forward to doing a good performance at the event.

On the second day when we started there was only one method that was in our mind to get a remote connection, SSH. But the irony
was that we could not use it as the password was reset at the beginning. It may be that we did not have knowledge on how
bypass it. Initially nobody did get any points. But later one started to score. After sometime we got a different method of
attack and we were able to use it effectively. From what my seniors have told, automating the task could fetch us more points.
So we automated the task. With this we were in the top three for about half of the event.

But everything reversed within a few minutes.... Some guy used the vulnerability in the service to inject `rm -rf` command to
the root directory of the service. By the time we solved this issue by copying files from the backup we had... we lost many
points for lagging behind and we came down the scoreboard. Even then we were confident that we could make it to the top by the
end of the day. Again problem came in. We could not make the script run correctly. The original one was not backed-up. Solving
this issue was like a NP-Hard problem for us at that time. By the time we figured out few new methods time was finished and we
had to wind the event. We had to satisfy with fifth position in the event. This was decided on the basis of our performance in
both second and the final round.

Even though we could not win the competition, it was a great learning opportunity and a chance to meet many new people. I
would like to recommend students or people who are interested in security field to attend this kind of CTF competitions.
This could give you a exposure to different kind of techniques and methods that other experts use. And an opportunity to talk
to them as well.

Anyway now I am hopping to be a part of future verions of InCTF and many other events of similar kind. :)
