CSS: markdown_styles.css
date: 2016-05-10
modified: 2016-08-10

# DevOps16 - Create Python Virtual Environment

##### [Source: Matt Makai @ fullstackpython.com](https://www.fullstackpython.com/blog/python-3-django-gunicorn-ubuntu-1604-xenial-xerus.html)


[Home](../index.html)
: [1 - Ubuntu 16.04 user setup](devops16_1_ubuntu16_setup.html)
: [2 - Install Nginx](devops16_2_install_nginx.html)
: [3 - Install Phusion Passenger](devops16_3_install_phusionpassenger.html)
: [4 - Install MongoDB](devops16_4_install_mongodb.html)
: [5 - Python venv](devops16_5_python_venv.html)
: [6 - Deploy via Git](devops16_6_deploy_flask_app_w_git.html)



##### [Ubuntu](/ubuntu.html)'s latest Long Term Support (LTS) [operating system](/operating-systems.html) was released last month, in April 2016. The 16.04 update for Ubuntu is known as "Xenial Xerus" and it's the first Ubuntu release to include [Python 3](/python-2-or-3.html) as the default Python installation. We can use this new Ubuntu release along with Python version 3.5 to start a new [Flask](/flask.html) web application project.


## Tools We'll Need

##### We'll need the Ubuntu 16.04 release along with a few other libraries to complete our project. You don't have to install these tools just yet, we will get to them as we progress through the walkthrough. Our requirements and their current versions as of May 10, 2016 are:

* [Ubuntu 16.04 LTS (Xenial Xerus)](http://releases.ubuntu.com/16.04/)
* [Python](/why-use-python.html) version [3.5](https://docs.python.org/3/whatsnew/3.5.html) (default in Ubuntu 16.04)
* [Flask](/flask.html) web framework version [0.10](http://flask.pocoo.org/docs/0.10/)
* [Green Unicorn (Gunicorn)](/green-unicorn-gunicorn.html) version [19.4](http://docs.gunicorn.org/en/stable/news.html)

##### If you're running on Mac OS X or Windows, use virtualization software such as [Parallels](https://www.parallels.com/products/desktop/) or [VirtualBox](https://www.virtualbox.org/wiki/Downloads) with the [Ubuntu .iso file](http://releases.ubuntu.com/16.04/). Either the amd64 or i386 version of 16.04 is fine. I'm using amd64 for development and testing in this tutorial.


## System Packages

We can see the python3 system version Ubuntu comes with and where its
executable is stored using these commands.

```
python3 --version
which python3
```

Install required system packages, using superuser login with `sudo`.
Enter `y` to let the system package installation process do its job.

```
sudo apt-get install virtualenv python-pip python3-dev
```


## Virtualenv

##### In the previous section, [virtualenv](https://virtualenv.pypa.io/en/latest/) and [pip](https://pypi.python.org/pypi/pip) were installed to handle our [application dependencies](/application-dependencies.html).

Create a directory for the virtualenvs. Then create a new virtualenv.

```
# the tilde "~" specifies the user's home directory, like /home/matt
cd ~
mkdir venvs
# specify the system python3 installation
virtualenv --python=/usr/bin/python3 venvs/flaskproj
```

Activate the virtualenv.

```
source ~/venvs/flaskproj/bin/activate
```

Our prompt will change after we properly activate the virtualenv.
Our virtualenv is now activated with Python 3. We can install whatever
dependencies we want, in our case Flask and Gunicorn.


## Flask

Create a new directory under our home directory that will store our
Flask project. Change directory into the new folder.

```
mkdir ~/flaskproj
cd ~/flaskproj
```



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
