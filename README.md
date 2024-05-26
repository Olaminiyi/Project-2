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

### Installing MySQL

We have succeeded in setting up our nginx webserver and ensured its accessible over the internet. Next is to install mySQL which is a relational database management server to help store data and manage content on our web application.

Run `sudo apt install mysql-server`

![alt text](images/2.10.png)

Provide added security to our mysql server by running the code below. This script will remove some insecure default settings and lock down access to our database system.
```
sudo apt install mysql-secure_installation
```

![alt text](images/2.11.png)

On each prompt that comes press Y , add a password and save.

With mysql_server successfully configured, login into the mysql server.

`sudo mysql`

![alt text](images/2.12.png)

To exit from the web server we enter `exit`

![alt text](images/2.13.png)

### Installing PHP and its Modules

We use php to dynamically display contents of our webpage to users who make requests to the webserver.
Run
```
sudo apt install php-fpm php-mysql
```
> [!NOTE]
> php-fpm : which stands for PHP FastCGI Process Manager is a web tool used for speeding up the performance of a website by handling tremendous amounts of load simultaneously.

![alt text](images/2.14.png)

### Creating a Web Server Block For our Web Application
To serve our webcontent on our webserver, we create a directory for our project inside the `/var/www/` directory.
```
sudo mkdir /var/www/projectlempstack 
```
Then we change permissions of the projectlampstack directory to the current user system
```
sudo chown -R $USER:$USER /var/www/projectlempstack
```
![alt text](images/2.15.png)

Creating a configuration for our server block
```
sudo nano /etc/nginx/sites-available/projectlempstack
```

The following snippets represents the configuration required for our web server block to be functional

```
#/etc/nginx/sites-available/projectlempstack

server {
    listen 80;
    server_name projectlempstack www.projectlempstack;
    root /var/www/projectlempstack;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
```
We then link the configuration file to the sites-enabled directory
```
sudo ln -s /etc/nginx/sites-available/projectlempstack /etc/nginx/sites-enabled
```
To test our configuration for errors we run
```
sudo nginx -t
```
![alt text](images/2.16.png)

Currently our new server block has been created and configured but currently the default server block is the default block that comes with nginx install. To unlink it we 
```
sudo unlink /etc/sites-available/default
```
We then reload nginx for all configurations to take effect 
```
sudo reload nginx
```

Create an index.html file inside `projectlempstack` directory and write in contents to be accessed over the internet. Paste public IP address on a browser to see content.
http://<public-ip>:80

![alt text](images/2.17.png)

### Serving PHP Using Nginx

Create an info.php file inside the `/var/www/projectlempstack` directory.

On a browser enter `http://<public-ip>/info.php`

![alt text](images/2.18.png)

