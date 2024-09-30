---
layout: lab
toc: true
title: "Hardware Trojans I"
number: 3
under_construction: false
---

## Instructions

You will need to merge in the latest changes from the starter code repository before getting started.  Refer back to the [Lab Instructions]({% link _pages/lab_instructions.md %}) for how to do this.

Follow the instructions in the *lab3/trojan_1.pdf* file.

## What to Submit

Make sure your submission tag on Github includes the following files:
1. Your lab report (*lab3/report.pdf*).
1. The files for all three of your FPGA projects (*lab3/part1/*, *lab3/part2/*, and *lab3/part3/*). Make sure these contain your verilog source files (with changes to implement the Trojans), as well as your GAO files you used to test the Trojans.

## Helpful Tips

### Programming the FPGA

The setup in the lab has changed slightly since you completed Lab 0.  There is a conflict between the Linux driver that allows you to use the UART output of the microcontroller, and the software that allows you to program the FPGA.  Each time you plug in the board, the Linux driver for managing the UART will load.  This will prevent you from programming the FPGA.  If you want to program the FPGA, run this first:

    sudo remove_ftdi_serial.sh

While you don't have general sudo access on the lab computers, you have been given permissions to run this script as sudo.

This script will unload the Linux driver that is managing the UART, and allow you to program the FPGA.  If you ever need to use the UART again, you will need to unplug and replug the board.
