Circuit constraints are used in constraint programming to force an *Hamiltonian* circuit on a successor array. They are used commonly in industrial applications such as the [vehicle routing problem (VRP)](https://en.wikipedia.org/wiki/Vehicle_routing_problem) and
the [travelling salesman problem (TSP)](https://en.wikipedia.org/wiki/Travelling_salesman_problem). There's a great GIF on [STEM lounge](https://stemlounge.com/animated-algorithms-for-the-traveling-salesman-problem/) for visualizing the TSP.

<img src="/assets/tsp_nearest_neighbor.gif" width="1000" height="300" />
<br>

From [Francis and Stuckey](https://people.eng.unimelb.edu.au/pstuckey/circuit/):<br>
>If we consider the graph *G* with a vertex u<sub>i</sub> for each variable v<sub>i</sub> and edges (u<sub>i</sub>, u<sub>j</sub> ) where *j* is in the domain of v<sub>i</sub>, a solution to the circuit constraint is a Hamiltonian cycle of *G* <br>
They are used to model changeovers in the JSSP where there must be at least *x* minutes between two tasks being processed on the same machine. <br>

## Changeovers in the job shop scheduling problem
Changeovers are commonly used in manufacturing where between two successive tasks on a machine some time has to be given for cleaning the machinery, swap parts to manufacture different kind of products etc. <br>
Changeover constraints are different from precedence constraints in that they have to be applied to machines between tasks whereas precedence constraints are applied within jobs i.e between every task in a single job. 
Modelling changeover constraints in OR-Tools requires the [AddCircuit](https://developers.google.com/optimization/reference/python/sat/python/cp_model#addcircuit) constraint.

It's always useful to read the documentation for any code that you are using. Looking at the arguments for the AddCircuit constraint, we see that the input is a list of arcs. An arc is a tuple (source_node, destination_node, literal). The arc is selected if the literal is true.  <br>

Looking at an example of [transition times between tasks](https://github.com/google/or-tools/blob/stable/examples/python/jobshop_ft06_distance_sat.py) we see that there are 2 *for* loops for implementing this. The first *for* loop loops over all the tasks assigned to a machine. First a initial arc is created from a dummy node to a task.  

```python
arcs = []
for i in range(len(job_intervals)):
    # Initial arc from the dummy node (0) to a task.
    #This creates an arc from the dummy node (0)(source node) to the next task 
    # (destination node) assigned to the machine  
    start_lit = model.NewBoolVar('%i is first job' % i)
    arcs.append([0,i +1 , start_lit])

    #Final arc from an arc to the dummy node
    #This creates an arc from the last task to the dummy node
    end_lit =  model.NewBoolVar('%i is last job' % i)
    arcs.append([i + 1 , 0 , end_lit ])

    #We loop again though all the tasks assigned to the machine
    for j in range(len(job_intervals)):
        if i == j:
            continue

        #This boolean variable indicates which tasks follows which task
        lit =  model.NewBoolVar('%i follows %i' % (j, i))
        arcs.append([i + 1, j + 1, lit])
        
        #Add condition that if same resources are to be used for the task then block 300 minutes for cleaning
        if machine_resources[i] != machine_resources[j]:
          transition_time = 300
        else:
          transition_time = 10
      
```
