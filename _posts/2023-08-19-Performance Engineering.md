As I try and learn more about performance engineering of computer systems, I went through the youtube video from [MIT on performance engineering](https://www.youtube.com/watch?v=o7h_sYMk_oc). <br>
The lecture takes us through a little bit of history of the compute resources available and what the consequences the end of moore's law had on performance. Performance is currency for software and like money, more money gives better things. To better drive this point home, they take an example of square matrix multiply for n = 4096. <br>

<br> **(TO BE UPDATED)** <br>

### Part one with python 

The lecture starts out with python to compute the matrix multiply. They have standard hardware on which they test all their implementations but I ran the code on i7 laptop with about 32 GB of RAM. It took me a minute short of **5 hours** to run. They ran it on an Haswell (Intel Xeon E5-2666 v3) processor with 60GB DRAM and it took them about **5.8 hours**. So already we can see the difference due to 4 years of processor development (my laptop was purchased in 2022). 
<br>


```python
import sys, random
from time import *

n = 4096
A =  [[random.random()
      for row in range(n)]
      for col in range(n)]

B = [[random.random()
      for row in range(n)]
      for col in range(n)]
C = [[0 for row in range(n)]
      for col in range(n)]

start = time()
for i in range(n):
    for j in range(n):
        for k in range(n):
            C[i][j] = A[i][k] * B [k][j]
end = time()

print('%0.6f'%(end - start))

```

### Jumping to java
Python as a language is not known for it's speed. It's claim to fame is allowing is in allowing people to create useful software without having to learn programming in detail. <br>
<br>Enter Java. </br>

The same code in Java runs in about 46 minutes which is about **8.8 X faster than python**. This is major speedup and improving a major codebase by 8.8X would normally warrant a promotion. But C exists.

### Checking in C
C runs the matrix multiply code in about 19 minutes which is **2 X faster than java** and **18 X faster than python**. So why is C that much faster than Python. To answer this one must first understand how Python and C work. <br> 
Python is an interpreted language while C compiles directly into machine code. As the processors only understands machine code and it takes time to convert the human input in python to processor understandable machine code, it is much slower than C. Java is somewhere in-between these two languages in that converting it to machine code is much faster than python but still slower than C.
<br>
<br>
Now the next steps in optimization are not language based changes but changes in the way the code is written and in the way it is executed by the processor. 

### Loop the loop
<img src="/assets/looptheloop.gif" width="500" height="250" /> 
Changing the order of the loops in the C program has a significant impact on the runtime of the code. This is done while preserving the correctness of the code. Now why would changing the order of loops have such a major impact on the runtime? <br>
To answer this question we must understand how the matrices are represented in memory on the hardware. <br>

#### Cache me if you can



### Thoughts at the end
To an extent the way the lecture is laid out, it is a journey through the layers of abstraction for a computer. We start of with python which is widely available language used by people without significant training in computer science or engineering. It is very easy to write for a noob but the performance hit is massive. Then we get to Java which is a more serious programming language as compared to Python and we shave off 10 factors of performance hit here. Then we reach C. All performant systems are written in C. So doing this matrix multiply in C gives us significant speedup over both Java and Python. 

<br>
There is a quote from Harold Abelson that comes to my mind about the computer systems. At the end of the day programs are written by human programmers and as the cost of the processor has come down the cost of the programmer has gone up. So as a trade-off between the cost of the programmer and computer it doesn't make a lot of sense economically to go ahead and optimise the program further. <br>

This does raise an interesting question. How do the designers of processors feel about modern computers just using a miniscule peak of the maximum power that the processor offers. Car designers also have a similar issue in that a Jeep Compass can be driven easily at speeds north of 120 kmph but it spends quite a lot of time in the range of 0 - 15 kmph in the cities and 80 - 110 kmph on the highways. But the scale of the time away from peak performance for cars is much smaller than that for the silicon gods. 

