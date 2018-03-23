---
layout: post
title: Creating a website contact page part 1 - HTTP requests
excerpt_separator: <!--more-->
---

**I have built a content form, which is great, except that when it is filled in and the submit button is pressed the information entered doesn't actually go anywhere and the user is just left with an http error message.**

To fix this I thought I could send the input to a server and then figure out how to use python to send it in an email to me. This proved harder than I initially thought.
<!--more-->
<br>
<p align="center"><img src="/images/4-http/contactpage-and-error.png"
     alt="content form screenshot" width="90%" /></p>
<br>
### Breaking it down

So I broke the problem down into steps. Firstly, what does it mean to send a request to the server? Well it involves HTTP requests; so, how do http requests work?

If we look at the error message in more detail:

<p align="center"><img src="/images/4-http/http-error.png"
     alt="content form screenshot" width="60%" /></p>

> **"**The request could not be satisfied.

> This distribution is not configured to allow the HTTP request method that was used for this request. The distribution supports only cachable requests.**"**

<br>
This bascialy means that I have not configured the distribution () to POST requests (the request method used when clicking on the 'submit' button). The distribution will only support cachable requests i.e. GET requests. So in order to make my contact form work I need to configure POST requests.

In order to understand these request it is important to understand how http works. The next few steps take you though what I did and will give an idea of how GET and PUSH requests work. 

installing it locally doesn’t affect any other python programmes that you might want to run as a global install would do

<br>
### Creating a virtual environment and installing Flask

The first thing I did was to create a virtual environment on my computer to keep all the dependences together and then install Flask here so it can all work together but is isolated. Like a little bubble of programmes and files separated away from the rest of your computer.

Steps on the command line:

<p align="center"><img src="/images/4-http/createdir.png"
     alt="command line creating virtual environment" width="80%" /></p>	

Navigate to where you want to create the directory:

	me$ cd /Documents

Create a virtual environment (a directory) and give it a name e.g.'test-server':

	me$ python3 -m venv test-server

<p align="center"><img src="/images/4-http/createdir-explained.png"
     alt="creating virtual environment explained" width="70%" /></p>

This runs the module 'venv' (virtual environment) with the parameter test-server, which creates a directory called 'test-server'.

Then to activate the virtual environment in the command line we simply write:

	me$ source test-server/bin/activate

This changes the prompt to: (virtualenvironment-name) $

In my case from this:

	me$

to this:

	(test-server) me$

<p align="center"><img src="/images/4-http/prompt-name.png"
     alt="prompt name change" width="70%" /></p>

Then we can install Flask and all it’s dependencies within the virtual environment:

	(test-server) $ pip install flask

<br>
### Using Flask

<p align="center"><img src="/images/4-http/flaskdownload.png"
     alt="command to download flask" width="80%" /></p>

Then follow the steps which can be found on the <a href="http://flask.pocoo.org/">Flask site</a>:  

Create a python file - I called mine test-python.py - containing the following:

<p align="center"><img src="/images/4-http/python-file.png"
     alt="Python code closeup" width="60%" /></p>

Then create a file containing shell script (I called mine test-shellscsript.sh) containing the file name for your python file:

<p align="center"><img src="/images/4-http/shell-script.png"
     alt="shell script closeup" width="60%" /></p>

<br>
### Opening a port to listen for an http request

Then make sure you are in the test-server directory and run the shell script file:

	(test-server) me$ cd test-server
	(test-server) me$ test-shellscript.sh
This came back with an error: -bash: test-shellscript.sh: command not found
So I checked the permissions of the files within the test-server directory to check the shell file is executable:

	(test-server) me$ ls -al

Output:
-rw-r--r--@  1 jenjones  staff   52 21 Mar 21:37 test-shellscript.sh

Which is isn’t! No x. So I ran this code to change the access permissions so the shell file was executable:

	(test-server) me$ chmod +x test-shellscript.sh

Then I ran the file again: 

	(test-server) me$ test-shellscript.sh

Again!: 

-bash: test-shellscript.sh: command not found

WTF! So this is because I need to tell it that the to run the file from the directory I am currently in, otherwise it won’t find it. This can be shown by:

	me$ echo $PATH

Which gives back the path that terminal will look in to run the script file within our directory, test-server.

/Library/Frameworks/Python.framework/Versions/3.6/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin

As you can see the virtual server directory (test-server) is not checked so we need to tell it to run the file from the directory we are in. To do this add ./ before the command:

 	(test-server) me$ ./test-shellscript.sh 

Then the shell script executes our python file and we have an http port open, listening for anything. We bound our application to a port which is listening for an http request.

<br>
### Making an http request using curl

Then we open another terminal window and make an http request using curl to that port.:

	me$ curl 127.0.0.1:5000 --verbose

This will send an http request over the port, the python script will run and send back “Hello World”
This can be seen more easily in a browser using the IP address and port.

https://www.digitalocean.com/community/tutorials/how-to-install-python-3-and-set-up-a-local-programming-environment-on-ubuntu-16-04
