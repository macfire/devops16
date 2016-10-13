CSS: markdown_styles.css

# DevOps16 - Install Phusion Passenger + nginx on Ubuntu 16.04

##### [Source: phusionpassenger.com](https://www.phusionpassenger.com/library/install/nginx/install/oss/xenial/)


[Home](../index.html)
: [1 - Ubuntu 16.04 user setup](devops16_1_ubuntu16_setup.html)
: [2 - Install Nginx](devops16_2_install_nginx.html)
: [3 - Install Phusion Passenger](devops16_3_install_phusionpassenger.html)
: [4 - Install MongoDB](devops16_4_install_mongodb.html)
: [5 - Python venv](devops16_5_python_venv.html)
: [6 - Deploy via Git](devops16_5_deploy_flask_app_w_git.html)


### Use sudo user via ssh login for following commands (instead of `root`)


## 1 - Install Passenger packages

Update local apt package index.

```
sudo apt-get update
```

These commands will install Passenger + Nginx through Phusion's APT repository. If Nginx is already installed, then these commands will upgrade Nginx to Phusion's version (with Passenger compiled in).

```
# Install our PGP key and add HTTPS support for APT
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 561F9B9CAC40B2F7
sudo apt-get install -y apt-transport-https ca-certificates

# Add our APT repository
sudo sh -c 'echo deb https://oss-binaries.phusionpassenger.com/apt/passenger xenial main > /etc/apt/sources.list.d/passenger.list'
sudo apt-get update

# Install Passenger + Nginx
sudo apt-get install -y nginx-extras passenger
```


## Step 2: enable the Passenger Nginx module and restart Nginx


Open `/etc/nginx/nginx.conf`

```
sudo nano /etc/nginx/nginx.conf
```

Uncomment include `/etc/nginx/passenger.conf;`.

```
# include /etc/nginx/passenger.conf;

# Remove the '#' characters, like this:
include /etc/nginx/passenger.conf;
```

If you don't see a commented version of include `/etc/nginx/passenger.conf;` inside nginx.conf, then you need to insert it yourself. Insert it into `/etc/nginx/nginx.conf` inside the `http` block. For example:

```
...

http {
    include /etc/nginx/passenger.conf;
    ...
}
```
When you are finished with this step, restart Nginx:

```
$ sudo service nginx restart
```

## 3 - Check installation

After installation, validate the install by running `sudo /usr/bin/passenger-config validate-install`.
All checks should pass. If any of the checks do not pass, please follow the suggestions on screen.

```
$ sudo /usr/bin/passenger-config validate-install
 * Checking whether this Phusion Passenger install is in PATH... ✓
 * Checking whether there are no other Phusion Passenger installations... ✓
```


Check whether Nginx has started the Passenger core processes.
Run `sudo /usr/sbin/passenger-memory-stats`.
You should see Nginx processes as well as Passenger processes.

```
$ sudo /usr/sbin/passenger-memory-stats
Version: 5.0.8
Date   : 2015-05-28 08:46:20 +0200
...

---------- Nginx processes ----------
PID    PPID   VMSize   Private  Name
-------------------------------------
12443  4814   60.8 MB  0.2 MB   nginx: master process /usr/sbin/nginx
12538  12443  64.9 MB  5.0 MB   nginx: worker process
### Processes: 3
### Total private dirty RSS: 5.56 MB

----- Passenger processes ------
PID    VMSize    Private   Name
--------------------------------
12517  83.2 MB   0.6 MB    PassengerAgent watchdog
12520  266.0 MB  3.4 MB    PassengerAgent server
12531  149.5 MB  1.4 MB    PassengerAgent logger
...
```

If you do not see any Nginx processes or Passenger processes, then you probably have some kind of installation problem or configuration problem. Please refer to the [troubleshooting guide](https://www.phusionpassenger.com/library/admin/nginx/troubleshooting/).


## 4 - Update regularly

Nginx updates, Passenger updates and system updates are delivered through the APT package manager regularly.
You should run the following command regularly to keep them up to date:

```
$ sudo apt-get update
$ sudo apt-get upgrade
```

##### You do not need to restart Nginx or Passenger after an update, and you also do not need to modify any configuration files after an update. That is all taken care of automatically for you by APT.





<div class='footnotes'>
<p>These step-by-step instructions are taken from various tutorials on <a href="https://digitalocean.com">digitalocean.com</a>, <a href="https://www.phusionpassenger.com">phusionpassenger.com</a>, and other sites. Some sources have been linked. Most step descriptions have been shortened and simplified. Some step sequences are from one source while other may come from various sources.</p>
<p>These instructions and commands are placed on GitHub so I can conveniently find them. I am a novice with Ubuntu, Nginx, Passenger, command line, etc., so I probably can&#8217;t answer any questions. However, I&#8217;ll be glad to incorporate any corrections that are needed.</p>
<p><em>Use instructions and commands at your own risk.</em></p>

<div class='creative-commons'>
  <a class="creative-commons-image" href="https://creativecommons.org/licenses/by-nc-sa/4.0/">
	<img rel="license" alt="Creative Commons License" src="creativecommons.png"></a>
    <p>
		This work is licensed under a <a rel="license" href="https://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.
		</p>
</div>
</div>
