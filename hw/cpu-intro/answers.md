# OSTEP cpu-intro Homework
학번(Student ID): _______________
이름(Name): _______________

## Q1
Command: python3 process-run.py -l 5:0,5:0 -p 1 -c

Prediction:
Two processes run in round-robin mode with a time slice of 1. They will switch the CPU every time unit. PID0 runs for 5 time units and finishes, then PID1 runs for 5 time units and finishes. No I/O operations occur.

Result:
Time 0: PID0 RUN
Time 1: PID0 slice ends; PID1 RUN
Time 2: PID1 slice ends; PID0 RUN
Time 3: PID0 slice ends; PID1 RUN
Time 4: PID1 slice ends; PID0 RUN
Time 5: PID0 slice ends; PID1 RUN
Time 6: PID1 slice ends; PID0 RUN
Time 7: PID0 slice ends; PID1 RUN
Time 8: PID0 RUN, finish → DONE; PID1 RUN
Time 9: PID1 RUN, finish → DONE

Analysis:
The prediction is correct. A round-robin scheduler with time slice=1 alternates execution between the two processes. Since there is no I/O, the CPU switches between PID0 and PID1 each time unit until PID0 completes its 5 CPU bursts, after which PID1 continues running until finished.

---

## Q2
Command: python3 process-run.py -l 4:1,4:1 -p 1 -c

Prediction:
Two processes. Each runs for 1 time unit, then triggers I/O and becomes blocked. While blocked, the other process runs. When I/O completes, the process returns to the ready queue.

Result:
Time 0: PID0 RUN
Time 1: PID0 issue I/O → BLOCKED; PID1 RUN
Time 2: PID1 issue I/O → BLOCKED; CPU idle
Time 3: PID0 I/O done → READY; PID0 RUN
Time 4: PID0 issue I/O → BLOCKED; PID1 I/O done → READY; PID1 RUN
Time 5: PID1 issue I/O → BLOCKED; CPU idle
Time 6: PID0 I/O done → READY; PID0 RUN
Time 7: PID0 issue I/O → BLOCKED; PID1 I/O done → READY; PID1 RUN
Time 8: PID1 issue I/O → BLOCKED; CPU idle
Time 9: PID0 I/O done → READY; PID0 RUN, finish → DONE; PID1 I/O done → READY; PID1 RUN
Time 10: PID1 RUN, finish → DONE

Analysis:
The prediction matches the simulation. When a process invokes I/O, it enters the blocked state, and the scheduler selects another ready process. If all processes are blocked, the CPU becomes idle. After I/O completes, the process moves back to the ready queue to wait for CPU time.

---

## Q3
Command: python3 process-run.py -l 3:0,5:0 -p 2 -c

Prediction:
Round-robin scheduler with time slice=2. PID0 and PID1 take turns every 2 time units. PID0 only needs 3 CPU time units and finishes early, then PID1 consumes the remaining CPU time.

Result:
Time 0: PID0 RUN
Time 1: PID0 RUN
Time 2: Time slice ends; PID1 RUN
Time 3: PID1 RUN
Time 4: Time slice ends; PID0 RUN, finish → DONE; PID1 RUN
Time 5: PID1 RUN
Time 6: PID1 RUN
Time 7: PID1 RUN, finish → DONE

Analysis:
Prediction is correct. The time slice is set to 2. PID0 uses 2 units in its first turn, then 1 more unit on its next turn to finish. After PID0 completes, only PID1 remains and uses the CPU until finished.

---

## Q4
Command: python3 process-run.py -l 1:0,1:0,1:0,1:0 -p 4 -c

Prediction:
Four short processes, time slice=4. The first process runs fully and exits, then the second, third and fourth run sequentially. No context switches happen between them.

Result:
Time 0: PID0 RUN, finish → DONE
Time 1: PID1 RUN, finish → DONE
Time 2: PID2 RUN, finish → DONE
Time 3: PID3 RUN, finish → DONE

Analysis:
Prediction is correct. Each process only requires 1 time unit of CPU. Since the time slice is 4, every process completes before its time slice expires. No preemptive context switch occurs.

---

## Q5
Command: python3 process-run.py -l 1:0,4:0,1:0,1:0 -p 2 -c

Prediction:
Round-robin scheduler, time slice=2. Four processes without I/O. They take turns every 2 time units. PID0, PID2, PID3 finish quickly, then PID1 uses remaining CPU time.

Result:
Time 0: PID0 RUN
Time 1: PID0 RUN, finish → DONE; PID1 RUN
Time 2: PID1 RUN
Time 3: Time slice ends; PID2 RUN
Time 4: PID2 RUN, finish → DONE; PID3 RUN
Time 5: PID3 RUN, finish → DONE; PID1 RUN
Time 6: PID1 RUN
Time 7: Time slice ends; PID1 RUN
Time 8: PID1 RUN
Time 9: Time slice ends; PID1 RUN
Time 10: PID1 RUN
Time 11: Time slice ends; PID1 RUN
Time 12: PID1 RUN
Time 13: Time slice ends; PID1 RUN
Time 14: PID1 RUN
Time 15: Time slice ends; PID1 RUN
Time 16: PID1 RUN
Time 17: Time slice ends; PID1 RUN
Time 18: PID1 RUN
Time 19: Time slice ends; PID1 RUN
Time 20: PID1 RUN
Time 21: Time slice ends; PID1 RUN
Time 22: PID1 RUN, finish → DONE

Analysis:
The prediction is correct. Round-robin time slice =2. Processes without I/O are preempted only when time slice expires. After PID0, PID2, PID3 complete, the scheduler keeps switching to PID1 until it finishes all its required CPU time.

---
## Q6 Command: python3 process-run.py -l 1:1,4:1,1:1,1:1 -p 1 -c
Prediction:
Round‑robin scheduler with time slice 1. The scheduler switches CPU every single time unit. Each process will run for 1 CPU time unit and then issue an I/O operation and become blocked. While processes are blocked waiting for I/O, the CPU may go idle. After I/O finishes, processes return to the ready queue to wait for CPU.
Result:
Time 0: PID0 RUN
Time 1: PID0 issue I/O → BLOCKED; PID1 RUN
Time 2: PID1 issue I/O → BLOCKED; PID2 RUN
Time 3: PID2 issue I/O → BLOCKED; PID3 RUN
Time 4: PID3 issue I/O → BLOCKED; All processes blocked
Time 5: All processes blocked, CPU idle
Time 6: All processes blocked, CPU idle
Time 7: PID0 I/O done → READY; PID0 RUN
Time 8: PID0 finishes → DONE; PID1 I/O done → READY; PID1 RUN
Time 9: PID1 issue I/O → BLOCKED; PID2 I/O done → READY; PID2 RUN
Time 10: PID2 issue I/O → BLOCKED; PID3 I/O done → READY; PID3 RUN
Time 11: PID3 finishes → DONE; PID2 I/O done → READY; PID2 RUN
Time 12: PID2 finishes → DONE; PID1 BLOCKED
Time 13: PID1 BLOCKED
Time 14: PID1 BLOCKED
Time 15: PID1 I/O done → READY; PID1 RUN
Time 16: PID1 issue I/O → BLOCKED
Time 17: PID1 BLOCKED
Time 18: PID1 BLOCKED
Time 19: PID1 BLOCKED
Time 20: PID1 BLOCKED
Time 21: PID1 BLOCKED
Time 22: PID1 I/O done → READY; PID1 RUN
Time 23: PID1 issue I/O → BLOCKED
Time 24: PID1 BLOCKED
Time 25: PID1 BLOCKED
Time 26: PID1 BLOCKED
Time 27: PID1 BLOCKED
Time 28: PID1 BLOCKED
Time 29: PID1 I/O done → READY; PID1 RUN, finishes all work → DONE
Analysis:
The prediction is correct. Time slice is set to 1. Every process runs one time unit and then triggers an I/O operation and enters blocked state. When all processes are waiting for I/O, the CPU becomes idle. Once I/O completes, the process transitions back to the ready queue and waits to be scheduled on‑CPU.


---

## Q7
Command: python3 process-run.py -l 3:0,5:0 -p 1 -c

Prediction:
Round-robin scheduler with time slice 1. Two processes with no I/O. They alternate on the CPU every time unit. PID0 needs 3 CPU time units and will finish first, then PID1 continues to run until it completes its 5 time units.

Result:
Time 0: PID0 RUN
Time 1: PID0 time slice ends; PID1 RUN
Time 2: PID1 time slice ends; PID0 RUN
Time 3: PID0 time slice ends; PID1 RUN
Time 4: PID0 RUN, finishes → DONE; PID1 RUN
Time 5: PID1 time slice ends; PID1 RUN
Time 6: PID1 time slice ends; PID1 RUN
Time 7: PID1 time slice ends; PID1 RUN
Time 8: PID1 RUN, finishes → DONE

Analysis:
The prediction is correct. The round-robin scheduler uses time slice = 1. PID0 and PID1 take turns using the CPU every 1 time unit, as there are no I/O operations. After PID0 uses its required 3 CPU time units and completes, the scheduler only runs PID1 until it consumes its remaining 5 time units and finishes.
git add hw/cpu-intro/answers.md

