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

## Abstract (Part 1/6)

- Want to get started with Kernel-based Virtual Machine (KVM)?
- Want to run a virtual machine on your system using open source technologies?
- Want to interact with KVM virtual machines from command line interface (CLI)?

---

## Abstract (Part 2/6)

In this tutorial, Leonard will be teaching people to familiarize themselves with KVM technologies, which allows virtual machines to run with near native performance.

---
## Abstract (Part 4/6)

Participants must have a basic knowledge on how Linux operating system works, and must have a Linux based operating system running natively on a laptop in order to join this tutorial. We will be focusing on doing some tasks of creating, accessing, modifying, and deleting KVMs, from both graphical user interface (GUI) and CLI.

---

## Abstract (Part 5/6)

We will busy with these virt-manager tools in the tutorial. - `virt-install` is a command line tool which provides an easy way to provision operating systems into virtual machines. - `virt-viewer` is a lightweight UI interface for interacting with the graphical display of virtualized guest OS. It can display VNC or SPICE, and uses libvirt to lookup the graphical connection details.

---

## Abstract (Part 6/6)

At the end of this tutorial, participants are expected to know how to check if KVM is supported on their hardware, create and manager KVMs with confident, from both GUI and CLI.

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

## Installing Virtualization Packages (Fedora)

```shell
dnf groupinfo virtualization

dnf group install \
    virtualization \
    --with-optional \
    --assumeyes

```

- See Fedora's [Installation Documentation](https://docs.fedoraproject.org/en-US/quick-docs/getting-started-with-virtualization/#installing-virtualization-software).

---

## Installing Virtualization Packages (Ubuntu)

```shell
# apt-get install \
    bridge-utils \
    qemu-kvm \
    virt-manager
```

---

## Installing Virtualization Packages (CentOS)

```shell
# yum install \
    libvirt \
    qemu-kvm \
    virt-install \
    virt-install \
    virt-manager
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
usermod --append --groups=kvm,libvirt,qemu ${USER}

cat /etc/group | egrep "^(kvm|libvirt|qemu).*${USER}"
```

- Log out and log in again to apply this modification.

---

## Install Debian from Network

```shell
$ virt-install \
    --name Debian --os-variant debian11 --description 'Debian' \
    --vcpus 1 --ram 1024 \
    --location \
    https://ftp.debian.org/debian/dists/stable/main/installer-amd64 \
    --network bridge=virbr0 \
    --graphics vnc,listen=127.0.0.1,port=5901 \
    --noreboot --noautoconsole \
    --extra-args 'console=ttyS0,115200n8 serial'
$ virt-viewer --connect qemu:///session --wait Debian
```

<!--
Some speaker notes here that might be useful.

Opens an interactive console that you can use to manually install the guest virtual machine.

An example kernel command line option to specify the serial console (and it's settings) are:

    console=ttyS0,115200n8

This option tells the kernel to use ttyS0 (the first serial port), with settings of speed=115200 cps, no stop bits and 8 data bits. The serial ports of the host and the target should be configured to match in terms of the speed, stop bits and data bits. Minicom is a common serial communications program used on host machines for accessing the console from the host.
-->

---

## Install Ubuntu from ISO Image

```shell
$ virt-install \
    --name Ubuntu --os-variant ubuntu22.04 --description 'Ubuntu' \
    --vcpus 2 --ram 2048 \
    --network bridge=virbr0,model=virtio \
    --graphics vnc,listen=127.0.0.1,port=5902 \
    --cdrom ~/Downloads/ubuntu-22.04-desktop-amd64.iso \
    --noreboot --noautoconsole
$ virt-viewer --connect qemu:///session --wait Ubuntu
```

---

## Install Ubuntu from Network

```shell
$ virt-install \
    --name Ubuntu --os-variant ubuntu20.04 --description 'Ubuntu' \
    --vcpus 2 --ram 2048 \
    --location \
    http://archive.ubuntu.com/ubuntu/dists/focal/main/installer-amd64/ \
    --network bridge=virbr0,model=virtio \
    --graphics vnc,listen=127.0.0.1,port=5902 \
    --noreboot --noautoconsole \
    --extra-args='console=ttyS0,115200n8 serial edd=off'
$ virt-viewer --connect qemu:///session --wait Ubuntu
$ virsh console Ubuntu
```

<!--
Some speaker notes here that might be useful.

Opens an interactive console that you can use to manually install the guest virtual machine.
-->

---

## Install Fedora from ISO Image

```shell
$ virt-install \
    --name Fedora --os-variant fedora36 --description 'Fedora' \
    --vcpus 2 --ram 2048 \
    --network bridge=virbr0,model=virtio \
    --graphics vnc,listen=127.0.0.1,port=5904 \
    --cdrom ~/Downloads/Fedora-Workstation-Live-x86_64-36-1.5.iso \
    --noreboot --noautoconsole
$ virt-viewer --connect qemu:///session --wait Fedora
```

---

## Install Fedora from Network

```shell
$ virt-install \
    --name Fedora --os-variant fedora36 --description 'Fedora' \
    --vcpus 2 --ram 2048 \
    --location \
    https://download.fedoraproject.org/pub/fedora/linux/releases/36/Server/x86_64/os \
    --network bridge=virbr0,model=virtio \
    --graphics vnc,listen=127.0.0.1,port=5904 \
    --noreboot \
    --extra-args='console=ttyS0,115200n8 edd=off'
$ virsh console Fedora
```

<!--
Some speaker notes here that might be useful.

https://download.fedoraproject.org/pub/fedora/linux/releases/36/Server/x86_64/os

Opens an interactive console that you can use to manually install the guest virtual machine.
-->

---

## Install AlmaLinux from ISO Image

```shell
$ virt-install \
    --name AlmaLinux --os-variant almalinux9 --description 'AlmaLinux' \
    --vcpus 2 --ram 3072 \
    --network bridge=virbr0,model=virtio \
    --graphics vnc,listen=127.0.0.1,port=5903 \
    --cdrom ~/Downloads/AlmaLinux-9.0-x86_64-dvd.iso \
    --noreboot --noautoconsole
$ virt-viewer --connect qemu:///session --wait AlmaLinux
```

---

### Install AlmaLinux from Network

```shell
$ virt-install \
    --name AlmaLinux --os-variant almalinux9 --description 'AlmaLinux' \
    --vcpus 2 --ram 3072 \
    --location \
    https://almalinux.uib.no/9.0/BaseOS/x86_64/os/ \
    --network bridge=virbr0,model=virtio \
    --graphics vnc,listen=127.0.0.1,port=5905 \
    --noreboot \
    --extra-args='console=ttyS0,115200n8 edd=off'
$ virsh console AlmaLinux
```
<!--
Some speaker notes here that might be useful.

https://mirrors.almalinux.org/isos/x86_64/9.0.html

https://almalinux.uib.no/9.0/isos/x86_64/
https://almalinux.uib.no/9.0/BaseOS/x86_64/os/

Opens an interactive console that you can use to manually install the guest virtual machine.
-->

---

## Install CentOS from ISO Image

```shell
$ virt-install \
    --name CentOS --os-variant centos-stream9 --description 'CentOS' \
    --vcpus 2 --ram 3072 \
    --network bridge=virbr0,model=virtio \
    --graphics vnc,listen=127.0.0.1,port=5902 \
    --cdrom ~/Downloads/CentOS-Stream-9-latest-x86_64-dvd1.iso \
    --noreboot --noautoconsole
$ virt-viewer --connect qemu:///session --wait CentOS
```

---

## Install CentOS from Network

```shell
$ virt-install \
    --name CentOS --os-variant centos-stream9 --description 'CentOS' \
    --vcpus 2 --ram 3072 \
    --location \
    https://mirror.netsite.dk/centos-stream/9-stream/BaseOS/x86_64/os/ \
    --network bridge=virbr0,model=virtio \
    --graphics vnc,listen=127.0.0.1,port=5904 \
    --noreboot \
    --extra-args='console=ttyS0,115200n8 edd=off'
$ virt-viewer --connect qemu:///session --wait CentOS
$ virsh console CentOS
```

<!--
Some speaker notes here that might be useful.

https://www.centos.org/download/mirrors/
https://admin.fedoraproject.org/mirrormanager/mirrors/CentOS
https://mirror.netsite.dk/centos-stream/9-stream/BaseOS/x86_64/os/

Opens an interactive console that you can use to manually install the guest virtual machine.
-->

---

## Delete Virtual Machine

```console
$ virsh shutdown Debian # Graceful Shutown
Domain 'Debian' is being shutdown

$ virsh destroy Debian # Force Shutdown
Domain 'Debian' destroyed

$ virsh undefine Debian
Domain 'Debian' has been undefined
```

---

### Helpful Commands

```shell
$ virt-install --os-variant list

$ virsh nodeinfo

$ virsh edit

$ virt-df

$ virt-top

$ virt-viewer
```

---

### View Serial Console Message

```console
$ virsh console Fedora
Connected to domain 'Fedora'
Escape character is ^] (Ctrl + ])
```

---

### Error: Refusing to Undefine

```console
$ virsh undefine Ubuntu --remove-all-storage
error: Refusing to undefine while domain managed save image exists
```

```console
$ virsh managedsave-remove Ubuntu
Removed managedsave image for domain 'Ubuntu'
$ virsh undefine Ubuntu
Domain 'Ubuntu' has been undefined
```

---

## Error: Failed to Get MTU of Bridge

```text
stderr=failed to get mtu of bridge `virbr0': No such device
```

```console
# systemctl restart libvirtd
$ brctl show
bridge name	bridge id		STP enabled	interfaces
virbr0		8000.525400a87247	yes
```

<!--
Some speaker notes here that might be useful.

MTU means Maximum Transmission Unit.
-->

---

## Error: Hangs on Probing EDD

```text
Booting from Hard disk....
Probing EDD (edd=off to disable)... ok
```

```shell
$ virt-install \
    ...
    --extra-args='... edd=off'
```

<!--
Some speaker notes here that might be useful.

EDD means BIOS Enhanced Disk Device Services (EDD)
-->

---

## Error: Permission Denied Default Pool

- <https://serverfault.com/a/840520>

## Slide Title

Text Content

---

## Slide Title

Text Content

---

## Slide Title

Text Content

---

## Slide Title

Text Content

---

## Links (Part 1)

- <https://osseu2022.sched.com/event/15z24/getting-started-with-kernel-based-virtual-machine-kvm-leonard-sheng-sheng-lee-computas>
- <https://docs.fedoraproject.org/en-US/quick-docs/getting-started-with-virtualization/>
- <https://help.ubuntu.com/community/KVM/Installation>
- <https://ubuntu.com/blog/kvm-hyphervisor>
- <https://www.intel.com/content/www/us/en/virtualization/virtualization-technology/intel-virtualization-technology.html>

---

### Links (Part 2)

- <https://wiki.qemu.org/Features/TCG>
- <https://virt-manager.org/>
- <https://libvirt.org/docs.html>
- <https://wiki.debian.org/KVM>
- <https://wiki.debian.org/DebianInstaller/Preseed>
- <https://hands.com/d-i/>
- <https://www.debian.org/releases/stable/example-preseed.txt>

---

### <!--fit--> :pray:

---

### End
