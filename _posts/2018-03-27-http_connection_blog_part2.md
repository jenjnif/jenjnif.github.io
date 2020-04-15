---
layout: post
title: Creating a website contact page part 2 - simple HTTP request example
filter: http
excerpt_separator: <!--more-->
excerpt_image: images/4-http/http-diagram-annotated.JPG
---

**In part 1 of 'Creating a website contact page' I discovered that in order to submit the information entered in the contact form, stored on Amazon S3, I need to create a different server to handle POST requests**

The second in my series of HTTP request articles walks through how to build a very simple example of an HTTP request to get a better understanding of how they work.
<!--more-->

Other HTTP series posts:

1.	<a href="/2018/03/22/http_basics_part1_blog.html">What is an HTTP request and how do basic HTTP requests work?</a>

2. 	<a href="/2018/03/27/http_connection_blog_part2.html">A simple example of an HTTP request using Flask and Python.</a>

3. 	Building a server to deal with my contact form - coming soon

4. 	Serverless - another way to solve the contact form problem. - coming soon

<br>
### Quick recap

So, to quickly recap on how basic HTTP requests work; HTTP GET requests aquire data from the server and send it back to the browser/client:

<p align="center"><img src="/images/4-http/http-diagram-annotated.JPG"
     alt="http diagram" width="90%" /></p>

1. A client (browser) submits an HTTP request for a resource (data) to a server (GET/ HTTP/1.1) over a TCP connection

2. The server returns a response to the client

3. The response contains status information about the request and may contain the requested content. Assuming the data request is successful it responds with a message to say "I understand, here is your requested data" (HTTP/1.1 200 OK) along with the requested data. 

<br>
### Creating a virtual environment and installing Flask

In order to build an example of an HTTP request it is a good idea to create an isolated Python development environment to work in. This will keep it separate from any other libraries already installed. The environment can then be kept for further development work or easily deleted. Creating this local environment is like a little bubble of programmes and files separated away from the rest of the computer.

I created a virtual environment using the 'venv' module and installed Flask, a microframework for Python which I used to facilitate HTTP requests.

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

Then, to activate the virtual environment in the command line:

	me$ source test-server/bin/activate

This changes the prompt to: (virtualenvironment-name) $

<p align="center"><img src="/images/4-http/prompt-name.png"
     alt="prompt name change" width="70%" /></p>

Then install Flask and all it’s dependencies within the virtual environment:

	(test-server) $ pip install flask

<p align="center"><img src="/images/4-http/flaskdownload.png" alt="command to download flask" width="70%" /></p>

<br>
### Using Flask

To set up Flask simply follow these steps which can be found on the <a href="http://flask.pocoo.org/">Flask site</a>:  

Firstly, create a Python file - I called mine test-python.py - containing the following:

<p align="center"><img src="/images/4-http/python-file.png"
     alt="Python code closeup" width="70%" /></p>

Then create a file containing shell script - I called mine test-shellscsript.sh - containing the file name for your Python file:

<p align="center"><img src="/images/4-http/shell-script.png"
     alt="shell script closeup" width="70%" /></p>

<br>
### Opening a port to listen for an HTTP request

In order for our application to be able to listen for an HTTP request it needs to be bound to an open port. The next steps are how I did this.

Making sure you are in the test-server directory, run the shell script file:

	(test-server) me$ cd test-server
	(test-server) me$ test-shellscript.sh

The first time I did this it came back with an error: 

	-bash: test-shellscript.sh: command not found

This is something I am getting familiar with now and it is often caused by file permissions. I checked the permissions of the files within the test-server directory to see if the shell file was executable:

	(test-server) me$ ls -al

<p align="center"><img src="/images/4-http/nonexecutable-permissions.png"
     alt="code to show permissions" width="80%" /></p>

From the output for the test-shellscript.sh file you can see it is not executable as there would be an 'x' in the permissions.

In order to change the access permissions of the shell script file I ran this code:

	(test-server) me$ chmod +x test-shellscript.sh

The output below now shows an 'x' for root, user and global permissions, meaning they are all executable:

<p align="center"><img src="/images/4-http/executable-permissions-closeup.png"
     alt="code to show permissions" width="80%" /></p>

I ran the file again but got the same error!: 

	(test-server) me$ test-shellscript.sh

	-bash: test-shellscript.sh: command not found

The reason for this is that to run a programme in the shell it looks in the PATH for executables. For instance when you type the 'ls' command into bash it looks in the path of directories, finds the programme called 'ls' that lives in usr/bin and runs it. You can see the paths bash searches using the command:
	
	echo $PATH

<p align="center"><img src="/images/4-http/path.png"
     alt="how to use path" width="80%" /></p>

As you can see the virtual server directory (test-server) is not listed in PATH. Unless I add the test-server directory to PATH or tell bash where to run the shell script from i.e. the test-server, it will come back with an error as bash will not be able to find it.

I chose to run the shell 'test-shellscript.sh' by telling bash where to find it i.e. the directory I am currently in. To do this I just added './' before the command:

	(test-server) me$ ./test-shellscript.sh 

This time the shell script ran without any errors, executing the Python file and binding the application to a port. At this point the port is open, listening for an HTTP request.

<br>
### Making an HTTP request using curl

The next step is to make an HTTP request using curl to the port in a new terminal window:

	me$ curl 127.0.0.1:5000 --verbose

This sends an HTTP request over the port. The Python script runs and sends back “Hello World”. 

This can be seen in the graphic below:
1. Run the shell script. This opens a port and the server is ready and waiting for an HTTP request.
2. In another bash window (representing the client) we send an HTTP GET request to the open port using curl.
3. The Python script is run on the server...
4. ...which sends over the 'Hello World' response.

<p align="center"><img src="/images/4-http/server-running-hello.png"
     alt="the two sides of the GET request in bash" width="80%" /></p>

The result of the request can be seen more easily in a browser using the IP address and port:

<p align="center"><img src="/images/4-http/browser-hello.png"
     alt="hello world in browser" width="80%" /></p>

My next HTTP post will walk through how I built a server to deal with my contact form.

