---
layout: lab
toc: true
title: "Get To Know Your HAHAv3 Board"
short_title: "HAHAv3 Board"
number: 0
under_construction: false
---

## Preliminary

1. Complete your repository setup, following the instructions on [this page]({% link _pages/lab_instructions.md %}).

1. Install the GOWIN FPGA tools on your home drive.  Follow the instructions [here]({% link _pages/tools.md %}#installing).


## Instructions

### PART I: Test the Power Supply and Voltage Regulators
1. Connect the HaHa v3.0 board to the USB Power supply (J14) with a USB A-to-B cable to your Windows machine. Turn on the power to see if the power light is on.
1. Using an oscilloscope, check the voltages in the pins along header (P2) on the board. There should be ten pins on this header along with five test pins/points to the right. Starting from the top pin:
    - Pins 1-2 probe the input voltage.
    - Pins 3-4 output the adjustable voltage. Use button SW1 and SW2 to change the voltage.  *Note:* This changes the voltage for the FPGA on the board, but not the microcontroller.
        - **REPORT:** Answer this question: what is the adjustable voltage range?
        - **REPORT:** Take a picture showing the minimum voltage of the 3.3A test point.
    - Pins 7-8 have a constant 3.3 V output.
    - Pins 9-10 are ground pins.

**Note:** After doing voltage measurement, remember to set “PWR SEL” to “FIX” to make sure you can program both chips successfully.


### PART II: Test the FPGA and the Peripherals
1. Make sure the HaHa v3.0 board is still connected to the USB Power supply (J14) Open GOWIN FPGA Designer and navigate to *Tools >
Programmer* in the menu bar. 
    * The Programmer window will appear with another window called Cable Setting. Make sure the Port is set to "Gowin USB Cable(FT2CH)/0/…".  If it's showing a different cable type, try hitting the *Query/Detect Cable* button, and if that doesn't work, go back and make sure you followed the [steps for running the tool]({% link _pages/tools.md %}#running).
    * Click on the Find Device button (first button below menu bar that has a magnifying glass). 
    * Select GW1N-9C in the popup window. 
    * A new entry will now show up on the first row. Double click on Read Device Codes and change the following settings and then Press Save:
        * Access Mode = Embedded Flash Mode
        * Operation = embFlash Erase,Program,Verify
        * File name = <location/of/self_test_fpga_haha3.fs/file>

1. You are now ready to program the board. Navigate to *Edit > Program/Configure*. Wait until the programming is complete.

1. If this is successful, the 8 LEDs on the board should be blinking, and the 7-segment display a hexadecimal digit counting from 0 to F. To check the function of the 10 user switches, just switch any of them and you will see the 8 LEDs will change their pattern of blinking. There are two different patterns of blinking. Whenever any of the switches are flipped, the pattern should change.  To check the function of the pushbuttons, just push any of them, and you should see the 7-segment display stops counting and start to repeat blinking with a fixed number. 

1. **REPORT:** Answer these questions:
    - What are the two LED patterns occurring in the board?
    - Which patterns of switches trigger either of the LED patterns?
    - What happens to the digit on the 7-segment display when you keep pressing any of the key buttons?
    - Attach a picture of the board with one of the LED patterns and a hexadecimal digit displayed in 7-segment display.

### PART III: Test the Microcontroller and the Accelerometer
1. Program the provided `U_ACC.hex` file into the microcontroller.  See the [microcontroller programming instructions]({% link _pages/tools.md %}#programming)

    The microcontroller will read the acceleration data from the accelerometer and send it to the FPGA. The FPGA will show the value in binary on the eight LEDs if you place the most significant four switches to `1001`. Please rotate the board and see if the values are showing on the LEDs are changing. 
    
    *NOTE:* You may need to tap MCU RST button multiple times before the LEDs show the correct value and change as you move the board.

1. **REPORT:** Attach a photo of the board showing the acceleration value on the LEDs in your report.

1. Now we are doing to use the internal logic analyzer on the FPGA to observe certain signals that are connected to the FPGA.  

    * Open GOWIN FPGA Designer. Go to *File > New*. Select *FPGA Design Project*. Name the project `haha3_self_test` with the directory of your choosing. In the device selection window, use the following filters:
        * Series: GW1N
        * Device: GW1N-9
        * Device Version: C
        * Package: LQFP144
        * Speed: C6/I5
        * Select the Part Number: GW1N-UV9LQ144C6/I5. 
        
        The project creation process is just needed to unlock the GOWIN Analyzer Oscilloscope (GAO) tool. 
    
    * Go to *Tools > Gowin Analyzer Oscilloscope*. 
        * Press the folder button, and navigate to the location of the provided file (`self_test_fpga_haha3.rao`), and click *Open*. 
        * Check *Enable Programmer*, then press the button with the magnifying glass. Select *GW1N-9C*. If this device does not show up, you may need to change the *Cable* drop-down at the top.
        * Check the box below Enable. 
        * Under Fs File, click the empty field. Navigate to the location of the provided file (`self_test_fpga_haha3.fs`).
        * Press the small green play button (under *Enable Programmer*, and to the left of *Frequency*) to program the FPGA. 
        * After successfully programming, press the *Auto* button near the top, or press the F2 key shortcut.  This causes the logic analyzer to run repeatedly, allowing you to observe the signals as they change.
    
    * You should see the two signals. 
        * **REPORT:** Attach a screenshot of the GOWIN Analyzer Oscilloscope window showing the aforementioned signals.
        * **REPORT:** Answer the following:
            * What is the first signal in the oscilloscope? Where is this signal coming from on the board?
            * What is the second signal in the oscilloscope? Where is this signal coming from on the board?

### PART IV: Test the Temperature Sensor
1. Program the microcontroller with the provided `U_TEMP.hex` file. The microcontroller will read the data from its internal temperature sensor and send it to the FPGA. Put switches S9
– S6 to “1001” to see the values on the LEDs.
1. **REPORT:** Add the following:
    * Attach a photo of the board showing the microcontroller temperature value in your report.
    * Attach a screenshot of the GOWIN Analyzer Oscilloscope window showing the aforementioned signals.

### PART V: Test the Flash Memory
1. Program the microcontroller with the provided `U_FLASH.hex` file. The microcontroller will read a signature of the EEPROM and send it to the FPGA. Put switches S9 – S6 to `1001` to see the values on the LEDs. The LEDs should be showing 00101001 (0x29).
1. **REPORT:** Add the following:
    * Attach a photo of the board showing the value 0x29 on the LEDs in your report.
    * Attach a screenshot of the GOWIN Analyzer Oscilloscope window, showing 0x29 for the second signal.


## What to Submit

Create a lab report document that contains the items mentioned above. <span style="color:red">Make sure to save this as *lab_haha/report.pdf*,</span>
and submit as tag `lab_haha`, using the [submission instructions]({% link _pages/lab_instructions.md %}#submission).

## Board Issues
I will post any issues with boards here:
* Board 1, Board 5: Accelerometer is not working.  

## Helpful Tips

<!-- ### Programming the Microcontroller
Each time you want to program the microcontroller, you need to move the switch to BOOT and press the reset button.  To run your program, move the switch to APP and press the reset button again.   -->

### Temperature Sensor
There is a can of compressed air in the lab that you can blow on the microcontroller to see the temperature change. 

### Detecting the Board
Sometimes the FPGA programmer has trouble detecting the board.   Try power cycling the board, unplugging and re-plugging the USB cable, or restarting the GOWIN software.  Sometimes it takes a few tries to get it to work.

### Flashing the FPGA
During the lab, you will program the FPGA with a provided bitstream.  The FPGA on your board, like most FPGAs, is stores its configuration (ie the design) in SRAM.  This means that when you power off the board, the design is lost.  Many FPGA boards, including ours, contains a separate Flash chip, that will store a default configuration that will be loaded into the FPGA when the board is powered on.  Using the programmer software, you have the choice of programming either the FPGA SRAM (which will be lost when the board is powered off) or the Flash chip (which will be loaded into the FPGA each time the board is powered on).  For this lab, you will be programming the Flash chip, but in future labs, you will be programming the SRAM.


## Acknowledgement

These instructions were originally from Dr. Swarup Bhunia, University of Florida, and were modified for this class.