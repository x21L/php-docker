# PHP Development with Docker
## Use Case
We do have some workspace with a `src` directory. We want to automatically
upload code changes in this directory to the docker container.
The container is our webserver microservice.   
We use the following technologies in the container:

* PHP 7.2 
* Apache HTTPD

You can change the PHP version easily. Since we are using Apache
HTTPD you can of course use HTML and JavaScript too.

## Prerequisites
* Installed and working Docker
* Some text editor and a shell (Windows Terminal etc.)
* The following directory structure:

~~~txt
workspace/
├── src/
│   ├── index.php
│   └── other files (this is basically your htdocs folder from XAMPP)
├── Dockerfile
└── docker-compose.yml
~~~

## Dockerfile
> **NOTE:** this file does not have a file extension!

This file is very simple and just downloads the base PHP image with 
the Apache service from [Dockerhub](https://hub.docker.com).

~~~shell
FROM php:7.2-apache
~~~
You can even change the version of php. `php:<some other versions>`.
[Here](https://hub.docker.com/_/php) can you find all the different 
versions. How to install PHP extensions is also described there.

## docker-compose
For an easier set up we write a YAML (Yet another Markup Language) file
with the desired config for our container. With `docker-compose` it is
possible to set up environments with multiple containers at once. But
we just stick with one for the beginning.

The YAML file looks like this:
~~~yaml
version: "3"
services:
  www:
    build: .
    ports:
      - "80:80"
    volumes:
      - ./src/:/var/www/html/
    networks:
      - default
~~~

**Attention:** In YAML `<space>` and `<tab>` are very important, since
there are no braces. Kinda like Python.

So what does everything mean in the file?
* `version: "3"` ... Use the third version of `docker-compose`
* `build: .` ... Use the Dockerfile in the current directory
* `- "80:80"` ... Bind the internal container port 80 with the host 
port 80
* `- ./src/:/var/www/html/` ... Maps the src folder from the host to the 
html folder of the Apache HTTPD

> `/var/www/html/` is the equivalent of the htdocs folder from XAMPP.

## Set Everything Up
We have defined our configuration files now it is time to spin 
everything up.

Open a terminal and make sure you are in the `workspace directory`. 
Type in the following command:
~~~shell
docker-compose up -d
~~~ 
`-d` enables the detached mode. You can use the terminal after the command.

Open you browser of choice and type in `localhost`.
Your `index.php` gets called.

* The container is accessible from every device in your LAN (e.g. same
Wifi). You can access it from your mobile phone, for testing the mobile 
view of your site. Just enter your computer's IP address in your 
preferred browser on your phone. Like `192.168.0.23`.

To find out your IP address with Windows open a terminal and type in
`ipconfig`. For Linux `ip a` and for Mac `ifconfig`.

## Ready
This setup is reproducible for every workspace and even other machines, 
just copy the commands. Even some IDEs recognize it and ask you set 
everything up.

**Enjoy the Docker experience!**

>**NOTE:** You can also clone this repository with your IDE and start 
working immediately.
