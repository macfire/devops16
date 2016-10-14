CSS: markdown_styles.css
TITLE: DevOps16 - Deploy Flask app via Git


# DevOps16 - Deploy Flask app via Git

##### [Source: digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-16-04)


[Home](../index.html)
: [1 - Ubuntu 16.04 user setup](devops16_1_ubuntu16_setup.html)
: [2 - Install Nginx](devops16_2_install_nginx.html)
: [3 - Install Phusion Passenger](devops16_3_install_phusionpassenger.html)
: [4 - Install MongoDB](devops16_4_install_mongodb.html)
: [5 - Python venv](devops16_5_python_venv.html)
: [6 - Deploy via Git](devops16_6_deploy_flask_app_w_git.html)


#### Replace `APP` and `APP_USER` with your app's name and your app user account's name.

## Push app code to Git repository

Transfer application's code to an existing Git repository. If you have not already setup a Git repository, go to Github, create a repository and push your application's code there.

```
git push
```

## Login or create superuser

##### Sites should be deployed using superuser login with root privileges, instead of using root user. If not setup, see [setting up admin user](devops16_1_ubuntu16_setup.html) here.


## Create  user for the app

Check to see if public key exists

```
cat ~/.ssh/id_rsa.pub
```

If not, create public key

```
ssh-keygen -t rsa
```

Create .ssh directory and the authorized_keys file.
Permissions are important!

```
mkdir ~/.ssh
chmod 700 ~/.ssh
touch ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```
Add public key to the authorized_keys file

```
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```


Create an operating system user account for your app.

```
sudo adduser APP_USER
```

##### Run each app under its own user account in order to limit damage that security vulnerabilities in the app can do. *Passenger will automatically run your app under this user account as part of its user account sandboxing feature.* Recommended to give the user account the same name as app.

Ensure that `APP_USER` has SSH key installed:

```
APP_USER=your_user_name

sudo mkdir -p "~$APP_USER/.ssh"
touch $APP_USER/.ssh/authorized_keys
sudo sh -c "cat $HOME/.ssh/authorized_keys >> ~$APP_USER/.ssh/authorized_keys"
sudo chown -R $APP_USER: "~$APP_USER/.ssh"
sudo chmod 700 "~$APP_USER/.ssh"
sudo sh -c "chmod 600 ~$APP_USER/.ssh/*"
```

## Install Git on the server

#### Store app code on the server at `/var/www/APP_NAME/code`


Install git

```
sudo apt-get install -y git
```

Create directory to store application code. Recommend `/var/www/APP_NAME`.

```
APP_USER=your_user_name
APP_NAME=your_app_name

sudo mkdir -p /var/www/$APP_NAME
sudo chown $APP_USER: /var/www/$APP_NAME
```

Add key github account. Use `cat` and copy output from terminal. Paste in GitHub ssh keys settings.

```
cat ~/.ssh/id_rsa.pub
```

Add github.com to known hosts

```
ssh -T git@github.com
```

Pull the code from Git:

```
cd /var/www/APP_NAME
sudo -u APP_USER -H git clone git://github.com/APP_USER/APP_NAME.git code
```

To target git branch:

`````
cd /var/www/APP_NAME
sudo -u APP_USER -H git clone --branch=end_result git://github.com/url-here.git code
`````

Update git

```
sudo -u APP_USER -H git pull origin master
```

---

# Preparing the app's environment

## Create Virtual Environment

Navigate to desired folder to store venv

```
cd /var/www/APP_NAME
```

Create a python virtual environment for app.
If using python3.5, use:

```
pyvenv venv
```

## Install app dependencies

Install Python libraries through pip. For example, if your application depends on Flask:

```
sudo pip install flask
```

or

```
sudo pip install -r requirements.txt
```


## Install other services

##### Your app may also depend on services, such as PostgreSQL, Redis, etc. Installing services that your app depends on is outside of this tutorial's scope.

---

# Configuring Nginx and Passenger

##### Now that you are done with transferring your app's code to the server and setting up an environment for your app, it is time to configure Nginx so that Passenger knows how to serve your app.


## Edit Nginx configuration file

Create an Nginx configuration file and setup a virtual host entry that points to your app. This virtual host entry tells Nginx (and Passenger) where your app is located. Replace `APP_NAME` with your app's name.

```
sudo nano /etc/nginx/sites-enabled/APP_NAME.conf
```

Put this inside the file.
Replace `yourserver.com` with your server's host name, and replace `/var/www/APP_NAME/code` with your application's code directory path. However, make sure that Nginx is configured to point to the *public subdirectory* inside it!

```
server {
    listen 80;
    server_name yourserver.com;

    # Tell Nginx and Passenger where your app's 'public' directory is
    root /var/www/APP_NAME/code/public;

    # Turn on Passenger
    passenger_enabled on;
}
```



Restart Nginx

```
sudo service nginx restart
```

## Test drive

Should now be able to access app through the server's host name.
Run following from your local computer. Replace `yourserver.com` with server's hostname, exactly as it appears in the Nginx config file's `server_name` directive.

```
curl http://yourserver.com/
...your app's front page HTML...
```

##### If you do not see your app's front page HTML, then these are the most likely causes:

1. Incorrectly configured `server_name` directive. The `server_name` must exactly match the host name in the URL. *For example, if you use the command curl http://45.55.91.235/ to access your app, then the server_name must be 45.55.91.235.*
2. DNS records not setup. Setting up DNS is outside the scope of this tutorial. In the mean time, we recommend that you use your server's IP address as the server name.



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
