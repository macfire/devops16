CSS: markdown_styles.css
TITLE: DevOps16 - Migrate MongoDB databases and collections

# DevOps16 - Migrate MongoDB databases and collections

##### [Source: docs.mongodb.com](https://docs.mongodb.com/manual/tutorial/backup-and-restore-tools/)

[Home](../index.html)
: [1 - Ubuntu 16.04 user setup](devops16_1_ubuntu16_setup.html)
: [2 - Install Nginx](devops16_2_install_nginx.html)
: [3 - Install Phusion Passenger](devops16_3_install_phusionpassenger.html)
: [4 - Install MongoDB](devops16_4_install_mongodb.html)
: [5 - Python venv](devops16_5_python_venv.html)
: [6 - Deploy via Git](devops16_6_deploy_flask_app_w_git.html)
: [7 - Migrate Mongodb](devops16_7_migrate_mongodb.html)
: [Various commands](terminal_commands.html)



## Back Up a Database with mongodump

#### Exclude local Database

`mongodump` excludes the content of the local database in its output.
Required Access

To run `mongodump` against a MongoDB deployment that has access control enabled, you must have privileges that grant find action for each database to back up. The built-in backup role provides the required privileges to perform backup of any and all databases.

Changed in version 3.2.1: The backup role provides additional privileges to back up the system.profile collections that exist when running with database profiling. Previously, users required an additional read access on this collection.

## Basic `mongodump` Operations

The `mongodump` utility backs up data by connecting to a running `mongod` or `mongos` instance.

The utility can create a backup for an entire server, database or collection, or can use a query to backup just part of a collection.

When you run `mongodump` without any arguments, the command connects to the MongoDB instance on the local system (e.g. 127.0.0.1 or localhost) on port 27017 and creates a database backup named dump/ in the current directory.

To backup data from a `mongod` or `mongos` instance running on the same machine and on the default port of 27017, use the following command:

```
mongodump
```

The data format used by `mongodump` from version 2.2 or later is incompatible with earlier versions of mongod. Do not use recent versions of `mongodump` to back up older data stores.

You can also specify the --host and --port of the MongoDB instance that the `mongodump` should connect to. For example:

```
mongodump --host mongodb.example.net --port 27017
```

`mongodump` will write BSON files that hold a copy of data accessible via the mongod listening on port 27017 of the mongodb.example.net host.

 See Create Backups from Non-Local `mongod` Instances for more information.

To specify a different output directory, you can use the `--out` or `-o` option:

```
mongodump --out /data/backup/
```

To limit the amount of data included in the database dump, you can specify `--db` and `--collection` as options to `mongodump`. For example:

```
mongodump --collection myCollection --db test
```

This operation creates a dump of the collection named myCollection from the database test in a dump/ subdirectory of the current working directory.

`mongodump` overwrites output files if they exist in the backup data folder. Before running the `mongodump` command multiple times, either ensure that you no longer need the files in the output folder (the default is the dump/ folder) or rename the folders or files.


## Point in Time Operation Using Oplogs

Use the `--oplog` option with `mongodump` to collect the oplog entries to build a point-in-time snapshot of a database within a replica set. With `--oplog`, `mongodump` copies all the data from the source database as well as all of the oplog entries from the beginning to the end of the backup procedure. This operation, in conjunction with `mongorestore --oplogReplay`, allows you to restore a backup that reflects the specific moment in time that corresponds to when mongodump completed creating the dump file.

## Create Backups from Non-Local mongod Instances

The `--host` and `--port` options for `mongodump` allow you to connect to and backup from a remote host. Consider the following example:

```
mongodump --host mongodb1.example.net --port 3017 --username user --password pass --out /opt/backup/mongodump-2013-10-24
```

On any ```mongodump``` command you may, as above, specify username and password credentials to specify database authentication.
Restore a Database with ```mongorestore```


## Access Control

To restore data to a MongoDB deployment that has access control enabled, the restore role provides access to restore any database if the backup data does not include system.profile collection data.

If the backup data includes system.profile collection data and the target database does not contain the system.profile collection, `mongorestore` attempts to create the collection even though the program does not actually restore system.profile documents. As such, the user requires additional privileges to perform createCollection and convertToCapped actions on the system.profile collection for a database.

If running `mongorestore` with `--oplogReplay`, the restore role is insufficient to replay the oplog. To replay the oplog, create a user-defined role that has anyAction on anyResource and grant only to users who must run `mongorestore` with `--oplogReplay`.


## Basic mongorestore Operations

The `mongorestore` utility restores a binary backup created by mongodump. By default, `mongorestore` looks for a database backup in the dump/ directory.

The `mongorestore` utility restores data by connecting to a running `mongod` or `mongos` directly.

`mongorestore` can restore either an entire database backup or a subset of the backup.

To use `mongorestore` to connect to an active `mongod` or `mongos`, use a command with the following prototype form:

```
mongorestore --port <port number> <path to the backup>
```

Consider the following example:

```
mongorestore dump-2013-10-25/
```

Here, ```mongorestore``` imports the database backup in the dump-2013-10-25 directory to the mongod instance running on the localhost interface on the default port 27017.


## Restore Point in Time Oplog Backup

If you created your database dump using the --oplog option to ensure a point-in-time snapshot, call mongorestore with the --oplogReplay option, as in the following example:

```
mongorestore --oplogReplay
```


You may also consider using the `mongorestore --objcheck` option to check the integrity of objects while inserting them into the database, or you may consider the mongorestore `--drop` option to drop each collection from the database before restoring from backups.
Restore Backups to Non-Local mongod Instances

By default, `mongorestore` connects to a MongoDB instance running on the localhost interface (e.g. 127.0.0.1) and on the default port (27017). If you want to restore to a different host or port, use the `--host` and `--port` options.

Consider the following example:

```
mongorestore --host mongodb1.example.net --port 3017 --username user --password pass /opt/backup/mongodump-2013-10-24
```

As above, you may specify username and password connections if your mongod requires authentication.
