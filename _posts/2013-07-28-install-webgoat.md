---
layout: post
title: "Install WebGoat"
title_id: "How to install Webgoat in Arch Linux"
description: ""
categories: [technology, how-to]
tags: [webgoat, java, arch linux, installation, owasp]
---

Today as a part of my Computer Scurity course I had to install [WebGoat](https://www.owasp.org/index.php/Category:OWASP_WebGoat_Project),
which is platform for learning how to exploit vulnarabilities in web applications. It could be of great help in learning
secure programming practices. It work on top of Java and Tomcat server. As I started the installation I had to face many
problems associated with it. Mainly with the versions of JDK and JRE used. I had lost almost half a to solve this problem.
So I thought I could share my experience if it could help someone to install WebGoat.

I downloaded WebGoat v5.2 from [Sourceforge](http://sourceforge.net/projects/owasp/files/WebGoat/WebGoat%205.2/). Along
with the WebGoat-OWASP_Standard-5.2.zip, we also require WebGoat-5.2.war file. Unzip the WebGoat zip file to your curent
directory.Change into the new unzipped directory. Then remove all the files name webgoat in `./tomcat/webapp/` and place
the downloaded war file in this directory.

Find out the versions of jre and jdk installed in your system using:  `java  -version`

Also find the vakue for the environmental value JAVA_HOME:  `echo $JAVA_HOME`

Open the webgoat.sh file from the root folder. In the function `is_java_1dot6` change all the `1.6` to the your current
version.

{% highlight bash %}
$ java -version
java version "1.7.0_21"
OpenJDK Runtime Environment (IcedTea 2.3.9) (ArchLinux build 7.u21_2.3.9-4-x86_64)
OpenJDK 64-Bit Server VM (build 23.7-b01, mixed mode)

$ echo $JAVA_HOME
/usr/lib/jvm/java-7-openjdk
{% endhighlight %}

Here java version is shown as 1.7.0_21. Therefore you can replace the older version number given in the shell script to the
new version number `1.7`.You have to add below given two lines to the shell script:

{% highlight bash %}
JAVA_HOME=/usr/lib/jvm/java-7-openjdk
export  JAVA_HOME
{% endhighlight %}

Now start the tomcat server.

{% highlight bash %}
$ sh webgoat.sh start80      # works on default port of 80
{% endhighlight %}

OR

{% highlight bash %}
$ sh webgoat.sh start8080    # works on port 8080
{% endhighlight %}

Go to http://127.0.0.1/webgoat/attack OR http://127.0.0.1:8080/webgoat/attack in any browser to start using WebGoat interface.
If you get an ERROR 403 while starting on port 80, it may be due to IIS that is using that port.

And if it is ERROR 404, check the url you typped on the browser.

I suppose I have included most of the thing that I have done. Hope this helps. :)

