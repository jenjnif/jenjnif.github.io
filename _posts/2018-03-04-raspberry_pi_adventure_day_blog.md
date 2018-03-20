---
layout: post
title: Raspberry Pi adventure day 1
excerpt_separator: <!--more-->
---

**Last week I got a gift in the post. I love getting post, regardless of how underwhelming the content but this was extremely exciting - I opened this jiffy to find a Raspberry Pi Zero!**  


Sunday 4 March 2018 was the day I plugged it in and turned it on.  
<!--more-->
<p align="center"><img src="/images/RP_closeup.jpeg"
     alt="Raspberry Pi" width="300px" /></p>
Historically, I have not played much with computers, electronics or soldering irons but in recent years I've become more and more drawn to programming and last year I decided to finally act on my desire to learn to be a developer. This led me down a path along which, amongst huge amounts of excitement last May, I discovered Raspberry Pi. Then, about a month ago, a Raspberry Pi surprise gift package turned up in the post. Although my first foray did not go as exactly as expected - I did not solder the header onto my Raspberry Pi and create a robot worthy of fame and fortune on YouTube or even write a python program - I did manage to transform it into a server, use Jekyll to convert <a href="jenjnif.github.io">my GitHub Pages website</a> files into HTML and CSS and transfer them onto the Raspberry Pi and then view the site over the RP's network. 

<br>
**You need to plug it in**

So, first things first, I needed to figure out how to set it up - we don't all know that  Raspberry Pi's need to be plugged in. I took all the parts, laid them out and figured out what they were.

<p align="center"><img src="/images/RP.jpeg"
     alt="Raspberry Pi"/></p>

The Raspberry Pi Zero specifications can be found on the Raspberry Pi <a href="https://www.raspberrypi.org/products/raspberry-pi-zero/">website.</a> In my surprise package I also recieved: a Zero 40 Pin Header, aMicro USB/USB adaptor, aMini HDMI/HDMI adaptor and a Micro SD card 4gb.

<p align="center"><img src="/images/accessories.jpeg"
     alt="Raspberry Pi accessories"/></p>

I inserted the SD card, attached the micro usb adaptor to the USB on my keyboard and the micro HDMI cable to the HDMI on my TV monitor, switched the TV on and turned it to HDMI2. It was only then that I figured out that I had to plug the Raspberry pi into a power source! So, I hunted down a micro USB power cable and plugged that in too. This <a href="https://www.raspberrypi.org/app/uploads/2012/12/quick-start-guide-v1.1.pdf">pdf</a> was very useful when I was setting it all up.

<br>
**The Raspberry Pi booted up**

The screen flickered to life and the Raspberry Pi started to boot up. The pre-installed operating system on my SD card is Linux which uses bash so it is very similar to the command line on my mac which made using it a lot simpler. When the RP had finished booting it asked me for the default user id and password. These can be found on the <a href="https://www.raspberrypi.org/documentation/linux/usage/users.md">Raspberry Pi documentation for Linux</a>.

<p align="center"><img src="/images/RP_booting.JPG"
     alt="Raspberry Pi accessories"/></p>

Initially, I had been set the challenge of getting a python programme up and running on the RP which comes with Python already installed on it. I had a little play with that and then found that there are also pre-installed <a href="https://www.raspberrypi.org/documentation/usage/python/">python games</a> on there too. However, as I had just created a new website on Github pages, I decided I would try to get my site onto my RP instead and leave python programming and gaming for another Sunday in.

So, the next step was to try to connect the RP to wifi which turned out to be very simple following the <a href="https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md">documentation</a> on the Raspberry Pi site.

I installed <a href="https://www.raspberrypi.org/documentation/remote-access/web-server/apache.md">Apache</a> - an http server - which initially failed to install and I had no idea why. Yet another example of the multitude of times where having the help of a professional programmer proved invaluable! They recommended that I check whether there were any <a href="https://www.raspberrypi.org/documentation/raspbian/updating.md">updates for the system's package index files</a>:
	
	pi@raspberrypi:~ $ sudo apt-get update
	
Running this update command fetches packages from their locations and updates them if there is a newer version. When I tried to install Apache again it ran fine. Installing Apache creates an html folder, 'index.html', in the /var/www/html folder of the RP which you can delete, edit or replace and I wanted to send a file to my RP to replace this website holder. To securely transfer files between my local host (my Mac) and a remote host (my RP) I <a href="https://www.raspberrypi.org/documentation/remote-access/ssh/scp.md">used Secure copy protocol (SCP) which is based on SSH protocol (requires an SSH server)</a>.

SSH is pre-installed on the RP but needs to be enabled:
	
	pi@raspberrypi:~ $ sudo systemctl enable ssh

Then to start SSH running:

	pi@raspberrypi:~ $ sudo systemctl start ssh

<br>
**Preparing my site with Jekyll**

The site I wanted to use for this project was built using <a href="https://pages.github.com/">GitHub Pages</a> and is made up of HTML, CSS and markdown language. GitHub Pages uses Jekyll to automatically convert any markdown on into HTML and without this Jekyll conversion the site would not display properly. So I had two choices with my files: 1 - I could convert the markdown to HTML myself; or 2 - Use Jekyll directly to do what it does on GitHub Pages i.e. create HTML pages from any markdown language. I chose option 2.

When I downloaded <a href="https://jekyllrb.com/docs/installation/"> Jekyll</a> I ignored any additions and initially made the mistake of doing all four steps from the <a href="https://jekyllrb.com/docs/quickstart/">quick-start guide</a> not realising that this created a whole new template website for me. All I actually wanted was to use Jekyll to produce the files for my site with the markdown language all converted to HTML so I deleted the template site.

<p align="center"><img src="/images/Jekyll_QS_guide.png"
     alt="Jekyll quick-start guide" width="600px" /></p>

There are a couple of ways to create these files, both of which I tried. First, navigate to the folder containing my GitHub Pages files on my Mac then fire up a Jekyll server:

	me$ Jekyll serve

This creates a 'site' folder within my website folder containing all the converted files. Starting up a local server in this way also allows me to view my site on http://localhost:4000/ to make any changes before putting it live.

As I didn't actually need a Jekyll server for the purpose of transferring the site folder to my RP I could have done the second option:

	me$ Jekyll build

This uses Jekyll to build the 'site' folder without the need to start up a server. Either way, a 'site' folder is created with all the files converted to HTML and CSS ready for me to transfer over to my RP.

<br>
**Transferring my site to the Raspberry Pi**

My initial attempt to make a zip file of my site from the command line didn't seem to contain any files within folders, such as my image files within the images folder:

	me$ zip ../site.xml *

For this reason I compressed the files manually. Then used SCP on my Mac to transfer the 'site.zip' folder onto the Raspberry Pi:
	
	me$ scp site.zip pi@raspberrypi:~

On inputting the correct password for the RP the folder was copied over.
Then I was able to SSH into the Raspberry Pi to control it from the terminal on my Mac:

	me$ ssh pi@raspberrypi
	# Again it asks for a password.

I navigated to the directory containing the index.html file, created when Apache was downloaded, removed it and unzipped my new 'site' folder:

	# first I changed the file extension to show it was a zip file:
	pi@raspberrypi:~ $ mv site.xml site.zip

	# to change directory on my RP:
	pi@raspberrypi:~ $ cd /var/www/html/ 
 
	# to remove the index.html file:
	pi@raspberrypi:/var/www/html $ sudo rm index.html

	# Then unzip the 'sites' folder transferred earlier making sure to use the path to the home directory (~/) 		
	pi@raspberrypi:/var/www/html $ sudo unzip ~/site.zip

<p align="center"><img src="/images/mysite.png"
     alt="Image of Jen Jones' site" width="600px" /></p>

After checking that all my files were there, including the blogs and the images, I typed http://raspberrypi/ into my browser (you can also use the IP address for your Raspberry Pi) and there was my website - but no ramsey.jpg image!! After a little help and investigation I realised that the image was not showing up because the permissions of the image were set to make it available only as a read or write file for root.

	# to see permissions of all files in directory
	pi@raspberrypi:~/images $ ls -al

	# to change the permissions so the file is available for user, group and global
	pi@raspberrypi:/var/www/html/images $ sudo chmod +r ramsey.jpg

	# output showing the file is now available:
	(-rwxr--r-- 1 root root 326372 Mar  3 19:45 ramsey.jpg)

Interpreting this from right to left:

The ramsey.jpg was created at 7:45pm on March 3 and is 326372 bytes in size. 

The right hand root shows it belongs to the group root and the left hand route shows it belongs to root in particular. This could be a particular user. It 1 file. Then the file permission symbols:
	
The dash - before the rw means that this is a normal file that contains any type of data. A directory, for example, would have a d instead of a dash.
	
The rwxr are the permissions for the user. The rw means that root (the user who the file belongs to) can read and write to the file. The x means it is an executable programme which can be executed by the owner (root). The group has read permissions and there are no global permissions.

It worked, the image appeared and my site was up! 

During this project I also installed Links - a text-based browser - to view my site. I found it useful to view the site like this as it shows why it is important to use the correct HTML tags ensuring that it renders properly when using a text based browser. This is good for accessibility issues, screen readers and so on - a very important consideration when building any kind of site. I will be exploring this more whilst building a new website for a client to ensure that it is accessible for anyone wishing to visit.

I closed the connection to my Raspberry Pi and started the challenge of writing it all up for my own learning but also with others who might be trying to do a similar thing. I really enjoyed playing around with my RP and found it hugely beneficial to solidify my knowledge by writing up about it afterwards. Once step closer to my world famous robot.


