Downloaded from https://downloads.sourceforge.net/project/louvain/louvain_v0.2.tgz?r=https%3A%2F%2Fsourceforge.net%2Fprojects%2Flouvain%2Ffiles%2F&ts=1513173087&use_mirror=ayera

I included #include <unistd.h> in main_community.cpp to get rid of error saying getpid() was not declared in the scope, as per https://stackoverflow.com/questions/17836865/getting-errors-while-compiling (issue caused due to compiler version difference)

Remaining changes are just in main_community.cpp, modified to accept a seed argument
