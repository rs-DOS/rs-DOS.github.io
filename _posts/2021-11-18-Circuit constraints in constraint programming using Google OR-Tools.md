## Use of circuit constraints 
Circuit constraints are used in constraint programming to force an *Hamiltonian* circuit on a successor array. They are used commonly in industrial applications such as the [vehicle routing problem (VRP)](https://en.wikipedia.org/wiki/Vehicle_routing_problem) and
the [travelling salesman problem (TSP)](https://en.wikipedia.org/wiki/Travelling_salesman_problem). There's a great GIF on [STEM lounge](https://stemlounge.com/animated-algorithms-for-the-traveling-salesman-problem/) for visualizing the TSP.

<img src="/assets/tsp_nearest_neighbor.gif" width="1000" height="600" />
<br>

From [Francis and Stuckey](https://people.eng.unimelb.edu.au/pstuckey/circuit/):<br>
>If we consider the graph *G* with a vertex u<sub>i</sub> for each variable v<sub>i</sub> and edges (u<sub>i</sub>, u<sub>j</sub> ) where *j* is in the domain of v<sub>i</sub>, a solution to the circuit constraint is a Hamiltonian cycle of *G* <br>

Circuit constraints make sense when modelling routing where the job of an agent is to traverse all the vertices in a graph. In simpler terms, a roomba must start from its station and return to back to it's station after vacuuming the room.
The start and the end points remain the same. <br>
<img src="/assets/cat_roomba_vrp.gif" width="500" height="350" />
<br>

This is a very common problem for delivery companies where the truck departs the warehouse full of packages and returns to the warehouse after delivering goods to customers all over the city. The driver doesn'y want to visit the same customer twice (vast of time and fuel) and he must return to the warehouse to load more packages for the next round.
A circuit constraint does make sense in *routing* problems. <br>
At the first glance, circuit constraints doen't make sense in job shop scheduling problems where the schedule is directed acyclic graph. 
They are used to model changeovers in the JSSP where there must be at least *x* minutes between two jobs being processed on the same machine. <br>

## Changeovers in the job shop scheduling problem

Changeovers are scheduled on every machine  This is to allow for cleaning the machinery, swap parts to manufacture different kind of products etc.<br>

Changeover constraints are different from precedence constraints in that they have to be applied to machines between jobs whereas precedence constraints are applied within jobs i.e between every task in a single job. 
Modelling changeover constraints in OR-Tools requires the [AddCircuit](https://developers.google.com/optimization/reference/python/sat/python/cp_model#addcircuit) constraint.
