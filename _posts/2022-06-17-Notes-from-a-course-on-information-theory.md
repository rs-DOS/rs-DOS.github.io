Information is simply the data received about a fact that resolves uncertainty. 
<br>Information theory as a field was created by Claude Shannon's work, [A Mathematical Theory of Communication](https://people.math.harvard.edu/~ctm/home/text/others/shannon/entropy/entropy.pdf).
I think his treatise is the first to look information mathematically. Shannon was interested in reducing the communication losses
in transmission over phone lines. So a lot of textbooks use examples of transmission of text messages across phone lines. <br>
Information theory is also closely related to probability.<br>
Essentially more surpising the outcome of an event, the more information it carries. <br>
Information is quantified in the language of the probability: <br> Given a discrete random variable **X**, with **N** possible values x<sub>1</sub>, x<sub>2</sub>,....,x<sub>n</sub> and their associated probabilities p<sub>1</sub>, p<sub>2</sub>,....,p<sub>n</sub>, the information received when learning that choice was x<sub>i</sub>: <br>
<p align="center">
L(X<sub>i</sub>) = log<sub>2</sub>(1/p<sub>i</sub>)
</p>
<br>
The question I had was why was base 2 used for the log. Surprisingly the [Wikipedia](https://en.wikipedia.org/wiki/Units_of_information) page on this is quite useful and accessible to a noob : <br>
<p align="center">
the information that can be stored in a system is proportional to the logarithm of N possible states of that system, denoted log<sub>b</sub>N
</p>
<br>
Effectively, 1 bit is the answer to a yes/no question. *Yes* and *No* have equal probabilities 
