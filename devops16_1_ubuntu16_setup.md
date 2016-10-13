CSS: markdown_styles.css

# DevOps16 - Ubuntu 16.04 setup

##### [Source: digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-16-04)


[Home](../index.html)
: [1 - Ubuntu 16.04 user setup](devops16_1_ubuntu16_setup.html)
: [2 - Install Nginx](devops16_2_install_nginx.html)
: [3 - Install Phusion Passenger](devops16_3_install_phusionpassenger.html)
: [4 - Install MongoDB](devops16_4_install_mongodb.html)
: [5 - Python venv](devops16_5_python_venv.html)
: [6 - Deploy via Git](devops16_6_deploy_flask_app_w_git.html)


## 1 - Root Login

Via terminal. Change password as may be prompted.

```
ssh root@SERVER_IP_ADDRESS
```

## 2- Create New User

Create new user to carry out day-to-day tasks, to avoid using `root`

```
adduser USER_NAME
```

Create `.ssh` folder and `authorized_keys` file

```
mkdir ~/.ssh
chmod 700 ~/.ssh
touch ~/.ssh/authorized_keys
```

## 3 - Root Privileges

Add new user to `sudo` group. By using `sudo`, user will have root privileges.<br />
[sudoers tutorial][sudoers_tutorial]

```
usermod -aG sudo USER_NAME
```

## 4 - Add Public Key Authentication

### Generate a Key Pair (if not already exists)

Generate key on LOCAL machine

```
(local$) ssh-keygen
```


### Copy the Public Key

#### Option 1: Use ssh-copy-id

If `ssh-copy-id` is installed locally, then use this to add key to remote user's `.ssh/authorized_keys` file. (Note: will need a password)

````sh_local
(local$) ssh-copy-id USER_NAME@REMOTE_SERVER_IP
````

#### Option 2: Manually Install the Key

Print SSH public key to terminal. Select and copy.

```
(local$) cat ~/.ssh/id_rsa.pub
```

On server, as `root` user, temporatily switch to new user

```
su - USER_NAME
```

Open file in `.ssh/authorized_keys` to edit.
Paste key. `CTRL-X` to exit file, `y` to save changes, then `ENTER`

```
nano ~/.ssh/authorized_keys
```

Restrict permissions of the `authorized_keys`

```
chmod 600 ~/.ssh/authorized_keys
```

return to `root` user

```
exit
```


## 7 - Set up basic firewall

Ubuntu 16.04 servers can use the UFW firewall to make sure only connections to certain services are allowed. We can set up a basic firewall very easily using this application.

Different applications can register their profiles with UFW upon installation. These profiles allow UFW to manage these applications by name. OpenSSH, the service allowing us to connect to our server now, has a profile registered with UFW.

You can see this by typing:

```
sudo ufw app list
Output
Available applications:
  OpenSSH
```

We need to make sure that the firewall allows SSH connections so that we can log back in next time. We can allow these connections by typing:

```
sudo ufw allow OpenSSH
```

Afterwards, we can enable the firewall by typing:

```
sudo ufw enable
```

Type "y" and press ENTER to proceed. You can see that SSH connections are still allowed by typing:

```
sudo ufw status
Output
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
```



[sudoers_tutorial]: ](https://www.digitalocean.com/community/tutorials/how-to-edit-the-sudoers-file-on-ubuntu-and-centos)



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
