[![](https://badge.imagelayers.io/bbania/ssh-gateway:latest.svg)](https://imagelayers.io/?images=bbania/ssh-gateway:latest 'Get your own badge on imagelayers.io')

# SSH Gateway

A simple Python SSH menu in a Docker container.

![alt Gateway Menu](https://raw.githubusercontent.com/bubbl/ssh-gateway/master/screenshots/gateway.png "Gateway Menu")

## Configuration

* Pull the image from Docker Registry:

```
docker pull bbania/ssh-gateway
```

* Create an SSH key with

```
ssh-keygen -t rsa -b 4096 -C <your_comment>
```

* Create `ssh_config` file with list of your hosts.
  * File format:

```
Host node1
  IdentityFile ~/.ssh/id_rsa
  HostName my.host.ip
  User user
  Port 22
```

* Edit menulist.py `menu_data` to set menu entries for your hosts
  * Format:
    * **title** - provides menu title
    * **subtitle** - additional title for menu
    * **type** (takes either `MENU` or `COMMAND` values):
      * **MENU** - creates a menu entity (requires `subtitle` to be set as well as `options` for submenu)
      * **COMMAND** - executes a shell command

![alt Gateway Submenu](https://raw.githubusercontent.com/bubbl/ssh-gateway/master/screenshots/node_menu.png "Submenu")

Example menu_data in [menulist.py](https://github.com/bubbl/ssh-gateway/blob/master/menulist.py).

* Add the ssh key to remote hosts.

## Running container

Pass menulist.py, ssh key and ssh_config to the container as volumes:

```
docker run --rm -it \
  -v /path/to/ssh_config:/etc/ssh/ssh_config \
  -v /path/to/ssh-key:/root/.ssh/id_rsa \
  -v /path/to/menulist.py:/menulist.py
  bbania/ssh-gateway
```

