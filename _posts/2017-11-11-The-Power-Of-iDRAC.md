---
title: The Power of iDRAC
date: 2017-11-11 00:00:00 Z
published: false
layout: post
---

A few weeks I decided to move back to ESXi as the host operating system for my virtualization server. RIT students get ESXi Enterprise editions for free, it's used to host security competition infrastructure, and it has interesting features like the PowerCLI. I didn't really feel like sitting in the basement for hours on end, so I decided to finally try out the remote console capabilities of my iDRAC.

The firmware was too outdated to run on a current version of Java, so I set up a Windows 7 VM with an earlier version of Java which did the trick. I had to rename the downloaded console file which is a known issue, but then I was able to load into the console.

It was a great feeling to watch the BIOS boot as soon as my server was powered on.
