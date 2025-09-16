---
layout: lab
toc: true
title: "Hardware Trojans I"
number: 3
under_construction: true
---

In this lab you will modify an encryption circuit to insert a hardware Trojan.  


## Preliminary

### Background
Recently Intel announced a flaw in the implementation of the "TSX" instruction for its Haswell series of Central Processing Unit (CPU). This announcement came almost a year into the product’s lifecycle and almost three years since the beginnings of Haswell’s architecture was laid out. This is a legitimate mistake on Intel’s part – there is no foul play or trickery here, although it shows how difficult it is to fully test a complex design. 

Researchers at the University of Massachusetts were able to modify an Intel Ivy Bridge processor – the series that Haswell replaced – and significantly impair the Random Number Generator (RNG) of the processor. They did this by modifying the silicon that made up actual transistor. Their modification is completely undetectable without a Scanning Electron Microscope (SEM) and a known good chip to authenticate against. If the security of the RNG is compromised then everything generated from it is also compromised, for example, private encryption keys.

An intelligent adversary is expected to hide such tampering with an IC’s behavior in a way that makes it extremely difficult to detect with conventional post-manufacturing testing. Intuitively, it means that the adversary would ensure that such tampering is manifested or triggered under very rare conditions at the internal nodes, which are unlikely to arise during testing but can occur during long hours of field operation.

The figures below show general models of combinational and sequential Trojans. These abstract models of Trojans are useful for studying the space of possible Trojans, and, similar to fault models, help in test vector generation for Trojan detection.

<p align="center">
  <img src="{% link media/labs/trojan_combinational.png %}" alt="Combinational Trojan" width="350"/>

  <img src="{% link media/labs/trojan_sequential.png %}" alt="Sequential Trojan" width="350"/>
</p>

### DES

DES is a symmetric-key algorithm for encrypting electronic data. Although it is now considered insecure, it was highly influential in the advancement of modern cryptography. It uses 16 round Feistel structure. The block size is 64-bit, of which DES has an effective key length of 56 bits, since 8 of the 64 bits of the key are parity bits used for error correction and are not used by the encryption algorithm. The encryption steps are illustrated in the figure below. 

<p align="center">
  <img src="{% link media/labs/DES.png %}" alt="DES Overview" width="500"/>
</p>

The left image below shows how data is processed in each round, and the right image shows the Feistel function (F-function) used in each round.

<p align="center">
    <img src="{% link media/labs/DES_rounds.png %}" alt="DES Rounds" width="250"/>
    <img src="{% link media/labs/DES-f-function.png %}" alt="DES Feistel Function" width="500"/>
</p>

More details about DES can be found [here](https://en.wikipedia.org/wiki/Data_Encryption_Standard).



### Lab Directory

You will be using the `lab_trojan_i` directory for this lab. 
You have been provided several 12 Verilog files related to the DES implementation on the FPGA. 


## Instructions

### Part I: Implement DES 
1. Create a subdirectory in your folder, called `part1`. 
1. Within this, create a new GOWIN project called `part1` with top module called `top`, that instantiates the `des` module from `des.v` file.  
    * There are two modules inside this file: `des` and `des_o`. 
    * `des_o` contains the DES implementation, and `des` is the wrapper top module that instantiates `des_o`. 
    * Plaintext and key are given in `des`. 
    * There is a RAM module called `ram1` instantiated, which is a 64-bit wide, 32-word deep RAM. It will store all the encryption results of the 16 rounds. 
    * Refer to the appendix for starting this project. 
1. Compile and program the FPGA.
1. Use the Gowin Analyzer Oscilloscope (GAO) to view the DES encryption. 
    * The encryption process happens repeatedly, so you can use GAO to view the signals repeatedly performing the encryption; however, after the first encryption cycle (which happens very fast, before you can start capture with GAO), the result in the RAM should not change.
    * If you look at `des.v`, you will see the clock is being slowed down by 50x, resulting in a 1MHz clock. You may want to clock the logic analyzer using this slower clock signal so that you are not viewing the same data repeatedly. You can shorten the capture window down to 32, which will be large enough to capture all rounds of encryption at least once. 
1. **REPORT:** Answer the following questions in your report:
    1. Use the plaintext and key provided and add them in the `des.v Verilog file:
             
           desIn = 64'hA42F891BD376CE05
           key64 = 64'h0123456789ABCDEF
           
        Store all the encryption results for the 16 rounds in an implemented RAM and show the result of the final round (upper 64 bits of the 1024-bit RAM). You can verify the correct encryption using an online tool, such as <http://des.online-domain-tools.com/>

    1. Which module is creating the key for each round? How?
    1. Which module is doing the Feistel function?
    1. How many Logic Elements are used?

### Part II: Insert a combinational Trojan 
1. Create a new folder, `part2`, that contains a new Gowin project. Start with a working DES circuit, identical to what you did in Part 1 (You can copy all of the files into the `src` directory for this new project).
1. Insert a combinational Trojan into the DES circuit. 
    * The trigger condition of the Trojan is when the output of the F function (Figure 6) satisfies for some value of the least significant 4 bits (see next section). 
    * When the Trojan is triggered, the LSB of the input key (NOT round keys) for the DES is inverted (ie. invert key56[0]). 
    * When the trigger condition is not true, the key becomes the original input key. 
    * You will want to register your trigger signal to prevent a combinational loop from occurring. That means that in any cycle where the trigger is true, the following cycle will have key56[0] inverted.
1. **REPORT:** Answer the following questions in your report:
    1. When the trigger condition is 4’b0110, will the Trojan be triggered? How many times is it triggered in the 16 rounds? Turn in a screenshot of the GAO window.
    1. When the condition is 4’b1001, repeat answering question 1. again. Take another screenshot of the GAO window.
    1. When the condition is 4’b1010, repeat answering question 1. again. Take another screenshot of the GAO window.


## Part III: Insert a sequential Trojan
1. Create a new folder, `part3`, that contains another new Gowin project. Once again, copy over your working DES design from Part 1.
1. Insert a sequential Trojan into the DES circuit. 
    * Use the same clock as the DES circuit. 
    * The trigger condition of the Trojan is when the least significant 2 bits of the F function output in order go through some order of three values at the negative edge of the clock (see next section). 
    * Like Part II, when the Trojan is triggered, the LSB of the input key (NOT round keys) for the DES is inverted (ie. invert key56[0]). The key only needs to be inverted for one cycle, and can then revert to the original value.
1. **REPORT:** Answer the following questions in your report:
    1. Consider the sequential trigger `2'b01→2'b10→2'b11`
        a. How many states are needed in total? How many additional registers have you implemented?
        b. Will the Trojan be triggered? How many times is it triggered in the 16 rounds? Turn in a screenshot of the GAO window.
    1. When the condition is `2'b11→2'b10→2'b00`, repeat answering question 1. again.
    1. When the condition is `2'b11→2'b01→2'b00`, repeat answering question 1. again.


## What to Submit

Make sure your submission tag on Github includes the following files:
1. Your lab report (*lab_trojan_i/report.pdf*).
1. The files for all three of your FPGA projects (*lab_trojan_i/part1/*, *lab_trojan_i/part2/*, and *lab_trojan_i/part3/*). Make sure these contain your verilog source files (with changes to implement the Trojans), as well as your GAO files you used to test the Trojans.

## Acknowledgement

These instructions were originally from Dr. Swarup Bhunia, University of Florida, and were modified for this class.
