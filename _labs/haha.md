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

1. Follow the instructions in the *lab0/Experiment_0_self_test_HaHav3.pdf* file.

## What to Submit

Create a lab report document that contains the items detailed in the *lab0/Experiment_0_self_test_HaHav3.pdf* file.   Make sure to save this as *lab0/report.pdf*, and submit using the [submission instructions]({% link _pages/lab_instructions.md %}).

## Helpful Tips

### Flashing the FPGA
During the lab, you will program the FPGA with a provided bitstream.  The FPGA on your board, like most FPGAs, is stores its configuration (ie the design) in SRAM.  This means that when you power off the board, the design is lost.  Many FPGA boards, including ours, contains a separate Flash chip, that will store a default configuration that will be loaded into the FPGA when the board is powered on.  Using the programmer software, you have the choice of programming either the FPGA SRAM (which will be lost when the board is powered off) or the Flash chip (which will be loaded into the FPGA each time the board is powered on).  For this lab, you will be programming the Flash chip, but in future labs, you will be programming the SRAM.

### Using the Oscilloscope