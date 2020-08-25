---
title: Installing Bazel and other packages from the OpenPower Lab Repository
layout: page
date: 2020-08-24
authors: [jr-santos]

aliases: [/blog/installing-bazel-from-repository.html]

---

Bazel is a free software tool that allows for the automation of building and testing of software. Similar to build tools like Make, Maven, and Gradle, Bazel builds software applications from source code using a set of rules.

It is not officially supported by the Power architecture, because of that, we provide the binary and the possibility to use the package through the Advanced Packaging Tool (APT), and Red Hat Package Manager (RPM).

In order for installation via APT or YUM, the user must add our repository to his system. To do this, just do the following steps.

## Add the repository and install Bazel

To add the repository to the system, and to always obtain the new versions of software that we provide, follow these steps.

### Add APT repository

Edit the file: */etc/apt/sources.list*

Insert the line at the end of the file:

`deb https://oplab9.parqtec.unicamp.br/pub/repository/debian/ ./`

Download our [GPG key](https://oplab9.parqtec.unicamp.br/pub/key/openpower-gpgkey-public.asc), and use the command below to add it to the system:

`sudo apt-key add openpower-gpgkey-public.asc`

After that, update the system using the command below:

`sudo apt update`

### Install Bazel

To install, use the command:

`sudo apt install bazel`

### Add RPM repository

Create and edit the file: */etc/yum.repos.d/open-power.repo*

Add the text to it:

```
[Open-Power]
name=Unicamp OpenPower Lab - $basearch
baseurl=https://oplab9.parqtec.unicamp.br/pub/repository/rpm/
enabled=1
gpgcheck=0
repo_gpgcheck=1
gpgkey=https://oplab9.parqtec.unicamp.br/pub/key/openpower-gpgkey-public.asc
```

After that, performs the following command with super-user capabilities to update the system.

`yum update`

> To be a super user, use the command: `sudo su`

### Install Bazel

To install, it use the command:

`yum install bazel`

> To install using other means, see [Installing Bazel on Power and Other Unsupported Architectures/Systems](/post/installing-bazel-power-other-architectures-systems/).
