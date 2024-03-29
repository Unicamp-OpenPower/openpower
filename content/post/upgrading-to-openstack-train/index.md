---
title: Upgrading to OpenStack Train
layout: page
date: 2020-03-04
authors: [lcnzg, vcouto, marcelo-martins, jr-santos]

aliases: [/blog/upgrading_to_openstack_train.html]

---

Recently, we’ve upgraded Minicloud’s (a Power architecture based server) Openstack environment to it’s latest version (Openstack Train), and this post aims to tackle some of the issues which we’ve faced.

**The Minicloud Environment:**

Our server holds multiple machines of two Power architectures: Power8 and Power9 servers. As for our Openstack implementation, we use a Power8 to be the controller and the remaining machines are designated as compute nodes.

**Preparations:**

There are some details to consider before upgrading, these mainly revolve around softwares and firmwares versions, as well as network architecture.

**As for firmware, make sure all your machines are up to date with the latest patches**, otherwise there can be unforeseen errors even if Openstack is correctly installed. An example of such errors is KVM’s safe cache capability which is not supported on older firmware versions.

Before starting, format all bare metal machines to make sure you’re getting a clean install (we’ve used the Ubuntu Server 18.04 as the OS for the server).** Software versions were picked considering Openstack dependencies** and the latest release available for power architecture.

As for the network, it won’t be addressed in this post, hence, if necessary, [Alta3 Research](https://alta3.com/) has a handy [playlist](https://www.youtube.com/watch?v=8FYgmM3tUCM) addressing this matter.

<span style="text-decoration:underline;">Note: avoid running an apt upgrade command after the environment is set, as some packages might break or lose it’s configurations, also, disable automatic package upgrades.</span>

**Firmware Updates:**

In case of Power machines, all you’ll need to realize an firmware update is located in [IBM’s Fix Central](https://www.ibm.com/support/fixcentral/). Simply find the requested hardware info (_lshw_ command should do the job) and search for your machine model. After finding your model, inserting it’s serial number and selecting the latest fix, you will find a page with all software and instruction for the update.

**Adjusting Simultaneous Multithreading:**

One of the main problems you’ll face with Power8 servers is the Simultaneous Multithreading (SMT) functionality. Essentially, SMT allows a better resource usage, but it can also cause errors. In our case, the SMT was completely disabled in P8 machines and set to 4 in P9 machines.

When running Openstack with SMT enabled on Power8, we dealt with VMs being allocated but remaining in a paused state as they were unable launch due to SMT configurations.

The following settings can be used to set a service with _systemd_ which will disable SMT on power machines:

    [Unit]
    Description=ppc64 set SMT off
    Before=libvirt-bin.service

    [Service]
    Type=oneshot
    RemainAfterExit=true
    ExecStart=/usr/sbin/ppc64_cpu --smt=off
    ExecStop=/usr/sbin/ppc64_cpu --smt=on

    [Install]
    WantedBy=multi-user.target


**Installation Checklist:** Here’s a helpful step by step installation checklist for an environment with multiple node:

|Steps to Execute|Controller|Compute|
|---|---|---|
|[Host networking](https://docs.openstack.org/install-guide/environment-networking.html)|![](check.png)|![](check.png)|
|[Network Time Protocol (NTP)](https://docs.openstack.org/install-guide/environment-ntp.html)|![](check.png)|![](check.png)|
|[OpenStack packages](https://docs.openstack.org/install-guide/environment-packages.html)|![](check.png)|![](check.png)|
|[SQL database](https://docs.openstack.org/install-guide/environment-sql-database.html)|![](check.png)|![](cross.png)|
|[Message queue](https://docs.openstack.org/install-guide/environment-messaging.html)|![](check.png)|![](cross.png)|
|[Memcached](https://docs.openstack.org/install-guide/environment-memcached.html)|![](check.png)|![](cross.png)|
|[Etcd](https://docs.openstack.org/install-guide/environment-etcd.html)|![](check.png)|![](cross.png)|
|[keystone](https://docs.openstack.org/keystone/train/install/)|![](check.png)|![](cross.png)|
|[glance](https://docs.openstack.org/glance/train/install/)|![](check.png)|![](cross.png)|
|[placement](https://docs.openstack.org/placement/train/install/)|![](check.png)|![](check.png)|
|[nova](https://docs.openstack.org/nova/train/install/)|![](check.png)|![](check.png)|
|[neutron](https://docs.openstack.org/neutron/train/install/)|![](check.png)|![](cross.png)|
|[horizon](https://docs.openstack.org/horizon/train/install/)|![](check.png)|![](check.png)|

**Troubleshooting:**

In this section we’ll share some of the errors we had during installation and the solutions we found to each of them. Note that these errors are not specifically of POWER architecture installations.

#### MariaDB note:
Some SQL commands were failing due to unknown reasons even with the correct dependencies. A solution we found for this issue was bumping our mariaDB version from 10.2 to 10.4.

#### Apache & Horizon Login:
A small change to Horizon from the previous OpenStack release was the dashboard login page URL settings. Simply using **<IP address>/horizon** would redirect to the login page in previous versions. This might require some redirection tweaks in the Apache server configuration file.

#### Virtual Interface Exception:
When attempting to create a VM, the following error was presented by the Nova module: *VirtualInterfaceCreateException: Virtual Interface creation failed.*\
To fix this, we[ followed the instructions from a post](https://ask.openstack.org/en/question/26938/virtualinterfacecreateexception-virtual-interface-creation-failed/) in which two lines of configurations are added to the _nova.conf_ file: **vif_plugging_is_fatal: false** and **vif_plugging_timeout: 0.**

Good luck upgrading.
<!-- Docs to Markdown version 1.0β18 -->
