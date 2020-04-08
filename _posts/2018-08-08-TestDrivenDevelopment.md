---
layout: post
title: Test Driven Development - Meeting the Testing Goat
excerpt_separator: <!--more-->
---

**As part of my job I implement features and fix bugs for a web application involving a session booking system (that I find very confusing!) with various login access levels and specifications.**

<p align="center"><img src="/images/testing_one/goat.JPG"
     alt="A mountain goat in Canada" width="60%" /></p>

As many programmers will have found before me, projects quickly become warrens. Fixing one thing illuminates further issues and breaks other parts of the code. Before you know it you are in a complicated web of despair. It quickly became clear how important test-driven development (TDD) is, especially as projects grow and become more complex as my one has. 
<!--more-->

<br>

### Note about versions
This TDD project uses Django 3.0.4 and Python 3.6.4.

If you follow the project along in GitHub you will see a few re-starts in various versions of Django but I am now sticking to version Django 3.0.4. 

To learn about test-driven development I followed along with a book which uses Python 3 and Django version 1.7. The book is about learning TDD rather than Django/Python and so asks that you use the same versions to follow along.

However, I thought that my blog would be more helpful to write if I could highlight any changes between Django 1.7 and 3.0.4 rather than just re-writing the book. When viewing <a href="https://docs.djangoproject.com/en/3.0/" target="_blank">the official Django documentation</a> the version you want to see can be selected at the bottom.

<br>

### The beginning

For those of you that don't know - writing tests helps to catch any errors in your working code. You might write a test for a very small part of your code such as a single method (unit testing). For example, imagine simple booking system that has teachers and students and each student has x number of sessions. You might write a unit test that checks that the following code returns a list of sessions for a particular learner:

    def create_list_sessions(learner)
        # some code...
        return session_list

Or, write a test for a chunk of functionality (functional tests). For example, you might test that when a teacher clicks on a link to one of their students that a new page opens up with the students profile and sessions list which is created using the create_list_sessions method above. 

These tests can then be re-run when you change a part of the code to check that they all still all pass. An example might be if you wanted to add a mandatory expiry date field to any session then you could write a test for this and then write your new code. You would then be able to check if your new test passed but also to make sure none of your previous tests were now failing. If any fail, it might mean your new section of code has broken some functionality and the tests will alert you to this.

On my journey to testing enlightenment my excellent employer gave me a book which became my go-to guide to learning more about the subject of testing using Python - ‘Test-Driven Development with Python’ by Harry J.W Percival available on the <a href="http://www.obeythetestinggoat.com/" target="_blank">Testing Goat website</a>. If you haven’t come across it before and would like to learn more about testing then I highly recommend it.

<br>

<figure class="image" align="center">
  <img src="/images/testing_one/test_goat.png" alt="Front cover of the book" width="40%">
  <figcaption>Front cover of Test-Driven Development with Python book</figcaption>
</figure>

<br>

Test-Driven Development with Python presumes so little knowledge to get started that I think it would be suitable for most beginner programmers. It uses Python, Django and Selenium and explains how to download all three along with basic information about other useful tools. The book even explains the importance of version control (simply put, saving all versions of your code) and walks through the first commit with you.

<br>

### Functional tests

The book begins by writing a very simple functional test (shown below) to check that Django is installed and that the server is running. This first test fails as expected because no code has been written yet. The next step is to get Django up and running on the server - the first lesson of TDD! Write a test that fails and then work on making it pass before writing your next failing test.

<br>

<figure class="image" align="center">
  <img src="/images/testing_one/first_test.png" alt="The first test" width="60%">
  <figcaption>First basic functional test</figcaption>
</figure>

<br>

Next comes the all important commit for version control. The steps are so simple to follow and so clearly explained that it makes it fairly painless to follow and understand. There is also some useful documentation on the <a href="https://docs.djangoproject.com/en/3.0/topics/testing/" target="_blank">Django site</a> for more information about testing.

Following this initial test the purpose of the Django app is revealed, we are building a to-do list. The next few steps are to extend the functional test to test the minimum viable to-do list app before tidying up a few loose ends.

<br>

<figure class="image" align="center">
  <img src="/images/testing_one/minimum_viable_test.png" alt="A snippet of my minimum viable functional test" width="90%">
  <figcaption>Extended functional test to test minimum viable app</figcaption>
</figure>

<br>

The test is still failing as can be seen in the terminal window below. The test specifies that the home page has 'To-Do' in the title, which it doesn't, yet.

<br>

<figure class="image" align="center">
  <img src="/images/testing_one/fail_home_page.png" alt="Minimum viable test failure" width="90%">
  <figcaption>Terminal output showing functional test failure</figcaption>
</figure>

<br>

You can continue to follow my journey through the book on <a href="https://github.com/jenjnif/TestingGoat" target="_blank">my Github repo</a>. Next step: <a href="/2020/03/27/TDDUnitTests.html">Unit Tests</a>.

Test-Driven Development with Python blog series:

1.  <a href="/2018/08/08/TestDrivenDevelopment.html">Test Driven Development - Meeting the Testing Goat.</a>

2.  <a href="/2020/03/27/TDDUnitTests.html">Test Driven Development - Unit Tests.</a>

