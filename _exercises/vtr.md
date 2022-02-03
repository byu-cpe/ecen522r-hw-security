---
layout: lab
toc: true
title: "VTR: Open Source FPGA CAD Tool"
short_title: VTR
number: 1
# repo: lab_graphs
---

## Acknowledgement
<span style="color:blue">
This exercise was developed by Professor Jason Anderson (University of Toronto).  I have modified it slightly for our class.
</span>

## Preliminary

### Background
The purpose of this exercise is two-fold: 1) to use an automatic placement tool based on the simulated annealing optimization strategy, and to gain some familiarity with the properties of that strategy; and, 2) to experiment with the negotiated congestion routing algorithm described in class. 

The placement and routing (P&R) tool you will use in this exercise is called **VPR**. It is a tool originally created at the University of Toronto by Professor Vaughn Betz as part of his Ph.D research in the late 90s. It has been enhanced extensively since then by other graduate students, most recently by Dr. Jason Luu and Dr. Kevin Murray. As well as being a P&R tool, VPR is a framework for FPGA CAD and architectural research, used by researchers all around the world. VPR takes a text description of the target FPGA architecture as input. The description specifies, among other things, the make-up of the FPGA logic blocks, the routing segment lengths, the switch block style, and the routing delays. By changing the architecture description file, one can evaluate the speed and area-efficiency of different FPGAs.

VPR is part of a larger FPGA CAD tool suite called VTR (Verilog-to-Routing), which, besides placement and routing
with VTR, includes RTL synthesis, logic optimization and technology mapping.

### Downloading
Clone the official VTR repository (<https://github.com/verilog-to-routing/vtr-verilog-to-routing>).  I have tested this exercise using a recent commit of VTR, you may want to specifically check out that commit:

```
git clone git@github.com:verilog-to-routing/vtr-verilog-to-routing.git ~/vtr
cd ~/vtr
git checkout 36789e456ab292038b8518bf2c835c9b9dcaaa0e
```

### Building 
Follow the instructions at <https://docs.verilogtorouting.org/en/latest/BUILDING/>.

A binary executable, `vpr/vpr` will be produced. 

### Instruction Manual
Documentation for running VPR can be found at <https://docs.verilogtorouting.org/en/latest/vpr/command_line_usage/>.

### Architecture File
Use the architecture file *vtr_flow/arch/timing/k6_frac_N10_mem32K_40nm.xml*. You may find reading the architecture file interesting, as it gives a description of an FPGA architecture. Take a look at the top of the file. The architecture file describes an FPGA with ten 6-input LUTs per logic block, columns of RAM and columns of multipliers. The routing segments span 4 logic-block tiles.

You may want to create a temporary working folder, and copy the architecture file there:

```
cd ~/vtr
mkdir work
cd work
cp ../vtr_flow/arch/timing/k6_frac_N10_mem32K_40nm.xml .
```

### Netlists
You will use four netlists for this exercise.  Unlike in the other course assignments, which used synthetic circuits, these are real circuits.  Some of these benchmarks are already available in netlist form, others will need to be compiled from Verilog. 

You can copy the *alu4.blif* and *ex4p.blif* netlists to your work directory:
```
cd ~/vtr/work
cp ../vtr_flow/benchmarks/blif/6/alu4.blif .
cp ../vtr_flow/benchmarks/blif/6/ex4p.blif .
```

You can then synthesize the *diffeq1* and *sha* circuits into netlists and copy them to your work directory.  The *run_vtr_flow.py* is used to run the VTR flow (which places all temporary files in a *temp* directory), and the *-end abc* argument stops the flow after the ABC tool performs logic optimization and technology mapping:
```
../vtr_flow/scripts/run_vtr_flow.py ../vtr_flow/benchmarks/verilog/diffeq1.v  k6_frac_N10_mem32K_40nm.xml -end abc
cp temp/diffeq1.abc.blif ./diffeq1.blif
../vtr_flow/scripts/run_vtr_flow.py ../vtr_flow/benchmarks/verilog/sha.v  k6_frac_N10_mem32K_40nm.xml -end abc
cp temp/sha.abc.blif ./sha.blif
```

## Exercise A - Placement and Simulated Annealing
The VPR program allows the user to set various Simulated Annealing parameters: the starting temperature, the ending temperature, the rate at which the temperature decreases, the number of moves per temperature, and the initial random seed. NOTE: For all steps in Exercise A, run VPR with the following parameters: `--pack` `--place` (prevent routing) and `--place_algorithm bounding_box` (minimize HPWL only and ignore timing).  You can use `--disp on` to enable graphics.  If you have graphics enables, the window will pop-up before placement, and then you can click *Proceed* to perform placement.  
If you set *Toggle Nets* to *Nets*, you can see the rat’s nest of wires become less tangled.

1. Run the vpr program once, with its default parameters, on the four circuit netlists. Plot the score (cost function) versus the temperature. 
  * Use a scatter plot, with temperature on the x-axis and cost on the y-axis.  
  * It looks best if you set the x-axis to reserve order (descending temperature values), and x-axis to log scale.
  * Enable a line between the scatter plot points.
  * Draw the four circuits as four series on the same plot, or use four different plots.  

2. Run VPR 5 times for each circuit with different random seeds. Calculate and report the mean and standard deviation of the resulting final scores. Comment on the sensitivity of the scores to the random seeds? Do all circuits exhibit the same sensitivity to the random seed?

    You may find a bash script like this helpful:

    ```bash
    for CIRCUIT in ./*.blif
    do
        for SEED in 1 2 3 4 5
        do
            echo $CIRCUIT $SEED
            ../vpr/vpr k6_frac_N10_mem32K_40nm.xml $CIRCUIT --pack --place --place_algorithm bounding_box --seed $SEED | grep "Placement cost:"
        done
    done
    ```

3. For each circuit, run VPR 5 times (with different seeds) with 5 different starting temperatures (5 * 5 * 4 circuits = 100 runs in total). Be sure to choose some starting temperatures that are low enough to actually impact the placement results (HINT: you may need to use starting temperatures considerably less than 1!). For each circuit, report the mean and standard deviation of the final score for each starting temperature.

4. In *vpr/src/place/place.cpp*, find the `assess_swap()` function to see the code that determines if a move should be accepted or rejected. You will see the function call to `std::exp(-delta_c/t)` that we discussed in class. Hack this function to disable hill climbling in the placer, meaning that swaps will only be done greedily. Repeat step #2 above with the new acceptance function. Report the mean and standard deviation of the resulting scores. Discuss quality differences observed versus the results for #2 above.  **Undo this change to place.cpp before proceeding.**

5. VPR implements range limits for move distances, as discussed in class. Use the `--place_rlim_escape` argument to allow all moves to ignore the swap distance limit.  Repeat step #2 above. Report the mean and standard deviation of the resulting scores. Discuss quality differences observed versus the results for #2 and #4 above.

# Exercise B - Timing-Driven Routing
Note: For this part of the Exercise, please use the original place.c for VPR and not the new one you hacked up in Part A.

1. VPR also implements timing-driven routing using an improved version of the PathFinder approach described in class. Run the placement tool with the default options and the timing-driven router. Report the post-routing critical path delay and the router run-time (see VPR output). NOTE: Do not set `--pack`, `--place` or `--place_algorithm` for Exercise B. Set the #tracks/channel (W) to 40 using `--route_chan_width 40 for *alu4*, 60 for *diffeq1*, 50 for *ex4p*, and 60 for *sha*.

1. Using the same parameters as in the previous step, explore the different parameters of the router and try to improve the critical path delay for the four circuits. Comment on the parameters you experimented with and their effect on the critical path delay. Be sure to vary the *present congestion* (`--pres_fac_mult`) and *historical congestion* (`--acc_fac`) parameters talked about in class.

1. VPR implements A* routing. Disable A* routing using the using `--astar_fac 0`. Repeat step #1. Comment on how router run-time and quality is affected by disabling A* routing.

## Submission

Send your report to [jgoeders@byu.edu](mailto:jgoeders@byu.edu) with the subject: 629 Exercise 1
