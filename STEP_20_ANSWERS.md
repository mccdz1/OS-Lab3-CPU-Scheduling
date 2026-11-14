# BCA151 Operating Systems Lab Activity 3
## Step 20: Guide Questions and Summarized Answers

**Student Name:** Michael Cadiz  
**Course & Year:** BCA 2nd Year  
**Date:** November 14, 2026  
**Repository:** [https://github.com/mccdz1/OS-Lab3-CPU-Scheduling](https://github.com/mccdz1/OS-Lab3-CPU-Scheduling)  

---

### Question 1: What is the main difference between Multilevel Queue and Multilevel Feedback Queue scheduling?
**Answer:**
* **Multilevel Queue (MLQ):** Processes are permanently assigned to one queue upon arrival and cannot move between queues.
* **Multilevel Feedback Queue (MLFQ):** Processes can dynamically move between queues based on their CPU burst behavior (CPU-heavy jobs move down, waiting jobs move up through aging to prevent starvation).

---

### Question 2: Why does MLQ use different scheduling algorithms for different process queues?
**Answer:**
Different types of processes have different requirements:
* **Interactive processes (Queue 1)** require fast response times, so **Round Robin (RR)** is used to give each process frequent, small slices of CPU time.
* **Batch processes (Queue 2)** prioritize overall throughput, so **FCFS** is used to avoid unnecessary context-switching overhead.

---

### Question 3: How does RMS determine which task receives the CPU first?
**Answer:**
RMS assigns priority based on task period length:
* **Shorter period = Higher priority.**
* The task with the shortest period runs first (e.g., $T_1$ with period 4 ms has higher priority than $T_2$ with period 5 ms and $T_3$ with period 10 ms).

---

### Question 4: Why is RMS considered a fixed-priority scheduling algorithm?
**Answer:**
Because each task's priority is set once before runtime based on its period and **never changes**. Every recurring instance of that task always has the exact same priority.

---

### Question 5: How does EDF determine which task should execute next?
**Answer:**
EDF checks the **absolute deadline** ($\text{Release Time} + \text{Relative Deadline}$) of all ready tasks. The task with the **earliest (closest) deadline** is selected to run next.

---

### Question 6: Why is EDF considered a dynamic-priority scheduling algorithm?
**Answer:**
Because priority is not tied permanently to a task. It is recalculated dynamically for each job instance at runtime based on its absolute deadline, so relative priorities between tasks change over time.

---

### Question 7: What happens when a newly released task has an earlier deadline than the currently running task under EDF?
**Answer:**
The currently running task is immediately **preempted** (paused). Its remaining execution time is preserved in the ready queue, and the CPU is immediately given to the newly released task with the earlier deadline.

---

### Question 8: Why is CPU utilization important in real-time scheduling?
**Answer:**
CPU utilization ($U = \sum \frac{C_i}{T_i}$) measures the total proportion of processor capacity required by all tasks. It provides a mathematical test to determine whether tasks can meet all their deadlines without overloading the system.

---

### Question 9: Can a task set with CPU utilization below 100% still miss a deadline? Explain the conditions under which this may happen.
**Answer:**
**Yes**, a task set can still miss deadlines with $U < 100\%$ under any of the following conditions:
1. **RMS Utilization Limit Exceeded:** Under RMS, if utilization is between $77.98\%$ and $100\%$, tasks may miss deadlines if periods do not align harmonically.
2. **Short Deadlines ($D < T$):** If relative deadlines are shorter than periods, workload demand can spike during short intervals.
3. **Execution Time Overruns:** If a task runs longer than its estimated worst-case execution time ($C$).
4. **Priority Inversion / Resource Blocking:** When a high-priority task is blocked waiting for a resource held by a lower-priority task.
5. **System Overhead:** Context switching, timer interrupts, and OS scheduling delays consume extra CPU cycles.

---

### Question 10: Which algorithm would you recommend for periodic real-time tasks, RMS or EDF? Justify your answer.
**Answer:**
* **Recommend EDF** when you need **maximum CPU utilization** because it is mathematically optimal on uniprocessors and can handle up to $100\%$ CPU capacity without missing deadlines.
* **Recommend RMS** for **safety-critical and embedded systems** because it is simpler to implement, has lower runtime overhead, and behaves predictably under overload (only lower-priority tasks miss deadlines, while high-priority critical tasks continue running safely).
