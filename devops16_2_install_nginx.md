CSS: markdown_styles.css

# DevOps16 - Install Nginx on Ubuntu 16.04

##### [Source: digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-16-04)


[Home](../index.html)
: [1 - Ubuntu 16.04 user setup](devops16_1_ubuntu16_setup.html)
: [2 - Install Nginx](devops16_2_install_nginx.html)
: [3 - Install Phusion Passenger](devops16_3_install_phusionpassenger.html)
: [4 - Install MongoDB](devops16_4_install_mongodb.html)

#### Use sudo user via ssh login for following commands (instead of `root`)


## 1 - Install Nginx

Update local apt package index.

```
sudo apt-get update
```

Install `nginx`

```
sudo apt-get install nginx
```

## 2 - Adjust the Firewall

Nginx registers itself as a service with `ufw`, our firewall, upon installation.
List application configurations that `ufw` knows how to work with.

```
sudo ufw app list
```

Three (3) profiles should be available for nginx.

```
Output
Available applications:
  Nginx Full
  Nginx HTTP
  Nginx HTTPS
  OpenSSH
```

It is recommended to enable the most restrictive profile that will still allow the traffic you've configured.
Since SSL is not yet configured, only need to allow traffic on port 80 at this time.
You can enable this by typing:

```
sudo ufw allow 'Nginx HTTP'
```

Verify the change

```
sudo ufw status
```

You should see HTTP traffic allowed in the displayed output:

```
Output
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere                  
Nginx HTTP                 ALLOW       Anywhere                  
OpenSSH (v6)               ALLOW       Anywhere (v6)             
Nginx HTTP (v6)            ALLOW       Anywhere (v6)
```


## 3 - Check Web Server

##### At the end of the installation process, Ubuntu 16.04 starts Nginx. The web server should already be up and running.

Check with the systemd init system to make sure the service is running:

```
systemctl status nginx
```

```
Output
● nginx.service - A high performance web server and a reverse proxy server
   Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2016-04-18 16:14:00 EDT; 4min 2s ago
 Main PID: 12857 (nginx)
   CGroup: /system.slice/nginx.service
           ├─12857 nginx: master process /usr/sbin/nginx -g daemon on; master_process on
           └─12858 nginx: worker process
```

##### Verify by accessing the default Nginx landing page through your server's domain name or IP address. If a domain name is not set up for server, learn how to [set up a domain with DigitalOcean here.](https://digitalocean.com/community/articles/how-to-set-up-a-host-name-with-digitalocean) If you do not want to set up a domain name for your server, use server's public IP address.


To find IP address from the command line:
You will get back a few lines. You can try each in your web browser to see if they work.

```
ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\/.*$//'
```


An alternative, should give you your public IP address as seen from another website:

```
sudo apt-get install curl
curl -4 icanhazip.com
```

When you have your server's IP address or domain, enter it into your browser's address bar:

```
http://server_domain_or_IP
```

##### You should see the default Nginx landing page, which should look something like this:

<img src='https://assets.digitalocean.com/articles/nginx_1604/default_page.png' />



## Step 4: Manage the Nginx Process

##### Now that you have your web server up and running, we can go over some basic management commands.

To stop web server:

```
sudo systemctl stop nginx
```
To start web server when it is stopped:

```
sudo systemctl start nginx
```

To stop and then start the service again:

```
sudo systemctl restart nginx
```

If simply making config changes, Nginx can often reload without dropping connections. Use:

```
sudo systemctl reload nginx
```

To disable Nginx auto-start up at boot:

```
sudo systemctl disable nginx
```

To re-enable the service to start up at boot:

```
sudo systemctl enable nginx
```

## Step 5: Get Familiar with Important Nginx Files and Directories

##### Now that you know how to manage the service itself, you should take a few minutes to familiarize yourself with a few important directories and files.

### Content

* `/var/www/html`: The actual web content, which by default only consists of the default Nginx page you saw earlier, is served out of the `/var/www/html` directory. This can be changed by altering Nginx configuration files.

### Server Configuration

* `/etc/nginx`: The nginx configuration directory. All of the Nginx configuration files reside here.
* `/etc/nginx/nginx.conf`: The main Nginx configuration file. This can be modified to make changes to the Nginx global configuration.
* `/etc/nginx/sites-available`: The directory where per-site "server blocks" can be stored. Nginx will not use the configuration files found in this directory unless they are linked to the `sites-enabled` directory (see below). Typically, all server block configuration is done in this directory, and then enabled by linking to the other directory.
* `/etc/nginx/sites-enabled/`: The directory where enabled per-site "server blocks" are stored. Typically, these are created by linking to configuration files found in the `sites-available` directory.
* `/etc/nginx/snippets`: This directory contains configuration fragments that can be included elsewhere in the Nginx configuration. Potentially repeatable configuration segments are good candidates for refactoring into snippets.


### Server Logs

* `/var/log/nginx/access.log`: Every request to your web server is recorded in this log file unless Nginx is configured to do otherwise.
* `/var/log/nginx/error.log`: Any Nginx errors will be recorded in this log.

### Conclusion

##### Now that you have your web server installed, you have many options for the type of content to serve and the technologies you want to use to create a richer experience.

##### Learn [how to use Nginx server blocks](https://www.digitalocean.com/community/articles/how-to-set-up-nginx-server-blocks-virtual-hosts-on-ubuntu-14-04-lts) here. If you'd like to build out a more complete application stack, check out this article on [how to configure a LEMP stack on Ubuntu 14.04](https://www.digitalocean.com/community/articles/how-to-install-linux-nginx-mysql-php-lemp-stack-on-ubuntu-14-04).





<div class='footnotes'>
<p>These step-by-step instructions are taken from various tutorials on <a href="https://digitalocean.com">digitalocean.com</a>, <a href="https://www.phusionpassenger.com">phusionpassenger.com</a>, and other sites. Some sources have been linked. Most step descriptions have been shortened and simplified. Some step sequences are from one source while other may come from various sources.</p>
<p>These instructions and commands are placed on GitHub so I can conveniently find them. I am a novice with Ubuntu, Nginx, Passenger, command line, etc., so I probably can&#8217;t answer any questions. However, I&#8217;ll be glad to incorporate any corrections that are needed.</p>
<p><em>Use instructions and commands at your own risk.</em></p>

<div class='creative-commons'>
  <a class="creative-commons-image" href="https://creativecommons.org/licenses/by-nc-sa/4.0/">
	<img rel="license" alt="Creative Commons License" src="/assets/community/creativecommons-08b32a9279fcd47fcd78ac6a26331389.png"></a>
    <p>
		This work is licensed under a <a rel="license" href="https://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.
		</p>
</div>
</div>
