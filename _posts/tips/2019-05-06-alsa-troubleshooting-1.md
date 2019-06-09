---
title: 'Checking Which Device Producing Sound with ALSA'
excerpt: 'Journey on troubleshooting Arch Linux no headphone sound'
category: tips
layout: post
share: false
comments: false
tags: [tips, linux]
---

After I installed Arch Linux in my desktop (powered by AMD Ryzen with X370 chipset), I could not properly hear the sound from all analog connections. Which means, my headphone and speaker do not work. For now I am using HDMI out from my GTX1080 through HDMI to my crappy monitor speaker. Therefore, no chat session in all my Dota 2 games and that affected me somehow.

So this tips post and few oncoming are dedicated on my journey on troubleshooting this issue. 

Let's start with the reference guide first! One good thing about Arch Linux is, their wiki is literally the most complete Linux guide ever. All Linux should have pre-built low-level audio driver bundled as ALSA or [Advanced Linux Sound Architecture](https://wiki.archlinux.org/index.php/Advanced_Linux_Sound_Architecture#Unmuting_the_channels). It replaces older original Open Sound System (OSS). You can check the link for complete guide.

For my case, I just want to start with basic listing all audio device which my desktop has. The command is `aplay -l` it will list all audio playback hardware devices in your pc/laptop. 

```
card 0: NVidia [HDA NVidia], device 3: HDMI 0 [HDMI 0]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: NVidia [HDA NVidia], device 7: HDMI 1 [HDMI 1]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: NVidia [HDA NVidia], device 8: HDMI 2 [HDMI 2]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: NVidia [HDA NVidia], device 9: HDMI 3 [HDMI 3]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 1: Generic [HD-Audio Generic], device 0: ALC1220 Analog [ALC1220 Analog]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 1: Generic [HD-Audio Generic], device 1: ALC1220 Digital [ALC1220 Digital]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
```

Above snippet shows all the devices and the analog output shall be produced by the Realtek ALC1220 Analog, which in this case has `card-id 1` and `device-id 0`. Now we just need to see if these devices can play some sample audios. Here is a sample command to play a white noise audio through ALC1220 Analog device.


```
    aplay /dev/urandom -f dat -D plughw:1,0
```

`plughw` has value 1,0 which you provide card-id and device-id. Same for your other device, these values you must set accordingly. If you hear the white noise, then at least the problem is not the hardware.

Continue to the next journey!