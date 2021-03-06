## Start node process on startup

To start a node process on startup use [rc.local](https://www.raspberrypi.org/documentation/linux/usage/rc-local.md).

```sh
sudo nano /etc/rc.local
```

### Enable log for `rc.local`
This file is a script that will be run on startup. Note that this script doesn't output logs anywhere, so it is a good practice to enable that first. Simply add following at the beginning of the script:
```sh
exec 1>/var/log/rc.local.log 2>&1  # send stdout and stderr from rc.local to a log file
set -x                             # tell sh to display commands before execution
```

### Add node to path
At the time the script is started, `node` is not in the path. To add it, simply extend path:
```sh
PATH=$PATH:/home/pi/.nvm/versions/node/v14.15.5/bin
```
### Start server with logging
Now just run the script and specify where to log outputs (stdout and stderr).
```sh
nodemon -w  /path/to/node/project  /path/to/node/project/index.js \
   >  /path/to/node/project/app-regular.log \ # stdout
  2>  /path/to/node/project/app-error.log &   # stderr
```
### Symlink error
If you get error that nodemon couldn't start because of circular symlink, then check if `-w` flag is used when running with nodemon.
