---
layout: lab
toc: true
title: "Lab 1: Router"
short_title: Router
number: 1
# repo: lab_graphs
---

## Objectives
You are to write an implementation of an FPGA router, and work to optimize the quality of result, runtime, and/or memory usage.  

## Preliminary

### Github Repo

Code and submission is done using Github Classroom.  Use this [invitation link](https://classroom.github.com/a/mE_1ReuX) to create a private Github repository that you will use for the class. 

After you create your repository, import the starter code from <https://github.com/byu-cpe/ecen629_student>. 

### Code

The starter code sets up the structures for the FPGA architecture, the routing resource graph, as well as drawing the FPGA and routing.  You will only need to implement the actual router code.

### FPGA Architecture
The FPGA architecture is illustrated in the figure below.  In the first part of this assignment, each pin can connect to all tracks in the
neighboring channel (F<sub>c</sub> = W, as shown in the figure). 
Each routing segment endpoint can connect to **three** other segments (F<sub>s</sub> = 3). The routing segments span one logic block tile. Switches are bidirectional. Switch
blocks use the Wilton topology. 

<img src="{% link media/labs/arch.png %}" width="600">

\begin{figure}[h!]
	\centering
		\includegraphics[page=2,trim={2cm 19cm 5cm 3cm},clip,scale=0.7]{jason_a1}
  \caption{.}
	\label{fig:arch}
\end{figure}


\subsection*{Code Structure}
\begin{itemize}
	\item asst\_routing.cpp: The main executable.  This takes two command-line arguments, the path to the circuit file, and the FPGA channel width.
	\item Design.cpp/.h: A class containing all nets in the design.
	\item Drawer.cpp/.h: A class responsible for drawing the FPGA and routing.
	\item FPGA.cpp/.h: A class that contains a list of all FPGA tiles, as well as the FPGA size and channel width.
	\item FpgaTile.cpp/.h: A class representing an FPGA tile, containing pointers to neighboring tiles, as well as pointers to the RRNode pins and wires in the tile.
	\item Net.cpp/.h: A class representing a single net in the design, containing one source RRNode and a list of sink RRNodes.
	\item Router.cpp/.h: An empty class to implement your router.
	\item RRNode.cpp/.h: A class representing a single routing resource node (wire segment or pin), with pointers to all connected RRNodes.  To assign a Net to be routing using an RRNode, you can call \texttt{RRNode->setNet()}.  When this is called, the net will be drawn appropriately.
	\item circuits/: Contains a folder of test circuits.
\end{itemize}

\subsection*{Input Circuit File}
The provided program takes as input from a file the following format:

\begin{itemize}
	\item The first line consists of one integer, n, where n gives the n x n dimensions of the chip in logic blocks. The grid cells are numbered from 0 to n-1 in each dimension.	
	\item 
The next set of lines has the form ``XS YS PS XD1 YD1 PD1 XD2 YD2 PD2 ...''. Each of these lines gives a net in the design, with source located at (x=XS,y=YS,pin=PS), followed by a list of desinations sinks (x=XD1, y=YD1, pin=PD1), (x=XD2, y=YD2, pin=PD2), etc.
This list is terminated by the line: -1 -1 -1 -1 -1 -1. 
\end{itemize}

Example input file:

\begin{table}[h!]
\centering
\begin{tabular}{p{5cm}l}
10 & (10 x 10) grid \\
1 2 4 2 3 2 &Pin 4 on block at (1,2) connects to pin 2 at (2,3) \\
0 0 4 1 2 3 &Pin 4 on block at (0,0) connects to pin 3 at (1,2) \\
-1 -1 -1 -1 -1 -1 &(end of pin pair list) \\
\end{tabular}
\caption*{}
\label{}
\end{table}

\subsection*{Graphics Package}
The graphics package, EasyGL, is available courtesy of Professor Vaughn Betz, University of Toronto (\url{http://www.eecg.toronto.edu/~vaughn/easygl/easygl.html}). 


\subsection*{What to Implement}
Implement a router that produces a valid routing of all nets in the design.  Your router may share wiring among the loads driven by a source pin.

For debugging purposes, you may find it helpful to write your program so that it can display the
progress of your algorithm as it routes each step for each connection; that is, you may wish to display each step
of the router expansion (though this is not mandatory for this assignment). 

You should test your program on the test files provided in the assignment repository.  You should determine the minimum channel width (W) for which your router will successfully find a solution.  You should then relax the channel width (W+2) and report results for this as well.

Collect any of the following results You should collect the following results:

\begin{table}[h!]
\centering
\begin{tabular}{l|p{2cm}|c|c|c}
\toprule
Circuit & W & \# Routing Segments & Runtime & Memory Usage \\
\hline \hline
small\_dense & $W_{min}$:&&&\\ \hline
small\_dense & $W_{min}+2$:&&&\\ \hline \hline

med\_dense & $W_{min}$:&&&\\ \hline
med\_dense & $W_{min}+2$:&&&\\ \hline \hline

large\_dense & $W_{min}$:&&&\\ \hline
large\_dense & $W_{min}+2$:&&&\\ \hline \hline

xl & $W_{min}$:&&&\\ \hline
xl & $W_{min}+2$:&&&\\ \hline \hline

huge & $W_{min}$:&&&\\ \hline
huge & $W_{min}+2$:&&&\\ \hline \hline

\bottomrule

\end{tabular}
\caption{}
\label{}
\end{table}

Runtime and memory usage can be obtained in Ubuntu by running your command using \texttt{/usr/bin/time -v <command>}.  Memory is available as \textit{Maximum resident set size}.

Innovate to improve the performance of your router.  

\subsection*{What to hand in}

\begin{enumerate}
	\item Commit your final updates to your Github Classroom repo before the deadline. 
	\item Commit a PDF report (two page max, min 10 pt) to your repo, containing the following:
	\begin{itemize}
	\item Your final results in a table as shown above.
	\item A screenshot of the routing of a design of your choice.
	\item A description of steps you took to improve any of the reported meterics.  Where helpful, include charts, figures or code snippets to explain what you chained and how impactful the optimizations were.
	\end{itemize}
\end{enumerate}


\subsection*{Acknowledgement}
This assignment was created by Professor Jason Anderson from the University of Toronto. I modified it for our class.

\end{document}
