CSS: markdown_styles.css

# DevOps16 - Install MongoDB on Ubuntu 16.04

##### [Source: digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-install-mongodb-on-ubuntu-16-04)

[Home](../index.html)
: [1 - Ubuntu 16.04 user setup](devops16_1_ubuntu16_setup.html)
: [2 - Install Nginx](devops16_2_install_nginx.html)
: [3 - Install Phusion Passenger](devops16_3_install_phusionpassenger.html)
: [4 - Install MongoDB](devops16_4_install_mongodb.html)
: [5 - Python venv](devops16_5_python_venv.html)
: [6 - Deploy via Git](devops16_5_deploy_flask_app_w_git.html)


#### Use sudo user via ssh login for following commands (instead of `root`)


## Introduction

MongoDB is a free and open-source NoSQL document database used commonly in modern web applications. This tutorial will help you set up MongoDB on your server for a production application environment.

* As of publication time, the official Ubuntu 16.04 MongoDB packages have not yet been updated to use the new `systemd` init system [which is enabled by default on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/what-s-new-in-ubuntu-16-04#the-systemd-init-system). Running MongoDB using those packages on a clean Ubuntu 16.04 server involves following an additional step to configure MongoDB as a `systemd` service that will automatically start on boot.

## Prerequisites

To follow this tutorial, you will need:

* One Ubuntu 16.04 server set up by following this initial server setup tutorial, including a sudo non-root user


## Step 1 — Adding the MongoDB Repository

##### MongoDB is already included in Ubuntu package repositories, but the official MongoDB repository provides most up-to-date version and is the recommended way of installing the software. In this step, we will add this official repository to our server.

Ubuntu ensures the authenticity of software packages by verifying that they are signed with GPG keys, so we first have to import they key for the official MongoDB repository.

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
```

After successfully importing the key, you will see:

```
Output
gpg: Total number processed: 1
gpg:               imported: 1  (RSA: 1)
```

Next, we have to add the MongoDB repository details so apt will know where to download the packages from.

Issue the following command to create a list file for MongoDB.

```
echo "deb http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
```

After adding the repository details, we need to update the packages list.

```
sudo apt-get update
```

## Step 2 — Installing and Verifying MongoDB

Now we can install the MongoDB package itself. This command will install several packages containing latest stable version of MongoDB along with helpful management tools for the MongoDB server.

```
sudo apt-get install -y mongodb-org
```

##### In order to properly launch MongoDB as a service on Ubuntu 16.04, we additionally need to create a unit file describing the service. A _unit file_ tells `systemd` how to manage a resource. The most common unit type is a service, which determines how to start or stop the service, when should it be automatically started at boot, and whether it is dependent on other software to run.

We'll create a unit file to manage the MongoDB service.
Create a configuration file named `mongodb.service` in the `/etc/systemd/system` directory using nano or your favorite text editor.

```
sudo nano /etc/systemd/system/mongodb.service
```

Paste in the following contents, then save and close the file.

```
/etc/systemd/system/mongodb.service
[Unit]
Description=High-performance, schema-free document-oriented database
After=network.target

[Service]
User=mongodb
ExecStart=/usr/bin/mongod --quiet --config /etc/mongod.conf

[Install]
WantedBy=multi-user.target
```

##### This file has a simple structure:

* __Unit__ section contains the overview (e.g. a human-readable description for MongoDB service) as well as dependencies that must be satisfied before the service is started. In our case, MongoDB depends on networking already being available, hence network.target here.
* __Service__ section how the service should be started. The User directive specifies that the server will be run under the mongodb user, and the ExecStart directive defines the startup command for MongoDB server.
* __Install__ section tells `systemd` when the service should be automatically started. The multi-user.target is a standard system startup sequence, which means the server will be automatically started during boot.

Next, start the newly created service with `systemctl`.

```
sudo systemctl start mongodb
```

While there is no output to this command, you can also use `systemctl` to check that the service has started properly.

```
sudo systemctl status mongodb
```

```
Output
● mongodb.service - High-performance, schema-free document-oriented database
   Loaded: loaded (/etc/systemd/system/mongodb.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2016-04-25 14:57:20 EDT; 1min 30s ago
 Main PID: 4093 (mongod)
    Tasks: 16 (limit: 512)
   Memory: 47.1M
      CPU: 1.224s
   CGroup: /system.slice/mongodb.service
           └─4093 /usr/bin/mongod --quiet --config /etc/mongod.conf
```

The last step is to enable automatically starting MongoDB when the system starts.

```
sudo systemctl enable mongodb
```

##### The MongoDB server now configured and running, and you can manage the MongoDB service using the ```systemctl``` command (e.g. ```sudo systemctl mongodb stop```, ```sudo systemctl mongodb start```).


## Step 3 — Adjusting the Firewall (Optional)

##### Assuming you have followed the [initial server setup](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04) tutorial instructions to enable the firewall on your server, MongoDB server will be inaccessible from the internet.

##### If you intend to use the MongoDB server only locally with applications running on the same server, it is a recommended and secure setting. However, if you would like to be able to connect to your MongoDB server from the internet, we have to allow the incoming connections in `ufw`.

##### To allow access to MongoDB on its default port 27017 from everywhere, you could use ```sudo ufw allow 27017```. However, enabling internet access to MongoDB server on a default installation gives unrestricted access to the whole database server.

In most cases, MongoDB should be accessed only from certain trusted locations, such as another server hosting an application. To accomplish this task, you can allow access on MongoDB's default port while specifying the IP address of another server that will be explicitly allowed to connect.

```
sudo ufw allow from your_other_server_ip/32 to any port 27017
```

You can verify the change in firewall settings with `ufw`.

```
sudo ufw status
```

You should see traffic to `27017` port allowed in the output.If you have decided to allow only a certain IP address to connect to MongoDB server, the IP address of the allowed location will be listed instead of Anywhere in the output.

```
Output
Status: active

To                         Action      From
--                         ------      ----
27017                      ALLOW       Anywhere
OpenSSH                    ALLOW       Anywhere
27017 (v6)                 ALLOW       Anywhere (v6)
OpenSSH (v6)               ALLOW       Anywhere (v6)
```

##### More advanced firewall settings for restricting access to services are described in [UFW Essentials: Common Firewall Rules and Commands](https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands).


## Conclusion

##### More in-depth instructions regarding MongoDB installation and configuration in [these DigitalOcean community articles](https://www.digitalocean.com/community/search?q=mongodb).






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
