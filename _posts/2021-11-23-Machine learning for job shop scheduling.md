There is an interesting paper from [Tassel et. al](https://arxiv.org/pdf/2104.03760.pdf) on using reinforcement learning for the job shop scheduling problem. The [JSSP](https://rs-dos.github.io/2021/11/08/Job-shop-scheduling-in-python.html) is an *NP-Hard* problem.
This blog is basically my notes on their paper.<br>
## Motivation for the paper
Reinforcement learning has been quite successful in recent times from [AlphaGo](https://deepmind.com/research/case-studies/alphago-the-story-so-far) to [Atari](https://deepmind.com/research/publications/2019/playing-atari-deep-reinforcement-learning). The authors are interested
in trying out this technology that is quite successful in one field and apply it to a different field. They propose three advantages of using RL for scheduling: <br>
* RL agents can offer more flexibility as compared to traditional methods
* Traditional methods like linear programming or [constraint programming](https://rs-dos.github.io/2021/11/08/Job-shop-scheduling-in-python.html) are deterministic and cannot model stochastic events such as machine failures, random processing times etc
* In industrial settings where the instances share a lot of similarity, *lifelong learning* can allow the agent to reuse what it has learnt from scheduling the past instances
* RL provides a possibility to incrementally schedule the incoming jobs as they appear in the queue by considering the impact of a schedule for known jobs on the new ones as compared to traditional methods that focus only on a given set of jobs<br>

## Background
### Markov Decision Processes (MDP)
MDPs are a 4-tuple: 
*M* = (*S* ,*A* ,*P<sub>a</sub>* ,*R<sub>a</sub>*) <br>
* *S* is the state space
* *A* is the action space
* *P<sub>a</sub>(*s* ,*s'*)* is the probability of going from state *s* to *s'*
* *R<sub>a</sub>(*s* ,*s'*)* is the reward for going form state *s* to *s'*


 
