---
layout: post
title: Databases - basic building blocks
excerpt_separator: <!--more-->
---

**The aim of this project was to expand my understanding of how databases work by creating and updating a very simple database.**

<p align="center"><img src="/images/todo_list/table_sketch.jpg"
     alt="Sketch of table for database." width="60%" /></p>

Up until now my work has mainly involved bug fixes for a web application and database and I have had enough of an understanding of databases to get by. However, I am currently attempting to re-write some of the code and this has meant getting to know databases a lot better.
<!--more-->

<br>

### The specifics

Databases more often than not use a language called SQL to query data, create tables, add data etc. There are many pieces of database software which can be used to do this and all have their own twist on how they use SQL. They might be optimised for performance in different industries, types of data etc. One example of this kind of software is <a href="https://www.postgresql.org/about/" target="_blank">PostgreSQL</a>, which I decided to use for two reasons. Firstly, I use it at work so I have a vested interest in learning it. Secondly, PostgreSQL (psql) is open source, and there is a docker container for it (another thing I want to explore more).

So, what is a Docker container?? In its simplest form it is a self contained bubble, where packages can be downloaded and code can be written and kept separate from the rest of the computer. You can create Docker images and specify what programmes and packages it contains and then use these specific 'blueprints' to create other similar applications. 

Some of these 'blueprints' have already been created to make life easier. The <a href="https://hub.docker.com/_/postgres" target="_blank">postgres Docker image</a> is just that. Using it creates a container that has already been configured specifically for Postgres.

<br>

### Setting up

From the command line using the instructions from the link above:

STEP 1

Create a Docker PostgreSQL image and start it running:

    me$ docker run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -d postgres

If you want to check that the Docker container is running you can list them all:

    me$ docker ps 

<br>

STEP 2

Once the Docker container is running with the Docker psql image then run the psql application (from inside the postgres container):

    me$ docker run -it --rm --link some-postgres:postgres postgres psql -h postgres -U postgres

<br>

ERRORS

I got the following error from step 2:

    psql: FATAL: password authentication failed for user "postgres"

The reason I got this was that I had not been paying enough attention to the commands I had entered. Once you carry out step two it asks for a password, which I made up, instead of entering the correct password specified in step 1.

Before I realised this I closed everything down to start again. This created another problem. When I tried to run a container again I got the error below. As you can see when I ran 'docker ps' no container was listed as running:


<p align="center"><img src="/images/todo_list/run_error.png"
     alt="Code snippet of conflict container name error." width="80%" /></p>


I removed the container to start again:

    me$  docker rm some-postgres

Then re-ran steps 1 and 2 and this time input the password that I entered in step 1. This changes the prompt to a postgres prompt (postgres=#) which confirms that you are now running the psql application.


<p align="center"><img src="/images/todo_list/run_psql.png"
     alt="Code snippet to show psql running." width="80%" /></p>

<br>

### Building a tiny database

Once the psql application is running you can create and delete tables using SQl commands such as:

    postgres=# CREATE TABLE

    postgres=# DROP TABLE


<p align="center"><img src="/images/todo_list/create_drop_table.png"
     alt="Code snippet to show how to create and delete a table." width="80%" /></p>


Or you can create tables with column headings and then insert some data:

<p align="center"><img src="/images/todo_list/second_row.png"
     alt="Code snippet creating a table and adding data." width="80%" /></p>


So, it's pretty simple to set up a basic database from scratch. Also, very importantly, if you get stuck in the psql application to quit the postgres image just type '/q':

    postgres-# \q

