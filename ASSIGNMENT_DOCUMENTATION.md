# Assignment 3 - Complete Documentation

**Student Name**: [Maisa Fahad AL-Anbar]  
**Student ID**: [445052201]  
**Date Submitted**: [6-6-2026]

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

### Entry 1 - [3-5-2026,10:59 pm]
**What I implemented**:  Set my student Id 445052201

**Challenges encountered**: it was easy

**How I solved it**: I looked for the designated place and put it there  

**Testing approach**: checked the code output

**Time spent**: 3 min

---

### Entry 2 - [4-5-2026,11:01 pm]
**What I implemented**: Add ReentrantLock to protect counter variables (contextSwitchCount,completedProcessCount, totalWaitingTime)

**Challenges encountered**: I had to ensure that the unlock() operation was placed correctly to avoid potential deadlocks

**How I solved it**: called .unlock() in the finally block This ensures "Mutual Exclusion meaning only one thread can modify the counters at any given time

**Testing approach**: ÷ compared the total number of completed processes with the initial number of processes created there is no deadlocks

**Time spent**: 2 hours

---

### Entry 3 - [4-5-2026,11:15 pm]
**What I implemented**: Add ReentrantLock to protect execution log (ArrayList- prevent ConcurrentModificationException)

**Challenges encountered**: Sometimes the program crashed with an error called ConcurrentModificationException This happened because two or more threads were trying to use the Execution Log at the exact same time Also, without protection some messages were missing from the final list

**How I solved it**: I defined logLock in the SharedResources class هn the logExecution method I called logLock.lock() before adding any string to the listو By using the finally { logLock.unlock(); } I guaranteed that the list remains consistent and the lock is always released

**Testing approach**: I checked the final Execution Log Summary to make sure the total number of entries was correct and that no messages were missing I also made sure the program finished without any "ConcurrentModification" errors

**Time spent**: 30 min

---

### Entry 4 - [5-5-2026,12:05 am]
**What I implemented**: Add Semaphore to control concurrent CPU access (binary semaphore with 1 permit)

**Challenges encountered**: CPU should only run one process at a time , Without a semaphore, all processes try to use the CPU and print on the screen at the same time This makes the output look very messy

**How I solved it**: I used cpuSemaphore.acquire() before the process starts its execution and cpuSemaphore.release() in the finally block after it finished This forces threads to wait for their turn

**Testing approach**: I monitored the terminal output while the program was running, I checked to see if only one process was printing its progress at a time

**Time spent**: 1 hour

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

**Your Answer**:

[Your answer here - 4-6 sentences with code examples]

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

[Your answer here - explain your implementation choices]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

[Your answer here - reference try-finally blocks, lock ordering, etc.]

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

[Your answer here - explain coarse-grained vs fine-grained locking, independence of counters, concurrency implications. Show understanding of when to use each approach. 5-8 sentences expected.]

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 

**Why they need protection**: 

**Synchronization mechanism used**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Justification**: 

---

### Critical Section #2: Execution Log

**What resource**: 

**Why it needs protection**: 

**Synchronization mechanism used**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Justification**: 

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 

**Number of permits and why**: 

**Where implemented**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Effect on program behavior**: 

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
```

**Results**: 
(Show that running multiple times produces consistent, correct results)

**Why synchronization is necessary**: 
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)

**Conclusion**: 

---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 

**Results**: 

**What this proves**: 

---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: 

**Actual values**: 

**Analysis**: 

---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]

**Purpose**: 

**Results**: 

**What I learned**: 

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
