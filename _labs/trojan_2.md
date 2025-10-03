---
layout: lab
toc: true
title: "Hardware Trojans II"
number: 4
under_construction: false
---

## Preliminary

### Temperature Sensor on HAHA V3.0 Board
The HAHA V3.0 board has a temperature sensor connected to the ADC of the microcontroller. 

Refer to `haha.h` for the functions that will allow you to read the ADC values from the temperature sensor.
The ADC is designed to return 0 at 0K, and increase linearly with temperature.
Each ADC is will behave slightly differently due to manufacturing variations, but has been calibrated at production at 85C (358K).  When you read the ADC value, you will need to use linear interpolation to convert the ADC value to a temperature in Celsius, based on the 0K and 358K calibration points.

### CM Bus from Microcontroller to FPGA
The HAHA V3.0 board has an 8-bit interconnection between the microcontroller and the FPGA called the CM bus.  You can use this bus to send data from the microcontroller to the FPGA.  

```c
// Initialize the 8-bit CM interconnect bus
void haha_inter_init(void);

// Send a byte of data to the FPGA by setting the 
// value on the 8-bit CM bus data lines, and then
// raising and lowering the clock signal
void haha_send_to_fpga(uint8_t data);
```

On the FPGA side, a simple circuit can be used to capture the data sent from the microcontroller:

```verilog
reg [7:0] data_from_mc;
always @(posedge CLK_inter) begin
    data_from_mc <= CM;
end
```

## Instructions

### Part 1: Use the Temperature Sensor
In this part, you will use the temperature sensor in the Microcontroller on the HAHA V3.0 Board. 

Send this calculated Celsius value to the FPGA over the 8-bit interconnection. You can heat the board to be higher than 40°C, but strictly below 80°C with a heater, i.e., a hairdryer, and see how the value changes. Alternatively, you can use a can of compressed air to cool it.

**REPORT**: Include the following in your lab report and submission:
* Commit your microcontroller code in the provided `sw/main.c` file.
* What is the output value of the temperature sensor under room temperature? Attach a picture/screenshot showing this value.  This could be output via UART, or on the FPGA via GAO tool or LEDs.
* What is the output value of the temperature sensor when you heat or cool the microcontroller? Attach a picture/screenshot showing this value.


### Part II: Design a system of your own 
Use the FPGA to design a circuit of your own. It can have any functions as you want. The only requirement is that it should receive the temperature data from the chip interconnection. Your design can be a counter which shows its value through LEDs or 7 segment display, or your design can show the temperature on the LEDs in binary, or your design can be an encryption machine that encrypts information, etc.

**REPORT**: Include the following in your lab report and submission:
* Commit your FPGA design files in the provided `hw/` directory.
* Describe your design and its normal function.
* How does your design receive the temperature data from the microcontroller?
* How many logic elements does your circuit use? 

### Part III: Insert a temperature triggered Trojan 
Modify your design by inserting a Hardware Trojan into it. The Trojan should be activated by the temperature signal: The Trojan will be triggered when the temperature becomes 40°C or higher. Alternatively, your Trojan can trigger when the board becomes lower than 26°C. 

Its payload could be any kinds: it could totally halt the original function; or it could induce delays to the design; or it could make the design consume more power, or it could cause information leakage; or it could make the design have other unexpected functions. Whatever the Trojan does, you should be able to observe the malicious effects the Trojan caused. 

Your design should come back to normal functions when the devices cool down to be under 40°C. (or above 26°C if you are using cooling).

**REPORT**: Include the following in your lab report and submission:
* Make sure your modified FPGA design files with the Trojan are committed in the provided `hw/` directory.
* Describe your Trojan. How is it triggered?
* What is the payload? How do you observe the phenomenon when the Trojan is activated? Attach pictures as needed.
* After inserting the Trojan, how many logic elements does your design use now? How much is the hardware overhead?
* What classification does your Trojan belong to? Refer to the figure on the [Trojan I lab page]({% link _labs/trojan_1.md %}) for the taxonomy diagram.

## What to Submit

Tag your submission as `lab_trojan_ii` and make sure to commit:
1. Your lab report (`lab_trojan_ii/report.pdf`).
1. Your software (`sw/*`) and hardware design files (`hw/*`).

## Acknowledgement

These instructions were originally from Dr. Swarup Bhunia, University of Florida, and were modified for this class.
