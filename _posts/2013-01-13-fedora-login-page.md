---
layout: post
title: "Fedora login page"
title_id: "Add image to login page for GDM in Fedora"
description: ""
categories: [technology, how-to, old_blog]
tags: [fedora, login, linux, gdm, background, x-server]
---

I have been trying to change the image in the log-in page of my Fedora system for a long time now. I always felt that it
was a very dull one and wanted to something very interesting there, it can give you a very good first impression. At last
i have found the way...

There is actually not much work in setting up this...

First what you have to do is to copy the image that you require to put in the background to the directory /var/lib/gdm. You
will be requiring the root access to copy to the final destination.There could be a problem with ownership of the file after
you copy it. So change both the group and user of the image as gdm.
{% highlight bash %}
sudo cp  /from/required/path/image.jpg  /var/lib/gdm/
{% endhighlight %}

next change the ownership of the file
{% highlight bash %}
sudo chown gdm:gdm /var/lib/gdm/image.jpg
{% endhighlight %}

Next you have to add the user "gdm" to the list of all users who can access the X Server of your system. For that you can:
{% highlight bash %}
xhost +SI:localuser:gdm
{% endhighlight %}

Then start the gnome control center to cahnge the background as gdm as the user.
{% highlight bash %}
sudo -u gdm dbus-launch gnome-control-center
{% endhighlight %}

Select the background option. Then select the image from the home folder of the user gdm. Close the gnome-control-center.
Then remove gdm from the users list who can access the X Server. You can use the following command for that:
{% highlight bash %}
xhost -SI:localuser:gdm
{% endhighlight %}

Now just logout from your account and see that the background image has changed.... :)
