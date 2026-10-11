## Q1
- Prediction / 预测:the state table is below
```
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu        READY             1          
  2        RUN:cpu        READY             1          
  3        RUN:cpu        READY             1          
  4        RUN:cpu        READY             1          
  5        RUN:cpu        READY             1          
  6        DONE           RUN:cpu           1          
  7        DONE           RUN:cpu           1          
  8        DONE           RUN:cpu           1          
  9        DONE           RUN:cpu           1          
 10        DONE           RUN:cpu           1          
```
- Reasoning / 理由:
Both processes only contain CPU instructions with no I/O operations. The system switches the CPU only when a process finishes or issues an I/O request. PID 0 gets the CPU first and runs all 5 instructions continuously. During this time, PID 1 stays ready and waits. After PID 0 completes, the CPU switches to PID 1, which then runs its 5 instructions. No I/O occurs, so no extra context switches happen.
- Verified result / 验证结果: ../../../hw/cpu-intro/q1.txt
Stats: Total Time 10
Stats: CPU Busy 10 (100.00%)
Stats: IO Busy  0 (0.00%)
- Analysis / 分析:CPU Busy 10 (100.00%) Process 0 cpu，cpu，cpu，cpu，cpu Process 1 cpu，cpu，cpu，cpu，cpu No scheduling rule.
## Q2
-	Prediction / 预测:the state table is below
```
Time        PID: 0        PID: 1           CPU           IOs
1           RUN:cpu       READY             1          
2           RUN:cpu       READY             1          
3           RUN:cpu       READY             1          
4           RUN:cpu       READY             1          
5           DONE          RUN:io            1          
6           DONE          BLOCKED                        1
7           DONE          BLOCKED                        1
8           DONE          BLOCKED                        1
9           DONE          BLOCKED                        1
10          DONE          BLOCKED                        1
11          DONE        RUN:io_done         1          
 ```
-	Reasoning / 理由:
PID 0 contains four CPU instructions, while PID 1 contains one I/O instruction. Context switching occurs only when a process terminates or initiates an I/O request.From time 1 to 4, PID 0 occupies the CPU and executes its four CPU instructions sequentially, and PID 1 remains in the READY state.At time 5, PID 0 finishes execution, and the CPU switches to PID 1. PID 1 issues the I/O request and transitions to the BLOCKED state, waiting for the I/O device.During time 6 to 10, PID 1 stays blocked while waiting for I/O completion, and the CPU remains idle as no other process is available.At time 11, the I/O operation completes. PID 1 regains the CPU and executes the `io_done` step.
-	Verified result / 验证结果:../../../hw/cpu-intro/q2.txt
Stats: Total Time 11
Stats: CPU Busy 6 (54.55%)
Stats: IO Busy  5 (45.45%)
-	Analysis / 分析:CPU Busy 6 IO Busy  5 Process 0 cpu cpu cpu cpu Process 1 io io_done No scheduling rule.
## Q3
-	Prediction / 预测:the state table is below
```
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY            1          
  2         BLOCKED       RUN:cpu           1             1
  3         BLOCKED       RUN:cpu           1             1
  4         BLOCKED       RUN:cpu           1             1
  5         BLOCKED       RUN:cpu           1             1
  6         BLOCKED       DONE                            1
  7       RUN:io_done     DONE              1          
```
-	Reasoning / 理由:
PID 0 initially acquires the CPU and executes an I/O instruction. Under this scheduling rule, context switching occurs when a process issues an I/O request or completes execution. At time 1, PID 0 initiates an I/O operation and transitions to the BLOCKED state, while the I/O device starts working. The system performs a context switch and assigns the CPU to PID 1. From time 2 to 5, PID 0 remains blocked waiting for I/O completion, and PID 1 continuously uses the CPU to execute its four CPU-bound instructions. At time 6, PID 1 terminates and enters the DONE state, while PID 0 is still blocked awaiting I/O. At time 7, the I/O operation completes. PID 0 regains the CPU to execute the io_done step, and the entire workload finishes.
-	Verified result / 验证结果:../../../hw/cpu-intro/q3.txt
Stats: Total Time 7
Stats: CPU Busy 6 (85.71%)
Stats: IO Busy  5 (71.43%)
-	Analysis / 分析:
Process 0
  io
  io_done

Process 1
  cpu
  cpu
  cpu
  cpu
## Q4
-	Prediction / 预测:the state table is below
```
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1          
  2        BLOCKED         READY                           1
  3        BLOCKED         READY                           1
  4        BLOCKED         READY                           1
  5        BLOCKED         READY                           1
  6        BLOCKED         READY                           1
  7*     RUN:io_done       READY             1          
  8           DONE        RUN:cpu            1          
  9           DONE        RUN:cpu            1          
 10           DONE        RUN:cpu            1          
 11           DONE        RUN:cpu            1          
```
-	Reasoning / 理由:
PID 0 first gets the CPU and runs the io instruction, issues an I/O request and enters the BLOCKED state. With the SWITCH_ON_END policy, context switch only occurs when a process fully finishes execution, not when a process starts I/O. So CPU will NOT switch to PID1 immediately; PID1 stays in READY. From time 2 to 6, PID0 remains blocked waiting for I/O, PID1 stays ready and cannot get the CPU. At time7, I/O completes. PID0 gets CPU again to run io_done, then PID0 finishes and becomes DONE. At time8, since PID0 terminates, context switch happens. CPU is assigned to PID1, which runs its four cpu instructions continuously until time11.
-	Verified result / 验证结果:../../../hw/cpu-intro/q4.txt
Stats: Total Time 11
Stats: CPU Busy 6 (54.55%)
Stats: IO Busy  5 (45.45%)
-	Analysis / 分析:
Process 0
  io
  io_done

Process 1
  cpu
  cpu
  cpu
  cpu
  The scheduling rule is-S SWITCH_ON_END
## Q5
-	Prediction / 预测:the state table is below
```
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1          
  2        BLOCKED       RUN:cpu             1             1
  3        BLOCKED       RUN:cpu             1             1
  4        BLOCKED       RUN:cpu             1             1
  5        BLOCKED       RUN:cpu             1             1
  6        BLOCKED         DONE                            1
  7      RUN:io_done       DONE              1          
```
-	Reasoning / 理由:
PID 0 first acquires the CPU and executes the io instruction, issues an I/O request and enters the BLOCKED state. Under the SWITCH_ON_IO policy, context switch occurs when a process issues I/O or finishes execution. The system immediately switches CPU to PID1. From time 2 to 5, PID0 is blocked waiting for I/O, while PID1 continuously uses the CPU to run its four cpu instructions. At time6, PID1 completes all its instructions and becomes DONE. PID0 is still waiting for I/O. At time7, I/O completes. PID0 obtains the CPU to run io_done and finishes execution.
-	Verified result / 验证结果:../../../hw/cpu-intro/q5.txt
Stats: Total Time 7
Stats: CPU Busy 6 (85.71%)
Stats: IO Busy  5 (71.43%)
-	Analysis / 分析:Process 0
  io
  io_done

Process 1
  cpu
  cpu
  cpu
  cpu
The scheduling rule is -S SWITCH_ON_IO
## Q6
-	Prediction / 预测:the state table is below
```
Time        PID: 0        PID: 1        PID: 2        PID: 3           CPU           IOs
  1         RUN:io         READY         READY         READY             1          
  2        BLOCKED       RUN:cpu         READY         READY             1             1
  3        BLOCKED       RUN:cpu         READY         READY             1             1
  4        BLOCKED       RUN:cpu         READY         READY             1             1
  5        BLOCKED       RUN:cpu         READY         READY             1             1
  6        BLOCKED       RUN:cpu         READY         READY             1             1
  7         READY         DONE         RUN:cpu         READY             1          
  8         READY         DONE         RUN:cpu         READY             1          
  9         READY         DONE         RUN:cpu         READY             1          
 10         READY         DONE         RUN:cpu         READY             1          
 11         READY         DONE         RUN:cpu         READY             1          
 12         READY         DONE           DONE         RUN:cpu            1          
 13         READY         DONE           DONE         RUN:cpu            1          
 14         READY         DONE           DONE         RUN:cpu            1          
 15         READY         DONE           DONE         RUN:cpu            1          
 16         READY         DONE           DONE         RUN:cpu            1          
 17      RUN:io_done      DONE           DONE          DONE              1          
 18         RUN:io        DONE           DONE          DONE              1          
 19        BLOCKED        DONE           DONE          DONE                           1
 20        BLOCKED        DONE           DONE          DONE                           1
 21        BLOCKED        DONE           DONE          DONE                           1
 22        BLOCKED        DONE           DONE          DONE                           1
 23        BLOCKED        DONE           DONE          DONE                           1
 24      RUN:io_done      DONE           DONE          DONE              1          
 25         RUN:io        DONE           DONE          DONE              1          
 26        BLOCKED        DONE           DONE          DONE                           1
 27        BLOCKED        DONE           DONE          DONE                           1
 28        BLOCKED        DONE           DONE          DONE                           1
 29        BLOCKED        DONE           DONE          DONE                           1
 30        BLOCKED        DONE           DONE          DONE                           1
 31      RUN:io_done      DONE           DONE          DONE              1          
```
-	Reasoning / 理由:
We use `SWITCH_ON_IO` and `IO_RUN_LATER`. The system switches CPU when a process issues I/O or finishes execution. When I/O completes, the process does not immediately seize the CPU; it goes to the ready queue and waits for its turn.At time1, PID0 runs `io`, becomes blocked, and CPU switches to PID1.From time2 to 6, PID0 waits blocked. PID1 runs all 5 cpu instructions and finishes at time6.From time7 to11, PID0 is ready and waiting in queue. CPU switches to PID2, PID2 runs all 5 cpu instructions and finishes at time11.From time12 to16, CPU switches to PID3, PID3 runs all 5 cpu instructions and finishes at time16.At time17, all other processes are done. PID0 gets CPU to run `io_done`.At time18, PID0 runs another `io` and becomes blocked. No other ready processes exist.Time19–23: PID0 waits blocked for I/O.At time24, I/O completes, PID0 runs `io_done`.At time25, PID0 executes the third `io` and enters blocked state again.Time26–30: PID0 waits blocked for I/O.At time31, I/O completes, PID0 runs final `io_done`.
-	Verified result / 验证结果:../../../hw/cpu-intro/q6.txt
Stats: Total Time 31
Stats: CPU Busy 21 (67.74%)
Stats: IO Busy  15 (48.39%)
-	Analysis / 分析:The scheduling rule is -S SWITCH_ON_IO -I IO_RUN_LATER
## Q7
-	Prediction / 预测:the state table is below
```
Time        PID: 0        PID: 1        PID: 2        PID: 3           CPU           IOs
  1         RUN:io         READY         READY         READY             1          
  2        BLOCKED       RUN:cpu         READY         READY             1             1
  3        BLOCKED       RUN:cpu         READY         READY             1             1
  4        BLOCKED       RUN:cpu         READY         READY             1             1
  5        BLOCKED       RUN:cpu         READY         READY             1             1
  6        BLOCKED       RUN:cpu         READY         READY             1             1
  7      RUN:io_done      DONE           READY         READY             1          
  8         RUN:io        DONE           READY         READY             1          
  9        BLOCKED        DONE          RUN:cpu        READY             1             1
 10        BLOCKED        DONE          RUN:cpu        READY             1             1
 11        BLOCKED        DONE          RUN:cpu        READY             1             1
 12        BLOCKED        DONE          RUN:cpu        READY             1             1
 13        BLOCKED        DONE          RUN:cpu        READY             1             1
 14      RUN:io_done      DONE            DONE         READY             1          
 15         RUN:io        DONE            DONE         READY             1          
 16        BLOCKED        DONE            DONE        RUN:cpu            1             1
 17        BLOCKED        DONE            DONE        RUN:cpu            1             1
 18        BLOCKED        DONE            DONE        RUN:cpu            1             1
 19        BLOCKED        DONE            DONE        RUN:cpu            1             1
 20        BLOCKED        DONE            DONE        RUN:cpu            1             1
 21      RUN:io_done      DONE            DONE          DONE             1          
 ```
-	Reasoning / 理由:
We use `SWITCH_ON_IO` and `IO_RUN_IMMEDIATE`. The system switches CPU when a process issues I/O or finishes execution. When I/O completes, the process that issued I/O immediately takes the CPU without waiting in the ready queue.At time1, PID0 runs `io` and becomes blocked, CPU switches to PID1.From time2 to 6, PID0 waits blocked. PID1 runs all 5 cpu instructions and finishes at time6. PID0’s I/O completes at time6, so PID0 grabs CPU immediately.At time7, PID0 runs `io_done`.At time8, PID0 runs another `io` and becomes blocked, CPU switches to PID2.From time9 to13, PID0 waits blocked. PID2 runs all 5 cpu instructions and finishes at time13. PID0’s I/O completes, PID0 retakes CPU.At time14, PID0 runs `io_done`.At time15, PID0 executes the third `io` and enters blocked state, CPU switches to PID3.From time16 to20, PID0 waits blocked. PID3 runs all 5 cpu instructions and finishes at time20. PID0’s I/O completes and seizes CPU.At time21, PID0 runs the final `io_done`.
-	Verified result / 验证结果:../../../hw/cpu-intro/q7.txt
Stats: Total Time 21
Stats: CPU Busy 21 (100.00%)
Stats: IO Busy  15 (71.43%)
-	Analysis / 分析The scheduling rule is -S SWITCH_ON_IO -I IO_RUN_IMMEDIATE
## Q8-s1 python3 process-run.py -s 1 -l 3:50,3:50
-	Prediction / 预测:the state table is below
```
Time        PID: 0        PID: 1           CPU           IOs
1          RUN:cpu        READY              1          
2          RUN:io         READY              1          
3          BLOCKED        RUN:cpu            1             1
4          BLOCKED        RUN:cpu            1             1
5          BLOCKED        RUN:cpu            1             1
6          BLOCKED         DONE                            1
7          BLOCKED         DONE                            1
8        RUN:io_done       DONE              1          
9           RUN:io         DONE              1          
10         BLOCKED         DONE                            1
11         BLOCKED         DONE                            1
12         BLOCKED         DONE                            1
13         BLOCKED         DONE                            1
14         BLOCKED         DONE                            1
15       RUN:io_done       DONE              1          
```
-	Reasoning / 理由:
Using the command process-run.py -s 1 -l 3:50,3:50, seed 1 generates the base instructions for two processes: PID0 has cpu and io. The simulator automatically appends io_done, followed by another io with its matching io_done. Its full instruction sequence is cpu → io → io_done → io → io_done; PID1 generates three consecutive cpu instructions, with the sequence cpu → cpu → cpu.
-	Verified result / 验证结果:../../../hw/cpu-intro/q8-s1.txt
Stats: Total Time 15
Stats: CPU Busy 8 (53.33%)
Stats: IO Busy  10 (66.67%)
-	Analysis / 分析:process-run.py -s 1 -l 3:50,3:50 No scheduling rule.
## Q8-s1 python3 process-run.py -s 1 -l 3:50,3:50 -I IO_RUN_IMMEDIATE
-	Prediction / 预测:the state table is below
```
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2         RUN:io         READY             1          
  3        BLOCKED       RUN:cpu             1             1
  4        BLOCKED       RUN:cpu             1             1
  5        BLOCKED       RUN:cpu             1             1
  6        BLOCKED          DONE                           1
  7        BLOCKED          DONE                           1
  8*   RUN:io_done          DONE             1          
  9         RUN:io          DONE             1          
 10        BLOCKED          DONE                           1
 11        BLOCKED          DONE                           1
 12        BLOCKED          DONE                           1
 13        BLOCKED          DONE                           1
 14        BLOCKED          DONE                           1
 15*   RUN:io_done          DONE             1          
 ```
-	Reasoning / 理由:
Process 0 executes five instructions sequentially: `cpu`, `io`, `io_done`, `io`, `io_done`. Process 1 has three consecutive `cpu` instructions: `cpu`, `cpu`, `cpu`.The scheduling rule is -IO_RUN_IMMEDIATE.
-	Verified result / 验证结果:../../../hw/cpu-intro/q8-s1.txt
Stats: Total Time 15
Stats: CPU Busy 8 (53.33%)
Stats: IO Busy  10 (66.67%)
-	Analysis / 分析:process-run.py -s 1 -l 3:50,3:50 The scheduling rule is -I IO_RUN_IMMEDIATE
## Q8-s1 python3 process-run.py -s 1 -l 3:50,3:50 -S SWITCH_ON_END
-	Prediction / 预测:the state table is below
```
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2         RUN:io         READY             1          
  3        BLOCKED         READY                           1
  4        BLOCKED         READY                           1
  5        BLOCKED         READY                           1
  6        BLOCKED         READY                           1
  7        BLOCKED         READY                           1
  8*   RUN:io_done         READY             1          
  9         RUN:io         READY             1          
 10        BLOCKED         READY                           1
 11        BLOCKED         READY                           1
 12        BLOCKED         READY                           1
 13        BLOCKED         READY                           1
 14        BLOCKED         READY                           1
 15*   RUN:io_done         READY             1          
 16           DONE       RUN:cpu             1          
 17           DONE       RUN:cpu             1          
 18           DONE       RUN:cpu             1   
 ```       
-	Reasoning / 理由:Process 0 executes five instructions sequentially: `cpu`, `io`, `io_done`, `io`, `io_done`. Process 1 has three consecutive `cpu` instructions: `cpu`, `cpu`, `cpu`.The scheduling rule is -S SWITCH_ON_END
-	Verified result / 验证结果:../../../hw/cpu-intro/q8-s1.txt
Stats: Total Time 18
Stats: CPU Busy 8 (44.44%)
Stats: IO Busy  10 (55.56%)
-	Analysis / 分析:process-run.py -s 1 -l 3:50,3:50 The scheduling rule is -S SWITCH_ON_END
## Q8-s2 python3 process-run.py -s 2 -l 3:50,3:50
-	Prediction / 预测:the state table is below
```
Time        PID: 0        PID: 1           CPU           IOs
1           RUN:io        READY             1          
2           BLOCKED       RUN:cpu           1             1
3           BLOCKED       RUN:io            1             1
4           BLOCKED       BLOCKED                         2
5           BLOCKED       BLOCKED                         2
6           BLOCKED       BLOCKED                         2
7         RUN:io_done     BLOCKED           1             1
8           RUN:io        BLOCKED           1             1
9           BLOCKED     RUN:io_done         1             1
10          BLOCKED       RUN:io            1             1
11          BLOCKED       BLOCKED                         2
12          BLOCKED       BLOCKED                         2
13          BLOCKED       BLOCKED                         2
14        RUN:io_done     BLOCKED           1             1
15          RUN:cpu       BLOCKED           1             1
16           DONE       RUN:io_done         1          
```
-	Reasoning / 理由:Process 0 executes instructions in the order: `io`, `io_done`, `io`, `io_done`, `cpu`.
Process 1 executes instructions in the order: `cpu`, `io`, `io_done`, `io`, `io_done`.
No scheduling rule.
-	Verified result / 验证结果:../../../hw/cpu-intro/q8-s2.txt
Stats: Total Time 16
Stats: CPU Busy 10 (62.50%)
Stats: IO Busy  14 (87.50%)
-	Analysis / 分析:process-run.py -s 2 -l 3:50,3:50 No scheduling rule.
## Q8-s2 python3 process-run.py -s 2 -l 3:50,3:50 -I IO_RUN_IMMEDIATE
-	Prediction / 预测:the state table is below
```
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1          
  2        BLOCKED       RUN:cpu             1             1
  3        BLOCKED        RUN:io             1             1
  4        BLOCKED       BLOCKED                           2
  5        BLOCKED       BLOCKED                           2
  6        BLOCKED       BLOCKED                           2
  7*   RUN:io_done       BLOCKED             1             1
  8         RUN:io       BLOCKED             1             1
  9*       BLOCKED   RUN:io_done             1             1
 10        BLOCKED        RUN:io             1             1
 11        BLOCKED       BLOCKED                           2
 12        BLOCKED       BLOCKED                           2
 13        BLOCKED       BLOCKED                           2
 14*   RUN:io_done       BLOCKED             1             1
 15        RUN:cpu       BLOCKED             1             1
 16*          DONE   RUN:io_done             1          
```
-	Reasoning / 理由:Process 0 executes instructions in the order: `io`, `io_done`, `io`, `io_done`, `cpu`.
Process 1 executes instructions in the order: `cpu`, `io`, `io_done`, `io`, `io_done`.The scheduling rule is  -I IO_RUN_IMMEDIATE
-	Verified result / 验证结果:../../../hw/cpu-intro/q8-s2.txt
Stats: Total Time 16
Stats: CPU Busy 10 (62.50%)
Stats: IO Busy  14 (87.50%)
-	Verified result / 验证结果:
-	Analysis / 分析:process-run.py -s 2 -l 3:50,3:50 The scheduling rule is  -I IO_RUN_IMMEDIATE
## Q8-s2 python3 process-run.py -s 2 -l 3:50,3:50 -S SWITCH_ON_END
-	Prediction / 预测:the state table is below
```
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1          
  2        BLOCKED         READY                           1
  3        BLOCKED         READY                           1
  4        BLOCKED         READY                           1
  5        BLOCKED         READY                           1
  6        BLOCKED         READY                           1
  7*   RUN:io_done         READY             1          
  8         RUN:io         READY             1          
  9        BLOCKED         READY                           1
 10        BLOCKED         READY                           1
 11        BLOCKED         READY                           1
 12        BLOCKED         READY                           1
 13        BLOCKED         READY                           1
 14*   RUN:io_done         READY             1          
 15        RUN:cpu         READY             1          
 16           DONE       RUN:cpu             1          
 17           DONE        RUN:io             1          
 18           DONE       BLOCKED                           1
 19           DONE       BLOCKED                           1
 20           DONE       BLOCKED                           1
 21           DONE       BLOCKED                           1
 22           DONE       BLOCKED                           1
 23*          DONE   RUN:io_done             1          
 24           DONE        RUN:io             1          
 25           DONE       BLOCKED                           1
 26           DONE       BLOCKED                           1
 27           DONE       BLOCKED                           1
 28           DONE       BLOCKED                           1
 29           DONE       BLOCKED                           1
 30*          DONE   RUN:io_done             1          
```
-	Reasoning / 理由:Process 0 executes instructions in the order: `io`, `io_done`, `io`, `io_done`, `cpu`.
Process 1 executes instructions in the order: `cpu`, `io`, `io_done`, `io`, `io_done`.The scheduling rule is  -S SWITCH_ON_END
-	Verified result / 验证结果:../../../hw/cpu-intro/q8-s2.txt
Stats: Total Time 30
Stats: CPU Busy 10 (33.33%)
Stats: IO Busy  20 (66.67%)
-	Analysis / 分析:process-run.py -s 2 -l 3:50,3:50 The scheduling rule is  -S SWITCH_ON_END
## Q8-s3 python3 process-run.py -s 3 -l 3:50,3:50
-	Prediction / 预测:the state table is below
```
Time        PID: 0        PID: 1           CPU           IOs
1           RUN:cpu       READY             1          
2           RUN:io        READY             1          
3           BLOCKED       RUN:io            1             1
4           BLOCKED       BLOCKED                         2
5           BLOCKED       BLOCKED                         2
6           BLOCKED       BLOCKED                         2
7           BLOCKED       BLOCKED                         2
8        RUN:io_done      BLOCKED           1             1
9           RUN:cpu       READY             1          
10           DONE      RUN:io_done          1          
11           DONE        RUN:io             1          
12           DONE         BLOCKED                         1
13           DONE         BLOCKED                         1
14           DONE         BLOCKED                         1
15           DONE         BLOCKED                         1
16           DONE         BLOCKED                         1
17           DONE      RUN:io_done          1          
18           DONE         RUN:cpu           1          
```
-	Reasoning / 理由:Process 0 executes instructions in the order: `cpu`, `io`, `io_done`, `cpu`.
Process 1 executes instructions in the order: `io`, `io_done`, `io`, `io_done`, `cpu`.
No scheduling rule.
-	Verified result / 验证结果:../../../hw/cpu-intro/q8-s3.txt
Stats: Total Time 18
Stats: CPU Busy 9 (50.00%)
Stats: IO Busy  11 (61.11%)
-	Analysis / 分析:process-run.py -s 3 -l 3:50,3:50 No scheduling rule.
## Q8-s3 python3 process-run.py -s 3 -l 3:50,3:50 -I IO_RUN_IMMEDIATE
-	Prediction / 预测:the state table is below
```
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2         RUN:io         READY             1          
  3        BLOCKED        RUN:io             1             1
  4        BLOCKED       BLOCKED                           2
  5        BLOCKED       BLOCKED                           2
  6        BLOCKED       BLOCKED                           2
  7        BLOCKED       BLOCKED                           2
  8*   RUN:io_done       BLOCKED             1             1
  9*         READY   RUN:io_done             1          
 10          READY        RUN:io             1          
 11        RUN:cpu       BLOCKED             1             1
 12           DONE       BLOCKED                           1
 13           DONE       BLOCKED                           1
 14           DONE       BLOCKED                           1
 15           DONE       BLOCKED                           1
 16*          DONE   RUN:io_done             1          
 17           DONE       RUN:cpu             1          
```
-	Reasoning / 理由:Process 0 executes instructions in the order: `cpu`, `io`, `io_done`, `cpu`.
Process 1 executes instructions in the order: `io`, `io_done`, `io`, `io_done`, `cpu`.The scheduling rule is  -I IO_RUN_IMMEDIATE
-	Verified result / 验证结果:../../../hw/cpu-intro/q8-s3.txt
Stats: Total Time 17
Stats: CPU Busy 9 (52.94%)
Stats: IO Busy  11 (64.71%)
-	Analysis / 分析:process-run.py -s 3 -l 3:50,3:50 The scheduling rule is  -I IO_RUN_IMMEDIATE
## Q8-s3 python3 process-run.py -s 3 -l 3:50,3:50 -S SWITCH_ON_END
-	Prediction / 预测:the state table is below
```
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2         RUN:io         READY             1          
  3        BLOCKED         READY                           1
  4        BLOCKED         READY                           1
  5        BLOCKED         READY                           1
  6        BLOCKED         READY                           1
  7        BLOCKED         READY                           1
  8*   RUN:io_done         READY             1          
  9        RUN:cpu         READY             1          
 10           DONE        RUN:io             1          
 11           DONE       BLOCKED                           1
 12           DONE       BLOCKED                           1
 13           DONE       BLOCKED                           1
 14           DONE       BLOCKED                           1
 15           DONE       BLOCKED                           1
 16*          DONE   RUN:io_done             1          
 17           DONE        RUN:io             1          
 18           DONE       BLOCKED                           1
 19           DONE       BLOCKED                           1
 20           DONE       BLOCKED                           1
 21           DONE       BLOCKED                           1
 22           DONE       BLOCKED                           1
 23*          DONE   RUN:io_done             1          
 24           DONE       RUN:cpu             1          
```
-	Reasoning / 理由:Process 0 executes instructions in the order: `cpu`, `io`, `io_done`, `cpu`.
Process 1 executes instructions in the order: `io`, `io_done`, `io`, `io_done`, `cpu`.The scheduling rule is  -S SWITCH_ON_END
-	Verified result / 验证结果:../../../hw/cpu-intro/q8-s3.txt
Stats: Total Time 24
Stats: CPU Busy 9 (37.50%)
Stats: IO Busy  15 (62.50%)
-	Analysis / 分析:process-run.py -s 3 -l 3:50,3:50 The scheduling rule is  -S SWITCH_ON_END