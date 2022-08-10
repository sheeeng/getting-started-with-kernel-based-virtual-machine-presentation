---
title: 'Getting Started with Kernel-based Virtual Machine (KVM)'
description: 'Presentation slides for Getting Started with Kernel-based Virtual Machine (KVM) workshop at Open Source Summit Europe 2022.'
header: '**Getting Started with Kernel-based Virtual Machine (KVM)** / Open Source On-Ramp / **Open Source Summit Europe 2002**'
author: 'Leonard Sheng Sheng Lee'
footer: 'Leonard Sheng Sheng Lee / Made with [Marp](https://marp.app/)'
keywords: linux,kvm,virtualization,marp,marp-cli,slide
marp: true
paginate: true
theme: uncover
backgroundImage: url('https://marp.app/assets/hero-background.svg')
---

# Getting Started with Kernel-based Virtual Machine (KVM)

![bg 100% opacity blur](https://marp.app/assets/hero-background.svg)

---

### <!--fit--> [Code of Conduct](https://events.linuxfoundation.org/open-source-summit-europe/attend/code-of-conduct/)

---

## <!--fit--> :raising_hand_man: :raising_hand: :raising_hand_woman:

---

## Overview of Kernel-based Virtual Machine (KVM)

- An open source virtualization technology built into Linux®. Turn Linux into a hypervisor.
- Allows a host machine to run multiple, isolated virtual environments called guests or virtual machines (VMs).
- Available from Linux 2.6.20 or newer.

<!--
Some speaker notes here that might be useful.

The default virtualization technology supported in Ubuntu is KVM. For Intel and AMD hardware KVM requires virtualization extensions. But KVM is also available for IBM Z and LinuxONE, IBM POWER as well as for ARM64.
-->

---

## Overview of Kernel-based Virtual Machine (KVM)

- QEMU (Quick Emulator) is part of the KVM experience being the userspace backend for it, but it also can be used for hardware without virtualization extensions by using its Tiny Code Generator (TCG) mode.

<!--
Some speaker notes here that might be useful.

The Tiny Code Generator (TCG) is the core binary translation engine that is responsible for QEMU ability to emulate foreign processors on any given supported host.
-->

---

## Hardware Virtualization Support

- KVM requires a CPU with virtualization extensions.
  - Intel® Virtualization Technology (Intel® VT)
    - CPU flag is `vmx` (Virtual Machine Extensions).
  - AMD virtualization (AMD-V)
    - CPU flag is `svm` (Secure Virtual Machine).

---

## Hardware Virtualization Support

- Run the following command to check:

```shell
egrep '^flags.*(vmx|svm)' /proc/cpuinfo
```

- Nothing printed? your system does not support the relevant virtualization extensions. You can still use QEMU/KVM, but the emulator will fall back to software virtualization, which is much slower.

---

## Installing Virtualization Software

```shell
sudo dnf group install \
    virtualization \
    --with-optional \
    --assumeyes
```

```shell
sudo apt-get install \
    bridge-utils cpu-checker libvirt-clients \
    libvirt-daemon qemu qemu-kvm \
    --assume-yes
```

---

## Slide 3

Text

---

## Slide 4

Text

---

## Links

- <https://osseu2022.sched.com/event/15z24/getting-started-with-kernel-based-virtual-machine-kvm-leonard-sheng-sheng-lee-computas>
- <https://docs.fedoraproject.org/en-US/quick-docs/getting-started-with-virtualization/>
- <https://help.ubuntu.com/community/KVM/Installation>
- <https://ubuntu.com/blog/kvm-hyphervisor>
- <https://www.intel.com/content/www/us/en/virtualization/virtualization-technology/intel-virtualization-technology.html>
- <https://wiki.qemu.org/Features/TCG>
- <https://virt-manager.org/>

---

## Abstract (Part 1)

Want to get started with Kernel-based Virtual Machine (KVM)? Want to run a virtual machine on your system using open source technologies? Want to interact with KVM virtual machines from command line interface (CLI)? In this tutorial, Leonard will be teaching people to familiarize themselves with KVM technologies, which allows virtual machines to run with near native performance.

---

## Abstract (Part 2)

Participants must have a basic knowledge on how Linux operating system works, and must have a Linux based operating system running natively on a laptop in order to join this tutorial. We will be focusing on doing some tasks of creating, accessing, modifying, and deleting KVMs, from both graphical user interface (GUI) and CLI.

---

## Abstract (Part 3)

We will busy with these virt-manager tools in the tutorial. - virt-install is a command line tool which provides an easy way to provision operating systems into virtual machines. - virt-viewer is a lightweight UI interface for interacting with the graphical display of virtualized guest OS. It can display VNC or SPICE, and uses libvirt to lookup the graphical connection details.

---

## Abstract (Part 4)

At the end of this tutorial, participants are expected to know how to check if KVM is supported on their hardware, create and manager KVMs with confident, from both GUI and CLI.

---

### Contact

Leonard Sheng Sheng Lee

<https://github.com/sheeeng>

---

### <!--fit--> :pray:
---

### End
