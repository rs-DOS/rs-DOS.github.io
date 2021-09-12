#Grocery shopping and dynamic programming
This is an example plucked from the ML book [Patterns, Predictions and Actions](<https://mlstory.org/>) by Moritz Hardt and Benjamin Recht 

Bob really likes to eat cheerios for breakfast
This would inventory for any SKU we sell. Let state Xt denote the inventory position for that SKU at time t. The action Ut is the inventory replenishment for that SKU at time t and Wt denotes the inventory consumption for that SKU.

The inventory position at any time t is
						X_t = X_t-_1_ + U_t_-_1_ - W_t_-_1_
The inventory position today is whatever inventory was available yesterday plus whatever inventory arrived yesterday as an replenishment minus the inventory sold off yesterday. 

This is an example of the optimal control problem where the control action is the replenishment rate. The uncertainty is in how much inventory can be sold off and ,in the real world, how much replenishment will arrive on time. 


