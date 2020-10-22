# install-docker-on-cent-OS
install vagrant and docker on cent OS

- - -

## Setup cent OS 7 Environment


:exclamation: **If you are Windows user, please disable HyperV** :exclamation:

Clone this project and enter to project folder


### Install Vagrant
[Download link](https://www.vagrantup.com/downloads)

:+1: **Check point:**
```
$ vagrant version
Installed Version: 2.2.10
Latest Version: 2.2.10
 
You're running an up-to-date version of Vagrant!
```


### Install VirtualBox
[Download link](https://www.virtualbox.org/wiki/Downloads)

### Start Vagrant
```
$ vagrant up
```

### Enter to Virtual Machine
```
$ vagrant ssh node-1
$ vagrant ssh node-2
```
:+1: **Check point:** 
In each VM, type below command
```
$ cat /etc/os-release 
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"
```

- - -

## Instal docker container runtime
### Uninstall old versions
```
$ sudo su
$ yum remove docker \
  docker-client \
  docker-client-latest \
  docker-common \
  docker-latest \
  docker-latest-logrotate \
  docker-logrotate \
  docker-engine
```

### Set up the repository
```
$ yum install -y yum-utils

$ yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

### Install docker

:exclamation: **centOS 7 ** :exclamation:
```
$ yum install docker-ce docker-ce-cli containerd.io
```

:exclamation: **centOS 8 ** :exclamation:
```
$ yum install https://download.docker.com/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.6-3.3.el7.x86_64.rpm
$ yum install docker-ce docker-ce-cli
```

### Add vagrant insto docker group
```
$ usermod -aG docker vagrant
```

### Disable firewalld
```
$ systemctl disable firewalld
$ systemctl stop firewalld
```

### Setup registry parameter
```
$ vi /etcdocker/daemon.json
{ "insecure-registries":["192.168.5.11:5000"] }
```

### Start docker 
```
$ systemctl start docker
```

### Alerter docker.sock authority
```
$ cd /var/run/
$ chmod -R 755 docker.sock
```

### Enable docker service
```
$ systemctl enable docker
```

### Restart docker to apply the change
```
$ systemctl restart docker
```
If it cannot work, just reboot VM


### Check docker version
```
$ docker version
Client: Docker Engine - Community
 Version:           19.03.13
 API version:       1.40
 Go version:        go1.13.15
 Git commit:        4484c46d9d
 Built:             Wed Sep 16 17:03:45 2020
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.13
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.13.15
  Git commit:       4484c46d9d
  Built:            Wed Sep 16 17:02:21 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.3.7
  GitCommit:        8fba4e9a7d01810a393d5d25a3621dc101981175
 runc:
  Version:          1.0.0-rc10
  GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
```
:+1: **Check point:**
```
$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
0e03bdcc26d7: Pull complete 
Digest: sha256:8c5aeeb6a5f3ba4883347d3747a7249f491766ca1caa47e5da5dfcf6b9b717c0
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```
