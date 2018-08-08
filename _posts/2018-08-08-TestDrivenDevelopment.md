---
layout: post
title: Test Driven Development - Meeting the Testing Goat
excerpt_separator: <!--more-->
---

**As part of my job I implement features and fix bugs for a web application involving quite a confusing session booking system with various access levels and specifications.**  

<p align="center"><img src="/images/TestingOne/Goat.JPG"
     alt="Sunday visit to Eagle Crag" width="60%" /></p>

As many programmers will have found before me, projects quickly become warrens. Fixing one thing illumintates further issues and breaks other parts of the code. Before you know it you are in a complicated web of dispair. It quickly became clear how important tests are, especially as projects grow and become more complex as my one has. 
<!--more-->

<br>

### The beginning

On my journey to testing enlightenment my excellent boss gave me a book which became my go-to guide to learning more about the subject of testing using Python - ‘Test-Driven Development with Python’ by Harry J.W Percival available on the <a href="http://www.obeythetestinggoat.com/" target="_blank">Testing Goat website</a>. If you haven’t come across it before and would like to learn more about testing then I highly recommend it.

<p align="center"><img src="/images/TestingOne/TestGoat.png"
     alt="Sunday visit to Eagle Crag" width="40%" /></p>

The book presumes so little knowledge to get started that I think it would be suitable for most beginner programmers. It uses Python 3, Django and Selenium and explains how to download all three along with basic information about other useful tools. The book even explains the importance of version control and walks through the first commit with you.

It begins by writing the initial test to check that Django is installed and ready.

<p align="center"><img src="/images/TestingOne/FirstTest.png"
     alt="A snippet of my first test" width="80%" /></p>

This first test obviously fails as the next step is to install and set up Django! Next comes the all important commit for version control. The steps are so simple to follow and so clearly explained that it makes it fairly painless to follow and understand. There is also some useful <a href="https://docs.djangoproject.com/en/2.1/topics/testing/" target="_blank">Django documentation</a> for more information about how testing works in Django.

You can follow my journey through the book on <a href="https://github.com/jenjnif/TestingGoat" target="_blank">my Github repo</a>.
