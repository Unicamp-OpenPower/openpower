---
title: Install Docker from package
layout: page
date: 2021-12-04
authors: [jr-santos]

aliases: [/blog/install-docker-from-package.html]

---

Docker is a set of systems that use virtualization to deliver software in packages, which are called containers.
The containers are isolated from the operating system and other containers, they have unique libraries,
as well as packages and configuration files. In addition, they can run different operating systems and
have a lower resource cost than traditional virtualization.

We provide the [docker-ce](https://oplab9.parqtec.unicamp.br/pub/ppc64el/docker/), an edition of Docker that is free software,
updated by the community. However, the package is officially not available for the Power architecture (ppc64 / ppc64le).
Thus, the laboratory makes the latest versions available for installation from the package managers:
Advanced Packaging Tool (APT) and Red Hat Package Manager (RPM).

This tutorial shows the step-by-step installation of Docker using
the ".deb/.rpm" package. If you want to install using a package manager, read
[Installing Docker from the OpenPower Lab Repository](/post/installing-docker-from-repository/).

## Requirements

First of all, you need to install the Docker requirements.
You can perform this operation using one of the commands below:

### To Ubuntu/Debian:

```bash
sudo apt update
sudo apt install -y wget libseccomp2 libc6 libdevmapper1.02.1 libsystemd0 apparmor ca-certificates git libltdl7 pigz procps xz-utils runc
```

### To RHEL/CentOS/Fedora:

```bash
sudo yum update
sudo yum install -y wget libseccomp glibc-utils device-mapper-libs systemd-libs ca-certificates git libtool-ltdl pigz procps xz runc
```

## Install Docker

To install Docker-ce, it is necessary to download and install the packages:
**containerd**, **docker-ce**, **docker-ce-cli** and **docker-ce-rootless-extras**.
All these packages are available for download through our [FTP](https://oplab9.parqtec.unicamp.br/),
use one of the commands below to perform this operation:

> Containerd was used in version 1.5.7 and Docker in 20.10.6. Change package versions! For the latest versions,
access [Oplab FTP](https://oplab9.parqtec.unicamp.br/pub/ppc64el/).

### To Ubuntu/Debian

```bash
wget https://oplab9.parqtec.unicamp.br/pub/repository/debian/ppc64el/containerd/containerd-1.5.7-ppc64le.deb https://oplab9.parqtec.unicamp.br/pub/ppc64el/docker/version-20.10.6/ubuntu-focal/docker-ce-rootless-extras_20.10.6~3-0~ubuntu-focal_ppc64el.deb https://oplab9.parqtec.unicamp.br/pub/ppc64el/docker/version-20.10.6/ubuntu-focal/docker-ce-cli_20.10.6~3-0~ubuntu-focal_ppc64el.deb https://oplab9.parqtec.unicamp.br/pub/ppc64el/docker/version-20.10.6/ubuntu-focal/docker-ce_20.10.6~3-0~ubuntu-focal_ppc64el.deb
```

To install on Ubuntu, use the **dpkg**:

```bash
sudo dpkg -i containerd-1.5.7-ppc64le.deb docker-ce-rootless-extras_20.10.6~3-0~ubuntu-focal_ppc64el.deb docker-ce-cli_20.10.6~3-0~ubuntu-focal_ppc64el.deb docker-ce_20.10.6~3-0~ubuntu-focal_ppc64el.deb
```

### To RHEL/CentOS/Fedora:

```bash
wget https://oplab9.parqtec.unicamp.br/pub/repository/rpm/ppc64le/containerd/containerd-1.5.7-1.ppc64le.rpm https://oplab9.parqtec.unicamp.br/pub/ppc64el/docker/version-20.10.6/centos-8/docker-ce-rootless-extras-20.10.6-3.el8.ppc64le.rpm https://oplab9.parqtec.unicamp.br/pub/ppc64el/docker/version-20.10.6/centos-8/docker-ce-cli-20.10.6-3.el8.ppc64le.rpm https://oplab9.parqtec.unicamp.br/pub/ppc64el/docker/version-20.10.6/centos-8/docker-ce-20.10.6-3.el8.ppc64le.rpm
```

To install on CentOS 8, use the **yum localinstall**:

```bash
sudo yum localinstall containerd-1.5.7-1.ppc64le.rpm docker-ce-rootless-extras-20.10.6-3.el8.ppc64le.rpm docker-ce-cli-20.10.6-3.el8.ppc64le.rpm  docker-ce-20.10.6-3.el8.ppc64le.rpm
```

## Enable and verify Docker

Before using Docker, you need to enable it and start it.
This process is also used to verify that the installation was successful.

```bash
sudo systemctl enable docker
sudo systemctl start docker
sudo systemctl status docker
```

Expected output:

```bash
ubuntu@docker-build:~$ sudo systemctl enable docker
Created symlink /etc/systemd/system/multi-user.target.wants/docker.service → /usr/lib/systemd/system/docker.service.
ubuntu@docker-build:~$ sudo systemctl start docker
ubuntu@docker-build:~$ sudo systemctl status docker
● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Sat 2021-12-04 00:15:12 UTC; 3min 33s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 88776 (dockerd)
      Tasks: 16
     Memory: 76.6M
     CGroup: /system.slice/docker.service
             └─88776 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Dec 04 00:15:10 docker-build dockerd[88776]: time="2021-12-04T00:15:10.992533127Z" level=warning msg="Your kernel does >
Dec 04 00:15:10 docker-build dockerd[88776]: time="2021-12-04T00:15:10.992943323Z" level=warning msg="Your kernel does >
Dec 04 00:15:10 docker-build dockerd[88776]: time="2021-12-04T00:15:10.993053898Z" level=warning msg="Your kernel does >
Dec 04 00:15:10 docker-build dockerd[88776]: time="2021-12-04T00:15:10.993340051Z" level=info msg="Loading containers: >
Dec 04 00:15:11 docker-build dockerd[88776]: time="2021-12-04T00:15:11.419025732Z" level=info msg="Default bridge (dock>
Dec 04 00:15:11 docker-build dockerd[88776]: time="2021-12-04T00:15:11.585401847Z" level=info msg="Loading containers: >
Dec 04 00:15:12 docker-build dockerd[88776]: time="2021-12-04T00:15:12.507244532Z" level=info msg="Docker daemon" commi>
Dec 04 00:15:12 docker-build dockerd[88776]: time="2021-12-04T00:15:12.507417125Z" level=info msg="Daemon has completed>
Dec 04 00:15:12 docker-build systemd[1]: Started Docker Application Container Engine.
Dec 04 00:15:12 docker-build dockerd[88776]: time="2021-12-04T00:15:12.675971607Z" level=info msg="API listen on /run/d>

```

If the output looks like the one above, then your Docker is installed
and ready to use.
