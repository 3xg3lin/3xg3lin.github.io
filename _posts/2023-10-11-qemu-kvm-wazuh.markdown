---
layout: post
title: "Wazuh installation on linux via QEMU/KVM"
date: 2023-10-11 16:45:59 +0300
---

Firstly, I installed QEMU/KVM on my machine. Installation process is going to be different from distro to distro. So, you should follow the instructions for your linux with quick search on the enthernet. After that I write the following code:
```bash
# virt-install \
    --name Ubuntu-unity \
    --memory 4096 \
    --vcpus 3 \
    --disk size=50 \
    --cdrom /home/ghost/Downloads/ubuntu-22.04.3-desktop-amd64.iso \
    --os-variant ubuntu22.04 \
    --network default
```
