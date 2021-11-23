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




