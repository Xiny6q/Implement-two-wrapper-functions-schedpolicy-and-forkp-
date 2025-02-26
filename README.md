Download link :https://programming.engineering/product/implement-two-wrapper-functions-schedpolicy-and-forkp/


# Implement-two-wrapper-functions-schedpolicy-and-forkp-
Implement two wrapper functions schedpolicy() and forkp(),
Implement two wrapper functions schedpolicy() and forkp(), where schedpolicy() is used to pass the scheduling policy to the xv6 kernel and return the current policy, and forkp() passes a base priority value of the process beign created. forkp() must also be implemented as a new system call to create a new process and store its base priority value in its process table entry. After implementing these, complete submitjobs.c to ensure compilation.

Usage of submitjobs.c: submitjobs < batchfilename.txt

The batch file has a specific format: the first line specifies the scheduling algorithm as an integer code and each subsequent line represents a process. Each process line starts with a positive integer specifying its base priority value and next it specifies the command to run the process. See user/batch.txt as an example. All batch files must be listed in Makefile under JOBFILES. Any new batch file must be added to this list.

Schedulers

In the assignment, four scheduling algorithms are to be implemented (and experimented with):

SCHED_NPREEMPT_FCFS: non-preemptive first come first serve [code 0]

SCHED_NPREEMPT_SJF: non-preemptive shortest job first [code 1]

SCHED_PREEMPT_RR: preemptive round-robin [code 2]

SCHED_PREEMPT_UNIX: preemptive UNIX scheduler [code 3]; the base priority value of a process is used only in this scheduler

Printing statistics

After a batch of processes completes execution, the following statistics should be printed by xv6.

Batch execution time: This is the execution time of the entire batch. This is measured as the number of ticks from the stime of the batch process that got scheduled first to the endtime of the batch process to finish last. To know when a batch finishes, you may have to maintain the batch size as observed from the forkp() calls (increment batch size by one in forkp()).

Average turnaround time of the batch: For each finishing batch process, compute its turnaround time and maintain a running sum of the turnaround times of the batch processes. Take an average after all batch processes have finished.

Average waiting time in RUNNABLE state: Whenever a batch process enters the RUNNABLE state, start counting waiting time. When it comes out of RUNNABLE state and moves to RUNNING state, end counting waiting time. Accumulate all waiting time instances for a batch process. Maintain a running sum of the waiting times of the finished batch processes. Take an average after all batch processes have finished.

Average, minimum, and maximum completion time of the processes in a batch. A large difference between the maximum and minimum completion times relative to the average is an indication of lack of fairness meaning that some process in the batch finishes very quickly while some other process finishes quite late.

For the SCHED_NPREEMPT_SJF scheduler, print the total number of non-zero CPU bursts observed across all processes in a batch, average CPU burst length observed in a batch, maximum CPU burst length observed in a batch, and minimum CPU burst length observed in a batch. Also, print the same statistics for the estimated non-zero CPU burst lengths. Additionally, compute the absolute difference between the estimated burst length and the actual burst length provided both are non-zero, maintain a running sum of these estimation errors, and print an average per estimation instance.

Make sure to reset all statistics counters at the end of a batch so that the statistics of the next executed batch are printed correctly. A sample output is shown below.

Batch execution time: 7643

Average turn-around time: 4911

Average waiting time: 4842

Completion time: avg: 6620, max: 8658, min: 4582

CPU bursts: count: 56, avg: 136, max: 204, min: 1

CPU burst estimates: count: 56, avg: 117, max: 197, min: 50

CPU burst estimation error: count: 46, avg: 50

Evaluating Scheduling Algorithms

The programs that will be used to form the batches are testloop1.c, testloop2.c, testloop3.c, testloop4.c, and testlooplong.c. These can be found in the xv6-riscv/user/ directory. The testloop1.c program has large CPU bursts interspersed with small I/O bursts (just sleep(1) calls). The testloop2.c program uses yield() instead of sleep() to end each CPU burst. The testloop3.c program uses shorter CPU bursts than testloop2.c. The testloop4.c program has no I/O burst at all except one at the end. The testlooplong.c program has a reasonably large number of CPU bursts interspersed with yield() calls. I have included seven batch files: batch1.txt, batch2.txt, batch3.txt, batch4.txt, batch5.txt, batch6.txt, and batch7.txt. The scheduling policies may have to be changed in these batch files to evaluate different scheduling policies on these batches. The following evaluations need to be conducted-

Comparison between non-preemptive FCFS and preemptive round-robin:

Evaluate batch1.txt for SCHED_NPREEMPT_FCFS and SCHED_PREEMPT_RR. Report the differences in statistics. Explain the observation.

Evaluate batch2.txt for SCHED_NPREEMPT_FCFS and SCHED_PREEMPT_RR. Report the differences in statistics. Explain the observation.

Evaluate batch7.txt for SCHED_NPREEMPT_FCFS and SCHED_PREEMPT_RR. Report the differences in statistics. Explain the observation.

CPU burst estimation error using exponential averaging:

Evaluate batch2.txt and batch3.txt for SCHED_NPREEMPT_SJF and report any observed differences in the CPU burst estimation error. Pay attention to how the following ratio changes: (the average CPU burst estimation error per estimation instance)/(the average CPU burst length). Explain the observation.

Comparison between non-preemptive FCFS and non-preemptive SJF:

Evaluate batch4.txt for SCHED_NPREEMPT_FCFS and SCHED_NPREEMPT_SJF. Report the differences in statistics. Explain the observation.

Comparison between preemptive round-robin and preemptive UNIX:

Evaluate batch5.txt for SCHED_PREEMPT_RR and SCHED_PREEMPT_UNIX. Report the differences in statistics. Explain the observation.

Evaluate batch6.txt for SCHED_PREEMPT_RR and SCHED_PREEMPT_UNIX. Report the differences in statistics. Explain the observation.
