# Communicating Sequential Processes

## 1. Introduction

Within programs, assignment is well-understood, input and output are not. 

There are three constructs which are most widely used to structure programs:

* repetive (while loop)
* alternative (if/else/then)
* sequential (lines of code, semicolons)

There are other constructs which are not as universally agreed upon:

* subroutines
* procedures
* entries
* coroutines
* classes
* processes and monitors
* classes
* forms
* actors

Parallelism is usually hidden from the programmer. In multicore CPUs, there must be a means of dealing with synchronizing communication between cores during the execution of a program. There are a number of adequate ways to this:

* semaphores
* conditional critical regions
* monitors and queues
* path expressions

They work, but there is no criterion for choosing between them. This paper will attempt to find a single solution to all of these problems by:

1. adopting Dijkstra's guarded commands as sequential control structures
2. a parallel command that specifies execution of constituent processes, which start at the same time. The parallel command does not end until each constituent process has finished. No global variables.
3. input and output commands are specified for communication between concurrent processes
4. One process can name another process to pass its output to that process's input. The second process must wait for the first process to finish before executing.
