# Chapter7 CPU Scheduling Simulation Lab

## Overview

This laboratory investigates the basic CPU scheduling policies introduced in the OSTEP chapter on CPU scheduling. The objective is to compare the behavior of first-come-first-served (FIFO), non-preemptive shortest-job-first (SJF), and round-robin (RR) scheduling under workloads that vary in job length and arrival pattern. The experimental traces in the project directory provide concrete evidence for the theoretical trade-offs among efficiency, fairness, and starvation.

The principal metrics used in this analysis are response time, turnaround time, and waiting time. Response time measures the interval from job arrival to first CPU execution; turnaround time measures the time from job submission to completion; waiting time measures the total period during which a job waits in the ready queue. These quantities make it possible to assess how each policy affects system responsiveness and average completion time.

## Q1 & Q2: FIFO vs Non-preemptive SJF

The first two experiments compare FIFO and non-preemptive SJF under workloads in which all jobs arrive at the same time. In the first case, the jobs have identical length of 200 units, and in the second case the jobs are 100, 200, and 300 units long.

For the equal-burst case, both FIFO and non-preemptive SJF produce the same execution order because there is no meaningful distinction between jobs: Job 0 runs first, followed by Job 1 and then Job 2. The measured statistics are therefore identical:

- Average response time: 200.00
- Average turnaround time: 400.00
- Average waiting time: 200.00

The second case follows the same pattern when all jobs are ready at time 0 and the shortest job is also the earliest one to be admitted. Job 0 (100 units) runs first, followed by Job 1 (200 units), and then Job 2 (300 units). Consequently, FIFO and non-preemptive SJF again produce the same schedule and identical metrics:

- Average response time: 133.33
- Average turnaround time: 333.33
- Average waiting time: 133.33

This result illustrates an important observation: when jobs are all released simultaneously, or when smaller jobs happen to arrive before longer ones, SJF and FIFO may exhibit indistinguishable behavior. In this setting, the theoretical advantage of SJF over FIFO is not evident because the ordering of jobs is effectively fixed by the workload itself.

## Q3 & Q4: Jobs with identical burst time

The special case of identical burst times is particularly instructive. When multiple jobs have the same CPU demand and are released at the same instant, the scheduler has no basis for distinguishing among them. As a consequence, both FIFO and non-preemptive SJF reduce to the same scheduling order, namely arrival order. This is exactly what is observed in the Q1 traces: the workload consisting of three jobs of equal duration yields identical results for FIFO and SJF because the execution order is effectively determined by arrival time rather than by estimated job length.

The implication is that fairness and efficiency are not determined solely by the algorithm but also by the workload characteristics. If all jobs are equivalent in burst time and start simultaneously, the scheduler cannot improve on the natural arrival order without violating the principle of deterministic, queue-based execution.

## Q5: Round Robin (time slice q=1)

The RR experiment uses a time quantum of one unit. In this policy, each job executes for one time slice before being preempted and returned to the end of the ready queue. This makes RR a preemptive scheduler designed to promote fairness among competing tasks.

For the workload of 100, 200, and 300 units, the execution trace shows the jobs interleaving repeatedly:

- Job 0: response time 0.00, turnaround 298.00, wait 198.00
- Job 1: response time 1.00, turnaround 499.00, wait 299.00
- Job 2: response time 2.00, turnaround 600.00, wait 300.00
- Average response time: 1.00
- Average turnaround time: 465.67
- Average waiting time: 265.67

Compared with FIFO and non-preemptive SJF, RR produces much lower response time because every job is scheduled quickly and repeatedly. However, this fairness comes at a cost: the average turnaround time increases substantially because jobs are frequently interrupted and requeued. This trade-off is fundamental to RR: it prioritizes responsiveness and fairness, while sacrificing aggregate completion efficiency.

## Q6: Starvation in non-preemptive SJF

The final experiment demonstrates the principal weakness of non-preemptive SJF when short jobs continue to arrive. The workloads 100/200, 100/500, and 100/1000 reveal a strong dependence of the long job's delay on the length of the short job that runs first. In each trace, Job 0 completes quickly because it is the shortest job, while the longer job remains in the queue until after Job 0 finishes.

The resulting measurements are as follows:

- For 100, 200: turnaround = 300, average turnaround = 200
- For 100, 500: turnaround = 600, average turnaround = 350
- For 100, 1000: turnaround = 1100, average turnaround = 600

These results show that the long job's waiting time grows as the disparity in job lengths increases. In a system with frequent arrivals of very short jobs, non-preemptive SJF may effectively starve longer jobs, because the scheduler persistently chooses the shortest available task and never revisits a running job once it has started. This illustrates a critical design limitation: SJF minimizes average waiting time only under favorable assumptions, while the price is reduced fairness for jobs with larger service requirements.

## Algorithm Comparison Table

| Algorithm | Scheduling policy | Fairness | Response time | Turnaround time | Main characteristic |
| --- | --- | --- | --- | --- | --- |
| FIFO | First-come-first-served | Moderate | 133.33 (100,200,300 case) | 333.33 | Simple and predictable, but insensitive to burst length |
| Non-preemptive SJF | Shortest available job first | Moderate | 133.33 (100,200,300 case) | 333.33 | Minimizes waiting time when burst lengths are known and stable |
| RR (q=1) | Time-slice rotation | High | 1.00 | 465.67 | Excellent fairness and low latency, but larger average turnaround |
| Non-preemptive SJF in starvation case | Shortest-job policy under repeated short arrivals | Low for long jobs | 50.00 | 600.00 (100,1000 case) | Severe delay for long jobs; starvation risk |

## Conclusion

The CPU scheduling lab confirms the central lesson of operating systems: there is no universally optimal scheduler for all performance objectives. FIFO is straightforward and easy to reason about, but it does not account for job size and therefore lacks efficiency under mixed workloads. Non-preemptive SJF can minimize average waiting time when the burst lengths are accurately known, yet it becomes vulnerable to starvation when long jobs are continually delayed by shorter ones. RR provides strong fairness and low response time, but this is achieved by trading away completion efficiency.

In summary, the simulation results show that scheduling policy must be chosen according to the system’s goals. Interactive systems emphasize responsiveness and fairness, whereas batch systems may prioritize throughput and average turnaround. The traces in this chapter therefore serve as a clear demonstration of the classic scheduling dilemma: increasing fairness often reduces efficiency, and maximizing efficiency may compromise starvation behavior and long-term temporal fairness.
