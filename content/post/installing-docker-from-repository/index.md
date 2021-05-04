---
title: Installing Docker from the OpenPower Lab Repository
layout: page
date: 2021-01-22
authors: [jr-santos]

aliases: [/blog/installing-docker-from-repository.html]

---

Docker is a set of systems that use virtualization to deliver software in packages, which are called containers. The containers are isolated from the operating system and other containers, they have unique libraries, as well as packages and configuration files. In addition, they can run different operating systems and have a lower resource cost than traditional virtualization.

We provide the [docker-ce](https://oplab9.parqtec.unicamp.br/pub/ppc64el/docker/), an edition of Docker that is free software, updated by the community. However, the package is officially not available for the Power architecture (ppc64 / ppc64le). Thus, the laboratory makes the latest versions available for installation from the package managers: Advanced Packaging Tool (APT) and Red Hat Package Manager (RPM).


## Add the repository and install Docker

To add the repository to the system, and to always obtain the new versions of software that we provide, use the command for your package manager.

### Add APT repository

To install, use the command:

```
sudo echo "deb https://oplab9.parqtec.unicamp.br/pub/repository/debian/ ./" >> /etc/apt/sources.list; wget https://oplab9.parqtec.unicamp.br/pub/key/openpower-gpgkey-public.asc; sudo apt-key add openpower-gpgkey-public.asc; sudo apt -y update; sudo apt -y install docker-ce
```

### Add RPM repository

To install, use the command:

```
wget https://oplab9.parqtec.unicamp.br/pub/repository/rpm/open-power-unicamp.repo; sudo mv open-power-unicamp.repo /etc/yum.repos.d/; sudo yum -y update; sudo yum -y install docker-ce
```

> wget requirement.

> To install using other means or other packages, see [Project Power Repository](project/power-repository/) or [Project Power Builds](/project/power-builds/).
