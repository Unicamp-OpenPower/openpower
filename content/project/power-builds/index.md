---
title: POWER Builds
summary: Open Source projects validations on POWER
tags:
- CI
- Builds

# Optional external URL for project (replaces project detail page).
#external_link: ""

image:
  caption:
  focal_point: Smart

links:
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
#slides: example
---

Our infrastructure is being used to validate several Open Source projects.

# Open-source projects


|Status|Project|FTP Link (ppc64le binaries)|
|---|---|---|
| ![](https://travis-ci.org/Unicamp-OpenPower/bazel-releases.png) | [Bazel](https://bazel.build) | [https://oplab9.parqtec.unicamp.br/pub/ppc64el/bazel](https://oplab9.parqtec.unicamp.br/pub/ppc64el/bazel/)
| ![](https://travis-ci.org/Unicamp-OpenPower/containerd-releases.png) | [Containerd](https://containerd.io) | [https://oplab9.parqtec.unicamp.br/pub/ppc64el/containerd](https://oplab9.parqtec.unicamp.br/pub/ppc64el/containerd/)
| ![](http://minicloud.parqtec.unicamp.br:60000/job/containerd-cri-release/badge/icon?) | [Containerd-cri](https://github.com/containerd/cri) | [https://oplab9.parqtec.unicamp.br/pub/ppc64el/containerd-cri](https://oplab9.parqtec.unicamp.br/pub/ppc64el/containerd-cri/)
| ![](http://minicloud.parqtec.unicamp.br:60000/job/docker-ce-releases/badge/icon?) | [Docker CE](https://docs.docker.com/install) | [https://oplab9.parqtec.unicamp.br/pub/ppc64el/docker](https://oplab9.parqtec.unicamp.br/pub/ppc64el/docker)
| ![](https://travis-ci.org/Unicamp-OpenPower/glide-releases.png) | [Glide](https://glide.sh) | [https://oplab9.parqtec.unicamp.br/pub/ppc64el/glide](https://oplab9.parqtec.unicamp.br/pub/ppc64el/glide)
| ![](http://minicloud.parqtec.unicamp.br:60000/job/grafana-releases/badge/icon?) | [Grafana](https://grafana.com) | [https://oplab9.parqtec.unicamp.br/pub/ppc64el/grafana](https://oplab9.parqtec.unicamp.br/pub/ppc64el/grafana)
| ![](https://travis-ci.org/Unicamp-OpenPower/kiali-releases.png) | [Kiali](https://www.kiali.io) | [https://oplab9.parqtec.unicamp.br/pub/ppc64el/kiali](https://oplab9.parqtec.unicamp.br/pub/ppc64el/kiali)
| ![](https://travis-ci.org/Unicamp-OpenPower/kubeadm-releases.png) | [Kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/) | [https://oplab9.parqtec.unicamp.br/pub/ppc64el/kubeadm](https://oplab9.parqtec.unicamp.br/pub/ppc64el/kubeadm)
| ![](https://travis-ci.org/Unicamp-OpenPower/kubectl-releases.png) | [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) | [https://oplab9.parqtec.unicamp.br/pub/ppc64el/kubectl](https://oplab9.parqtec.unicamp.br/pub/ppc64el/kubectl)
| ![](https://travis-ci.org/Unicamp-OpenPower/kubelet-releases.png) | [Kubelet](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/) | [https://oplab9.parqtec.unicamp.br/pub/ppc64el/kubelet](https://oplab9.parqtec.unicamp.br/pub/ppc64el/kubelet)
| ![](https://api.travis-ci.org/Unicamp-OpenPower/matchbox-releases.png) | [Matchbox](https://matchbox.psdn.io/) | [https://oplab9.parqtec.unicamp.br/pub/ppc64el/matchbox](https://oplab9.parqtec.unicamp.br/pub/ppc64el/matchbox)
| ![](https://travis-ci.org/Unicamp-OpenPower/minikube-releases.png) | [Minikube](https://kubernetes.io/docs/setup/minikube) | [https://oplab9.parqtec.unicamp.br/pub/ppc64el/minikube](https://oplab9.parqtec.unicamp.br/pub/ppc64el/minikube)
| ![](https://travis-ci.org/Unicamp-OpenPower/minio-releases.png) | [Minio](https://min.io) | [https://oplab9.parqtec.unicamp.br/pub/ppc64el/minio](https://oplab9.parqtec.unicamp.br/pub/ppc64el/minikube)
| ![](https://travis-ci.org/Unicamp-OpenPower/minio-mc-releases.png) | [Minio-MC](https://min.io) | [https://oplab9.parqtec.unicamp.br/pub/ppc64el/minio-mc](https://oplab9.parqtec.unicamp.br/pub/ppc64el/minikube)
| ![](https://api.travis-ci.org/Unicamp-OpenPower/rclone-releases.png) | [Rclone](https://rclone.org/) | [https://oplab9.parqtec.unicamp.br/pub/ppc64el/rclone](https://oplab9.parqtec.unicamp.br/pub/ppc64el/rclone)
| ![](https://travis-ci.org/Unicamp-OpenPower/restic-releases.png) | [Restic](https://restic.net) | [https://oplab9.parqtec.unicamp.br/pub/ppc64el/restic](https://oplab9.parqtec.unicamp.br/pub/ppc64el/restic)
| ![](https://travis-ci.org/Unicamp-OpenPower/terraform-releases.png) | [Terraform](https://www.terraform.io) | [https://oplab9.parqtec.unicamp.br/pub/ppc64el/terraform](https://oplab9.parqtec.unicamp.br/pub/ppc64el/terraform)


<!---  *********** OLD PROJECTS ***********
| Glibc | The GNU C Library is used as the C library in the GNU system and in GNU/Linux systems, as well as many other systems that use Linux as the kernel.
| GDB   | The GNU Project debugger, allows you to see what is going on `inside' another program while it executes -- or what another program was doing at the moment it crashed.
| Go    | Go is an open source programming language that makes it easy to build simple, reliable, and efficient software.
| LLVM  | The LLVM Project is a collection of modular and reusable compiler and toolchain technologies.
| Open CASCADE | A 3D modeling kernel that consists of reusable C++ object libraries that are available as Open Source.
| Computing on Linear Algebra | Blast library
| Openjdk | an open-source implementation of the Java Platform, Standard Edition, and related projects
Plus several individual users who are using Power for academic research (HTM) or for testing/developing code (tightvnc, qemu, JNA, etc.)
# Research projects
|Project Name|Description|People Involved|
|---|---|---|
|HTM|Hardware Transactional Memory on Power architecture|University of Campinas and University of Alberta|
|Code Verification|Code verification between multiple architectures|University of Campinas|
|Seismic - CRS| Common Reflection Surface|University of Campinas|
-->
