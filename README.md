# dingo-raspberry-pi
Installing a Raspberry Pi 4 on Clearpath Robotics Dingo

<img src="/images/dingo-raspberry-pi-1.png" alt="Image of dingo and pi" width="600" >

Dingo is an indoor mobile robot that can be configured for differential-drive, or omnidrirectional-drive.
You can find more information about Dingo at the [Clearpath Robotics](https://clearpathrobotics.com/dingo-indoor-mobile-robot/) website.


## Hardware Overview

We mounted the Raspberry Pi 4 on the outside of the Dingo.
We conisdered this an advantage for the applications that want a low cost computer for Dingo.
The external mounting improves the Pi's WiFi signal, and gives quick access to the Pi's microSD card.


### Tools

You will need these tools:

* Wire stripper
* Wire cutter
* Crimper for Molex mini-fit-jr, 18 AWG crimps
* Hex key, 3 mm across flats

### Parts
| Quantity | CPR item number | Description | Manufacturer | Manufacturer # | Supplier | Supplier # |
| :---: | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | 021604 | microSD, 64 GB | Samsung | MB-ME64HA/AM | Amazon | B08879MG33 |
| 1 | 021758 | Cable—Ethernet, 2X RJ45, 300 | Monoprice | 13518 | — | — |
| 1 | 024011 | Bracket— Raspberry Pi | — | — | — | — |
| 4 | 024049 | Pin, Push | Westcott | 165841638 | Amazon | B003M5MTWC |
| 1 | 024051 | Computer, Single Board—Raspberry Pi 4, 4 GB | Raspberry Pi Foundation | Raspberry Pi 4 Model B, 4 GB | — | — |
| 1 | 024056 | Cable Assembly—USB-C X Molex | — | — | — | — |



#### 024011 Bracket— Raspberry Pi

<img src="/images/024011-ART_2.png" alt="Image of pi mounting bracket" width="600" >

This is a 3D printed bracket to hold the Pi on top of the Dingo. 
We printed our bracket in-house using PLA filament. ( fused deposition modeling, using polylactic acid filament )
A STEP214 model of this bracket is included in this repository as 024011-GEO_1.STL



#### 024056 Cable Assembly—USB-C X Molex 
(Raspberry Pi 4 Power Cable )

| Quantity | CPR item number | Description | Manufacturer | Manufacturer # | Supplier | Supplier # |
| :---: | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | 022206 | Connector, Housing | Molex | 39-01-2045 | Digi-Key | WM23701-ND |
| 2 | 022252 | Terminal, Crimp | Molex | 40-13-0852 | Digi-Key | WM24935-ND |
| 1 | 024192 | Cable—Barrel | Tensility | 10-02317 | Digi-Key | 839-1332-ND |
| 1 | 024193 | Connector, Adapter—USB-C to Barrel Female Ø2.1 X 5.5 | Adafruit | 4536 | Digi-Key | 1528-4536-ND |
| 1 | 024194 | Connector, Adapter—90° USB-C C USB-C | Adafruit | 4432 | Digi-Key | 1528-4432-ND |



### Hardware Installation Guide

These steps assume you have already printed the Pi's mounting bracket ( 024011 ), made the Pi's power cable ( 024056 ), and collected the parts and tools mentioned earlier.

0. Make sure your Dingo is turned off.
1. Remove the four round-head screws at the rear of the Dingo's trough cover.
2. Place the Pi mounting bracket ( 024011 ) over the four mounting holes. The bracket's flat section faces the rear of the Dingo.
3. Install the four round-head screws ( from step 1 ) into the Dingo's trough cover.
4. Place the Pi 4b onto the mounting bracket.
5. Install four push-pins into the mounting bracket.
6. Remove the Dingo rear trough cover's four flat-head screws.
7. Remove the Dingo's rear trough cover.
8. Connect the ethernet cable ( 021758 ) to the Dingo's MCU circuit board. 
<img src="/images/dingo-mcu.png" alt="Image of dingo MCU" width="600" >
9. Connect the power cable ( 024056 ) to the *PWR1* jack of the Dingo's MCU circuit board. This is a white jack in the centre of the circuit board.
10. Place the trough cover on the Dingo. The ethernet and power cable should pass through the trough cover's semi-circle cutout, so you can access the cables after fastening the cover.
11. Install four flat-head screws ( from step 6 ) into the trough cover.
12. Connect the power cable to the Raspberry Pi's USB type-C jack.
13. Connect the ethernet cable to the Raspberry Pi's RJ45 jack.



## Software Overview

We tested our Raspeberry Pi Dingo with Ubuntu server Bionic arm64.
Canonical will support this Raspberry Pi operating system until 2023.



### OS installation onto Pi
1. From your development computer, download [Ubuntu Bionic Raspberry Pi 3 (64-bit ARM)](http://cdimage.ubuntu.com/ubuntu/releases/18.04/release/ubuntu-18.04.5-preinstalled-server-arm64+raspi3.img.xz)
   You can see the similar images from Canonical [here](http://cdimage.ubuntu.com/ubuntu/releases/18.04/release/)
2. From your development computer, download and install [Raspberry Pi Imager](https://www.raspberrypi.org/software/)
3. Insert a 16 GB or larger microSD card into your development computer. We suggest the 64 GB card included in the parts list since this has sufficient speed, and additional storage for rosbags.
4. From your development computer, launch the Raspberry Pi Imager application.
5. In the Raspberry Pi Imager application:
   * Click *Operating System*
   * Select *Use custom* at the bottom of the OS list.
   * Select the OS downloaded in step 1. 
   * Clcik *Storage*
   * Select your micro SD in the storage options. 
   * Click 'Write'
<img src="/images/pi-imager-1.png" alt="Image of Pi imager 1" width="600" >

<img src="/images/pi-imager-2.png" alt="Image of Pi imager 2" width="600" >

<img src="/images/pi-imager-3.png" alt="Image of Pi imager 3" width="600" >

6. After the Raspberry Pi Imager finishes writing; remove the microSD card from your development computer.
7. Insert the microSD card into your Raspberry Pi 4.
8. Connect the following cables and then connect power to the Raspberry Pi:
   * RJ45 ethernet to an internet connection
   * HDMI to a monitor
   * USB keyboard
   * USB type-C power ( 3 A at 5 V )
9. Once the Pi has booted, enter the username and password:
   *username: *ubuntu*
   *password: *ubuntu*
   *new password *choose a password*
10. Find the IP address of your Raspberry Pi:
   *ifconfig *
11. From a terminal on your developement computer, SSH into the Pi:
   *ssh ubuntu@xxx.xxx.xxx.xxx*
   note: the Pi and your development computer will need to be on the same local-area-network.


### ROS installation
12. On your development computer's terminal, through SSH:
   *wget -c https://raw.githubusercontent.com/clearpathrobotics/ros_computer_setup/main/install.sh && bash install.sh*
   note: this script is desribed [Clearpath ros_computer_setup](https://github.com/clearpathrobotics/ros_computer_setup)

