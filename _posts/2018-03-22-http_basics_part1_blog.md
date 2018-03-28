---
layout: post
title: Creating a website contact page part 1 - basic HTTP requests
excerpt_separator: <!--more-->
---

**Recently, I built a contact form for a website using HTML and CSS. However, when the form was filled in and the submit button pressed, the information entered stayed put and an error message appeared. So... how do I fix this?**

The answer to this problem lies in HTTP requests. 

<p align="center"><img src="/images/4-http/contactpage-and-error.png" alt="content form screenshot" width="90%" /></p>

The next four posts will explore how to use HTTP requests to submit information entered in contact forms and then sent it in an email to a chosen recipient.
<!--more-->

1.	<a href="/2018/03/22/http_basics_part1_blog.html">What is an HTTP request and how do basic HTTP requests work?</a>

2. 	<a href="/2018/03/27/http_connection_blog_part2.html">A simple example of an HTTP request using Flask and Python.</a>

3. 	 Building a server and using HTTP to submit information from a contact form - coming soon

4. 	Serverless: an alternative way to solve the contact form problem - coming soon

<br>
### Breaking it down

When you create a form using HTML, you will might write something like this to build a submit button:

<p align="center"><img src="/images/4-http/submit-snippet-annotated.png" alt="example submit button code" width="80%" /></p>

Let's think for a minute about how the form submit button works:
The HTML defines a submit button, which when pressed, performs a POST HTTP request to the specified url sedning the form data to a web page on the server.

Without a server side url to post data to, I currently have no way of dealing with this request. Somehow, I need to deal with this request and then use server side Python to email me the contents.

The question is- how does one deal with a http request? The answer is ultimately to have a server listening on a port on a computer with an ip address linked to the domain name. But dont we already deal with a GET http request to show the form HTML itself? In my case the answer is 'sort of'... I am hosting the HTML files in s3 which handles http GET requests and returns the pages. However, something else will be required for the POST.

<br>
### HTTP requests

So, I broke the problem down into steps. Firstly, what does it mean to send a request to the server? Well, it involves HTTP, which enables communications between clients and servers. There are a few different types of HTTP requests, GET and POST being the most common. The diagram below is a simplified version of how HTTP GET requests aquire data from the server and sends it back to the browser/client:

<p align="center"><img src="/images/4-http/http-diagram-annotated.JPG"
     alt="http diagram" width="90%" /></p>

1. A client (browser) submits an HTTP request for a resource (data) to a server (GET/ HTTP/1.1) over a TCP connection

2. The server returns a response to the client

3. The response contains status information about the request and may contain the requested content. Assuming the data request is successful it responds with a message to say "I understand, here is your requested data" (HTTP/1.1 200 OK) along with the requested data. 

For more information on different types of http requests and erros this <a href="https://www.codecademy.com/articles/http-requests" title="http article codeacademy">article</a> from codeacademy.com is a great introduction.

<br>
### My contact form

When submit is clicked on the contact form I have built an error message is sent back:

<p align="center"><img src="/images/4-http/http-error.png"
     alt="content form screenshot" width="90%" /></p>

## **"** The request could not be satisfied.

## This distribution is not configured to allow the HTTP request method that was used for this request. The distribution supports only cachable requests.**"**

This is clear when you post to S3 as this is the error it gives.
This basically means that I have not configured the distribution to POST requests (the request method used when clicking on the 'submit' button). The distribution will only support cachable requests i.e. GET requests. So in order to make my contact form work I need to configure POST requests.

To send the input submitted on the contact form over to the server I will need to use POST requests, these follow the same principals and I will cover them later.