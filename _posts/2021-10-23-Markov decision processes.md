Markov decision process is a mathematical tool for modelling stochastic control problems. Another way to look at them is that they can be used for 
sequential decision making. Sidenote: There is a [great lecture series on YouTube](https://www.youtube.com/watch?v=2pWv7GOvuf0&list=PLqYmG7hTraZDM-OYHWgPebj2MfCFzFObQ) by David Silver which explains MDPs and how they are used in reinforcement learning.
<br>
A key assumption is that all the states follow the *Markov* property which basically states that the future is independent of the past given the present.
## Defining MDPs
An MDP is a 4-tuple *M* : 
*M* = (*S* ,*A* ,*P<sub>a</sub>* ,*R<sub>a</sub>*) <br>
* *S* is the state space
* *A* is the action space
* *P<sub>a</sub>(*s* ,*s'*)* is the probability of going from state *s* to *s'*
* *R<sub>a</sub>(*s* ,*s'*)* is the reward for going form state *s* to *s'*

Sometimes a discount factor is added to the problem definition to model the variance associated with time horizons. Basically &gamma; models how much we care
about short-term rewards (&gamma; = 0) and long term rewards (&gamma; =1)

### State space
### Action space
### Transition probability matrix
### Rewards and discount factor

## MDPs for inventory control
[Adapated from 6.246 Reinforcement Learning: Foundations and Methods — Lec3 — 1](https://web.mit.edu/6.246/www/notes/L3-notes.pdf)<br>
A common question in supply chain management is when and by how much to replenish the shelves in a store. Replenishment can be thought of as an stochatic control problem. <br>
<img src="/assets/warehouse manager.jpg" width="200" height="200" /><br>
Let's say that you are a warehouse manager looking after just one SKU (stock keeping unit).You review the inventory at the end of each month and decide how much inventory to order.
There is a cost associated with maintaining some inventory in the warehouse *h(s)*, with ordering inventory *C(a)*. Selling the items earns you an income of *f(q)*. If the demand is greater than the available inventory then the customer leaves your store to never come back i.e there are no backorders. Only constraint that we are considering now is maximum capacity of the warehouse *M*. <br>

**State space** - *s* ∈ *S* = {0, 1, ...., M} which is the number of goods we can store in our warehouse; constrained by maximum capacity of the warehouse <br>
**Action space** - For a state *s*, *a* ∈ *A(s)* = {0, 1, ..., M − s}. Action space depends on the current state as we cannot order more than the capacity of the warehouse. Basically the only action the warehouse manager can take is choosing how much to order <br>
**Reward** - *r<sub>t</sub>* = - *C(a<sub>t</sub>)* - *h(s<sub>t</sub> + a<sub>t</sub>)*  + *f([s<sub>t</sub> + a<sub>t</sub> - s<sub>t - 1</sub>]<sup>+</sup>)*  <br>
  where, *C(a<sub>t</sub>)* is the ordering cost for *a* number of units <br>
   *h(s<sub>t</sub> + a<sub>t</sub>)* is the holding cost of current inventory + ordered inventory  <br>
 *f([s<sub>t</sub> + a<sub>t</sub> - s<sub>t - 1</sub>]<sup>+</sup>)* is the income from sales <br>
 
 The goal is to find a policy (π) of re-ordering that achieves a lot of reward over the long run. 


 




