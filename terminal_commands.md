CSS: markdown_styles.css
TITLE: Terminal Commands

# Terminal Commands

[Home](../index.html)
: [1 - Ubuntu 16.04 user setup](devops16_1_ubuntu16_setup.html)
: [2 - Install Nginx](devops16_2_install_nginx.html)
: [3 - Install Phusion Passenger](devops16_3_install_phusionpassenger.html)
: [4 - Install MongoDB](devops16_4_install_mongodb.html)
: [5 - Python venv](devops16_5_python_venv.html)
: [6 - Deploy via Git](devops16_6_deploy_flask_app_w_git.html)
: [7 - Migrate Mongodb](devops16_7_migrate_mongodb.html)
: [Various commands](terminal_commands.html)
: [File transfers](file_transfer.html)


## Log files

### Nginx

Nginx error log

```
cat /var/log/nginx/error.log
```

Nginx access log

```
cat /var/log/nginx/access.log
```

## $ scp : Transfer files from local to remote server

Manual

```
man scp
```

From remote to local

```
scp remote_user@remote_host:/path/to/remote/file /path/to/local/file
```

From local to remote

```
scp /path/to/local/file remote_user@remote_host:/path/to/remote/file
```


## Generate SECRET_KEY

Basic

```
import os

os.urandom(24)
```

With hex formatting

```
import os
import binascii

binascii.hexlify(os.urandom(24))
```
