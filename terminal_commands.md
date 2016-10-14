CSS: markdown_styles.css
TITLE: Terminal Commands

# Terminal Commands

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
