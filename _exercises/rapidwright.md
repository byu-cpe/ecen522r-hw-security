---
layout: lab
toc: true
title: "RapidWright"
number: 2
# repo: lab_graphs
---

## Preliminary

### Background

The purpose of this assignment is to get some basic experience using RapidWright (<https://www.rapidwright.io/>), a (mostly) open-source Java tool that provides you with a programming interface to Vivado designs.  RapidWright contains it's own Java classes to represent Xilinx FPGA devices, as well as the designs that can be implemented on the device.  RapidWright interacts with Vivado through Xilinx checkpoint (.dcp) files.  You can create designs in Vivado, export them to a checkpoint and import them into RapidWright for analysis or modifications.  The reverse process is also possible.

In this exercise you will create a checkpoint in Vivado, import it into RapidWright, and then do some simple analysis on the *Device* and the *Design*.  You will write some code to determine a distribution of how far nets travel on the FPGA, and how many programmable connections they pass through.

### Python API

Although RapidWright is written in Java (available at <https://github.com/Xilinx/RapidWright>), it is possible to access the Java classes and methods directly from Python, using the [JPype](https://github.com/jpype-project/jpype) tool.
Setting up this environment takes only a few minutes, and instructions are provided [here](https://www.rapidwright.io/docs/Install_RapidWright_as_a_Python_PIP_Package.html).

If you prefer to use Java that's fine too.

### Documentation

The documentation for the various RapidWright classes can be found at <https://www.rapidwright.io/javadoc/>.

You will likely find classes helpful, and probably others not listed here:
  * [Device](https://www.rapidwright.io/javadoc/com/xilinx/rapidwright/device/Device.html)
  * [Design](https://www.rapidwright.io/javadoc/com/xilinx/rapidwright/design/Design.html)
  * [Net](https://www.rapidwright.io/javadoc/com/xilinx/rapidwright/design/Net.html)
  * [PIP](https://www.rapidwright.io/javadoc/com/xilinx/rapidwright/device/PIP.html)

## Implementation 

### Generate Design
You should create a design in Vivado that you will import into RapidWright.  You are free to use a design of your choosing.  If you aren't sure how to create a design in Vivado, please reach out to the instructor.  Try and choose a design that isn't too small (at least a few thousand LUTs)

In Vivado you should perform *Implementation*, and then export both an EDIF netlist (you can do this using `write_edif`) and a checkpoint (using `write_checkpoint`).


Make sure you can import your design into RapidWright.  You can do so like this:

```python
design = Design.readCheckpoint("dcp_path.dcp", "edif_path.edf")
```

When I import my DCP I see an output message like this:

    ==============================================================================
    ==                         Reading DCP: design.dcp                          ==
    ==============================================================================
    Loading device from file xc7a200tsbg484-1
    XML Parse & Device Load:     0.378s
                EDIF Parse:     0.457s
            Read XDEF Header:     0.017s
            Read XDEF Caches:     0.020s
        Read XDEF Placement:     0.248s
    INFO: Building uncommon Wire->Node cache...
        This might take a few seconds for large devices on the first call.  
        It is generally triggered when getting the Node from an uncommon Wire object.  
        To avoid printing this message, set Device.QUIET_MESSAGE=true or set the ENVIRONMENT variable RW_QUIET_MESSAGE=1.
    INFO: Finished building uncommon Wire->Node cache
        Read XDEF Routing:     1.137s
    ------------------------------------------------------------------------------
            [No GC] *Total*:     2.257s

Once you have your design loaded, print the number of Cells and Nets:

```python
print("Number of Cells:", len(design.getCells()))
print("Number of Nets:", len(design.getNets()))
```

### Device Exploration

Write some code that outputs the device name and number of LUTs in the device.  There are different ways to do this.  Don't worry too much about whether you get the exact number correct, just try and explore the Device classes and come up with a method that seems to work.

### Net Distances

Calculate and plot a histogram of distances from source to sink for all Nets. **For nets with fanout, find the distance to the furthest sink.**
 
* Use [Manhattan distance](https://www.statology.org/manhattan-distance-python/).
* Some nets are contained entirely within a Tile and do not require intertile routing.  For these nets, the *getSource()* function will return null/None and you can count them as 0 distance.
* It doesn't matter what units you use for these distances, as long as you are consistent.  I used the *getRow()* and *getColumn()* functions in the *Tile* class, but there are other coordinate systems you could use.
* Supposing you store the distances in a dictionary where the key is the distance and the value is the number of nets with that distance, you can use *matplotlib* to create a simple bar graph like so:
    
    ```python
    plt.bar(distances.keys(), distances.values(), 1.0, color="g")
    plt.show()
    ```

### Net PIP Hops
Calculate and plot a histogram of the number of PIPs a Net traverses through from source to sink (PIPs are programmable interconnect points that connect the wires in the FPGA). For nets with fanout, calculate the maximum number of PIPs from source to any sink.
  * This is a bit harder that it sounds at first, as RapidWright does not supply this information directly.  You can call `Net->getPIPs()`, but this returns a flattened list of all PIPs for the net, and does not represent the actual tree structure that a multi-sink net would have.  
  * For example, consider this picture below showing a single Net that connects to three different sink pins.  There are six total PIPs that are used to route the net, but you would count this net as having three PIP hops from source to furthest sink.  
    <img src="{% link media/rapidwright_pips.png %}" width="800">
  * In the picture above, the wire segments connect between tile pins and PIPs, and between PIPs and other PIPs, and are represented in the *Node* class in RapidWright.
  * *net.getSource().getConnectedNode()* will give you the *Node* that the source pin connects to.  
  * For PIPs, *getStartNode()* and *getEndNote()* will give you the wire segments (Nodes) that the PIP connects.
  * With this information and a bit of code, you can take the PIP list and figure out the maximum number of PIPs a net traverses through.

## Submission

Write a very brief report that includes: 
  * Describe what circuit you used.
  * What is the device name and number of LUTs?
  * How many Cells and Nets?
  * Include your two histograms.  Send your **report and code** to [jgoeders@byu.edu](mailto:jgoeders@byu.edu) with the subject: 629 Exercise 2