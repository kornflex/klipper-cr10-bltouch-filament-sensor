# Klipper Configuration files for Creality CR10 v1 with Bltouch & Filament Sensor

**Creality CR10** is a great 3D printer. See my review on [Chinatech.fr](https://www.chinatech.fr/tests-produits-chinois/test-imprimante-3d-creality-cr10.html)

To improve your 3D printer, you can use [Klipper](https://github.com/KevinOConnor/klipper) instead of Marlin or manufacturer firmware.

First of all, if you are using the default firmware from the manufacturer, you need to unlock/flash the bootloader ([FR](http://www.cr10.fr/ameliorations/marlin/)/[EN](https://www.instructables.com/Flashing-a-Bootloader-to-the-CR-10/)) before installing Marlin/Klipper.

<br>
- - -

This article will describe shortly what to do to switch from Marlin to Klipper.
You will find a lot of official docs here : [https://github.com/KevinOConnor/klipper/tree/master/docs](https://github.com/KevinOConnor/klipper/tree/master/docs)

3 things you need to know :

* klipper : firmware / klippy host
* fluidd : web interface
* moonraker : API Web Server for Klipper

# Fluiddpi

## Installation

on SDCard, you need to download and install Fluiddpi : [https://docs.fluidd.xyz/installation/fluiddpi](https://docs.fluidd.xyz/installation/fluiddpi)
<br>
## Update OS

apt-get update && apt-get upgrade
<br>
## Update Klipper/Fluidd/Moonraker

Use this software : [kiauh](https://github.com/th33xitus/kiauh)

Or launch updates from the [webUI](https://docs.fluidd.xyz/features/updates)

# Configure your printer

## Get your printer.cfg

Find the [default config of your printer](https://github.com/KevinOConnor/klipper/tree/master/config) without improvements ( you can print something but you will need to modify some things to print quickly ).

For the Creality Cr10 v1 : [https://github.com/KevinOConnor/klipper/blob/master/config/printer-creality-cr10-2017.cfg](https://github.com/KevinOConnor/klipper/blob/master/config/printer-creality-cr10-2017.cfg)

Upload it in your Fluidd webUI and name it : printer.cfg
<br>
## Get the USB port

Find the USB port of your printer : `ls /dev/serial/by-id/*`

ls -l will redirect you to : `/dev/ttyUSB0`
<br>
## Flash printer

`make menuconfig`

choose
`AVR atmega1284p`

`make`

<br>
## BLTouch

I use these links to find how to configure it :
[https://www.lesimprimantes3d.fr/forum/topic/20330-tuto-installer-et-configurer-klipper/](https://www.lesimprimantes3d.fr/forum/topic/20330-tuto-installer-et-configurer-klipper/)
[https://www.klipper3d.org/Config\_Reference.html#bltouch](https://www.klipper3d.org/Config_Reference.html#bltouch)
[https://www.klipper3d.org/BLTouch.html](https://www.klipper3d.org/BLTouch.html)

<br>
Bed mesh :

[https://github.com/KevinOConnor/klipper/blob/master/docs/Bed\_Mesh.md](https://github.com/KevinOConnor/klipper/blob/master/docs/Bed_Mesh.md)
[https://docs.fluidd.xyz/features/bed\_mesh](https://docs.fluidd.xyz/features/bed_mesh)

<br>
## ZOFFSET

[https://www.klipper3d.org/Probe\_Calibrate.html#calibrating-probe-z-offset](https://www.klipper3d.org/Probe_Calibrate.html#calibrating-probe-z-offset)

You will need to do the "paper test"
<br>
## Filament Sensor

[https://www.reddit.com/r/klippers/comments/l2iacb/ender\_5\_plus\_klipper\_fluidd\_filament\_sensor/?utm\_source=amp&utm\_medium=&utm\_content=post\_body](https://www.reddit.com/r/klippers/comments/l2iacb/ender_5_plus_klipper_fluidd_filament_sensor/?utm_source=amp&utm_medium=&utm_content=post_body)
[https://github.com/KevinOConnor/klipper/blob/master/docs/RPi\_microcontroller.md](https://github.com/KevinOConnor/klipper/blob/master/docs/RPi_microcontroller.md)

You will need to use the raspberry pi GPIO.

You need to compile the MCU drivers.

# Camera

[https://docs.fluidd.xyz/features/cameras](https://docs.fluidd.xyz/features/cameras)
<br>
# Configure your slicer

## Macros

Klipper use macro. Some Gcodes are unknown from Klipper, like G29.
[https://mmone.github.io/klipper/G-Codes.html](https://mmone.github.io/klipper/G-Codes.html)

After changing macros, Prusaslicer just have few lines :

Start gcode :
<br>
```
START_PRINT BED_TEMP=[first_layer_bed_temperature] EXTRUDER_TEMP=[first_layer_temperature]
G29 ;Bed Level
PURGE
```
<br>
End gcode :
<br>
```
END_PRINT
```

## Thumbnails

[https://docs.fluidd.xyz/features/slicer-uploads](https://docs.fluidd.xyz/features/slicer-uploads)
[https://docs.fluidd.xyz/features/thumbnails](https://docs.fluidd.xyz/features/thumbnails)

# First print

Try to move all axis, heat bed and extruder, check if the filament sensor pause/resume the print.
Launch a cube test print for example.

## Calibration

After the first cube calibration print, you will see that size isn't good.
So, with the last klipper firmware, you don't need to change the steps/mm like in Marlin.
Klipper use rotation\_distance, based on the hardware of the printer. So, rotation\_distance is good. Nothing to change.

So, why the sizes are not good ? Just because you need to calibrate the extruder !
[https://github.com/KevinOConnor/klipper/blob/master/docs/Rotation\_Distance.md#calibrating-rotation\_distance-on-extruders](https://github.com/KevinOConnor/klipper/blob/master/docs/Rotation_Distance.md#calibrating-rotation_distance-on-extruders)

<br>
premiere impression pour tester
[https://www.lesimprimantes3d.fr/forum/topic/33061-changer-de-firmware-de-marlin-%C3%A0-klipper/](https://www.lesimprimantes3d.fr/forum/topic/33061-changer-de-firmware-de-marlin-%C3%A0-klipper/)
[https://www.lesimprimantes3d.fr/forum/topic/20330-tuto-installer-et-configurer-klipper/](https://www.lesimprimantes3d.fr/forum/topic/20330-tuto-installer-et-configurer-klipper/)

<br>
# Improve your prints

## Resonance Compensation

[https://github.com/KevinOConnor/klipper/blob/master/docs/Resonance\_Compensation.md](https://github.com/KevinOConnor/klipper/blob/master/docs/Resonance_Compensation.md)

My test is good. I have a small ringing frequency : About 23Hz. So I don't need to set "input shaper".
I have to invest into stiffening the printer.

## Measuring Resonances

[https://github.com/KevinOConnor/klipper/blob/master/docs/Measuring\_Resonances.md](https://github.com/KevinOConnor/klipper/blob/master/docs/Measuring_Resonances.md)

I don't do it for now.
<br>
## Pressure advance

[https://github.com/KevinOConnor/klipper/blob/master/docs/Pressure\_Advance.md#tuning-pressure-advance](https://github.com/KevinOConnor/klipper/blob/master/docs/Pressure_Advance.md#tuning-pressure-advance)

I have : 16,3mm
So  max\_accel : 3000 , like the default config.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<span class="colour" style="color: rgb(32, 33, 36);">klipper : firmware / klippy host</span>
<span class="colour" style="color: rgb(32, 33, 36);">fluidd : web interface</span>
<span class="colour" style="color: rgb(32, 33, 36);">moonraker : API Web Server for Klipper</span>
<span class="colour" style="color: rgb(32, 33, 36);">klipper : firmware / klippy host</span>
<span class="colour" style="color: rgb(32, 33, 36);">fluidd : web interface</span>
<span class="colour" style="color: rgb(32, 33, 36);">moonraker : API Web Server for Klipper</span>