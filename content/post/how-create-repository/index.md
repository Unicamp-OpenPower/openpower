---
title: Creating and adding a Linux repository
layout: page
date: 2020-08-18
authors: [jr-santos]

aliases: [/blog/how-create-repository.html]

---

## Introduction

The OpenPower@Unicamp laboratory performs [CI/CD](/project/power-builds/) activity for several open-source programs that are not available for the Power architecture. Besides, we make the binaries generated from these programs available on our [FTP](https://oplab9.parqtec.unicamp.br/).

> To add our repositories to the system, jump to [step 5](#step-5---adding-the-repository-to-the-system).

## Problem

Many of the programs we make available have frequent updates and can generate several versions within a month. Based on this, several users needed to constantly access FTP to keep their programs up to date.

As a result, we decided to create two repositories. A repository for distros which use the Advanced Packaging Tool (APT), and another for distros that use the Red Hat Package Manager (RPM).

## Steps

### Step 1 - Prerequisites

To create a repository, we first need the programs to be hosted on a place with a public IP and that can be accessed by the browser from anywhere in the world.

For us, this was a simple matter, our FTP has these characteristics.

### Step 2 - Create directory

This information must be accessible to the general public through the browser, so it is necessary to establish where this information will be in the system.

FTP is a specific path of the server system. Currently, the path /pub/ppc64el is used to store programs binaries. Then we create the path /pub/repository/debian/ to store the APT repository information, and /pub/repository/rpm/ to store the RPM repository information.

> The steps to create an FTP or make a server system path accessible by browsers will not be covered in this guide.

### Step 3 - Add programs

As we are developing binaries and programs for a specific architecture, we create a folder with the architecture name. For APT, it is *ppc64el*, and for RPM, *ppc64le*.

The next step is to add all the programs into the folder with architecture name. As each of our programs has several versions, we created a folder with the name of the program, and all of its versions are inside it.

> To create a program in APT format, you can see [this tutorial](https://terminalroot.com.br/2014/12/como-criar-pacotes-deb.html).

> To create a program in RPM format, you can see [this tutorial](https://opensource.com/article/18/9/how-build-rpm-packages).

### Step 4 - Create compatible file systems

For the file system to recognize the repository, it is necessary to create some files. These files contain information about the program and their path within the directory. Besides that, they can be signed with a GPG key, which serves to prove the authenticity of the programs present in the repository.

> To create a gpg key, you can see [this tutorial](https://www.redhat.com/sysadmin/creating-gpg-keypairs).

#### Generate essential APT files

To create the files, run the following commands inside the folder that will be the path to your directory. In our case in /pub/repository/debian/

```
dpkg-scanpackages -m . >> Packages
gzip -c Packages >> Packages.gz
apt-ftparchive release . > Release
keyname=name-your-gpgkey
rm -fr Release.gpg; gpg --default-key ${keyname} -abs -o Release.gpg Release
rm -fr InRelease; gpg --default-key ${keyname} --clearsign -o InRelease Release
```

#### Generate essential RPM files

To create the files, run the following commands inside the folder that will be the path to your directory. In our case in /pub/repository/rpm/

```
createrepo --database /pub/repository/rpm/
gpg --detach-sign --armor repodata/repomd.xml
```

### Step 5 - Add the repository to the system

To add the repository to the system, and always stay updated on the new versions of the software we make available, follow the next steps.

#### Add APT repository

Edit the file: */etc/apt/sources.list*

Insert the line at the end of the file:

`deb https://oplab9.parqtec.unicamp.br/pub/repository/debian/ ./`

Download our [GPG key](https://oplab9.parqtec.unicamp.br/pub/key/openpower-gpgkey-public.asc), and use the command below to add it to the system:

`apt-key add openpower-gpgkey-public.asc`

Lastly, update the system and install any of the program we provide.

#### Add RPM repository

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

Lastly, update the system and install any of the program we provide.
