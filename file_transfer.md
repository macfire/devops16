CSS: markdown_styles.css
TITLE: File Transfers

# File Transfers

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


## From Ubuntu to Rackspace container

##### [Source: support.rackspace.com](https://support.rackspace.com/how-to/migrate-to-rackspace-from-another-hosting-provider/)

##### Use one of the following methods to transfer your data from your existing hosting provider to your new cloud server. The migration method that you use depends on the nature of your data.

##### _Note: Back up the data on your existing server before you migrate it._


## rsync

##### You can use rsync to connect the servers and transfer the data. The following steps describe how to establish communication between your existing server instance and new Rackspace server instance.

Log in as root to your existing server.

Verify whether rsync is installed on your existing server.

```
rsync -version
```

If rsync is not installed, install it based on your distribution.

```
apt-get install rsync
```


Use SSH to log in to your Rackspace server from your existing server. Generate an SSH key from your existing server if you have not done so already:

```
ssh-keygen -t rsa -b 4096 -v
```


Transfer your SSH key to your Rackspace server. Substitute your new Rackspace server’s IP address in the following command.

```
ssh-copy-id 111.344.65.781
```

Copy your existing server files by using rsync. Replace the IP address after root@ with the IP address of your Rackspace cloud server.

_Note: Adjust the following command to fit your specific situation. For example, you might need additional excludes or to change the source and destination paths._

```
rsync --exclude="/sys/*" --exclude="/proc/*" -aHSKDvz -e ssh / root@111.344.65.781:/media/xvda/
```

----

##### [Source: DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-use-rsync-to-sync-local-and-remote-directories-on-a-vps)


##### Note: use a trailing slash at end of source directory to transfer _contents_ of directory instead of directory itself.

rsync flags

    `-r` = recursive

    `-a` = stands for "archive" and syncs recursively and preserves symbolic links, special and device files, modification times, group, owner, and permissions.

    `-n` (or --dry-run) = dry-run for testing

    `-P` = combines flags `--progress` and `--partial`


Sync with remote system

```
rsync -anP ~/dir1/ username@remote_host:destination_directory
```

## OpenStack Swift

[Source: Openstack](https://www.openstack.org/software/releases/ocata/components/swift)

[https://support.rackspace.com/how-to/cloud-files-uploading-large-files/](https://support.rackspace.com/how-to/cloud-files-uploading-large-files/)

Example login via commandline

```
swift -A https://auth.api.rackspacecloud.com/v1.0 -U username -K api_key stat -v
```

Uploading multiple files. Important: be sure to call command from directory containing files to be uploaded.

```
swift -A https://auth.api.rackspacecloud.com/v1.0 -U username -K api_key upload <Rackspace container/folder> <file name_1> <filename_2> <filename_3> --changed --skip-identical
```
