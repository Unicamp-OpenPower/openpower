---
title: POWER Repository
summary: Open Source packages build on POWER
tags:
- Repository
- Packages
- APT
- RPM

# Optional external URL for project (replaces project detail page).
#external_link: ""

image:
  caption:
  focal_point: Smart
---
We created a repository with all the open source projects that we build on Power.

# Open-source packages

|Project|Package|Description of package|
|---|---|---|
| [Bazel](https://bazel.build) | bazel | Build and test software of any size, quickly and reliably.
| [Containerd](https://containerd.io) | containerd | An industry-standard container runtime with an emphasis on simplicity, robustness and portability.
| [Containerd-cri](https://github.com/containerd/cri) | containerd-cri | Containerd Plugin for Kubernetes Container Runtime Interface.
| [Containerd-cri](https://github.com/containerd/cri) | containerd-cri-cni | Containerd Plugin for Kubernetes Container Runtime Interface.
| [Docker CE](https://docs.docker.com/install) | docker-ce | Docker Engine is the industry’s de facto container runtime that runs on various operating systems.
| [Docker CE](https://docs.docker.com/install) | docker-ce-cli | Docker Engine is the industry’s de facto container runtime that runs on various operating systems.
| [Glide](https://glide.sh) | glide | Package Management for Go.
| [Grafana](https://grafana.com) | grafana-cli | The tool for beautiful monitoring and metric analytics & dashboards for Graphite, InfluxDB & Prometheus & More.
| [Grafana](https://grafana.com) | grafana-server | The tool for beautiful monitoring and metric analytics & dashboards for Graphite, InfluxDB & Prometheus & More.
| [Kiali](https://www.kiali.io) | kiali | Service mesh management for Istio.
| [Kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/) | kubeadm | The command to bootstrap the cluster.
| [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) | kubectl | The command line util to talk to your cluster.
| [Kubelet](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/) | kubelet | The component that runs on all of the machines in your cluster and does things like starting pods and containers.
| [Matchbox](https://matchbox.psdn.io/) | matchbox | Matchbox is a service that matches bare-metal machines to profiles that PXE boot and provision clusters.
| [Minikube](https://kubernetes.io/docs/setup/minikube) | minikube | Minikube is a tool that makes it easy to run Kubernetes locally.
| [Minio](https://min.io) | minio | High Performance, Kubernetes Native Object Storage.
| [Minio-MC](https://min.io) | mc | MinIO Client is a replacement for ls, cp, mkdir, diff and rsync commands for filesystems and object storage.
| [Rclone](https://rclone.org/) | rclone | "rsync for cloud storage" - Google Drive, Amazon Drive, and more.
| [Restic](https://restic.net) | restic | Fast, secure, efficient backup program.
| [Terraform](https://www.terraform.io) | terraform | Use Infrastructure as Code to provision and manage any cloud, infrastructure, or service.


## Add the repository and install some package

To add the repository to the system, and to always obtain the new versions of software that we provide, follow these steps.

### Add APT repository

Edit the file: */etc/apt/sources.list*

Insert the line at the end of the file:

`deb https://oplab9.parqtec.unicamp.br/pub/repository/debian/ ./`

Download our [GPG key](https://oplab9.parqtec.unicamp.br/pub/key/openpower-gpgkey-public.asc), and use the command below to add it to the system:

`sudo apt-key add openpower-gpgkey-public.asc`

After that, update the system using the command below:

`sudo apt update`

### Install package

To install, use the command:

`sudo apt install package`

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

### Install Package

To install, it use the command:

`yum install package`
