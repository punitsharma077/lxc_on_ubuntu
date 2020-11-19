# LXC On Ubuntu

This repository contains the backend code for Linux Labs. 

We have used `lxd-demo-server` tool provided by `Canonical` itself for this project.
Simply put, it's a small Go daemon exposing a REST API that users (mostly our javascript 
client) can interact with to create temporary test containers and attach to that container's console.
Those containers come with a bunch of resource limitations and an expiry, when the container expires,
it's automatically deleted.

The backend code of the original LXD online demo service:
[https://linuxcontainers.org/lxd/try-it](https://linuxcontainers.org/lxd/try-it)

The main client can be found at the URL above, with its source available here:  
[https://github.com/lxc/linuxcontainers.org](https://github.com/lxc/linuxcontainers.org)

The lxd-demo-server can be found at:
[https://github.com/lxc/lxd-demo-server](https://github.com/lxc/lxd-demo-server)

## Dependencies

First install `golang` from [golang.org/doc/install](https://golang.org/doc/install)

Make sure to follow the installation steps completely and also set the `PATH` environment variable correctly.

Install `build-essential` package:
```
sudo apt-get install build-essential
```

In case you have installed LXD from `apt` previously:

To remove LXD run:
```
sudo apt remove --purge lxd lxd-client
```

Install LXD from `snap` and configure LXD:
```
sudo snap install lxd
sudo lxd init
```
Make sure to stick to all the `default` values while doing `sudo lxd init`, if needed give more space to the storage pool.

Other than that, you can pull all the other necessary dependencies with:
```
go get github.com/punitsharma077/lxc_on_ubuntu
```

## Building it

To build it run:
```
cd go/src/github.com/punitsharma077
go build
```

## Running it

### Setting up the configuration file
To run your own, you should first set the configurations in the `lxd-demo.yaml` file.
Some parameters are:
```
image : To specify which Ubuntu Image will be used to create LXCs.
command : To specify that terminal needs to be run on creation.
profiles : Can specify the profiles to be used for the LXCs.
quota_cpu : To specify number of cores allotted per LXC.
quota_processes : To specify maximum numbers of processes that can be spawned.
quota_ram : To specify amount of RAM allocated per LXC.
quota_sessions : Number of concurrent sessions allowed per user.
quota_time : Time before the LXC expires, in seconds.
server_containers_max : Maximum number of LXCs that can be run at a time.
```
Change these values as per the need.
After changing run:
```
go build
```

### Runnning the daemon
Once done, simply run the daemon with:
```
sudo ./lxc_on_ubuntu
```

If there is no error and the cursor is at a new line, the daemon is up and running.
If you encounter the message - `Waiting for the LXD server to come online.` then do `CTRL+C`. This error message comes when the daemon is not able connect to the LXD daemon over the local unix socket. To fix it run the following commands:
```
sudo apt remove lxd-client -y
hash -r
sudo ln -s /var/snap/lxd/common/lxd /var/lib/lxd
```

Having done this, re-run the command to start the daemon.

Now you can open `127.0.0.1:8080` in the browser and the you should be able to see the LXC On Ubuntu Linux Lab.

To stop the daemon do `CTRL+C` in the terminal it is running.

The daemon isn't verbose at all, it will only log critical LXD errors.
