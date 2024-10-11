---
layout: post
title: "AngularJS with Flask"
description: "Some problems faced when trying to use AngularJS as a front end
for Flask"
category: [technology, python, web-app, old_blog]
tags: [angular, js, flask, python, single, page, application]
---

As of now, I don't remember now what my actual plans were to do.
I just had one major thought, to make some application just as a way to refresh my
memory of [Flask](http://flask.pocoo.org/). But I also wanted to learn something new, as I have not done
anything new for time now. While trying to merge these together I came across,
SPA or [Single Page Applications](https://en.wikipedia.org/wiki/Single-page_application). This idea somehow created a curiosity in me.
The research that I started off because of this curiosity lead me to try out
[AngularJS](http://angularjs.org/). Particularly at this infant stage of me in Javascript world, I
had no specific bias towards any frameworks. So I just planned to take up
AngularJS.

Thus, I started to try out each attributes in Angular like ng-bind, and the list
goes on; I have no plans to elaborate on these things now. One main thing that I
oversaw was the format in which angluar takes in the expression to evaluate from
an HTML page(view).

```html
<div ng-bind="{{ "{{ some-variable" }} }}"></div>
```

This should have some to my notice even before the problem had come, and me
struggling to print some **Hello World** string on the web page. The problem
with this particular syntax is actually a clash between the
[Jinja](http://jinja.pocoo.org/) templating
engine used by flask and the new AngularJS front-end that we have introduced.
Both use the same syntax to for expression that that is to be dynamically
evaluated inside the template page. 


With some basic research I was able to find few solutions for this. I am just
trying to compile all the reading that I had done at a single place for future
reference:

* Use ng-view to insert another web page content into the one rendered by flask

```html
<div ng-view></div>
```

* There is way to change the `{{ "{{ "}}}}` operator in angular to someother string. This
  change can help both pieces of codes angular and jinja library to peacefully
  work together

  The below basically changes the config of the application to change its
  starting and ending templatng tags to something else

```javascript
myModule.config(function($interpolateProvider) {
  $interpolateProvider.startSymbol('[[');
    $interpolateProvider.endSymbol(']]');
    });
```

-------------------------------------------------------------------------------------------------

Another major trouble that I had, was with ng-repeat. I was trying to insert a model
inside each `<li> </li>`tags. \
Say the sample code is given below:

```html
<!-- Something here -->
<ul>
    <li ng-repeat="k in dict-of-keys">
        <div class="modal fade" id="k-edit" role="dialog">
            <!-- Something else here -->
            <h4 class="modal-title">{{ "{{ dict-of-keys[k]"}} }}</h4>
            <!-- Something else else here -->
        </div>
    </li>
</ul>
```

The above html code was my first try. However, I think or whatever I did  was not
able to one issue resolved. All the list and modals, everything was working fine
but the issue was with the data content in everything. All the models inside the
ng-repeat lists was same and also it was the content of the first list entry.

Actually a closer look at the above code was enough to solve the problem. The
root cause is that the model div gets created with the content of the first
iteration of ng-repeat and then this model/content gets propagated throughout
rest of the list entries. i.e. Here we have to uniquely identify the model div
for each `<li>` tag.

```html
<!-- Something here -->
<ul>
    <li ng-repeat="k in dict-of-keys">
        <div class="modal fade" id="k-edit-{{"{{ k "}}}}" role="dialog">
            <!-- Something else here -->
            <h4 class="modal-title">{{ dict-of-keys[k] }}</h4>
            <!-- Something else else here -->
        </div>
    </li>
</ul>
```

Changing the id attribute from `id="k-edit"` to `id="k-edit-{{"{{ k "}}}}"` will help
us to uniquely map each model div with the list entry and thus the data in each
iteration is correctly displayed. This is not a very big issue on an average
maybe. But me as a starter in this front-end development, it was a good
learning experience for me. I have also seen many people asking very similar
kinds of questions in stack overflow and elsewhere. Thus, I am just a bit
relieved to see that I am not a lone crusader here.

I'll update it as and when I face anything new(perhaps old for some people
out there :P).

> On a side note, the issue with templating tags ie "{{"{{ "}}}}", happened while
> I was typing this post as well. The Liquid templating engine which is used by
> Jekyll also had the same templating tag. All the "{{"{{ "}}}}" I had put in
> the text was first consider for templating, then I had them all escaped. [Here](http://stackoverflow.com/questions/3426182/how-to-escape-liquid-template-tags)
> is a post regarding it.
>
> The raw-endraw can also be a solution. Refer
> [here](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers).
