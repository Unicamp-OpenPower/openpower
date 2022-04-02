---
title: Simulating Microwatt with GHDL
layout: page
date: 2022-04-02
authors: [iagocaran]

aliases: [/blog/simulating-microwatt-with-ghdl.html]

---

GHDL is an open-source analyzer, compiler and synthesizer for VHDL generating machine code from a Hardware Description Language. As it is not an interpreter, it allows for high speed simulation which is crucial for applications like the Microwatt, a tiny Open POWER ISA softcore.

In this tutorial you will be guided step-by-step in the setup of your environment for simulating Microwatt with GHDL.

Disclaimer: For the purpose of this tutorial I will be using an Ubuntu 20.04 system and I'll assume you're familiar with it.

## Prerequisites

First of all, make sure you have this packages installed:

```bash
sudo apt update
sudo apt install -y build-essential gnat zlib1g-dev llvm-dev clang git
```

This includes the project's dependencies, as well as git and the basic packages to build a project.

## Installing GHDL

We'll now clone and build GHDL.

```bash
git clone https://github.com/ghdl/ghdl.git
cd ghdl
./configure --prefix=/usr/local --enable-libghdl --with-llvm-config
make
make install
```

## Install Microwatt

We are now ready to clone and build Microwatt.

```bash
git clone https://github.com/antonblanchard/microwatt.git
cd microwatt
make
```

### Testing

Now that everything is installed we can run some code.
Fortunately the project already includes a hello_world ready to be executed, just change the file to be executed and then run the simulation:

```bash
ln -s hello_world/hello_world.bin main_ram.bin
./core_tb > /dev/null
```

You should see a lightbulb being drawn on the terminal.
But if you want something more substantial, you will find bundled a python runtime:

```bash
rm main_ram.bin
ln -s micropython/firmware.bin main_ram.bin
./core_tb > /dev/null
```

### (Optional) Build MicroPython

To compile code for the Power platform you will need a cross compiler, luckily it is already in the Ubuntu repository.
With that, just compile the project normally.

```bash
sudo apt install -y gcc-powerpc64le-linux-gnu
git clone https://github.com/micropython/micropython.git
cd micropython/ports/powerpc
make -j$(nproc) UART=lpc_serial
cd ../../../
rm main_ram.bin
ln -s ../micropython/ports/powerpc/build/firmware.bin main_ram.bin
./core_tb > /dev/null
```

Note that it is necessary to set the UART interface to lpc_serial when simulating using GHDL.

## Compiling code

To compile code for the Power architecture you need the cross compiler shown in the optional section on MicroPython.
With it installed, just duplicate the hello_world folder and use it as a base for your code.
It is important to remember that the communication should be done using the headers provided by MicroWatt, as the terminal is redirected to the UART port.

## Problems using WSL and Windows

You can do this entire process within WSL for Windows 10 and 11, but some people will experience random crashes when using the Home version of these systems. This is due to some incompatibility of the Virtual Machine Platform component with your hardware. A possible solution is to upgrade to Windows Pro, but I opted to use Ubuntu instead.

## Reference

* [Building GHDL from sources](https://ghdl.github.io/ghdl/development/building/index.html)
* [A tiny Open POWER ISA softcore written in VHDL 2008](https://github.com/antonblanchard/microwatt)

