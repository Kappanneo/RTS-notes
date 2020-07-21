# RTS

Real Time Systems notes.

---

_Task model_

In a task set on $n$ tasks, each task $i$ (where $i\leq n$) is defined (statically) as: $\tau_i := \{C_i,T_i,D_i,O_i\}$, where:
- $C_i$ is the worst-case execution time (WCET);
- $T_i$ is the period;
- $D_i$ is the deadline;
- $O_i$ is the offset.

For each task, its $k$-th instance is defined (at run-time) as: $\tau_{i,k} := \{a_{i,k},s_{i,k},f_{i,k},R_{i,k}\}$, where:
- $a_{i,k}$ is the arrival time;
- $s_{i,k}$ is the start time;
- $f_{i,k}$ is the completion time;
- $R_{i,k}$ is the rensponse time ($f_{i,k} - a_{i,k}$).

NOTE: capital-letter parameters are __time spans__ while lower case ones are __absolutes times__.

---

_Scheduling algorithms_
* **Cyclic executive** (static)
Simple and deterministic approach based on a fixed __time table__ => the feasibility is asserted at design time, mutual exclusion and precedence contraints are handled explicitly. CONS: does not handle well external events.
* **Pseudo-parallel executive** (dynamic)
On-line approach based on a __ready queue__ and some sort of __priority__ => can handle sporadic events. CONS: syncronization overhead, less predictable, jitter, hard to prove correctness (expensive feasibility testing).
We focus on:
    * **RM:** rate-monotonic scheduling => $p_i\sim\frac{1}{T_i}$ (static priority assignment);
    * **DM:** deadline-monotonic scheduling => $p_i\sim\frac{1}{D_i}$ (static priority assignment);
    * **EDF**: earliest-deadline-first scheduling (dynamic priority assignment).
    
---

_Hyperperiod Analysis_

Brute-force feasibility test which is always applicable and exact but has an exponential complexity unless the periods of the tasks in the task set are harmonically related (their periods are multiples of each other). It simulates the execution of the tasks, in general using the A* algorithm ($O(n)$), but in a RTS scenario it can be done by simulating a myopic priority-based scheduler executing pseudo-parallelly. 

**NOTE**: Hyperperiod = least common multiplier (LCM) of the periods ($T_i$).

---

_Processor Utilization Analysis_ (Liu & Layland)

$$U = \sum_{i=1}^n\frac{C_i}{T_i}$$

Given the following assumptions:
1. the tasks are independent,
2. there is no aperiodic task,
3. synchronous task set ($D_i = T_i \forall \tau_i$),
4. preemption is possible and has no cost,

* for __RM__ scheduling:
  
    It is sufficient (positive outcome => success, negative outcome =/> failure) that $U \leq U_{RM}$ where $U_{RM} =  n(2^{\frac{1}{n}}-1)$ for the task set to be schedulable.
  
  * $U_{RM}(n)$ = 1 (100%), 0,828 (82,2%), 0,780 (78%) for n = 1, 2, 3
  * $\lim_{n \rightarrow \infty}n(2^{\frac{1}{2}}-1) = \ln2 \approx 0,693$ => worst-case utilization  = 69.3%

* for __EDF__ scheduling:

    It is sufficient and necessary (positive outcome <=> success) that $U \leq 1$ for the task set to be schedulable.
    
---

_Response Time Analysis_

Feasibility test which works for all the static scheduling algorithms.