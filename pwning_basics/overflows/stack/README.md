# Stack Based Buffer Overflows

So now that you have an idea of what buffer flows are and a basic idea of how
they work, let's talk about **stack-based** buffer overflows. As briefly 
explained in [buffer overflows](https:\\temporary-link), the stack is a region
of memory used by the functions within our program for temporarily storing 
variables related to that function. In addition to these variables, the stack 
holds the registers (special positions in memory) that dictate how our program
functions.

Previously, I used the example of a bucket under a faucet to explain the 
concept of a buffer overflow. In _stack-based_ buffer overflows, our "bucket"
is typically a local function variable such as a string that is held on the
stack.

## Typical Stack Layout And Functions

The stack is responsible for laying out "frames" for our program's functions.
Each of these functions has its own "frame" within which it will operate. In
order to keep track of these frames, processors use special memory segments
called "registers" to keep track of the start and end of the current frame,
track the last instruction address before starting a function and entering a new
frame, and the other important information. When the program runs, it saves the
parameters it is called with and the other information. When we encounter a
function, the stack is "grown" by increasing the distance between the "Stack
Pointer" (ESP, RSP for x86 and x86_64) and the "Base Pointer" (EBP, RBP for
x86 and x86_64). The two pointers start out pointing to the same address then
in order to "grow" the stack, the program subtracts some number of bytes from
the "Stack Pointer" to create our "Stack Frame". At the "top" of this frame
are our local variables, followed by typically the "stack canary" (more on this
later), then by our saved Instruction Pointer, then Saved Base Pointer, then
finally the functions parameters.

If we were to crudely visualize the stack with frames, it would look something
like this:  

```
             [ Previous Stack Frame        ]  
HIGH ADDRESS [ Local Variable(s)           ] <--- Start function foo()  
             [ More local variables        ]  
             [ ...                         ]  
             [ Stack Canary                ] <--- Important for later 
             [ Saved Instruction Pointer   ]  
LOW ADDRESS  [ Saved Base Pointer          ]  
             [ Parameters                  ] <--- Parameters of function foo()
```

> **Note** This layout is **extremely general** but should be enough to at
least visualize for the examples contained here. For a more detailed
understanding of call frames, stack memory, and more, I would **highly**
advise going through [Saumil Shah's How Functions Work](https://www.slideshare.
net/saumilshah/how-functions-work-7776073) on SlideShare. Additionally
nothing beats playing with these concepts for yourself in a debugger.

---

## A Basic Example

As a basic example lets examine this super basic C program (ignoring headers for now):

```C
int main (int argc, char** argv) {

    char name[100];
    printf("Please tell me your name:\n");
    scanf("%s", name);
    printf("Hello %s!\n", name);
    return 0;

}
```

In the example above, our program creates an `array` called `name` with space for `100` characters. This
variable creates a space for up to 100 characters to be stored. This is an example of a **Stack-based**
buffer.



In the line with `scanf`, the user is prompted to enter a name of _up to_ 100 characters. This information
is then stored in the _buffer_ `name`. Simple enough right?

To illustrate this further, let's compile and run the above program (using `gcc` on Manjaro Linux):

```bash
user@manjaro:~$ gcc -m32 sample.c -fno-stack-protector -fno-pie -o sample
user@manjaro:~$ file sample
sample: ELF 32-bit LSB pie executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=37f7974752cf0416b7e0cd7ec6f664acd34a9149, not stripped
```

> **Compilation Notes**:
In the above example, the flags `-m32`, `-fno-stack-protector`, and `-fno-pie` 
are used. Their meanings are as follows:  
> `-m32` : Compile the program as 32-bit
> `-fno-stack-protector` : Disable stack smashing protection
> `-fno-pie` : Ensure the program is not compiled as a ["Position Independant Executable"](https://en.wikipedia.org/wiki/Position-independent_code)
> The reason for these flags is to make this example easier to digest as 
64-bit builds, stack protections, and position independant execution complicates 
the topic more than needed at this point in time.


We can verify that the program behaves as expected by simply running our newly 
created binary:

```bash
user@manjaro:~$ ./sample
Please tell me your name:
Ian
Hello Ian!
user@manjaro:~$
user@manjaro:~$ ./sample
Please tell me your name:
1234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890
Hello 1234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890!
```

We can see that as expected the program asks us for our name, takes up to 100 
characters and prints them out again. 

