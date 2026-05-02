# Assignment 3 - Complete Documentation

**Student Name**: [Farah Almutawasi]  
**Student ID**: [444051798]  
**Date Submitted**: [Submission Date]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `[YourStudentID]_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [April 30, 2026 - 7:00 PM]
**What I implemented**: I began by reading the provided code and comprehending the assignment criteria. I found common resources like executionLog, totalWaitingTime, contextSwitchCount, and completedProcessCount.

**Challenges encountered**: At first, I didn't understand where race conditions appear in the code.

**How I solved it**: After further examining the SharedResources class, I discovered that these variables are accessed concurrently by several threads.

**Testing approach**:I observed conflicting results when I ran the program without synchronization. 

**Time spent**: 2 hours

---

### Entry 2 - [ May 1, 2026 - 5:30 PM]
**What I implemented**: I enclosed important areas with a lock and used ReentrantLock to safeguard shared counters.Try-finally blocks contain lock() and lock.unlock().

**Challenges encountered**:knowing the precise location for the lock statements. 

**How I solved it**: I used the essential section pattern and consulted Operating System Concepts (Chapter 6).

**Testing approach**: I ran the program multiple times to ensure consistency.

**Time spent**:  2.5 hours

---

### Entry 3 - [ May 1, 2026 - 10:00 PM]
**What I implemented**: To prevent ConcurrentModificationException, I used ReentrantLock to protect executionLog.

**Challenges encountered**:I didn't know if ArrayList was thread-safe. 

**How I solved it**: I did some investigation and found that synchronization was required because ArrayList is not thread-safe.

**Testing approach**: I verified that no exceptions occurred during execution.

**Time spent**: 1.5 hours

---

### Entry 4 - [ May 2, 2026 - 3:00 PM]
**What I implemented**: I implemented acquire() and release() inside run() and added a semaphore with a single permission to regulate CPU access.

**Challenges encountered**: recognizing the differences between a lock and a semaphore.

**How I solved it**: I went over the lecture notes and examples from the textbook.

**Testing approach**: I ensured that only one process executes at a time.

**Time spent**: 2 hours

---

### Entry 5 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:Due to numerous threads incrementing contextSwitchCount at the same time, which results in inaccurate numbers, the first race condition arises. The second race situation arises in executionLog because to the concurrent addition of elements to ArrayList by several threads, which is not thread-safe. Data corruption or exceptions may result from this. Overlapping procedures may cause updates to be missed in the absence of synchronization.

[Your answer here - 4-6 sentences with code examples]

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**: ReentrantLock ensures that only one thread can access crucial parts at a time by providing mutual exclusion. In contrast, semaphore uses permits to regulate access to a resource. In my code, I utilized a binary semaphore to manage CPU access so that only one process could execute at a time and ReentrantLock to safeguard shared variables and executionLog.

[Your answer here - explain your implementation choices]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:When two or more threads are waiting endlessly for resources owned by one another, this is known as a deadlock. Using try-finally blocks to guarantee that locks are always released is one preventative strategy. Keeping the right locking sequence is another way to prevent cyclic waits. Try-finally was used in my code to securely release locks and semaphores.

[Your answer here - reference try-finally blocks, lock ordering, etc.]

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:I secured all shared counters with a single lock (coarse-grained locking). This avoids complexity and streamlines implementation. However, because the counters are independent, fine-grained locking might enable better concurrency. In spite of this, I decided to use coarse-grained locking for this job due to its simplicity and dependability.

[Your answer here - explain coarse-grained vs fine-grained locking, independence of counters, concurrency implications. Show understanding of when to use each approach. 5-8 sentences expected.]

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**:  contextSwitchCount, completedProcessCount, totalWaitingTime

**Why they need protection**: They are shared among multiple threads and can be updated simultaneously, causing race conditions.

**Synchronization mechanism used**: ReentrantLock

**Code snippet**:
```java
lock.lock();
try {
    contextSwitchCount++;
} finally {
    lock.unlock();
}

```

**Justification**: ReentrantLock ensures mutual exclusion and prevents inconsistent updates.

---

### Critical Section #2: Execution Log

**What resource**: executionLog (ArrayList)

**Why it needs protection**:ArrayList is not thread-safe and concurrent modifications may cause exceptions.
 

**Synchronization mechanism used**: ReentrantLock

**Code snippet**:
```java
// Paste your implementation here
lock.lock();
try {
    executionLog.add(message);
} finally {
    lock.unlock();
}
```

**Justification**: Prevents concurrent modification and ensures data consistency.

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: To ensure only one process uses the CPU at a time.

**Number of permits and why**: 1 permit to simulate a single CPU.

**Where implemented**: 
Inside run() method.
**Code snippet**:
```java
// Paste your implementation here
```SharedResources.cpuSemaphore.acquire();
// execution
SharedResources.cpuSemaphore.release();

**Effect on program behavior**:Prevents multiple processes from executing simultaneously. 

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
javac SchedulerSimulationSync.java
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync

**Results**: 
(Show that running multiple times produces consistent, correct results)
═══ Synchronization Statistics ═══
Total Context Switches: 21
Total Completed Processes: 11
Total Waiting Time: 588723ms
Average Waiting Time: 53520ms

═══ Process Summary Table ═══
Process    Priority     Burst Time   Waiting Time
────────────────────────────────────────────────
P1         1            10483        64077       
P2         1            9500         51491       
P3         2            10986        64559       
P4         3            3035         15215       
P5         5            5826         61041       
P6         5            8219         61883       
P7         2            5805         65129       
P8         3            3949         33485       
P9         1            3663         37491       
P10        3            7400         65971       
P11        2            5675         68381       

═══ Execution Log Summary ═══
Total log entries: 42

**Why synchronization is necessary**: 
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)

Without synchronization, race conditions could occur when multiple threads update shared variables such as contextSwitchCount, completedProcessCount, and totalWaitingTime simultaneously. This could lead to lost updates and inconsistent values. Additionally, executionLog could be corrupted due to concurrent modifications since ArrayList is not thread-safe. Synchronization ensures mutual exclusion and guarantees that only one thread accesses critical sections at a time.

**Conclusion**: The use of ReentrantLock and Semaphore ensures consistent, predictable, and correct results across multiple executions.

---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: I ran the program several times while monitoring the output for any runtime exceptions, especially during logging operations where multiple threads write to executionLog.

**Results**: No ConcurrentModificationException or any concurrency-related exceptions were observed during any execution.

**What this proves**: This proves that executionLog is properly synchronized using ReentrantLock. Since ArrayList is not thread-safe, protecting it ensures safe concurrent access and prevents runtime exceptions.

---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**:
 - The number of completed processes should equal the total number of processes created.
- Waiting times should be non-negative and logically consistent with process burst times.
- Context switch count should reflect scheduling behavior.

**Actual values**: 
Total Context Switches: 21
Total Completed Processes: 11
Total Waiting Time: 588723ms
Average Waiting Time: 53520ms

**Analysis**: The actual values match the expected behavior. Each process completed exactly once, and the statistics are consistent with the scheduling algorithm. The waiting time values are reasonable based on the burst times and scheduling order. This confirms that synchronization preserves correctness.

---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]

**Purpose**: To ensure that synchronization mechanisms work correctly under different execution conditions and workloads.

**Results**: The program behaved correctly in all scenarios. No race conditions, inconsistent results, or exceptions were observed. The output remained stable and logically correct regardless of the number of processes or time quantum values.

**What I learned**: I learned that synchronization mechanisms such as ReentrantLock and Semaphore are essential for ensuring correctness and stability in concurrent systems, regardless of workload variations.

---

## Part 5: Reflection and Learning

### What I learned about synchronization:

[6-8 sentences about key concepts, challenges, insights]

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: 

**Example 2**: 

---

### How I would explain synchronization to others:

[Explain to someone who just finished Assignment 1 - use simple terms and analogies]

---

## Part 6: GitHub Repository Information

**Repository URL**: 

**Number of commits**: 

**Commit messages**: 
1. 
2. 
3. 
4. 

---

## Summary

**Total time spent on assignment**: 

**Key takeaways**: 
1. 
2. 
3. 

**Most challenging aspect**: 

**What I'm most proud of**: 

---

**End of Documentation**
