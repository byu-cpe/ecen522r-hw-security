---
layout: lab
toc: true
title: "Get To Know Your HAHAv3 Board"
short_title: "HAHAv3 Board"
number: 0
under_construction: false
---

## Instructions

1. Complete your repository setup, following the instructions on [this page]({% link _pages/lab_instructions.md %}).

1. Install the GOWIN FPGA tools on your home drive.  To do this, you only need to follow step #2 and #3 of the "Install GOWIN FPGA Designer" section of *docs/HaHav3_Software_Installation_Tutorial_Linux.pdf*.

    To run the GOWIN tools on your system, simply run:

        ~/GOWIN/IDE/bin/gw_ide



1. Follow the instructions in the *lab0/Experiment_0_self_test_HaHav3.pdf* file.

    * For this lab, you will program the microcontroller using the avrdude command found on page 10 of *docs/HaHav3_Software_Installation_Tutorial_Linux.pdf*.

## What to Submit

Create a lab report document that contains the items detailed in the *lab0/Experiment_0_self_test_HaHav3.pdf* file.   Make sure to save this as *lab0/report.pdf*, and submit using the [submission instructions]({% link _pages/lab_instructions.md %}#submission).

## Board Issues
I will post any issues with boards here:
* Board 1, Board 5: Accelerometer is not working.  

## Helpful Tips

### Programming the Microcontroller
Each time you want to program the microcontroller, you need to move the switch to BOOT and press the reset button.  To run your program, move the switch to APP and press the reset button again.  

### Temperature Sensor
There is a can of compressed air in the lab that you can blow on the microcontroller to see the temperature change. 

### Detecting the Board
Sometimes the FPGA programmer has trouble detecting the board.   Try power cycling the board, unplugging and replugging the USB cable, or restarting the GOWIN software.  Sometimes it takes a few tries to get it to work.

### Flashing the FPGA
During the lab, you will program the FPGA with a provided bitstream.  The FPGA on your board, like most FPGAs, is stores its configuration (ie the design) in SRAM.  This means that when you power off the board, the design is lost.  Many FPGA boards, including ours, contains a separate Flash chip, that will store a default configuration that will be loaded into the FPGA when the board is powered on.  Using the programmer software, you have the choice of programming either the FPGA SRAM (which will be lost when the board is powered off) or the Flash chip (which will be loaded into the FPGA each time the board is powered on).  For this lab, you will be programming the Flash chip, but in future labs, you will be programming the SRAM.

## License Keys
You will need License keys to use the GOWIN software.  These are available in the [gowin_keys](https://github.com/byu-cpe/ecen522r_security_student/tree/main/gowin_keys) directory of your repository.  If you move to a new computer, you will need to change which key you are using.

### Using the Oscilloscope