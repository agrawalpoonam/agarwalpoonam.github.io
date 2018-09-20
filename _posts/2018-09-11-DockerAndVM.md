---
layout:            post
title:             "Docker Vs VirtualMachine"
date:              2016-11-01 15:28:00 +0300
tags:              DOcker +VirtualMachine
category:          Tutorial
author:            Poonam Agrawal
---


|Comparision Basis | Docker | Virtual machine |
|Architecture|![Docker]({{ site.github.url }}/media/img/Architecture_VM.png)|![Virtual Machine]({{ site.github.url }}/media/img/Architecture_VM.png)|
| Build   | Only Binaries and libraries of OS over which services run | Iamge of the entire OS    |
|  Virtualization  |  OS level virtualization by abstracting user space       | Hardware Virtualization    |
| Processing boundaries   | Container have private space for processing, can execute commands as root   |  Virtual Box have confined space for processing and can execue command as user   |
| kernel boundaries   | Container share host sytstem kernel   |  Virtual Box share host system kernel with Guest Operating System with Hypervisor   |


## Explaination Of the Architecture

#### Docker
- Image is a box with set of all containers with small layered images. An image is an executable package that includes everything needed to run an application--the code, a runtime, libraries, environment variables, and configuration files. 

- A container is launched by running an image. 

- A container is a runtime instance of an image--what the image becomes in memory when executed (that is, an image with state, or a user process).

#### Virtual Machine
- A virtual Machine is an image of operating system running on Hypervoisor over host operating system.


You can find the code in the github location.

