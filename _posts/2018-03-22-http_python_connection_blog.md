---
layout: post
title: Creating a website contact page part 1 - HTTP requests
excerpt_separator: <!--more-->
---

**I recently built a content form which looks pretty good. The only problem is that when the submit button is pressed the information entered doesn't actually go anywhere and the user is left with an error message.**

The end goal for my contact form is to have the information stored and sent to me in the form of an email. To achieve this I knew I would have to send a request to the server and then use server side Python to email me the data but I wasn't entirely sure where to start.

<!--more-->

<p align="center"><img src="/images/4-http/contactpage-and-error.png"
     alt="content form screenshot" width="90%" /></p>

### Breaking it down

So, I broke the problem down into steps. Firstly, what does it mean to send a request to the server? Well, it involves HTTP, which enables communications between clients and servers - here's how http requests work.

<p align="center"><img src="/images/4-http/http-diagram-annotated.JPG"
     alt="http diagram" width="90%" /></p>

There are a few different types of HTTP requests, GET and POST being the most common. The above diagram is a simplified version of how HTTP GET requests aquire data from the server and sends it back to the browser/client.

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

This bascialy means that I have not configured the distribution to POST requests (the request method used when clicking on the 'submit' button). The distribution will only support cachable requests i.e. GET requests. So in order to make my contact form work I need to configure POST requests.

To send the input submitted on the contact form over to the server I will need to use POST requests, these follow the same principals and I will cover them later.

<br>
### Creating a virtual environment and installing Flask

The first thing I did was to create a virtual environment on my computer to keep all the dependences together and then install Flask here so it can all work together but is isolated. Installing Flash locally doesn’t affect any other python programmes that you might want to run as a global install would do. Like a little bubble of programmes and files separated away from the rest of your computer.

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
     alt="command to download flask" width="70%" /></p>

Then follow the steps which can be found on the <a href="http://flask.pocoo.org/">Flask site</a>:  

Create a python file - I called mine test-python.py - containing the following:

<p align="center"><img src="/images/4-http/python-file.png"
     alt="Python code closeup" width="70%" /></p>

Then create a file containing shell script (I called mine test-shellscsript.sh) containing the file name for your python file:

<p align="center"><img src="/images/4-http/shell-script.png"
     alt="shell script closeup" width="70%" /></p>

<br>
### Opening a port to listen for an http request

In order for our application to be able to listen for an http request it needs to be bound to an open port. The next steps are how I did this.

Making sure you are in the test-server directory, run the shell script file:

	(test-server) me$ cd test-server
	(test-server) me$ test-shellscript.sh

The first time I did this it came back with an error: 

	-bash: test-shellscript.sh: command not found

This is something I am getting familiar with now as it is often caused by the file permissions. I checked the permissions of the files within the test-server directory to see if the shell file was executable:

	(test-server) me$ ls -al

<p align="center"><img src="/images/4-http/nonexecutable-permissions.png"
     alt="code to show permissions" width="80%" /></p>

From the output for the test-shellscript.sh file you can see it is not executable as there would be an x in the permissions:

	-rw-r--r--@  1 me  staff   52 21 Mar 21:37 test-shellscript.sh

In order to change the access permissions I ran a line of code to make the shell file executable. The output below now shows the x for root, user and global permissions:

	(test-server) me$ chmod +x test-shellscript.sh

<p align="center"><img src="/images/4-http/executable-permissions-closeup.png"
     alt="code to show permissions" width="80%" /></p>

Then I ran the file again but got the same error!: 

	(test-server) me$ test-shellscript.sh

	-bash: test-shellscript.sh: command not found

The reason for this is that when you try to run a programme in the shell (bash/the command line) it looks in the path for executables. You can see the paths bash searches by writing the following:

	me$ echo $PATH

	/Library/Frameworks/Python.framework/Versions/3.6/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin


<p align="center"><img src="/images/4-http/path.png"
     alt="how to use path" width="80%" /></p>

For instance when you type the ls command into bash it looks in the above path of directories, finds the programme called 'ls' that lives in user/bin and runs it. As you can see the virtual server directory (test-server) is not listed in the path. Unless I add the test-server directory to path or tell bash where to run the shell script from i.e. the test-server, it will come back with an error as bash will not be able to find it.

I chose to run the shell 'test-shellscript.sh' by telling bash where to find it i.e. the directory I am currently in. To do this add ./ before the command:

	(test-server) me$ ./test-shellscript.sh 

Finally, no more errors and the shell script runs, executes the python file, binding the application to a port. This means the port is open, listening for an http request.

<br>
### Making an http request using curl

Then we open another terminal window and make an http request using curl to that port.:

	me$ curl 127.0.0.1:5000 --verbose

This will send an http request over the port, the python script will run and send back “Hello World”
This can be seen more easily in a browser using the IP address and port.

https://www.digitalocean.com/community/tutorials/how-to-install-python-3-and-set-up-a-local-programming-environment-on-ubuntu-16-04
