---
layout: lab
toc: true
title: "Placer"
number: 2
repo: placer
---


## Preliminary

### Overview
You are to write an implementation of a simulated annealing placer.  The goal will be to come up with an implementation that achieves a good cost, based on the sum of half-perimeter wire length (HPWL) of all nets.  

<!-- 
### System Packages

You will need these system packages:

```
sudo apt install libx11-dev libumfpack5 libsuitesparse-dev
``` 
-->

### Code

The starter code sets up basic classes to keep track of Blocks, Nets, and graphics.  You will only need to implement the actual placer code.

  * *placer_main.cpp*: The main executable.  This takes a command-line argument, the path to the circuit file.	
  * *Design.cpp/.h*: A class containing all blocks and nets in the design.  You are already provided with functions to get the current half-perimeter wire length (`getHPWL()`), as well as several helper functions.
  * *FPGA.cpp/.h*: A class representing the FPGA chip:
    * It is a square of `size x size` tiles.  
    * The outermost tiles are for I/O blocks, which are fixed in place. The fixed blocks will be pre-placed when reading in the circuit file.  For all other blocks, you will need to place them.
    * `getBlock(...)` will return the block placed at a given location.  The function is overloaded and can be provided a `(int x, int y)` location, or an `(int xy)` location. The latter is a single integer representing the x and y location, where `x = xy / size`, `y = xy % size`.
  * *Block.cpp/.h*: A class representing a block that needs to be placed.  
    * Blocks contain an x,y location, a list of connected `Net`s, as well as a `fixed` property.
    * You can place or unplace a block using the `place` and `unplace` functions.  When you call the `place` function, it will call the appropriate `FPGA` `placeBlock` function that will allow the `FPGA` class to keep track of blocks placed at each location.  Do not directly call the `FPGA` `placeBlock` function in your code.
    * Placing can be done using a `(int x, int y)` location, or an `(int xy)` location.  
  * *Net.cpp/.h*: A class representing a single net in the design, containing a list of all Blocks that are part of the Net.	
  * *Drawer.cpp/.h*: A class responsible for drawing the chip and block placement.
  * *circuits/*: Contains a folder of test circuits.


### Input Circuit File
Netlist format:
  * The first line contains a single integer indicating the FPGA `size`:
  * The next lines list the fixed I/O blocks:

        blocknum x y

  * The next lines list the movable blocks and their net connections:

        blocknum netnum_1 netnum_2 ... 


## Implementation

### Part 1: Greedy Placement

Implement a greedy placement algorithm that:
1. Performs random placement with a given seed.  You will need to implement `void Design::randomizePlacement(int seed)`.
1. Performs a purely greedy version of the simulated annealing algorithm, where only swaps that reduce the cost metric (HPWL) are accepted.  Implement this in `void Design::greedyAnnealing(int seed)`. 
  * Make sure your algorithm is deterministic.  It should produce the same result for the same seed.
  * Your algorithm should repeatedly select two random moveable (non-I/O) x,y positions, and tentatively swap them.  If the swap reduces the HPWL, accept the swap.  If not, reject the swap.  Make sure your algorithm allows you to swap a populated tile with an empty tile.
  * You should exit your loop after 1000 consecutive failed swaps.
  

### Part 2: Simulated Annealing
Implement a simulated annealing algorithm that:
1. Performs an initial placement.  This can be done again using `void Design::randomizePlacement(int seed)`.
1. Performs a simulated annealing algorithm.  Implement this in `void Design::simulatedAnnealing(int seed)`. 
  * Make sure your algorithm is deterministic.  It should produce the same result for the same seed.
  * Your algorithm should repeatedly select two random moveable (non-I/O) x,y positions, and tentatively swap them.  Use the exponential equation discussed in class to determine whether to accept the swap.
  * Your algorithm should contain two loops.  The inner loop should try `N` swaps, where you can choose how `N` is determined.  The outer loop should decrease the temperature on each execution of the inner loop. 
  * You can choose your initial temperature, cooling schedule, and outer loop exit condition.  
  
### Part 3: Exploration
Explore some form of optimization to improve the quality of your result.  You can choose to explore optimizations that affect runtime or quality of result (QoR).  Some ideas include:
  * Different cooling schedules
  * Different initial temperatures
  * Different ways to determine `N`
  * Different ways to determine an initial placement
  * Restrictions on swap distance (rlim), depending on temperature
  * More efficient ways to keep track of HPWL and calculate cost deltas (if you use the provided function, it will recalculate the entire HPWL each time it is called)

## Report and Submission

1. Describe your simulated annealing algorithm, including a description of your initial temperature, cooling schedule, and outer loop exit condition, and how you determined `N`. This can incorporate the optimizations you explored in Part 3.

1. Include a table of results for the various circuits.  This can include the optimizations you explored in Part 3.  The table should include the following data:

| Circuit | Random HPWL | Greedy HPWL | Simulated Annealing HPWL | Relative HPWL | Simulated Annealing Runtime (s) | 
|---------|-------------|-------------|--------------------------|---------------|-----------------------------|
| small   |             |             |                          |               |                             |
| med1    |             |             |                          |                             |
| med2    |             |             |                          |                             |
| large1  |             |             |                          |                             |
| large2  |             |             |                          |                             |
| xl      |             |             |                          |                             |
| huge    |             |             |                          |                             |
| \<example row\> | 8599 | 5447 | 5139 | 0.94 | 6


1. Describe the optimizations you explored in Part 3, and provide results demonstrating how your optimization(s) impact runtime or QoR for at least one circuit.  For example, you might include the following:

    **Optimization 1**

    *Discussion of optimization 1*

    | Circuit | HPWL without optimization | HPWL with optimization | Relative HPWL |
    | med 1   | 5000  | 4560 | 0.90 | 


    **Optimization 2**

    *Discussion of optimization 2*

    | Circuit | Runtime without optimization | Runtime with optimization | Relative Runtime |
    | large1  | 10 | 8 | 0.80 |

See [Submission Instructions]({% link _pages/submission.md  %}).

