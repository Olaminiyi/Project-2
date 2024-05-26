# Deploying a LEMP Stack Application On AWS Cloud

A LEMP Stack application is an application which as opposed to a [LAMP Stack Application](https://github.com/Olaminiyi/Project-1) makes use of Nginx as the web server for hosting the web application. NGINX is an open source software for web serving, reverse proxying, caching, load balancing, media streaming, and more.

### Creating an Ubuntu EC2 Instance

Login to AWS Cloud Service console and create an Ubuntu EC2 instance. The virtual machine is a linux operating system which serves as the backbone for the LEMP Stack web application.

![alt text](images/2.1.png)

Login into the instance via ssh:

![alt text](images/2.2.png)

### Installing Nginx

Run a `sudo apt update` to download package information from all configured sources.

![alt text](images/2.3.png)
```
sudo apt install nginx
```
![alt text](images/2.4.png)

Spin up the nginx server and ensure it automatically starts on system reboot by running the following commands

```
sudo systemctl start nginx
sudo systemctl enable nginx
```
Run `systemctl status nginx` to check if the installation succeeds. A green text color shows that the server is active.

![alt text](images/2.5.png)

Accessing the default nginx web server block to see if everything works correctly. curl the local IP address of our local machine which in most case is `127.0.0.1` or the DNS name localhost on any web browser on our local machine.
```
curl http://127.0.0.1:80 or curl http://localhost:80
```

The below result shows nginx has been properly set up and we can deploy our web application.

![alt text](images/2.6.png)

It appears our default nginx server is accessible locally on our local machine. To check if we can access the default server block over the internet on our local machine, insert the public IP address of the server on a browser.

![alt text](images/2.7.png)

The search failed because there was no access to our nginx webserver over the internet even though we were using a public IP address. To correct this, we need to configure inbound security group rules for ubuntu ec2 instance on aws cloud. Enable TCP port `80`

![alt text](images/2.8.png)

When we insert our public IP address on our browser, the nginx default webpage shows up showing that the webserver block is now visible from the internet via the port 80 which was opened.

![alt text](images/2.9.png)