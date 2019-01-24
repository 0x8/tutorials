# Buffer Overflows
  
There are many methods for manipulating programs but perhaps one of the most
commonly used techniques in exploitation is buffer overflows. There are two 
types of memory overflows that this tutorial is interested in: **Stack-based** 
and **Heap-based**.
  
---

## What Is A Buffer?
  
A buffer is a segment of memory intended to be used for temporary storage of 
data. (See: [Data Buffer](https://en.wikipedia.org/wiki/Data_buffer))

In programming buffers are quite often fixed size or have otherwise expected 
size ranges. This concept is important when we look at exploitation because of 
buffer overflow attacks.
  
---
  
## What Is A Buffer Overflow Attack?
  
A buffer overflow attack is a method of exploitation in which an attacker is
able to put more into a buffer than it has been allocated to hold.
  
To simply illustrate this concept, examine the following scenario:  
  
Lets say that there is a glass box containing a laptop and an empty bucket 
that is positoned under a faucet of some sort. On the outside of the box, there 
is a valve that can be turned to begin filling the bucket from the faucet.
  
Simply put, a _buffer overflow_ attack in this scenario is leaving the faucet 
turned on until the bucket can no longer contain the water and it spills into 
the box (damaging the laptop).
  
The bucket (our _buffer_) holds a set amount of water (_data_). Through 
user-input (the _valve_) the bucket can be forced to _overflow_ and affect 
things around it. Make sense?
  
  
**Bringing It Back To Software**:  
In terms of computer programs, the glass box would represent a computer 
program, the bucket would represent some buffer in that program such as a 
variable, the water represents the data that the variable would be expected to 
store, and the valve represents user input.

In reality, things are seldom ever so simple. Usually users have a large number 
of ways to interact with programs and many are often validated. 

The good news for hackers, however, is that programmers are human and _humans 
make mistakes_. Capitalizing on these mistakes allows a good hacker to do a 
large number of things ranging from crashing a program to taking over a remote 
machine entirely.

---
  
## What Do We Mean By Memory?  
Oftentimes those who have not spent a signicant amount of time programming
or trudging through Computer Sciences courses will associate Hard Drive (or
SSD) space with the term "_memory_". In these guides when _memory_ is mentioned
it actually refers to the RAM, which your program will be loaded into when it
is executing. Keep this in mind throughout these guides.
    
---

## Regions of Memory: Stack vs. Heap

In programming we typically deal with two primary regions of memory: **stack
memory** and **heap memory**. While both of these memory regions store data
and are vulnerable to overflow attacks, how they go about storing that data
and how we attack them vastly differs.

**A Brief Explanation Of Stack Memory**:  
The region of memory known as the _stack_ is responsible for holding a large
amount of important information, namely our program registers and data used
by many functions. The stack controls the **flow** of our program. This
is further explained in the [stack](stack) section of these tutorials but for
now we can think of this region as holding things such as local variables and 
registers. The size of the stack granted to programs is dependant upon the
operating system on which the program is running.


**A Brief Explanation Of Heap Memory**:  
The region of memory known as the _heap_ is responsible for holding all of the
global data and dynamically allocated memory in programs. A surefire way to know
you are dealing with something in heap memory in C is the memory allocation
functions: `malloc()`, `calloc()`, and `realloc()`. Unlike stack memory, heap
memory is globally accessible and stored is not limited by the OS but rather
it is only limited by the physical memory your machine has access to. The 
specifics of how the heap works is further explained in the [heap](heap) section
of these tutorials.


> **Note**:
The desciption provided here of stack and heap memory is very terse.

## Additional Resources:  
[Stack vs Heap Memory](https://www.gribblelab.org/CBootCamp/7_Memory_Stack_vs_Heap.html)

[Smashing the Stack For Fun And Profit](http://www-inst.eecs.berkeley.edu/~cs161/fa08/papers/stack_smashing.pdf)

[Buffer Overflows on Wikipedia](https://en.wikipedia.org/wiki/Buffer_overflow)


---

> **Disclaimer**  
The information contained within this guide largely relates to the `C`
programming language but the concept itself is universal. It is also important 
to keep in mind that I am working on Manjaro Linux so the following should be
noted:  
> - Example Programs are compiled in ELF format
> - Not all Linux Systems are made equally
>   - Default compiler options may, _and often do_, vary between distros
>   - Default security mechanisms may, _and often do_, vary between distros

> **Note**  
I highly recommend starting out with a Virtual Machine running [64-bit Ubuntu](
https://www.ubuntu.com/download/desktop). In my _own opinion_ it is the most
beginner friendly version of linux for those who are not used to running Linux
operating systems. I plan to release a simple introductory guide in these
tutorials at some point in the future, but for now I'll refer new users to
the [bandit wargame](http://overthewire.org/wargames/bandit/) by OverTheWire.
While it might be a bit difficult, it is a great way to learn linux by example
and some other important concepts along the way.
