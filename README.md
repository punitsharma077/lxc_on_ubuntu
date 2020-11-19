# LXC On Ubuntu

This repository contains the backend code for Linux Labs. 


The backend code of the original LXD online demo service:
[https://linuxcontainers.org/lxd/try-it](https://linuxcontainers.org/lxd/try-it)

The main client can be found at the URL above, with its source available here:  
[https://github.com/lxc/linuxcontainers.org](https://github.com/lxc/linuxcontainers.org)

## Dependencies

First install `golang` from [golang.org/doc/install](https://golang.org/doc/install)

Make sure to follow the installation steps completely and also set the `PATH` environment variable correctly.

Install `build-essential` package:

```sudo apt-get install build-essential```

In case you have installed LXD from `apt` previously:

Run ```sudo apt remove --purge lxd lxd-client``` to remove the LXD.

Install LXD from `snap` and configure LXD:
```
sudo snap install lxd
sudo lxd init
```
Make sure to stick to all the `default` values while doing `sudo lxd init`, if needed give more space to the storage pool.

Other than that, you can pull all the other necessary dependencies with:

    go get github.com/punitsharma077/lxc_on_ubuntu

## Building it

To build it run:

```
cd go/src/github.com/punitsharma077
go build
```

## Running it

To run your own, you should start by copying the example configuration
file "lxd-demo.yaml.example" to "lxd-demo.yaml", then update its content
according to your environment.

Once done, simply run the daemon with:

`sudo ./lxc_on_ubuntu`

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
