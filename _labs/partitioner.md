---
layout: lab
toc: true
title: "Partitioner"
number: 3
repo: partitioner
---

## Preliminary

In this assignment, you are to implement a genetic partitioning algorithm.

You are to minimize the number of nets that connect blocks in the two partitions, while keeping the
sizes of the two partitions exactly equal (or differing by at most 1 if there are an odd number of blocks). This
means that each net (even a multi-fanout net) contributes either 0 or 1 to the total crossing count, never
more than this. A net contributes 1 to the crossing count if it connects cells that fall in both partitions.

### Code 

The starter code sets up basic classes to keep track of Blocks, Nets, and some starter code for the Genetic Algorithm partitioner. You will only need to implement the actual Genetic Algorithm.

  * *partitioner_main.cpp*: The main executable.  This takes two command-line arguments, the path to the circuit file, and a random seed.	
  * *Design.cpp/.h*: A class containing all blocks and nets in the design.  
  * *Block.cpp/.h*: A class representing a block in the design.  Blocks connect to a list of Nets in the design.
  * *Net.cpp/.h*: A class representing a single net in the design, containing a list of all Blocks that are part of the Net.	
  * *Chromosome.cpp/.h*: A class that represents a single chromosome (a valid partitioning).  Some of this code is provided, although other functions will need to be completed by you.  Note that this function caches the cut size of the partition, so if you modify the chromosome in your code, make sure you update the cut size otherwise your algorithm may be operating on stale data.
  * *GeneticPartitioner.cpp/.h*: A class that maintains a population of Chromosomes and performs partitioning using a Genetic Algorithm.  Code is provided to create the initial population, but you will need to complete the rest of the code.
  * *circuits/*: Contains a folder of test circuits.  Three of these circuits I have synthetically created and are quite large (*synth100*, *synth500* and *synth5000*).  I believe these circuits can be perfectly partitioned (ie a cute size of 0 is possible), although it's highly unlikely your algorithm will locate this optimal solution.

A script is provided (*scripts/run.py*) that will run your partitioner for all benchmark circuits and collect the results into a CSV file.

You are free to make any changes you want to the classes and structure, but your code should still work with the original *run.py* script.

### Input Circuit File
Each line of the input file has the format:

    block_num_connections netnum1 netnum2 ... netnumn

Each line of the file represents a block in the design. The first value on the line indicates the number of nets the block connects to, and the remaining values are the list of net numbers.

## Implementation

Complete the Genetic Algorithm partitioner.  Your solution should follow the major steps of a Genetic Algorithm (ie, have a population, selectively chose parents, perform some crossover, possibly perform a mutation, reduce population, etc).

See Bui paper discussed in class (<https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=508322>) for helpful details.

### Exploration

You should perform some exploration to improve your algorithm.  You could try:
  * Changing population size
  * Changing fitness function
  * Changing how crossover is done
  * Mutation strategy
  * Population reduction strategy
  * Exploring the impact of different random seeds

### Determinism

The second command-line argument that you provide to your program is a *seed* value, that is used to seed a `std::default_random_engine` in the  *Design* class.  This can be used to perform a variety of random functions.  For example, you can generate a random real number in the range 0.1 to 0.5 using the following code:

    std::uniform_real_distribution<> dist(0.1, 0.5);
    float val = dist(design->getRng());

Many other distributions are available in [#include \<random\>](https://en.cppreference.com/w/cpp/numeric/random), such as `uniform_int_distribution`.

If you are careful to always use the seeded random engine for any randomness you need in your program, you will ensure consistent deterministic random behavior, and be able to change the seed value to explore different random behaviors.


## Report and Submission

Prepare a brief report (max 2 pages) that includes:
  * A table of runtime and cut cost for each benchmark (this is produced by *run.py*).
  * A discussion of exploration/optimization you performed, with results, tables, figures, etc, to help communicate your exploration/optimization process.

See [Submission Instructions]({% link _other/submission.md  %}).
