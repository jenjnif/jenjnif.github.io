---
layout: post
title: Test Driven Development - Unit Tests
excerpt_separator: <!--more-->
---

**Some time ago I was working through 'Test-Driven Development with Python' by Harry Percival with the intention of writing up how I got on as I went. As often happens, work and life got in the way of writing, so sometime later I am recommencing my quest.**

<p align="center"><img src="/images/tdd_unit_tests/warning.JPG"
     alt="Photo from Chapter 3 of the book" width="60%" /></p>

After having created a failing first minimum viable functional test for a to-do list in <a href="/2018/08/08/TestDrivenDevelopment.html">Test Driven Development - Meeting the Testing Goat</a> the next step is to create a Django app and to start on unit tests.

<!--more-->

<br>

### Functional Tests and Unit Tests

The difference between functional and unit tests can get a bit blurred. In his book, Harry Percival defines the difference as functional tests, test the application from the point of view of the user, whereas unit tests test from the inside, from the point of view of the programmer. For example, a functional test might test what happens when a user clicks 'buy item x' on an online shop. This might involve testing many methods and other dependencies such as databases, web servers, etc. Unit tests, on the other hand, will test individual units, such as the methods that make up all the code tested in the functional test. In this way, there might be many unit tests testing the code for one functional test.

This book takes the approach of creating a failing functional test first, then writing and running failing unit tests for small parts of this functionalty before writing code to pass the unit tests. After this, run the functional test again, add any additional code needed before again, writing more failing unit tests and so on.

I don't want to go into too much detail from the book as I might as well just be re-writing it. So, as mentioned in my first blog on <a href="/2018/08/08/TestDrivenDevelopment.html">TDD,</a> I am mainly going to focus on the changes I found when using the latest version of Django. This is the first one:

<br>

### The module django.core.urlresolvers is no more!

So, I am at the point where I am writing the first unit test which should be a simple example of the aim; to write a test that fails in an expected way before writing code to pass it. But sometimes there are unexpected failures like this one:

<figure class="image" align="center">
  <img src="/images/tdd_unit_tests/unexpected_fail_annotated.png" alt="code and terminal output of an unexpected fail" width="100%">
  <figcaption>Code and terminal output of an unexpected fail</figcaption>
</figure>

<br>

This is one of the reasons why tests are so great. The first line of the unit test imports resolve from django.core.urlresolvers but the test error is clearly showing that this module is not found:

    ModuleNotFoundError: No module named ‘django.core.urlresolvers’

<br>

With a quick search on the <a href="https://docs.djangoproject.com/en/3.0/releases/2.0/">Django documentation</a> I find that this module was removed in Django 2.0 and its functionality moved to django.urls.

<br>

<figure class="image" align="center">
  <img src="/images/tdd_unit_tests/no_such_module.png" alt="Django documentation showing that django.core.urlresolvers was removed in Django version 2.0" width="100%">
  <figcaption>Django documentation showing that django.core.urlresolvers was removed in Django version 2.0</figcaption>
</figure>

<br>

So this line needs to be replaced with:

    from django.urls import resolve

<br>

<figure class="image" align="center">
  <img src="/images/tdd_unit_tests/django_core_urlresolvers_to_django_urls_annotated.png" alt="code changing django module to current one" width="100%">
  <figcaption>Removing the old Django module, django.core.urlresolvers, and replacing it with django.urls</figcaption>
</figure>

<br>

Try running the unit test again:

    $ python3 manage.py test

<br>

<figure class="image" align="center">
  <img src="/images/tdd_unit_tests/expected_home_page_error_annotated.png" alt="code and terminal output of the expected fail" width="100%">
  <figcaption>Code and terminal output of the expected fail</figcaption>
</figure>

<br>

The above is the failure I expected to see as I was trying to import something that I haven't written yet.

<br>

### urls.py

So, now I want to try to get my test to pass by creating a simple view in lists/view.py.

    from django.shortcuts import render

    # create your view here
    home_page = None

This will fail as Django hasn't been told how to find a URL to map to the root ("/") of the site yet. Here is the failure report.

<br>

<figure class="image" align="center">
  <img src="/images/tdd_unit_tests/no_home_url_setup.png" alt="terminal output showing Django cannot find a URL mapping to '/'" width="100%">
  <figcaption>Terminal output showing Django cannot find a URL mapping to '/'</figcaption>
</figure>

<br>

The reason I have given the previous two steps here is that the next part is a bit different from the book. Django version 1.7 uses regular expressions (regex) for its URL mapping but they scrapped this in later versions in favour of path. So, instead of just changing the superlists/urls.py file a couple of changes are necessary:

1. Create a urls.py file in lists at the same level as tests.py and add a path to the home page to it:

<p align="center"><img src="/images/tdd_unit_tests/list_urls_file.png"
     alt="new lists/urls.py file with contents" width="80%" /></p>

2. As mentioned above the urls.py file in superlists/superlists won't look the same as the book example because Django 3 doesn't use regular expressions. You will need to do something like this: 

<p align="center"><img src="/images/tdd_unit_tests/superlists_urls_file.png"
     alt="superlists/superlists/urls.py file contents" width="80%" /></p>

<br>

After making the above changes and ignoring those related to regex from Django 1.7 the test can be run again and get the expected error:

<figure class="image" align="center">
  <img src="/images/tdd_unit_tests/view_not_callable.png" alt="view not callable error for home page" width="100%">
  <figcaption>Expected error: view not callable for home page</figcaption>
</figure>

<br>

Then just a quick change in lists/views.py to create a simple function for home_page before running the unit test again, this time it should pass.

<br>

<figure class="image" align="center">
  <img src="/images/tdd_unit_tests/passing_home_page_test.png" alt="home page function in views.py along with passing test" width="70%">
  <figcaption>views.py home_page function and passing test terminal output</figcaption>
</figure>

<br>

Currently, there is one unit test passing and the functional test still fails. The final step in this section is to write another unit test to check that the home_page function returns a real HTTP response to the browser. After writing the new unit test and a bit of back and forth with minimal code changes to fix each failing error of the test it finally passes and when run again so does the functional test.

You can continue to follow my journey through the book on <a href="https://github.com/jenjnif/TestingGoat" target="_blank">my Github repo</a>. Next step: Chapter 4!

1.  <a href="/2018/08/08/TestDrivenDevelopment.html">Test Driven Development - Meeting the Testing Goat.</a>

2.  <a href="/2020/03/27/TDDUnitTests.html">Test Driven Development - Unit Tests.</a>
