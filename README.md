mapreduce_facility
==================

Design and Implementation Guidelines

==
General

If you make any assumptions or simplifications, please state them clearly in your report.
You are welcome to use Java's RMI, or your own RMI framework from Project 2. Both are allowed.
You are not allowed to store any data in AFS to share it between different components in your system. If the purpose of its use is not to replace some functionality of your system, you are allowed to use AFS as an exception. For example, for DFS bootstraping, it is acceptable to have original files stored in AFS so that all the data nodes can read them and create replicas as needed. Once the file system is set up, the data in AFS must no longer be used.

==
Report

As mentioned in the project handout, avoid describing your design in terms of classes and methods. We are interested in what components work together to accomplish the goals in your system, and how they do it. E.g., we would like to know how the servers and clients are deployed (where, how many etc.), rather than what classes and methods realize the server functionalities. The latter is best placed in comments in your code.

==
MapReduce Computation Model

It is not required that your MapReduce framework explicitly support global sorting of final results. Namely, it suffices to be able to have results produced by Reducers locally sorted. If you would like to support global sorting, you are welcome to make intermediate key-value pairs produced by Mappers available to Reducers running on different machines; this can be done by, for example, writing them in your distributed file system instead of the local file system of Mapper nodes, or directly transferring them to Reducers over the network if you wish.

==
Scheduling

Scheduler should try to enforce work conservation, and launch as many Mappers and Reducers as allowed by the task's computation model. Specifically, it should exploit multiple CPU cores on each compute node. On Andrew machines, reading /proc/cpuinfo will give you information about the number of cores.
Scheduler should have a reasonable policy considering the cost of data accesses (local or remote) and overall task throughput. For example, it is undesirable to leave some available cores idle simply because Mappers on them could access input data only remotely.

==
Distributed File System

You are allowed to assume that input files for Mappers are text files and Mappers are processing input line-by-line. You can then split them into replicas at line breaks, so that individual input entries processed by Mappers do not span multiple replicas. This will also simplify determining the cost of a particular scheduling decision.
Upon data node failures, it is recommended (although not required) that your file system create new replicas for those on the failing nodes, to keep the replication factor enforced. On bootstrapping, the replication factor must be enforced.
