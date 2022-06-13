I have got some free time on my hand and operating systems sounds like an interesting thing to read up on. I picked [OPERATING SYSTEM CONCEPTS by SILBERSCHATZ, GALVIN and GAGNE](https://www.os-book.com/OS9/) because someone on Twitter recommended it.
<br>
A process is the unit of work in a modern time-sharing system. Informally, a process is a job that the computer must complete. <br>
A process is an active entity where as a program is an passive entity. A program becomes a process when an executable file is loaded into memory.<br>
A process usually includes: 
<li> program code - text section
<li> program counter  - includes the current activity
<li> process stack - contains temporary data (such as function parameters, return addresses and local variables) </li>
<li> data section - contains global variables
<li> heap - memeory dynamicall allocated during process runtime
 <br> 
<br>
####Process in memory
<br>  
  <br>
<img src="/assets/process_in_memory.PNG" width="400" height="400" /><br>
  <br>
Stack is a <b>L</b>ast <b>I</b>n <b>F</b>irst <b>O</b>ut (LIFO) data structure. Frequently used with function calls. <br>
For x86 CPU's the stack starts out at high memory addresses and grows downwards (as shown by the arrow in the figure above). When a program exceeds all of the space allocated for the stack the program crashes beacuse of stackoverflow (Now I know how stackoverflow got its name). <br>
Stacks usually allow two operations: push and pop. Push is adding items to the top of the stack and pop is removing items from the top of the stack. Inserting or removing items from the middle of the stack is not permitted. <br>
[There's a great animation by Delroy A. Brinkerhoff](https://icarus.cs.weber.edu/~dab/cs1410/textbook/4.Pointers/memory.html) <br>
Heap memory is dynamically allocated by the program. Here there is no restriction that memory can only be accessed at the top. Data can be inserted or removed from anywhere in the heap.

###Interprocess Communication
A process is independent if it cannot affect or be affected by the other processes executing in the system. 
  
    
