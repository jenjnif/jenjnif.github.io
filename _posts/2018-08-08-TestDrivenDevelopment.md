---
layout: post
title: Test Driven Development - Meeting the Testing Goat
excerpt_separator: <!--more-->
---

**As part of my job I implement features and fix bugs for a web application involving a session booking system (that I find very confusing!) with various login access levels and specifications.**  

<p align="center"><img src="/images/TestingOne/Goat.JPG"
     alt="Sunday visit to Eagle Crag" width="60%" /></p>

As many programmers will have found before me, projects quickly become warrens. Fixing one thing illuminates further issues and breaks other parts of the code. Before you know it you are in a complicated web of despair. It quickly became clear how important test driven development (TDD) is, especially as projects grow and become more complex as my one has. 
<!--more-->

<br>

### The beginning

For those of you that don't know - writing tests helps to catch any errors in your working code. You might write a test for a very small part of your code such as a method (unit testing) e.g. testing that ' def create_list_sessions(item) ' returns a list of sessions; or for a chunk of functionality (functional tests) e.g. when a new session list is created using the create_list_sessions method, that it contains a persons name, address, date of birth etc and that these are added to the persons profile. These tests can then be re-run when you change a part of the code to check they all still all pass. For example, if you wanted to add a mandatiory expiry date field for a session then you could write a test for this and then write your new code. You would then be able to check if you new test passed but also to make sure none of your previous tests were now failing. If any fail, it might mean your new section of code has broken some functionality and the tests will alert you to this.

On my journey to testing enlightenment my excellent boss gave me a book which became my go-to guide to learning more about the subject of testing using Python - ‘Test-Driven Development with Python’ by Harry J.W Percival available on the <a href="http://www.obeythetestinggoat.com/" target="_blank">Testing Goat website</a>. If you haven’t come across it before and would like to learn more about testing then I highly recommend it.

<p align="center"><img src="/images/TestingOne/TestGoat.png"
     alt="Sunday visit to Eagle Crag" width="40%" /></p>

The book presumes so little knowledge to get started that I think it would be suitable for most beginner programmers. It uses Python 3, Django and Selenium and explains how to download all three along with basic information about other useful tools. The book even explains the importance of version control (simply put, saving all versions of your code) and walks through the first commit with you.

It begins by writing the initial test to check that Django is installed and ready.

<p align="center"><img src="/images/TestingOne/FirstTest.png"
     alt="A snippet of my first test" width="80%" /></p>

This first test obviously fails as the next step is to install and set up Django - the first lesson of TDD! Next comes the all important commit for version control. The steps are so simple to follow and so clearly explained that it makes it fairly painless to follow and understand. There is also some useful <a href="https://docs.djangoproject.com/en/2.1/topics/testing/" target="_blank">Django documentation</a> for more information about how testing works in Django.

You can continue to follow my journey through the book on <a href="https://github.com/jenjnif/TestingGoat" target="_blank">my Github repo</a>.
