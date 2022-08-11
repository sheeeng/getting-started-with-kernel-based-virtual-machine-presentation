---
title: 'Getting Started with Kernel-based Virtual Machine (KVM)'
description: 'Presentation slides for Getting Started with Kernel-based Virtual Machine (KVM) workshop at Open Source Summit Europe 2022.'
header: '**Getting Started with Kernel-based Virtual Machine (KVM)** / Open Source On-Ramp / **Open Source Summit Europe 2022**'
author: 'Leonard Sheng Sheng Lee'
footer: 'Leonard Sheng Sheng Lee / Made with [Marp](https://marp.app/) / Participants Agree to Abide by [Code of Conduct](https://events.linuxfoundation.org/open-source-summit-europe/attend/code-of-conduct/)'
keywords: linux,kvm,virtualization,marp,marp-cli,slide
marp: true
paginate: true
theme: uncover
backgroundImage: url('https://marp.app/assets/hero-background.svg')
---

# Getting Started with Kernel-based Virtual Machine (KVM)

![bg 100% opacity blur](https://marp.app/assets/hero-background.svg)

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
    - CPU flag is **`vmx`** (_Virtual Machine Extensions_).
  - AMD virtualization (AMD-V)
    - CPU flag is **`svm`** (_Secure Virtual Machine_).

---

## Hardware Virtualization Support

- Run the following command to check:

```shell
egrep --count '^flags.*(vmx|svm)' /proc/cpuinfo
```

- If output is 0, your system does not support the relevant virtualization extensions _or_ disabled on BIOS. You can still use QEMU/KVM, but the emulator will fall back to software virtualization, which is much slower.

---

## Installing Virtualization Packages

- [Fedora](https://docs.fedoraproject.org/en-US/quick-docs/getting-started-with-virtualization/#installing-virtualization-software)

```shell
dnf groupinfo virtualization

dnf group install \
    virtualization \
    --with-optional \
    --assumeyes
```

---

## Enable **`libvirtd`** Service

- The **`libvirtd`** service is a server side daemon and driver required to manage the virtualization capabilities of the KVM hypervisor.

- Start **`libvirtd`** service and enable it on boot.

```shell
systemctl start libvirtd

systemctl enable libvirtd
```

---

## Verify KVM Kernel Modules

- Verify that the KVM kernel modules are properly loaded.

```shell
lsmod | egrep 'kvm_*(amd|intel)'
```

- If output contains **kvm_intel** or **kvm_amd**, KVM is properly configured.

---

## Append Groups to Manage KVM

- Append current user to `kvm` and `libvirt` groups to create and manage virtual machines.

```shell
usermod --append --groups=kvm,libvirt ${USER}

cat /etc/group | egrep "^(kvm|libvirt).*${USER}"
```

- Log out and log in again to apply this modification.

---

## Install Virtual Machine / Terminal

- Launch Debian instance on KVM. :heart:

```shell
virt-install \
    --name Debian \
    --description 'Debian' \
    --vcpus 1 \
    --ram 1024 \
    --location https://ftp.debian.org/debian/dists/stable/main/installer-amd64 \
    --os-variant debian11 \
    --network bridge=virbr0 \
    --graphics vnc,listen=127.0.0.1,port=5901 \
    --noreboot \
    --extra-args 'console=ttyS0,115200n8 serial'
```

<!--
Some speaker notes here that might be useful.

Opens an interactive console that you can use to manually install the guest virtual machine.
-->

---

## Install Virtual Machine / Terminal

- Launch Ubuntu instance on KVM. :broken_heart:

```shell
virt-install \
    --name Ubuntu \
    --os-variant ubuntu20.04 \
    --vcpus 1 \
    --ram 1024 \
    --location http://ftp.ubuntu.com/ubuntu/dists/focal/main/installer-amd64/ \
    --network bridge=virbr0,model=virtio \
    --graphics vnc,listen=127.0.0.1,port=5902 \
    --extra-args='console=ttyS0,115200n8 serial'
```

<!--
Some speaker notes here that might be useful.

Opens an interactive console that you can use to manually install the guest virtual machine.
-->

---

## Install Virtual Machine / Terminal

- Launch Ubuntu instance on KVM. :broken_heart:

```shell
virt-install \
    --name Fedora36 \
    --description 'Fedora 36 Workstation' \
    --ram 4096 \
    --vcpus 2 \
    --disk path=/var/lib/libvirt/images/Fedora-Workstation-36/Fedora-Workstation-Guest.qcow2,size=20 \
    --os-variant fedora36 \
    --network bridge=virbr0 \
    --graphics vnc,listen=127.0.0.1,port=5903 \
    --cdrom /var/lib/libvirt/images/Fedora-Workstation-36/Fedora-Workstation-Live-x86_64-36-1.5.iso \
    --noautoconsole
```

---

## Slide 4

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
- <https://libvirt.org/docs.html>
- <https://wiki.debian.org/KVM>

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
