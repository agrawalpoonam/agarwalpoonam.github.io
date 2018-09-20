---
layout:            post
title:             "Docker Vs VirtualMachine"
date:              2016-11-01 15:28:00 +0300
tags:              DOcker +VirtualMachine
category:          Tutorial
author:            Poonam Agrawal
---

|Comparision Basis | Docker | Virtual machine |
| :---         |     :---     |          :--- |

||<div>
<figure>
<img src="{{ site.github.url }}/media/img/Architecture_Docker.png" />
<figcaption>Architecture Docker</figcaption>
</figure>
<br>
</div>|
<div>
<br>
<figure>
<img src="{{ site.github.url }}/media/img/Architecture_VM.png" />
<figcaption>Architecture VM</figcaption>
</figure>
</div>|

|Architechture | Architecture Docker | Architecture Virtual machine |
| Build   | Only Binaries and libraries of OS over which services run | Iamge of the entire OS    |
|  Virtualization  |  OS level virtualization by abstracting user space       | Hardware Virtualization    |
| Processing boundaries   | Container have private space for processing, can execute commands as root   |  Virtual Box have confined space for processing and can execue command as user   |
| kernel boundaries   | Container share host sytstem kernel   |  Virtual Box share host system kernel with Hypervisor   |


## Explaination Of the Architecture






#### Client:

- Uses Flask Apis to perform the requests.
- Send email to the server.



You can find the code in the github location: <a href="https://github.com/agrawalpoonam/smtpapp">
https://github.com/agrawalpoonam/smtpapp
</a>

