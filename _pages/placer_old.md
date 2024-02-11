---
layout: lab
toc: true
title: "Placer"
number: 2
repo: placer
under_construction: true
---

## Acknowledgement
<span style="color:blue">
This assignment was developed by Professor Jason Anderson (University of Toronto).  I have modified it slightly for our class.
</span>

## Preliminary

### Overview
You are to write an implementation of a basic analytical placer (AP), with overlap removal (spreading).  The goal will be to come up with an implementation that achieves a good cost, based on the sum of half-perimeter wire length (HPWL) of all nets.  As described in class, you will formulate the placement problem mathematically as a system of linear equations to be solved. You will use an existing package (UMFPACK) to solve the linear system.

### System Packages

You will need these system packages:

```
sudo apt install libx11-dev libumfpack5 libsuitesparse-dev
```

### Code


The starter code sets up basic classes to keep track of Blocks, Nets, and graphics.  You will only need to implement the actual placer code.

  * *placer_main.cpp*: The main executable.  This takes a command-line argument, the path to the circuit file.	
  * *Design.cpp/.h*: A class containing all blocks and nets in the design.  You are already provided with functions to get the current half-perimter wire length (`getHPWL()`), as well as several helper functions.
  * *Block.cpp/.h*: A clas representing a block that needs to be placed.  Blocks contain an x,y location, a list of connected `Net`s, as well as a `fixed` property.  A `imaginary` property is included in case you want to create fake blocks for spreading purposes.  Imaginary blocks won't be drawn, and won't affect the HPWL.
  * *Net.cpp/.h*: A class representing a single net in the design, containing a list of all Blocks that are part of the Net.	
  * *Drawer.cpp/.h*: A class responsible for drawing the chip and block placement.
  * *APEdge.cpp/.h*: A class that you can use to keep track of edges (springs) for the analyical placement formulation.  
  * *circuits/*: Contains a folder of test circuits.


### UMF Pack
UMFPACK is a tool for working with sparse matrices.  You should familiarize yourself with **compressed column storage**, which you can read about [here](https://people.math.sc.edu/Burkardt/cpp_src/umfpack/umfpack.html).  A sample cpp program for working with UMFPACK can also be found on that page (<https://people.math.sc.edu/Burkardt/cpp_src/umfpack/umfpack_simple.cpp>).

### Input Circuit File
The netlist file input format has two sections. The two sections are separated from one another by a -1 appearing by itself on a line. The first section specifies the blocks to be placed and the connectivity between them. Each line has the following form:

    blocknum netnum_1 netnum_2 netnum_3 ... netnum_n -1

Where blocknum is a positive integer giving the number of the cell, and the netnum_*i* are the numbers of the nets that are attached to that block. Every block that has the same netnum_*i* on its description line is attached. Note that each block may have a different number of nets attached to it. Each line is terminated by a -1.

Example input file:
```
1 2 3 4 -1
2 5 4 -1
3 5 6 2 -1
4 6 3 -1
-1
1 50 0
4 0 50
-1
```

In this example, block 1 is connected to nets 2, 3 and 4. Note that each net may be connected to more than two blocks (that is, there are multi-fanout nets). Also note that net numbers are not related to block numbers.

As discussed in class, the AP formulation requires there to be a set of pre-placed (fixed) objects, normally I/Os. The second section of the netlist file specifies the placement of fixed objects. It has the following form:

    blocknum x_position y_position

In the above example, block 1 is pre-placed at the position with x = 50, y = 0. The list of fixed blocks is terminated by a -1 by itself on a line.

## Implementation

1. Formulate and solve the analytical placement problem assuming the clique net
model (*Remember, in the clique model, each edge in the complete graph has weight of 2/p*). Do not do any overlap removal in this step. 

1. Implement some form of overlap removal to legalize the placement.  We will discuss different approaches in class.  

### Overlap Value

The code will evaluate your overlap by splitting the chip into a 10x10 grid, and then checking how many blocks are assigned to each grid location.  Any blocks in excess of 2 per grid location will be added to the *Overlap Cost*.  Try to spread *cct3* to obtain an overlap score of <= 15.

### Exploration

Explore some form of optimization to improve the quality of your result.  
 
## Report and Submission

1. A table indicating initial HPWL, final HPWL and final Overlap Cost for each circuit.
1. A description of your spreading algorithm.  
1. A description of any optimizations you performed.

See [Submission Instructions]({% link _pages/submission.md  %}).

