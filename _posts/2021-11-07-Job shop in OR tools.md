# Follow along tutorial of the scheduling problem in OR Tools
https://developers.google.com/optimization/scheduling/job_shop


```python
import collections

# Import Python wrapper for or-tools CP-SAT solver.
from ortools.sat.python import cp_model


def MinimalJobshopSat():
    """Minimal jobshop problem."""
    # Create the model.
    model = cp_model.CpModel()

    jobs_data = [  # task = (machine_id, processing_time).
        [(0, 3), (1, 2), (2, 2)],  # Job0
        [(0, 2), (2, 1), (1, 4)],  # Job1
        [(1, 4), (2, 3)]  # Job2
    ]

    machines_count = 1 + max(task[0] for job in jobs_data for task in job)
    all_machines = range(machines_count)

    # Computes horizon dynamically as the sum of all durations.
    horizon = sum(task[1] for job in jobs_data for task in job)

    # Named tuple to store information about created variables.
    task_type = collections.namedtuple('task_type', 'start end interval')
    # Named tuple to manipulate solution information.
    assigned_task_type = collections.namedtuple('assigned_task_type',
                                                'start job index duration')

    # Creates job intervals and add to the corresponding machine lists.
    all_tasks = {}
    machine_to_intervals = collections.defaultdict(list)

    for job_id, job in enumerate(jobs_data):
        for task_id, task in enumerate(job):
            machine = task[0]
            duration = task[1]
            suffix = '_%i_%i' % (job_id, task_id)
            start_var = model.NewIntVar(0, horizon, 'start' + suffix)
            end_var = model.NewIntVar(0, horizon, 'end' + suffix)
            interval_var = model.NewIntervalVar(start_var, duration, end_var,
                                                'interval' + suffix)
            all_tasks[job_id, task_id] = task_type(start=start_var,
                                                   end=end_var,
                                                   interval=interval_var)
            machine_to_intervals[machine].append(interval_var)

    # Create and add disjunctive constraints.
    for machine in all_machines:
        model.AddNoOverlap(machine_to_intervals[machine])

    # Precedences inside a job.
    for job_id, job in enumerate(jobs_data):
        for task_id in range(len(job) - 1):
            model.Add(all_tasks[job_id, task_id +
                                1].start >= all_tasks[job_id, task_id].end)

    # Makespan objective.
    obj_var = model.NewIntVar(0, horizon, 'makespan')
    model.AddMaxEquality(obj_var, [
        all_tasks[job_id, len(job) - 1].end
        for job_id, job in enumerate(jobs_data)
    ])
    model.Minimize(obj_var)

    # Solve model.
    solver = cp_model.CpSolver()
    status = solver.Solve(model)

    if status == cp_model.OPTIMAL:
        # Create one list of assigned tasks per machine.
        assigned_jobs = collections.defaultdict(list)
        for job_id, job in enumerate(jobs_data):
            for task_id, task in enumerate(job):
                machine = task[0]
                assigned_jobs[machine].append(
                    assigned_task_type(start=solver.Value(
                        all_tasks[job_id, task_id].start),
                                       job=job_id,
                                       index=task_id,
                                       duration=task[1]))

        # Create per machine output lines.
        output = ''
        for machine in all_machines:
            # Sort by starting time.
            assigned_jobs[machine].sort()
            sol_line_tasks = 'Machine ' + str(machine) + ': '
            sol_line = '           '

            for assigned_task in assigned_jobs[machine]:
                name = 'job_%i_%i' % (assigned_task.job, assigned_task.index)
                # Add spaces to output to align columns.
                sol_line_tasks += '%-10s' % name

                start = assigned_task.start
                duration = assigned_task.duration
                sol_tmp = '[%i,%i]' % (start, start + duration)
                # Add spaces to output to align columns.
                sol_line += '%-10s' % sol_tmp

            sol_line += '\n'
            sol_line_tasks += '\n'
            output += sol_line_tasks
            output += sol_line

        # Finally print the solution found.
        print('Optimal Schedule Length: %i' % solver.ObjectiveValue())
        print(output)


MinimalJobshopSat()
```

    Optimal Schedule Length: 11
    Machine 0: job_0_0   job_1_0   
               [0,3]     [3,5]     
    Machine 1: job_2_0   job_0_1   job_1_2   
               [0,4]     [4,6]     [7,11]    
    Machine 2: job_1_1   job_0_2   job_2_1   
               [5,6]     [6,8]     [8,11]    
    
    


```python
machines_count = 6
jobs_count = 6
all_machines = range(0, machines_count)
all_jobs = range(0, jobs_count)


durations = [[1, 3, 6, 7, 3, 6], [8, 5, 10, 10, 10, 4], [5, 4, 8, 9, 1, 7],
                 [5, 5, 5, 3, 8, 9], [9, 3, 5, 4, 3, 1], [3, 3, 9, 10, 4, 1]]

machines = [[2, 0, 1, 3, 5, 4], [1, 2, 4, 5, 0, 3], [2, 3, 5, 0, 1, 4],
                [1, 0, 2, 3, 4, 5], [2, 1, 4, 5, 0, 3], [1, 3, 5, 0, 4, 2]]

horizon = 150

```


```python
print(machines[0])
print(all_jobs[0])
print(all_machines[0])
```

    [2, 0, 1, 3, 5, 4]
    0
    0
    

Jobs are defined differently in this example. In the previous examples that I have seen the jobs are defined as a pair of the machine_id and processing time. Example:
[#Granulation(GRM_1,500),
#Compression	(COM_1,250),
#Coating	(COA_1,500),
#Packing	(PAC_1,100)]

In this example we have got two lists. The first is for the durations for all the jobs and the second list for the machines on which these jobs will run. </br>

One constraint is that no jobs should overlap on an machine i.e no machine can do two different things simultaneously. This disjunctive constraint is implemented by using the *AddNoOverlap* method of OR-Tools. 

In the previously encountered examples, the constraint is simple enough to add. First all machines are assigned job intervals and then *AddNoOverlap* is used to ensure that they do not sit on top of each other.


```python
#Create disjunctive constraints
for i in all_machines:
    print('for i in all_machines:  %i'%i)
    job_intervals = []
    job_indices = []
    job_starts = []
    job_ends = []
    for j in all_jobs:
        print('for j in all_jobs: %i'%j)
        for k in all_machines:
            print('for k in all_machines: %i'%k)
            if machines[j][k] == i:
                print("Condition True")
            else:
                print("Condition false")
                
                    
```


```python
#!/usr/bin/env python3
# Copyright 2010-2021 Google LLC
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
"""This model implements a variation of the ft06 jobshop.

A jobshop is a standard scheduling problem when you must sequence a
series of tasks on a set of machines. Each job contains one task per
machine. The order of execution and the length of each job on each
machine is task dependent.

The objective is to minimize the maximum completion time of all
jobs. This is called the makespan.

This variation introduces a minimum distance between all the jobs on each
machine.
"""

import collections

from ortools.sat.python import cp_model


def distance_between_jobs(x, y):
    """Returns the distance between tasks of job x and tasks of job y."""
    return abs(x - y)


def jobshop_ft06_distance():
    """Solves the ft06 jobshop with distances between tasks."""
    # Creates the model.
    model = cp_model.CpModel()

    machines_count = 6
    jobs_count = 2
    all_machines = range(0, machines_count)
    all_jobs = range(0, jobs_count)

    durations = [[1, 3, 6, 7, 3, 6], [8, 5, 10, 10, 10, 4]]

    machines = [[2, 0, 1, 3, 5, 4], [1, 2, 4, 5, 0, 3]]

    # Computes horizon statically.
    horizon = 150

    task_type = collections.namedtuple('task_type', 'start end interval')

    # Creates jobs.
    all_tasks = {}
    for i in all_jobs:
        for j in all_machines:
            start_var = model.NewIntVar(0, horizon, 'start_%i_%i' % (i, j))
            duration = durations[i][j]
            end_var = model.NewIntVar(0, horizon, 'end_%i_%i' % (i, j))
            interval_var = model.NewIntervalVar(start_var, duration, end_var,
                                                'interval_%i_%i' % (i, j))
            all_tasks[(i, j)] = task_type(start=start_var,
                                          end=end_var,
                                          interval=interval_var)

    # Create disjuctive constraints.
    for i in all_machines:
        job_intervals = []
        job_indices = []
        job_starts = []
        job_ends = []
        for j in all_jobs:
            for k in all_machines:
                if machines[j][k] == i:
                    job_intervals.append(all_tasks[(j, k)].interval)
                    job_indices.append(j)
                    job_starts.append(all_tasks[(j, k)].start)
                    job_ends.append(all_tasks[(j, k)].end)
        model.AddNoOverlap(job_intervals)

        arcs = []
        for j1 in range(len(job_intervals)):
            #print(j1)
            # Initial arc from the dummy node (0) to a task.
            start_lit = model.NewBoolVar('%i is first job' % j1)
            #print(start_lit)
            arcs.append([0, j1 + 1, start_lit])
            print(arcs)
            #a = len(arcs)
            #print("Number of rows is :%i " %a)
            #print(len(arcs[0]))
            # Final arc from an arc to the dummy node.
            arcs.append([j1 + 1, 0, model.NewBoolVar('%i is last job' % j1)])

            for j2 in range(len(job_intervals)):
                if j1 == j2:
                    continue

                lit = model.NewBoolVar('%i follows %i' % (j2, j1))
                arcs.append([j1 + 1, j2 + 1, lit])

                # We add the reified precedence to link the literal with the
                # times of the two tasks.
                min_distance = distance_between_jobs(j1, j2)
                model.Add(job_starts[j2] >= job_ends[j1] +
                          min_distance).OnlyEnforceIf(lit)

        model.AddCircuit(arcs)

    # Precedences inside a job.
    for i in all_jobs:
        for j in range(0, machines_count - 1):
            model.Add(all_tasks[(i, j + 1)].start >= all_tasks[(i, j)].end)

    # Makespan objective.
    obj_var = model.NewIntVar(0, horizon, 'makespan')
    model.AddMaxEquality(
        obj_var, [all_tasks[(i, machines_count - 1)].end for i in all_jobs])
    model.Minimize(obj_var)

    # Solve model.
    solver = cp_model.CpSolver()
    status = solver.Solve(model)

    # Output solution.
    if status == cp_model.OPTIMAL:
        print('Optimal makespan: %i' % solver.ObjectiveValue())



```


```python
jobshop_ft06_distance()
```

    [[0, 1, 0 is first job(0..1)]]
    [[0, 1, 0 is first job(0..1)], [1, 0, 0 is last job(0..1)], [1, 2, 1 follows 0(0..1)], [0, 2, 1 is first job(0..1)]]
    [[0, 1, 0 is first job(0..1)]]
    [[0, 1, 0 is first job(0..1)], [1, 0, 0 is last job(0..1)], [1, 2, 1 follows 0(0..1)], [0, 2, 1 is first job(0..1)]]
    [[0, 1, 0 is first job(0..1)]]
    [[0, 1, 0 is first job(0..1)], [1, 0, 0 is last job(0..1)], [1, 2, 1 follows 0(0..1)], [0, 2, 1 is first job(0..1)]]
    [[0, 1, 0 is first job(0..1)]]
    [[0, 1, 0 is first job(0..1)], [1, 0, 0 is last job(0..1)], [1, 2, 1 follows 0(0..1)], [0, 2, 1 is first job(0..1)]]
    [[0, 1, 0 is first job(0..1)]]
    [[0, 1, 0 is first job(0..1)], [1, 0, 0 is last job(0..1)], [1, 2, 1 follows 0(0..1)], [0, 2, 1 is first job(0..1)]]
    [[0, 1, 0 is first job(0..1)]]
    [[0, 1, 0 is first job(0..1)], [1, 0, 0 is last job(0..1)], [1, 2, 1 follows 0(0..1)], [0, 2, 1 is first job(0..1)]]
    Optimal makespan: 47
    


```python
#!/usr/bin/env python3
# Copyright 2010-2021 Google LLC
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
"""This model implements a variation of the ft06 jobshop.

A jobshop is a standard scheduling problem when you must sequence a
series of tasks on a set of machines. Each job contains one task per
machine. The order of execution and the length of each job on each
machine is task dependent.

The objective is to minimize the maximum completion time of all
jobs. This is called the makespan.

This variation introduces a minimum distance between all the jobs on each
machine.
"""

import collections

from ortools.sat.python import cp_model


def distance_between_jobs(x, y):
    """Returns the distance between tasks of job x and tasks of job y."""
    return abs(x - y)


def jobshop_ft06_distance():
    """Solves the ft06 jobshop with distances between tasks."""
    # Creates the model.
    model = cp_model.CpModel()

    machines_count = 6
    jobs_count = 2
    all_machines = range(0, machines_count)
    all_jobs = range(0, jobs_count)

    durations = [[1, 3, 6, 7, 3, 6], [8, 5, 10, 10, 10, 4]]

    machines = [[2, 0, 1, 3, 5, 4], [1, 2, 4, 5, 0, 3]]

    # Computes horizon statically.
    horizon = 150

    task_type = collections.namedtuple('task_type', 'start end interval')

    # Creates jobs.
    all_tasks = {}
    for i in all_jobs:
        for j in all_machines:
            start_var = model.NewIntVar(0, horizon, 'start_%i_%i' % (i, j))
            duration = durations[i][j]
            end_var = model.NewIntVar(0, horizon, 'end_%i_%i' % (i, j))
            interval_var = model.NewIntervalVar(start_var, duration, end_var,
                                                'interval_%i_%i' % (i, j))
            all_tasks[(i, j)] = task_type(start=start_var,
                                          end=end_var,
                                          interval=interval_var)

    # Create disjuctive constraints.
    for i in all_machines:
        job_intervals = []
        job_indices = []
        job_starts = []
        job_ends = []
        for j in all_jobs:
            for k in all_machines:
                if machines[j][k] == i:
                    job_intervals.append(all_tasks[(j, k)].interval)
                    job_indices.append(j)
                    job_starts.append(all_tasks[(j, k)].start)
                    job_ends.append(all_tasks[(j, k)].end)
        model.AddNoOverlap(job_intervals)

        arcs = []
        for j1 in range(len(job_intervals)):
            #print(j1)
            # Initial arc from the dummy node (0) to a task.
            start_lit = model.NewBoolVar('%i is first job' % j1)
            #print(start_lit)
            arcs.append([0, j1 + 1, start_lit])
            print(arcs)
            #a = len(arcs)
            #print("Number of rows is :%i " %a)
            #print(len(arcs[0]))
            # Final arc from an arc to the dummy node.
            arcs.append([j1 + 1, 0, model.NewBoolVar('%i is last job' % j1)])

            for j2 in range(len(job_intervals)):
                if j1 == j2:
                    continue

                lit = model.NewBoolVar('%i follows %i' % (j2, j1))
                arcs.append([j1 + 1, j2 + 1, lit])

                # We add the reified precedence to link the literal with the
                # times of the two tasks.
                min_distance = distance_between_jobs(j1, j2)
                model.Add(job_starts[j2] >= job_ends[j1] +
                          min_distance).OnlyEnforceIf(lit)

        model.AddCircuit(arcs)

    # Precedences inside a job.
    for i in all_jobs:
        for j in range(0, machines_count - 1):
            model.Add(all_tasks[(i, j + 1)].start >= all_tasks[(i, j)].end)

    # Makespan objective.
    obj_var = model.NewIntVar(0, horizon, 'makespan')
    model.AddMaxEquality(
        obj_var, [all_tasks[(i, machines_count - 1)].end for i in all_jobs])
    model.Minimize(obj_var)

    # Solve model.
    solver = cp_model.CpSolver()
    status = solver.Solve(model)

    # Output solution.
    if status == cp_model.OPTIMAL:
        print('Optimal makespan: %i' % solver.ObjectiveValue())



```


```python
jobshop_ft06_distance()
```

    [[0, 1, 0 is first job(0..1)]]
    [[0, 1, 0 is first job(0..1)], [1, 0, 0 is last job(0..1)], [1, 2, 1 follows 0(0..1)], [0, 2, 1 is first job(0..1)]]
    [[0, 1, 0 is first job(0..1)]]
    [[0, 1, 0 is first job(0..1)], [1, 0, 0 is last job(0..1)], [1, 2, 1 follows 0(0..1)], [0, 2, 1 is first job(0..1)]]
    [[0, 1, 0 is first job(0..1)]]
    [[0, 1, 0 is first job(0..1)], [1, 0, 0 is last job(0..1)], [1, 2, 1 follows 0(0..1)], [0, 2, 1 is first job(0..1)]]
    [[0, 1, 0 is first job(0..1)]]
    [[0, 1, 0 is first job(0..1)], [1, 0, 0 is last job(0..1)], [1, 2, 1 follows 0(0..1)], [0, 2, 1 is first job(0..1)]]
    [[0, 1, 0 is first job(0..1)]]
    [[0, 1, 0 is first job(0..1)], [1, 0, 0 is last job(0..1)], [1, 2, 1 follows 0(0..1)], [0, 2, 1 is first job(0..1)]]
    [[0, 1, 0 is first job(0..1)]]
    [[0, 1, 0 is first job(0..1)], [1, 0, 0 is last job(0..1)], [1, 2, 1 follows 0(0..1)], [0, 2, 1 is first job(0..1)]]
    Optimal makespan: 47
    
