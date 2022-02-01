# LAMP 
LAMP(Linux, Apache, MySQL and PHP) is a free and open-source software stack used for many web applications. This repository contains relevant installation/troubleshooting content related to the LAMP stack that I'd like to document. This will come in handy when I need a refresher and help any of my colleagues that are new. I will also include a list of useful linux commands, bash scripts and tools that I like to use in my workflow.

# Table of contents
* <a href=#lamp>Installation and configuration (Fedora)</a>
    1. <a href=#apache>Apache</a>
    2. <a href=#php>PHP</a>
    3. <a href=#mariadb>MariaDB</a>


## <a id="lamp">Installation and configuration (Fedora)</a>
## <a id="apache">Apache</a>
Here i'll be walking through the installation and configuration of an apache server for my local dev environment.

If apache is not already installed, you can install it using:
```
sudo dnf install httpd
```

You can start the apache server using:
```
sudo systemctl start httpd
```

And check the status of your server using:
```
sudo systemctl status httpd
```

### Configuring virtual hosts
Typically, a developer will be working on multiple web applications thus learning how to set up multiple virtual hosts is essential. Before we create and configure virtual hosts we need to create the relevant directories and set appropriate file permissions. ```/var/www/html``` is the default directory where files are served. I prefer to create a seperate directory and leave the default directory intact.

Here I am going to create a directory called sites using:
```
sudo mkdir -p /var/www/sites
```
We need to change the owner and file permissions so we that can read and write to this directory.
```
sudo chown -R $user:$user /var/www/sites/
```
```
sudo chmod -R 755 /var/www/sites/
```

It is convenient to create a symlink in the home directory to this file for easy access. We can create a symlink by navigating to the home directory(or whatever directory suits you).
```
ln -s /var/www/sites <insert path>
```

For example:
```
ln -s /var/www/sites ~/sites
```
or
```
ln -s /var/www/sites /home/<user>/sites
```
Now let's create a virtual host for the domain: example.test
First, let's ```cd``` into symlink we had just created and create a new directory called example.test with an index.html file
```
mkdir example.test
cd example.test
echo 'Hooray!' > index.html
```

Now we are ready to start configuring our virtual hosts. The apache config files can be found within ```/etc/httpd/```. Here we can create a directory for our config files using:
```
sudo mkdir vhosts
```
Now we need to instruct apache to search for virtual host files within this directory we can do this by editing the ```/etc/httpd/conf/httpd.conf``` file with sudo.
```
sudo vim /etc/httpd/conf/httpd.conf
```
At the end of the file append the following:
```
IncludeOptional vhosts/*.conf
```

Now let's create our virtual host file for example.test
```
cd /etc/httpd/vhosts
sudo vim example.conf
```
Write the following to your configuration file
```
<VirtualHost *:80>
    ServerName example.test
    ServerAlias www.example.test
    DocumentRoot /var/www/sites/example.test
</VirtualHost>
```
The number in the opening tag ```<VirtualHost *:80>``` refers to the port that apache will be listening on for requests to this domain.
```ServerName``` refers to the primary domain name that apache will use to identify the root directory that it will serve.
```ServerAlias``` refers to the alternate domain names that point to your primary domain eg. www.example.test.
When you create new vhosts simply copy the file and swap out example.test with your own domain and update the document root.

Since we are hosting our websites locally, the DNS will not resolve to a valid ip. We will need to modify the ```/etc/hosts``` file and point the domain to our local machine. 

```
sudo vim /etc/hosts
```
Append the following to the end of the file:
```
127.0.0.1   example.test www.example.test
```
You will need to restart apache everytime create a new virtual host or update the coniguration files Now restart apache using:
```
sudo systemctl restart httpd
```
We're all set!
Now type in http://example.test in your browser and you should see the content of the index.html we created not to long ago. (we use http:// since we have not set up SSL yet).

# Troubleshooting
* If you encounter a 403 error this likely means that you haven't set up the correct permissions for your /var/www/<domain> directory.
* Some people may also encounter this error due to selinux (Security Enhanced Linux) which is a security modile built into the linux kernel. It is possible to disable this feature via ```sudo setenforce 0``` but if you've all the permissions set up correctly you won't have to.
* If you get redirected to the default welcome page that means you either: 
    1. Made a typo in the virtual host file.
    2. Did not update the document root in the virtual host file.
    3. Forgot to include your config file directory in /etc/httpd/conf/httpd.conf
    3. index.html file was not created in the right directory.
If you made any changes remember to restart apache and see if the problem is resolved.

Of course there will be extra steps required to enable https on 443 or configuring apache on live server connected to the internet. I may cover this later on.
## <a id="php">PHP</a>
## <a id="maria">MariaDB</a>


