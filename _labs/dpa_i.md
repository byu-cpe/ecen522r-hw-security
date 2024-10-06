---
layout: lab
toc: true
title: "Side Channel: DPA (Part 1)"
short_title: "Side Channel: DPA I"
number: 6
under_construction: false
---

## Instructions

You will need to merge in the latest changes from the starter code repository before getting started.  Refer back to the [Lab Instructions]({% link _pages/lab_instructions.md %}) for how to do this.

To complete this lab you will use a collection of power traces that were captured from a DES encryption.  Download [this zip file](https://drive.google.com/file/d/1_nER-j0IvPwxPzvr-h9KBcCyjKydJV5A/view?usp=sharing) and extract it.  Place the *traces.npy* file in the *lab6/* directory. You can ignore the *read_traces.py* file as it is already included in the *lab6/* directory with additional comments.

Follow the instructions in the [lab6/side_channel_2.pdf](https://github.com/byu-cpe/ecen522r_security_student/blob/main/lab6/side_channel_2.pdf) file.

## What to Submit

Make sure your submission tag on Github includes the following files:
1. Your lab report (*lab6/report.pdf*).
1. Your Python files for the DPA attack (place them in *lab6/*).

## Helpful Tips

### Python Packages
You will need several Python packages to complete the lab.  I recommend you create a Python virtual environment to install these packages.  A Makefile has been provided in the *lab6/* directory that will create a virtual environment and install the necessary packages.  Run 
    
    make env
    
to create the virtual environment, and 

    source .venv/bin/activate
    
to activate it in your terminal.

### DES Selection 

This lab requires you to determine R0 and R1 values internal to the DES algorithm. Rather than having you implement this portion of the DES algorithm, I suggest you use a pre-existing implementation, and then modify it as needed.  I have provided *des.py* in the *lab6/* directory that you can use.  This code is from <https://medium.com/@ziaullahrajpoot/data-encryption-standard-des-dc8610aafdb3>.

I suggest copying the *encrypt* function to a new function that simply returns the *R0* and *R1* values (keep the original *encrypt* function in case you need it later).  You can return immediately after the *R1* value is computed as you don't actually need the remainder of the DES algorithm.  Add an additional parameter to the function that allows you to override the round one key with a provided binary string.  With this in place, it will be simple to create a selection function that sorts the power traces based on whether your bit of choice changes from *R0* to *R1*.

### DES Implementation

You may want to look back to your *lab3/des.v* and *lab3/crp.v* files for reference on how the DES algorithm works.  
