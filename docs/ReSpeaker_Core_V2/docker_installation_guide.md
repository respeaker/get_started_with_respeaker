# Docker Installation Guide


In this guide we're gonna to show you how to install docker on the ReSpeaker Core v2.

To install docker, we need to upgrade the kernel to at least `4.4.95-respeaker-r6`, i.e. system image version `xxx` or later. [TODO]: fix the versions

```shell
# preparations
$ sudo apt-get update
$ sudo apt-get install apt-transport-https ca-certificates curl gnupg2 lsb-release software-properties-common
$ curl -fsSL https://download.docker.com/linux/raspbian/gpg | sudo apt-key add -
$ sudo add-apt-repository "deb [arch=armhf] https://download.docker.com/linux/raspbian  $(lsb_release -cs)  stable"

# now let's install the docker-ce package
$ sudo apt-get update
$ sudo apt-get install docker-ce

# configure the docker to use overlay2 file system driver
$ sudo vim /etc/docker/daemon.json
```

Please note that, we should type the full path for `/etc/docker/daemon.json`, as the browsing permission of directory `/etc/docker` is disabled by default, so the `tab` prediction will not work. Now paste the following contents into the file and save it.

```json
{
       	"storage-driver": "overlay2"
}
```

```shell
# enable docker to start at boot
$ sudo systemctl enable docker
# restart it to take the new configuration into account
$ sudo systemctl restart docker

# add the current user to docker group
$ sudo groupadd docker
$ sudo usermod -aG docker $USER

# now logout and login, to let the user group permission go into effect

# test
$ docker run arm32v7/hello-world
```

If everything right, we should see the following outputs:

```shell
$ docker run arm32v7/hello-world

Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
ca4f61b1923c: Pull complete
Digest: sha256:be0cd392e45be79ffeffa6b05338b98ebb16c87b255f48e297ec7f98e123905c
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (arm32v7)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://cloud.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/engine/userguide/
 
```

Congrats! You are now having a small linux box which can run docker containers. Please note that, you should use the containers built for `arm32v7` or `armhf`, `x86` containers are not going to work.

 Enjoy!





