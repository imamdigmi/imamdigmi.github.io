---
date: 2017-01-14T18:02:13+07:00
lastmod: 2017-01-14T18:02:13+07:00

title: "Setup Docker on Ubuntu"
description: "Setup Docker Engine one Ubuntu 16.04"
author: "Imam Digmi"

weight: 50
draft: false
keywords: ["docker", "ubuntu", "setup"]
tags: ["docker", "ubuntu", "setup"]
categories: ["Docker"]

comment: true
toc: true
autoCollapseToc: false
contentCopyright: <a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/">CC BY-NC-ND 4.0</a>
reward: false
mathjax: false
mathjaxEnableSingleDollar: false
---


## Prerequisites
To follow this tutorial, you will need the following:
- 64-bit Ubuntu 16.04 Droplet
- Non-root user with sudo privileges Initial Setup Guide for Ubuntu 16.04 explains how to set this up.) <!-- more -->
> Note: Docker requires a 64-bit version of Ubuntu as well as a kernel version equal to or greater than 3.10. The default 64-bit Ubuntu 16.04 Droplet meets these requirements.

All the commands in this tutorial should be run as a non-root user. If root access is required for the command, it will be preceded by sudo. Initial Setup Guide for Ubuntu 16.04 explains how to add users and give them `sudo` access.

## Step 1 — Installing Docker
The Docker installation package available in the official Ubuntu 16.04 repository may not be the latest version. To get the latest and greatest version, install Docker from the official Docker repository. This section shows you how to do just that.

But first, let's update the package database:
```bash
sudo apt-get update
```
Now let's install Docker. Add the GPG key for the official Docker repository to the system:
```bash
sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
```
Add the Docker repository to APT sources:
```bash
sudo apt-add-repository 'deb https://apt.dockerproject.org/repo ubuntu-xenial main'
```
Update the package database with the Docker packages from the newly added repo:
```bash
sudo apt-get update
```
Make sure you are about to install from the Docker repo instead of the default Ubuntu 16.04 repo:
```bash
apt-cache policy docker-engine
```
You should see output similar to the follow:
```bash
docker-engine:
  Installed: 1.12.5-0~ubuntu-xenial
  Candidate: 1.12.6-0~ubuntu-xenial
  Version table:
     1.12.6-0~ubuntu-xenial 500
        500 https://apt.dockerproject.org/repo ubuntu-xenial/main amd64 Packages
	*** 1.12.5-0~ubuntu-xenial 500
        500 https://apt.dockerproject.org/repo ubuntu-xenial/main amd64 Packages
        100 /var/lib/dpkg/status
     1.12.4-0~ubuntu-xenial 500
        500 https://apt.dockerproject.org/repo ubuntu-xenial/main amd64 Packages
     1.12.3-0~xenial 500
        500 https://apt.dockerproject.org/repo ubuntu-xenial/main amd64 Packages
     1.12.2-0~xenial 500
        500 https://apt.dockerproject.org/repo ubuntu-xenial/main amd64 Packages
     1.12.1-0~xenial 500
        500 https://apt.dockerproject.org/repo ubuntu-xenial/main amd64 Packages
     1.12.0-0~xenial 500
        500 https://apt.dockerproject.org/repo ubuntu-xenial/main amd64 Packages
     1.11.2-0~xenial 500
        500 https://apt.dockerproject.org/repo ubuntu-xenial/main amd64 Packages
        500 http://apt.kubernetes.io kubernetes-xenial/main amd64 Packages
     1.11.1-0~xenial 500
        500 https://apt.dockerproject.org/repo ubuntu-xenial/main amd64 Packages
     1.11.0-0~xenial 500
        500 https://apt.dockerproject.org/repo ubuntu-xenial/main amd64 Packages
```
Notice that `docker-engine` is not installed, but the candidate for installation is from the Docker repository for Ubuntu 16.04. The `docker-engine` version number might be different.
Finally, install Docker:
```bash
sudo apt-get install -y docker-engine
```
Docker should now be installed, the daemon started, and the process enabled to start on boot. Check that it's running:
```bash
sudo systemctl status docker
```
The output should be similar to the following, showing that the service is active and running:
```bash
● docker.service - Docker Application Container Engine
   Loaded: loaded (/lib/systemd/system/docker.service; disabled; vendor preset: enabled)
   Active: active (running) since Sab 2017-01-14 14:07:19 WIB; 4h 16min ago
     Docs: https://docs.docker.com
 Main PID: 5933 (dockerd)
    Tasks: 40
   Memory: 97.1M
      CPU: 9.123s
   CGroup: /system.slice/docker.service
           ├─2254 docker-containerd-shim 9f885c49ddee1c728ce391ed96bb650fb92a606183c44446387748c1379bad6b /var/run/docker/libcontainerd/9f885c49ddee1c728ce391ed96bb650fb92a606183c44446387748c1379ba
           ├─5933 /usr/bin/dockerd -H fd://
           └─5953 docker-containerd -l unix:///var/run/docker/libcontainerd/docker-containerd.sock --shim docker-containerd-shim --metrics-interval=0 --start-timeout 2m --state-dir /var/run/docker/
```
Installing Docker now gives you not just the Docker service (daemon) but also the docker command line utility, or the Docker client. We'll explore how to use the docker command later in this tutorial.
## Step 2 — Executing the Docker Command Without Sudo (Optional)
By default, running the docker command requires root privileges — that is, you have to prefix the command with sudo. It can also be run by a user in the docker group, which is automatically created during the installation of Docker. If you attempt to run the docker command without prefixing it with sudo or without being in the docker group, you'll get an output like this:
```bash
docker: Cannot connect to the Docker daemon. Is the docker daemon running on this host?.
See 'docker run --help'.
```
If you want to avoid typing sudo whenever you run the docker command, add your username to the docker group:
```bash
sudo usermod -aG docker $(whoami)
```
You will need to log out of the Droplet and back in as the same user to enable this change.

If you need to add a user to the docker group that you're not logged in as, declare that username explicitly using:
```bash
sudo usermod -aG docker <username>
```
The rest of this article assumes you are running the docker command as a user in the docker user group. If you choose not to, please prepend the commands with sudo.