---
title: Upgrading to OpenStack Train
layout: page
date: 2020-03-04
authors: [vcouto, marcelo-martins, jr-santos]

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


<table>
  <tr>
   <td><strong>Steps to Execute</strong>
   </td>
   <td><strong>Controller</strong>
   </td>
   <td><strong>Compute</strong>
   </td>
  </tr>
  <tr>
   <td><a href="https://docs.openstack.org/install-guide/environment-networking.html">Host networking</a>
   </td>
   <td><strong><img src="check.png" height="10%"/></strong>
   </td>
   <td><strong><img src="check.png" height="10%"/></strong></strong>
   </td>
  </tr>
  <tr>
   <td><a href="https://docs.openstack.org/install-guide/environment-ntp.html">Network Time Protocol (NTP)</a>
   </td>
   <td><strong><img src="check.png" height="10%"/></strong></strong>
   </td>
   <td><strong><img src="check.png" height="10%"/></strong></strong>
   </td>
  </tr>
  <tr>
   <td><a href="https://docs.openstack.org/install-guide/environment-packages.html">OpenStack packages</a>
   </td>
   <td><strong><img src="check.png" height="10%"/></strong></strong>
   </td>
   <td><strong><img src="check.png" height="10%"/></strong></strong>
   </td>
  </tr>
  <tr>
   <td><a href="https://docs.openstack.org/install-guide/environment-sql-database.html">SQL database</a>
   </td>
   <td><strong><img src="check.png" height="10%"/></strong></strong>
   </td>
   <td><strong><img src="cross.png" height="8.5%"/></strong></strong>
   </td>
  </tr>
  <tr>
   <td><a href="https://docs.openstack.org/install-guide/environment-messaging.html">Message queue</a>
   </td>
   <td><strong><img src="check.png" height="10%"/></strong></strong>
   </td>
   <td><strong><img src="cross.png" height="8.5%"/></strong>
   </td>
  </tr>
  <tr>
   <td><a href="https://docs.openstack.org/install-guide/environment-memcached.html">Memcached</a>
   </td>
   <td><strong><img src="check.png" height="10%"/></strong></strong>
   </td>
   <td><strong><img src="cross.png" height="8.5%"/></strong>
   </td>
  </tr>
  <tr>
   <td><a href="https://docs.openstack.org/install-guide/environment-etcd.html">Etcd</a>
   </td>
   <td><strong><img src="check.png" height="10%"/></strong></strong>
   </td>
   <td><strong><img src="cross.png" height="8.5%"/></strong>
   </td>
  </tr>
  <tr>
   <td><a href="https://docs.openstack.org/keystone/train/install/">keystone installation for Train</a>
   </td>
   <td><strong><img src="check.png" height="10%"/></strong></strong>
   </td>
   <td><strong><img src="cross.png" height="8.5%"/></strong>
   </td>
  </tr>
  <tr>
   <td><a href="https://docs.openstack.org/glance/train/install/">glance installation for Train </a>
   </td>
   <td><strong><img src="check.png" height="10%"/></strong></strong>
   </td>
   <td><strong><img src="cross.png" height="8.5%"/></strong>
   </td>
  </tr>
  <tr>
   <td><a href="https://docs.openstack.org/placement/train/install/">placement installation for Train</a>
   </td>
   <td><strong><img src="check.png" height="10%"/></strong></strong>
   </td>
   <td><strong><img src="cross.png" height="8.5%"/></strong>
   </td>
  </tr>
  <tr>
   <td><a href="https://docs.openstack.org/nova/train/install/">nova installation for Train</a>
   </td>
   <td><strong><img src="check.png" height="10%"/></strong></strong>
   </td>
   <td><strong><img src="check.png" height="10%"/></strong></strong>
   </td>
  </tr>
  <tr>
   <td><a href="https://docs.openstack.org/neutron/train/install/">neutron installation for Train</a>
   </td>
   <td><strong><img src="check.png" height="10%"/></strong></strong>
   </td>
   <td><strong><img src="check.png" height="10%"/></strong></strong>
   </td>
  </tr>
  <tr>
   <td><a href="https://docs.openstack.org/horizon/train/install/">horizon installation for Train</a>
   </td>
   <td><strong><img src="check.png" height="10%"/></strong></strong>
   </td>
   <td><strong><img src="cross.png" height="8.5%"/></strong>
   </td>
  </tr>
  <tr>
   <td><a href="https://docs.openstack.org/install-guide/launch-instance.html">Launch an instance</a>
   </td>
   <td><strong><img src="check.png" height="10%"/></strong></strong>
   </td>
   <td><strong><img src="check.png" height="10%"/></strong></strong>
   </td>
  </tr>
</table>


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
