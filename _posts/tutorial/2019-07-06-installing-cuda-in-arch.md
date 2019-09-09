---
title: 'CUDA in Arch Linux'
excerpt: "Quick step by step on installing CUDA in Arch"
category: tutorial
layout: post
comments: true
tags: [tutorial, linux, cuda]
---

It is been a while since last time I used CUDA at work, and that was in 2012 when I developed a DFT (Discrete Fourier Transformation) in Tesla C2070. As a developer, development was fluid but scratching only a bit of surface due to fairly easy setup and most of works are done in my local PC with Windows on it.

However, in general setting up dedicated local GPGPU is rather costly thus smaller companies (even some big ones) are sharing resources. That means, the GPU is shared and all developers can use the resource when it is available. Most common environment is in general Unix-based and might be cumbersome to prepare for some. This is not just limited in CUDA specific development, but also in embedded platform, e.g., NVIDIA Drive PX, which is very expensive system. Automotive companies also set the system to be a shared resource for their developers.

This tutorial is direct answer on how I setup my day-to-day CUDA development in Arch Linux. Here is my desktop setup:

1. AMD Ryzen 1800X 8c/16t

   This was (totally) my best buy ever! Got the discount for this beast for just $325 and I pair it with 3333 Mhz CL14 G.Skill and this is nightmare for all multithreaded tasks! If you are in the market for CPU, get this baby or the 12nm version, Ryzen 2700X (or even wait for the 7nm 3x00X soon).

2. NVIDIA GTX 1080

   I used it mainly for gaming in Windows, but when I boot into Arch, then there is only one purpose for this old monster, crunching numbers.

3. ARCH Linux 5.1.9

   I do not know why I chose Arch but I guess I wanted to have simple, low resource and fully customized Linux distro and a lot of people are pointing to this gem. A bit tedious on installing the OS but if you want already nice Arch, then get Manjaro instead. It will save significant amount of your time.

Before I go, all informations here you can find the complete explanations of them in Arch's [GPGPU] section. 


[1]: https://wiki.archlinux.org/index.php/GPGPU#CUDA