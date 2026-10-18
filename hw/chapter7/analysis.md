## Q1 
- Prediction / 预测:
  SJF 
  ```
  Job   0 -- Response: 0.00    Turnaround 200.00  Wait 0.00
  Job   1 -- Response: 200.00  Turnaround 400.00  Wait 200.00
  Job   2 -- Response: 400.00  Turnaround 600.00  Wait 400.00
  Average -- Response: 200.00  Turnaround 400.00  Wait 200.00
  ```
  FIFO
  ```
  Job   0 -- Response: 0.00  Turnaround 200.00  Wait 0.00
  Job   1 -- Response: 200.00  Turnaround 400.00  Wait 200.00
  Job   2 -- Response: 400.00  Turnaround 600.00  Wait 400.00
  Average -- Response: 200.00  Turnaround 400.00  Wait 200.00
  ```
- Reasoning / 理由:
SJF: non-preemptive shortest-job-first. Since the three jobs have identical run times, the scheduler executes them in arrival order.
FIFO: First-In, First-Out. Jobs run in arrival order without preemption.
- Verified result / 验证结果:q1_FIFO.txt、q1_SJF.txt
- Analysis / 分析:
Three jobs Job0, Job1 and Job2 have the same CPU burst time of 200 and all arrive at time 0. Two scheduling algorithms are compared: non-preemptive SJF and FIFO.
## Q2
- Prediction / 预测:
  SJF 
  ```
  Job   0 -- Response: 0.00    Turnaround 100.00  Wait 0.00
  Job   1 -- Response: 100.00  Turnaround 300.00  Wait 100.00
  Job   2 -- Response: 300.00  Turnaround 600.00  Wait 300.00
  Average -- Response: 133.33  Turnaround 333.33  Wait 133.33
  ```
  FIFO
  ```
  Job   0 -- Response: 0.00    Turnaround 100.00  Wait 0.00
  Job   1 -- Response: 100.00  Turnaround 300.00  Wait 100.00
  Job   2 -- Response: 300.00  Turnaround 600.00  Wait 300.00
  Average -- Response: 133.33  Turnaround 333.33  Wait 133.33
  ```
- Reasoning / 理由:
FIFO executes jobs in arrival order: Job0 → Job1 → Job2. For SJF, all jobs are ready at the start, and the scheduler always selects the shortest job to run. Since the jobs are ordered from shortest to longest upon arrival, SJF also runs them in the sequence Job0 → Job1 → Job2.
- Verified result / 验证结果:q2_FIFO.txt、q2_SJF.txt
- Analysis / 分析:Three jobs Job0 (burst time = 100), Job1 (burst time = 200), Job2 (burst time = 300) all arrive at time 0.
## Q3
- Prediction / 预测:
```
Job 0  --  Response: 0.00  Turnaround 298.00  Wait 198.00
Job 1  --  Response: 1.00  Turnaround 499.00  Wait 299.00
Job 2  --  Response: 2.00  Turnaround 600.00  Wait 300.00
Average -- Response: 1.00  Turnaround 465.67  Wait 265.67
```
- Reasoning / 理由:Experiment: Round-Robin (RR) scheduling with time quantum q=1. Three jobs Job0 (100), Job1 (200), Job2 (300) all arrive at time 0.RR rule: Each job can only run for 1 time unit. Once its time quantum expires, the job is moved to the end of the ready queue, then the next job runs. Jobs are executed in turn repeatedly.
- Verified result / 验证结果:q3.txt
- Analysis / 分析:RR (Round-Robin) is a preemptive scheduling algorithm. Jobs in the ready queue take turns using the CPU. Each job can run for only one time unit; once its time quantum expires, it is preempted and moved to the tail of the ready queue.
## Q4
```
For non-preemptive SJF, it yields identical turnaround time as FIFO if all jobs arrive at the same time. Either:
Jobs arrive in order of increasing burst time; OR
All jobs have equal CPU burst lengths. Under these conditions, SJF selects the same execution order as FIFO.
```
## Q5
```
Non-preemptive SJF and RR have identical response times when **all jobs arrive at time 0**, and the RR time quantum **is greater than or equal to the maximum CPU burst time of all jobs**.
In this case, once a job gets the CPU, it will run to completion without preemption. RR degenerates into a non-preemptive scheduler and follows the same execution order as non-preemptive SJF, so the response times match.
Special case: if there is only one job in the system, SJF and RR always have the same response time regardless of quantum size.
```
## Q6
ARG jlist 100,200
```
Job   0 -- Response: 0.00    Turnaround 100.00  Wait 0.00
Job   1 -- Response: 100.00  Turnaround 300.00  Wait 100.00
Average -- Response: 50.00   Turnaround 200.00  Wait 50.00
```
ARG jlist 100,500
```
Job   0 -- Response: 0.00    Turnaround 100.00  Wait 0.00
Job   1 -- Response: 100.00  Turnaround 600.00  Wait 100.00
Average -- Response: 50.00   Turnaround 350.00  Wait 50.00
  ```
ARG jlist 100,1000
```
Job   0 -- Response: 0.00    Turnaround 100.00  Wait 0.00
Job   1 -- Response: 100.00  Turnaround 1100.00  Wait 100.00
Average -- Response: 50.00   Turnaround 600.00  Wait 50.00
```
In non-preemptive SJF, the response time of a long job does not depend on its own burst length. Its response time equals the sum of CPU burst times of all shorter jobs already present in the ready queue. As more short jobs keep arriving, the response time of long jobs increases. In the worst case, long jobs may suffer starvation, constantly waiting for incoming shorter jobs.
- Reasoning / 理由:
Non-preemptive SJF: once a job starts execution, it cannot be preempted. The scheduler selects the job with the shorter burst time first.
Job0 has a burst time of 100, and Job1 has burst times of 200, 500 and 1000 respectively. Both jobs arrive at time 0. Since Job0 is shorter, it always runs first.
- Verified result / 验证结果:q6_100,200.txt、q6_100,500.txt、q6_100,1000.txt
- Analysis / 分析:ARG jlist 100,200 ARG jlist 100,500 ARG jlist 100,1000
As the time quantum increases, the response time of RR scheduling increases. A larger quantum means jobs later in the ready queue must wait longer to get their first access to the CPU.
For N jobs arriving at time 0, the worst-case response time equation:
Rworst=(N-1)*q
Where N is the total number of jobs and q is the RR time quantum. This worst case happens when the target job is placed last in the ready queue.N-1 jobs each to run one full time quantum before it receives the CPU for the first time.