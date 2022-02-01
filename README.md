# LAMP 
LAMP(Linux, Apache, MySQL and PHP) is a free and open-source tech stack used for many web applications. This respository contains relevant installation/troubleshooting content related to the LAMP stack that I'd like to document. This will come in handy when I need a refresher and help my colleagues that are new. I will also include a list of useful linux commands, bash scripts and tools that I like to use in my workflow.

# Table of contents
* <a href=#lamp>Installation and configuration (Fedora)</a>
    1. <a href=#apache>Apache</a>
    2. <a href=#php>PHP</a>
    3. <a href=#mariadb>MariaDB</a>


## <a id="lamp">Installation and configuration (Fedora)</a>
## <a id="apache">Apache</a>
Here i'll be walking through the installation and configuration of an apache server on my local machine and setting up my dev environment.

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
Typically a developer will be working on multiple web applications thus learning how to set up multiple virtual hosts is essential. /var/www/html is the default directory where files are served. I prefer to create a seperate directory and leave the default directory intact.

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
It is convenient to create a symlink in the home directory to this file for easy access. We can create a symlink by navigating to the home directory(or whatever directory suits you)
```
ln -s /var/www/sites <insert name>
```

## <a id="php">PHP</a>
## <a id="maria">MariaDB</a>


