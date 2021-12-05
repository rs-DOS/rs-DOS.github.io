#### These are my notes as I read through the [Foundations of reinforcement learning](https://github.com/TikhonJelvis/RL-book) book by [Aswhin Rao](https://www.linkedin.com/in/ashwin2rao/) and [Tikhon Jelvis](https://www.linkedin.com/in/tikhon-jelvis/).

## Chapter 1
The first chapter basically about coding and designing programs. The book has code samples (written in python) within it with explanations of the concepts relevant to RL.
All the code used in the book is available on the GitHub page. 

## Chapter 2
Defines basic concepts of sequential decision making. <br>
Markov states - they have the property that the probabilities of state *S<sub>t + 1</sub>* only depends on the current state *S<sub>t</sub>*. This is commonly states as *"the future is independent of the past given the present"*   
Formally a *Markov process* consists of 
* a countable set of states *S* known as the state space. terminal states are a subset of the state space
* time-indexed sequence of random states *S<sub>t</sub>* with each transition satisfying the markov property
<br>
Usually it is assumed that the transition probability matrix, which defines the probability of *S<sub>t</sub>* moving to *S<sub>t + 1</sub>*, is stationary i.e it does not change with time *t*. Otherwise it would make the job of the modeler significantly more difficult. <br>
One interesting question introduced here is how do we start the markov process? Is the starting position arbitrary or does it follow some specific probability distribution.  

