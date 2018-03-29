---
layout: post
title: Creating a website contact page part 1 - basic HTTP requests
excerpt_separator: <!--more-->
---

**Recently, I used HTML and CSS to build a contact form for a website, which I then hosted on AWS using S3 buckets. However, when the form was filled in and the submit button pressed, the information entered stayed put and an error message appeared. So... how do I fix this?**

The answer to this problem lies in HTTP requests and more specifically how S3 deals with them. 
<!--more-->

<p align="center"><img src="/images/4-http/contactpage-and-error.png" alt="content form screenshot" width="90%" /></p>

The next four posts will explore how to use HTTP requests to submit information entered in contact forms and then sent it in an email to a chosen recipient.

1.	<a href="/2018/03/22/http_basics_part1_blog.html">What is an HTTP request and how do basic HTTP requests work?</a>

2. 	<a href="/2018/03/27/http_connection_blog_part2.html">A simple example of an HTTP request using Flask and Python.</a>

3. 	 Building a server and using HTTP to submit information from a contact form - coming soon

4. 	Serverless: an alternative way to solve the contact form problem - coming soon

<br>
### HTTP requests

HTTP enables communications between clients and servers. There are a few different types of HTTP requests, GET and POST being the most common. As you can infer from the name, HTTP GET requests 'get' data from the server. This is how webpages are returned and displayed for us.
The diagram below is a simplified version of how HTTP GET requests aquire data from the server and send it back to the browser/client:

<p align="center"><img src="/images/4-http/http-diagram-annotated.JPG"
     alt="http diagram" width="90%" /></p>

1. A client (browser) submits an HTTP request for a resource (data) to a server (GET/ HTTP/1.1) over a TCP connection.

2. The server returns a response to the client via the TCP connection.

3. The response contains status information about the request - if the data request is successful it responds with a message to say "I understand, here is your requested data" (HTTP/1.1 200 OK) along with the requested data and the TCP connection is closed.

For a more detailed explanation of GET requests and further information on different types of HTTP requests and erros this <a href="https://www.codecademy.com/articles/http-requests" title="http article codeacademy">article from codeacademy.com</a> is a great introduction.

<br>
### Submit buttons & HTTP POST requests

When creating a form using HTML, you might write something like this to build a submit button:

<p align="center"><img src="/images/4-http/submit-snippet-annotated.png" alt="example submit button code" width="90%" /></p>

The HTML defines a submit button, which when pressed, performs a POST HTTP request to a specified URL. This sends the form data to a specified web page on the server.

When the submit button is clicked on my contact form this Amazon S3 built-in error message is sent back:

<p align="center"><img src="/images/4-http/http-error.png"
     alt="content form screenshot" width="90%" /></p>

In short, this error means that the HTTP request failed! In order to send the input submitted over to the server I need to configure the form to handle POST requests. Why we need to do this should become much clearer when we look at how submit buttons are built.

<br>
### Amazon S3

Amazon Simple Storage Service (S3) is an object storage service where all my site files are stored in a resource (what AWS call a 'bucket'). Amazon S3 has the added functionallity of being configurable to host a static site, which means it will serve my HTML files to the public but with no dynamic componant. 

As previously mentioned, HTTP GET requests are fundamental in loading webpages; if S3 could not handle GET requests my website would not load. However, something else will be required to handle POST requests.

In the next post I will be building <a href="/2018/03/27/http_connection_blog_part2.html">a simple example of an HTTP request using Flask and Python</a> to discover more about how HTTP works.



