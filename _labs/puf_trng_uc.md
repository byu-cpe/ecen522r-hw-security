---
layout: lab
toc: true
title: "PUFs and TRNGs"
number: 2
under_construction: false
---

## Instructions

You will need to merge in the latest changes from the starter code repository before getting started.  Refer back to the [Lab Instructions]({% link _pages/lab_instructions.md %}) for how to do this.

Follow the instructions in the *lab2/hw_based_security_primitives_I.pdf* file.

## What to Submit

Make sure your submission tag on Github includes the following files:
1. Your lab report (*lab2/report.pdf*).
1. Your code to print your SRAM PUF (*lab2/part1/main.c*).
1. Your analysis of the SRAM PUF (some Python or other code in *lab2/part1/*)
1. Your code to print your TRNG output (*lab2/part2/main.c*).

## Helpful Tips

### Compiling Code for the Microcontroller

A [Makefile](https://github.com/byu-cpe/ecen522r_security_student/blob/main/sw_resources/Makefile) is provided in your repository that you can use for any of the labs.  Copy this Makefile to the directory where you want to develop your code (eg. *lab2/part1/*), and update the first line to correctly point to the location of the common software files in the *sw_resources* directory.  

For example, if you copy the Makefile to *lab2/part1/*, the line

        SW_RESOURCES=../sw_resources

should be changed to

        SW_RESOURCES=../../sw_resources

You can then compile your code by running `make` in the directory where your code is located.  This will compile your code and generate a hex file that you can load onto the microcontroller.  Use `make program` to load the hex file onto the microcontroller.  You no longer need to directly invoke the `avrdude` command.

### Software Library Code for the Microcontroller

The [sw_resources](https://github.com/byu-cpe/ecen522r_security_student/tree/main/sw_resources) directory contains common code for the HaHa board that you can use across multiple labs.  The [haha.h](https://github.com/byu-cpe/ecen522r_security_student/blob/main/sw_resources/haha.h) file is probably the most useful file to look over.  It contains several `haha_uart_*` functions that you can use to print to the UART.

You will notice that *haha.h* has this line:

        #include <avr/io.h>

which imports the various AVR definitions for the microcontroller.  You should be able to right-click on `avr/io.h` in VS Code and select "Go to Definition" (or press F12) to open this file.  VS Code knows where to look for this file because I have configured it [here](https://github.com/byu-cpe/ecen522r_security_student/blob/main/.vscode/c_cpp_properties.json#L7).  If "Go to Definition" doesn't work, make sure you have opened your lab directory correctly in VS Code.

If you look through the *<avr/io.h>* file, you will see that it has support for hundreds of different microcontrollers, and includes the appropriate header file for the microcontroller you are using.  You should see all of the #include statements greyed out, except for the `#  include <avr/iox16a4.h>` statement.  Again, VS Code can show you this information because I have configured the defines [here](https://github.com/byu-cpe/ecen522r_security_student/blob/main/.vscode/c_cpp_properties.json#L9).  When you actually run `make`, this definition will be automatically set during compilation because of the `-mmcu=atxmega16a4u` flag we provide to the compiler [here](https://github.com/byu-cpe/ecen522r_security_student/blob/main/sw_resources/Makefile#L4-L5).

You should look through the *<avr/iox16a4.h>* file (press F12 on the include statement), as it has helpful definitions you can use in the lab, such as the location of the SRAM memory.

### UART
When the boards are plugged into the lab computers, two device files, `/dev/ttyUSB0` and `/dev/ttyUSB1`, are created.  In my testing the `/dev/ttyUSB1` file was the one that worked for the UART, although it's possible that could change for you based on what other boards are plugged into the computer.  If these files are not showing up, try unplugging and replugging the board, or restarting the computer.

You can use the `screen` command to connect to the UART.  For example, to connect to the UART on the HaHa board, you can run:

        screen /dev/ttyUSB1 9600

When you want to exit the screen session, you can press `Ctrl-a` followed by `k`, and then `y` to confirm.