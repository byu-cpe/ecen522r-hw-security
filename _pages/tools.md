---
layout: lab
toc: true
title: "Installing and Running Tools"
short_title: "Tools"
order: 6
sidebar: true
under_construction: false
icon: fas fa-screwdriver-wrench
---

## GOWIN FPGA Tools

### Installing
1. Go to <https://www.gowinsemi.com/en/support/download_eda/>.  You will need to create a free account and login. 
1. Click the "Software for Linux" tab, and download the *Gowin V1.9.11.03 Education (Linux x64)* software.
1. Unpack the downloaded archive to your home directory:

       mkdir -p ~/GOWIN
       tar -xvf Gowin_V1.9.11.03_Education_Linux.tar.gz

1. Disable the built-in libraries for the IDE:

       cd ~/GOWIN/IDE
       mkdir -p ./lib/disabled
       [ -f ./lib/libfreetype.so.6 ]   && mv ./lib/libfreetype.so.6   ./lib/disabled/
       [ -f ./lib/libfontconfig.so.1 ] && mv ./lib/libfontconfig.so.1 ./lib/disabled/
       [ -f ./lib/libstdc++.so.6 ] && mv ./lib/libstdc++.so.6 ./lib/disabled/
       [ -f ./lib/libgcc_s.so.1 ]  && mv ./lib/libgcc_s.so.1  ./lib/disabled/

1. Disable the built-in libraries for the programmer:

       cd ~/GOWIN/Programmer/bin
       mkdir -p disabled
       [ -f ./libz.so.1 ] && mv ./libz.so.1 disabled/


### Running
1. Before you can program the FPGA on the board, you must

       sudo remove_ftdi_serial.sh

    *Explanation:* There is a conflict between the Linux driver that allows you to use the UART output of the microcontroller, and the software that allows you to program the FPGA. Each time you plug in the board, the Linux driver for managing the UART will load. This will prevent you from programming the FPGA. If you want to program the FPGA, run this first:

    While you don’t have general sudo access on the lab computers, you have been given permissions to run this script as sudo.

    This script will unload the Linux driver that is managing the UART, and allow you to program the FPGA. If you ever need to use the UART again, you will need to unplug and replug the board.

1. Run this in your terminal to update the library search path:
        
       export LD_LIBRARY_PATH=$HOME/GOWIN/IDE/lib

1. Run the software:

       ~/GOWIN/IDE/bin/gw_ide

## Microcontroller 

### Installing
These packages are already installed on the lab computers, so you can skip this step:

    sudo apt-get install -y gcc make gcc-avr avr-libc avrdude


### Programming
1. Make sure the microcontroller is powered by connect the board to the USB Power supply (J4) with a USB A-to-B cable. 
1. Place the switch to BOOT. 
1. Double tap MCU RST button.
1. Run the command to program the microcontroller:

       avrdude -v -c flip2 -p x16a4u -U application:w:<path/to/hex/file>:i

1. To run the programmed executable, move the switch from BOOT to APP and press the MCU RST button a couple times.