---
layout:            post
title:             "Linux Boot process"
date:              2018-11-27 23:18:00 +0300
tags:              Load Average+ CPU
category:          Tutorial
author:            Poonam Agarwal
---
## The Boot Process
On the ground level boot process starts with loading the operating system (where machine doesn't know anything about operating system) from any of the storage device.
Not wasting time and getting into deeper level.

On this initial stage we donot have privilege even of the file system. What we have is the BIOS - a collection of software routines that are initially loaded from chip into the memory and initialised when we switch on the computer. BIOS makes some hardware checks to ensure all hardaware componenets are at stable switched on state.

So, now we know that we have something called BIOS that could be of some help.
BIOS cannot load the operating system as a 'file' as we mentioned above that it doesn't know about file system.

BIOS must read specific sectors of data from specific physical location of the disk devices.
BIOS load the OS from the first sector of the disk device. Here we can have multiple disk devices that may confuse BIOS to which device it should read from.

CPU cannot differentiate between code and data, for CPU both are instructions.
Here comes the magic number which is a unsophisticated way of BIOS to determine the boot sector.
BIOS loops through each disk device, read the boot sector into memory and search for the magic number which is 0xaa55 and instructs CPU to begin executing the first boot sector it finds that ends with the magic number.

