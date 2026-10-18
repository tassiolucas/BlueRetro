**This repository has been archived and is no longer under active development or maintenance.**

```
After 6 years working on BlueRetro, the time has come for me to move on
and focus on my family and my real job.

The code remains available as-is for reference, learning, and community use,
but no new features, bug fixes, or pull requests will be accepted.

Thank you to everyone who contributed, tested, reported issues,
or supported the project over the years.
Your involvement made BlueRetro what it is today.

If you are looking to build upon this work, please feel free to
fork the repository in accordance with the license.

Thank you,
Jacques Gagnon
```

## About this fork: QRD Spark N5 pairing fix

This fork of the archived [darthcloud/BlueRetro](https://github.com/darthcloud/BlueRetro) adds a fix that lets the **QRD Spark N5** wireless controller (PS4/PS3/PC/iOS/Android/TV/Steam) pair with BlueRetro. The same fix should also apply to other QRD pads with the same behaviour, such as the QRD Maestro S3 ([darthcloud/BlueRetro#1287](https://github.com/darthcloud/BlueRetro/issues/1287)).

### The problem

In PS4 / DInput mode the controller fully impersonates a DualShock 4: it uses the name `Wireless Controller`, VID/PID `054C:09CC` and the stock DS4 HID descriptor. Pairing appears to succeed, because the BlueRetro LED stops blinking, but the controller LED keeps blinking and it never connects.

BlueRetro treats the pad as a PS4 controller and never encrypts the Bluetooth link for that controller type. A genuine DS4 accepts that. The QRD pad does not: it refuses the HID control channel with L2CAP result `0x0003` ("security block"). BlueRetro ignored that refusal, so the link sat idle until it timed out.

### The fix

When the HID control channel is refused with a security block on an unencrypted link, BlueRetro now enables link encryption and retries the HID connection once encryption is active. Controllers that already worked never hit this path, so their behaviour is unchanged.

Changed files: `main/bluetooth/l2cap.c`, `main/bluetooth/hci.c`, `main/bluetooth/hci.h` and `main/bluetooth/host.h`.

### How to use it with the QRD Spark N5

1. Build the firmware for your system (see [BlueRetroRoot](https://github.com/darthcloud/BlueRetroRoot)), for example `cp configs/hw1/playstation sdkconfig` followed by `idf.py build`, and flash it over USB or via [OTA](https://blueretro.io/ota.html).
2. Make sure BlueRetro is in pairing mode (LED pulsing).
3. Hold **Home + Share** on the Spark N5 to pair it in PS4 / DInput mode.

Tested on a DIY HW1 BlueRetro (`hw1/playstation`) with a PlayStation 2: pairing, all buttons and both analog sticks work.

Known limitation: automatic reconnection by pressing only the Home button does not work yet. Hold Home + Share to pair again each session.

# BlueRetro

<p align="center"><img src="/static/PNGs/BRE_Logo_Color_Outline.png" width="600"/></p>
<br>
<p align="justify">BlueRetro is a multiplayer Bluetooth controllers adapter for various retro game consoles & computers. Lost or broken controllers? Reproduction too expensive? Need those rare and obscure accessories? Just use the Bluetooth devices you already got! The project is open source hardware & software under the CERN-OHL-P-2.0 & Apache-2.0 licenses respectively. It's built for the popular ESP32 chip. Wii, Switch, PS3, PS4, PS5, Xbox One, Xbox Series X|S & generic HID Bluetooth (BR/EDR & LE) devices are supported. Parallel 1P (Computers, NeoGeo, Supergun, JAMMA, Handheld, etc), Parallel 2P (Atari 2600/7800, Master System, Computers, etc), NES, PCE / TG16, Mega Drive / Genesis, SNES, CD-i, 3DO, Jaguar, Saturn, PSX, PC-FX, JVS (Arcade), Virtual Boy, N64, Dreamcast, PS2, GameCube & Wii extension are supported with simultaneous 4+ players using a single adapter.</p>

## READ THIS FIRST
* [Project documentation](https://github.com/darthcloud/BlueRetro/wiki)

## Need help?
* [Open a GitHub discussion](https://github.com/darthcloud/BlueRetro/discussions)

## Makers sponsoring BlueRetro
Buying BlueRetro adapters from these makers helps support the continued development of the BlueRetro firmware!\
Thanks to all sponsors!

* [Laser Bear Industries](https://www.laserbear.net)
* [Humble Bazooka](https://www.humblebazooka.com)
* [RetroOnyx](https://www.retroonyx.com/)
* [RetroTime](https://8bitmods.com/retrotime)

## Community Contribution
* BlueRetro PS1/2 Receiver by [mi213](https://twitter.com/mi213ger): 3D printed case & PCB for building DIY PS1/2 dongle.\
  https://github.com/Micha213/BlueRetro-PS1-2-Receiver
* N64 BlueRetro Mount by [reventlow64](https://twitter.com/reventlow): 3d printed mount for ESP32-DevkitC for N64.\
  https://www.prusaprinters.org/prints/90275-nintendo-64-blueretro-bluetooth-receiver-mount
* BlueRetro Adapter Case by [Sigismond0](https://twitter.com/Sigismond0): 3d printed case for ESP32-DevkitC.\
  https://www.prusaprinters.org/prints/116729-blueretro-bluetooth-controller-adapter-case
* BlueRetro AIO by [pmgducati](https://github.com/pmgducati): BlueRetro Through-hole base and cable PCBs.\
  https://github.com/pmgducati/Blue-Retro-AIO-Units
* BlueRetro HW2 internal guides by [Nostalgic Indulgences](https://twitter.com/nosIndulgences): Internal install guides\
  https://github.com/nostalgic-indulgences/BlueRetro_Internal_Installation
* BlueRetro latency test by [GamingNJncos](https://twitter.com/GamingNJncos): Documentation on how to run BlueRetro latency test\
  https://github.com/GamingNJncos/BLE-3D-Saturn-Public/tree/main/BlueRetro_Latency_Testing
* BR4N64 by [TharathielCB](https://github.com/TharathielCB): Internal BlueRetro Flex-PCB for Nintendo 64\
  https://github.com/TharathielCB/BR4N64
* BlueMemCard by [ChrispyNugget](https://github.com/ChrispyNugget): Replacement PCB for PSX memory card that incorporates PicoMemcard and BlueRetro support\
  https://github.com/ChrispyNugget/BlueMemCard
* BlueRetro HW2 QSB for GameCube by [Arthrimus](https://github.com/Arthrimus): Internal Blueretro PCB with CurrentTrigger for GameCube\
  https://github.com/Arthrimus/BlueRetro-HW2-GameCube

<br><p align="center"><img src="https://cdn.hackaday.io/images/4560691598833898038.png" height="200"/></p>
