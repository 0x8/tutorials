# Buffer Overflows

There are many methods for manipulating programs but perhaps one of the most
commonly used techniques in exploitation is buffer overflows. There are two 
types of memory overflows that this tutorial is interested in: **Stack-based** 
and **Heap-based**.
  
---
  
**What Is A Buffer?**:  
A buffer is a segment of memory intended to be used for temporary storage of 
data. (See: [Data Buffer](https://en.wikipedia.org/wiki/Data_buffer))

In programming buffers are quite often fixed size or have otherwise expected 
size ranges. This concept is important when we look at exploitation because of 
buffer overflow attacks.
  
---
  
**What Is A Buffer Overflow Attack**:  
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

## Additional Resources:  
[Smashing the Stack For Fun And Profit]()

[Buffer Overflows on Wikipedia](https://en.wikipedia.org/wiki/Buffer_overflow)


---

> **Disclaimer**  
The information contained within this guide largely relates to the `C`
programming language but the concept itself is universal. It is also important 
to keep in mind that I am working on Manjaro Linux so the following should be
noted:  
- Example Programs are compiled in ELF format
- Not all Linux Systems are made equally
  - Default compiler options may, _and often do_, vary between distros
  - Default security mechanisms may, _and often do_, vary between distros

> **Note**  
I highly recommend starting out with a Virtual Machine running [64-bit Ubuntu](
https://www.ubuntu.com/download/desktop). In my _own opinion_ it is the most
beginner friendly version of linux for those who are not used to running Linux
Operating Systems. I plan to release a simple introductory guide in these
tutorials at some point in the future, but for now I'll refer new users to
the [bandit wargame](http://overthewire.org/wargames/bandit/) by OverTheWire.
While it might be a bit difficult, it is a great way to learn linux by example
and some other important concepts along the way.
