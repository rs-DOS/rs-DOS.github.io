As the size of the problem set increases it generally becomes harder and harder to find optimal solutions oe even feasible solutions. One way to deal with this is to introduce symmetry breaking constraints. If we look at scheduling problems, what we are essentially doing is assigning tasks to machines and esnuring that constraints are met. In constraint programming lingo the start and end time for the task would be variables and the domain of the variables would be all possible values for the variables. <br>

Let's say we have a task that has to be assigned to machine. The task must start at any time between 0 seconds and finish before 7 seconds. The task takes 2 seconds for the machine to process. The variables here are the start and end time of the task on the machine. The domain for both the variables is between 0 and 7. 
<br>
<img src="/assets/simple_task_symmetry_in_scheduling_post.png" width="200" height="200" /><br>
