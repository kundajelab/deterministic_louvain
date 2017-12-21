Av here.

For the Mac branch, malloc.h was removed from graph_binary.h as per http://earthworm.isti.com/trac/earthworm/ticket/141

Original code downloaded from https://downloads.sourceforge.net/project/louvain/louvain_v0.2.tgz?r=https%3A%2F%2Fsourceforge.net%2Fprojects%2Flouvain%2Ffiles%2F&ts=1513173087&use_mirror=ayera

I included #include <unistd.h> in main_community.cpp to get rid of error saying getpid() was not declared in the scope, as per https://stackoverflow.com/questions/17836865/getting-errors-while-compiling (issue caused due to compiler version difference)

Additional changes to accept and apply a seed are confined to main_community.cpp. The srand seed is applied at line 123.

main_community.cpp was modified in line 150-151 to print out the hierarchy even when the level is not -1 (and not display the graph), and in line 164 to terminate the loop once the level exceeds the display_level, so as to avoid unnecessary additional work.

Original readme below.

-----------------------------------------------------------------------------

Community detection
Version 0.2 - not compatible with the previous version, see below.

Based on the article "Fast unfolding of community hierarchies in large networks"
Copyright (C) 2008 V. Blondel, J.-L. Guillaume, R. Lambiotte, E. Lefebvre

This program or any part of it must not be distributed without prior agreement 
of the above mentionned authors.

-----------------------------------------------------------------------------

Author   : E. Lefebvre, adapted by J.-L. Guillaume
Email    : jean-loup.guillaume@lip6.fr
Location : Paris, France
Time	 : February 2008

-----------------------------------------------------------------------------

Disclaimer:
If you find a bug, please send a bug report to jean-loup.guillaume@lip6.fr
including if necessary the input file and the parameters that caused the bug.
You can also send me any comment or suggestion about the program.

Note that the program is expecting a friendly use and therefore does not make
much verifications about the arguments.

-----------------------------------------------------------------------------


This package offers a set of functions to use in order to compute 
communities on graphs weighted or unweighted. A typical sequence of 
actions is:

1. Conversion from a text format (each line contains a couple "src dest")
./convert -i graph.txt -o graph.bin
This program can also be used to convert weighted graphs (each line contain
a triple "src dest w") using -w option:
./convert -i graph.txt -o graph.bin -w graph.weights
Finally, nodes can be renumbered from 0 to nb_nodes - 1 using -r option
(less space wasted in some cases):
./convert -i graph.txt -o graph.bin -r


2. Computes communities and displays hierarchical tree:
./community graph.bin -l -1 -v > graph.tree

To ensure a faster computation (with a loss of quality), one can use
the -q option to specify that the program must stop if the increase of
modularity is below epsilon for a given iteration or pass:
./community graph.bin -l -1 -q 0.0001 > graph.tree

The program can deal with weighted networks using -w option:
./community graph.bin -l -1 -w graph.weights > graph.tree
In this specific case, the convertion step must also use the -w option.

The program can also start with any given partition using -p option
./community graph.bin -p graph.part -v


3. Displays information on the tree structure (number of hierarchical
levels and nodes per level):
./hierarchy graph.tree

Displays the belonging of nodes to communities for a given level of
the tree:
./hierarchy graph.tree -l 2 > graph_node2comm_level2

-----------------------------------------------------------------------------

Known bugs or restrictions:
- the number of nodes is stored on 4 bytes and the number of links on 8 bytes.

-----------------------------------------------------------------------------

Version history:
The following modifications have been made from version 0.1:
- weights are now stored using floats (integer in V0.1)
- degrees are stored on 8 bytes allowing large graphs to be decomposed
- weights are stored in a separate file, which allows disk usage reduction if
  different weights are to be used on the same topology
- any given partition can be used as a seed for the algorithm rather than just
  the trivial partition where each node belongs to its own community
- initial network can contain loops is network is considered weighted
- graph is not renumbered by default in the convert program
- an optional verbose mode has been added and the program is silent by default
- some portions of the code have been c++ improved (type * -> vector<type>)
These modifications imply that any binary graph file created with the previous
version of the code is not comptabile with this version. You must therefore
regenerate all the binary files.

Version 0.1:
- initial community detection algorithm

