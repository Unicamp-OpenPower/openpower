---
title: Options for Kubernetes-CRI (Container Runtime Interface)
layout: page
date: 2021-02-26
authors: [jr-santos]

aliases: [/blog/options-for-kubernetes-cri.html]

image:
  caption:
  focal_point: Smart

---

Docker has been used for a long time with Kubelet and Kubernetes. However, recently Docker announced that it would cut
[support for kubelet](https://www.zdnet.com/article/kubernetes-dropping-docker-is-not-that-big-of-a-deal/).

Because of this, this blog intends to present existing options to assume this role with Kubernetes.
All Kubernetes needs is a Container Runtime Interface (CRI), for that there are several portable software for the IBM Power architecture (ppc64le),
as it will be presented throughout the post.

## Main Runtimes

Through the Open Source community it is possible to find some packages like: [Containerd-CRI](https://github.com/containerd/cri), [Cri-O](https://github.com/cri-o/cri-o) e [Gvisor](https://github.com/google/gvisor).
All packages have the function of providing the support that Kubernetes needs, and some of them even have extra and more specialized features.

For most of these packages it was possible to port to the Power architecture, however, as will be presented later, some were not possible to perform portability.

## Containerd-CRI

### Description and Portability

CRI is a Containerd plugin for Kubernetes, it is a Container Runtime Interface (CRI) implemented to interact with Kubelet in the creation of containers using Containerd as Runtime.

The software does not have official support for the IBM Power architecture, despite that, OpenPower Lab did a build job and made the package functional in Power. In addition, we offer the package in the following formats: binary, .rpm and .deb.

### Build

The construction of the package was done following the recipe for CI/CD of the package, it is possible to obtain it through the [GitHub](https://github.com/Unicamp-OpenPower/containerd-cri-releases/blob/master/build.sh).
Based on it, we make the package available in binary format which can be obtained through [FTP](https://oplab9.parqtec.unicamp.br/pub/ppc64el/containerd-cri/).
The rest of the packages, available for Debian and RHEL based distros, can be installed through [OPenPower lab repository](https://openpower.ic.unicamp.br/project/power-repository/).

## Cri-O

### Description and Portability

Cri-O is a CRI (Container Runtime Interface) implementation, being a lighter alternative to Docker as a Runtime. Since Docker is no longer an option, it becomes one of the lightest options available.

The software does not have official support for the IBM Power architecture, despite that, OpenPower Lab did a build job and made the package functional in Power. In addition, the package is available in binary version and in .deb format.
Although it was not possible to generate the .rpm package, it is still possible to install the package with the installation of the binary.

### Build

The construction of the package can be obtained through [build](https://github.com/Unicamp-OpenPower/crio-build). The binary can be obtained by [FTP](https://oplab9.parqtec.unicamp.br/pub/ppc64el/crio/), despite this, it is necessary to install the [prerequisites](https://github.com/cri-o/cri-o/blob/master/install.md#runtime-dependencies).
The .deb format can be installed via the [OPenPower lab repository](https://openpower.ic.unicamp.br/project/power-repository/).

### Install

It was not possible to convert the binary to the RPM Package Manager, so the only way to install these systems is to install the [binary](https://oplab9.parqtec.unicamp.br/pub/ppc64el/crio/).
Before installing the binary it is necessary to install the [prerequisites](https://github.com/cri-o/cri-o/blob/master/install.md#runtime-dependencies).
After installing the prerequisites, just use the command below:

```bash
sudo tar -C / -xzvf crio-1.20.0.linux-ppc64le.tar.gz
```

Confirm the installation by executing the command:

```bash
crio --version
```

## Gvisor

### Description and Portability

GVisor is a software widely used by the community, it is focused on integration with Docker and Kubernetes.
The code is written in GO, uses [Bazel](https://github.com/bazelbuild/bazel) to build the source code and generate the binary.
However, Bazel requires several build files, and supported architectures must be specified. However, the Gvisor only has reference to AMD (x86_64).

As such, Bazel is unable to generate the binary for IBM Power (ppc64le). Therefore, Gvisor does not have support for the Power architecture, and portability is considered as difficult and necessary reverse engineering work in the source code.

## Links
- The OpenPower Lab repository contains several Open-Source software for Power. See the list of [available packages](https://openpower.ic.unicamp.br/project/power-repository/).
- See more about the lab's work and our research by reading other [posts](https://openpower.ic.unicamp.br/#posts).
