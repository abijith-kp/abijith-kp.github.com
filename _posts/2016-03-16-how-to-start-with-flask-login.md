---
layout: post
title: "How to start with Flask Login?"
description: ""
category: old_blog
tags: []
---

When I started off to code for my new project which I had named as
[Emolytics](http://github.com/abijith-kp/emolytics), I had some small starting
troubles with respect to using those features in flask which I had not exposure
with. I had give some dedicated time in getting to understand these things. Here
I am trying to pen down some points which I felt should be read by anyone who is
starting to code with flask.

Now I'll start off this with Flask-Login module, an extension in Flask which
provides great help in managing users and their sessions.

Initial Setup
-------------

Flask-Login module is to be installed:
{% highlight python %}
pip install Flask-Login
{% endhighlight %}

Next we can go directly into the coding section.
Along with the Flask application initialisation, a LoginManager object is also to
be created.
{% highlight python %}
from flask.ext.login import LoginManager

login_manager = LoginManager(app)
{% endhighlight %}

#### A login page:

This template page will help you to show the user a login page and collect the
user credentials and later we can process it to verify wheather the user has
registered or is a valid user.
A part of login.html will be like:
{% highlight html %}
<form action="" method="post">
<input type="text" placeholder="Username" name="username" value="{{ request.form.username }}">
   <input type="password" placeholder="Password" name="password" value="{{ request.form.password }}">
<input type="submit" value="Login">
</form>
{% endhighlight %}


#### End point function for login:

Corresponding login function is also defined under the decorator app.route. This
login function is going to be used later to find the original login endpoint to
which all the anonymous users will be redirected to when they try to access any
restricted pages.

For this functionality we have to assign the function name to the login_manager
variable login_view like: `login_manager.login_view = 'login'`. Whenever access
to a login_required page happens it is redirected to this login page with next
url as the url currently accessed.

Below a basic login function is shown for reference purpose:

{% highlight python %}
@app.route("/login", methods=["GET", "POST"])
def login():
    form = Form()
    if form.validate_on_submit():
        session['user_id'] = form.user.id        ## See below for explanation
        login_user(form.user)
        return redirect(request.args.get('next') or url_for('index'))
    return render_template("login.html", form=form)
{% endhighlight %}


#### Function to load the user:

Before every request the variable current_user is populated. This loading is
done using many hooks which can be defined. Some of the ways are mentioned below:

* From user_id attribute in session variable: Once the login is completed, a key
user_id can be put in the session dictionary, which will be taken by this hook
to load a user before all the requests.

* From the request arriving: The incomming requests will be given as the
arguement to this hook and after processing it should return the user object.

There are many more different hooks. You can just look in to flask.ext.login
help page.

{% highlight python %}
@login_manager.user_loader
def load_user_id(id):
    return User.query.filter(User.id==int(id)).first()

@login_manager.request_loader
def load_user_request(request):
    # do some processing here
    return user
{% endhighlight %}


#### User Model:

The decorator login_required will manupulate some class variables to keep track
of logged in users, anonymous users and similar things. So its better that
either we have them put explicitly into the User model class or use the
UserMixin class which will have all the necessarry data variables that was
mentioned earlier.

One major variable that is be present should be is_authenticated, which will be
set when the user validation is successful and will be unset when user logs out.
These events happen when the functions login_user and logout_user are called.

{% highlight python %}
class User(UserMixin):
    def __init__(self, username, password):
        self.username = username
        self.password = password

    def check_password(self, password):
        return self.password == password
{% endhighlight %}

Thanks :)
