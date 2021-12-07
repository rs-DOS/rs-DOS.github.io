#### These are my notes as I read through the [Foundations of reinforcement learning](https://github.com/TikhonJelvis/RL-book) book by [Ashwin Rao](https://www.linkedin.com/in/ashwin2rao/) and [Tikhon Jelvis](https://www.linkedin.com/in/tikhon-jelvis/).

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
Usually it is assumed that the transition probability matrix, which defines the probability of *S<sub>t</sub>* moving to *S<sub>t + 1</sub>* , is stationary i.e it does not change with time *t* . Otherwise it would make the job of the modeler significantly more difficult. <br>
One interesting question introduced here is how do we start the markov process? Is the starting position arbitrary or does it follow some specific probability distribution? <br>

There is a very good example of modelling inventory management as an MDP. <br>
Assume that you are the owner of a bicycle shop and need to order bicycle to meet the demand. The demand is random, but it follows a Poisson distribution. There are constraints on the storage space so you can store at most *C* . Any order you place arrives after 36 hours i.e lead time of 36 hours. <br>
α is the on-hand inventory and β is the on-order inventory. 
The state *S<sub>t</sub>* , is defined as the inventory position at 6 pm each day. You close the store at 6 and just before closing review the inventory  and order additional units if required. The policy that you follow is to order *C* - (α + β) if your inventory position is less than the capacity constraint. If the inventory position is greater than the store capacity, no order will be placed. <br>
This is modelled as a markov process with the sales of the bicyles being the random variables. A transition function is defined that maps the current inventory position to a a future inventory position. The sales are random and sampled from a poisson distribution. To illustrate the transition probabilities they have a very nice illustration.<br>
<p align="center">
  <img src="/assets/image.png" width="500" height="500" />
</p>

Assuming that the storage capacity is 2 and we begin the markov process with an empty store, we would immediately order 2 bicycles. Then there is 37% probability that the next state is with 2 bicycles in the store. Then there is a 37% probability that no sales are recorded on that day resulting in a self-loop. 26% probability that you sell of the 2 bicycles and end up in the start state with no bicycles in the store and none on order.   <br>
Markov reward processes include a notion of a numerical reward to the markov process. The rewards are given for each state transition. They are ransom and need to be specified with the probability distribution of these rewards. A discount factor is applied to future rewards. Introducing the concept of markov reward process to the inventory example we get a holding cost for all the bicycles that remain in the store overnight and a stockout cost that is incurred whenver demand is missed due to insufficient inventory. Usually significantly more weight is given to the stockout cost than the holding cost.<br>

