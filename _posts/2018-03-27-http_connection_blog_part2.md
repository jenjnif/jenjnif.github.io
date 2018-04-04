---
layout: post
title: Creating a website contact page part 2 - simple HTTP request example
excerpt_separator: <!--more-->
excerpt_image: images/4-http/http-diagram-annotated.JPG
---

**In part 1 of 'Creating a website contact page' I discovered that in order to submit the information entered in the contact form stored on Amazon S3 I need to create a different server to handle POST requests**

The second in my series of HTTP request articles walks through how to build a very simple example of an HTTP request to get a better understanding of how they work.
<!--more-->

Links to the other posts in the HTTP series:

1.	<a href="/2018/03/22/http_basics_part1_blog.html">What is an HTTP request and how do basic HTTP requests work?</a>

2. 	<a href="/2018/03/27/http_connection_blog_part2.html">A simple example of an HTTP request using Flask and Python.</a>

3. 	Building a server to deal with my contact form - coming soon

4. 	Serverless - another way to solve the contact form problem. - coming soon

<br>
### Quick recap

So, let's just quickly recap on how basic HTTP requests work. HTTP GET requests aquire data from the server and send it back to the browser/client:

<p align="center"><img src="/images/4-http/http-diagram-annotated.JPG"
     alt="http diagram" width="90%" /></p>

1. A client (browser) submits an HTTP request for a resource (data) to a server (GET/ HTTP/1.1) over a TCP connection

2. The server returns a response to the client

3. The response contains status information about the request and may contain the requested content. Assuming the data request is successful it responds with a message to say "I understand, here is your requested data" (HTTP/1.1 200 OK) along with the requested data. 

The server must be listening on a port on a computer with an IP address linked to the domain name. But dont we already deal with a GET http request to show the form HTML itself? In my case the answer is 'sort of'... 

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
