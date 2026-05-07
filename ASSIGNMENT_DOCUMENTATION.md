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
 
[1-contextSwitchCount: Non-atomic updates cause Lost Updates ,context switch accur mid operation and no mutual exclusion when multiple threads increment simultaneously, leading to inaccurate final stats
2-executionLog: Concurrent access to ArrayList causes ConcurrentModificationException or data corruption, resulting in missing log entries

public static void incrementContextSwitch() {
        // 
        // RACE CONDITION: Multiple threads might read and write simultaneously!
        contextSwitchLock.lock();
        try {
            contextSwitchCount++;
        } finally {
            contextSwitchLock.unlock();
        }
    }
    
    Total Context Switches: 20 ]

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

[Lock: is a mutual exclusion mechanism (Mutex) designed to protect shared data by allowing only one thread to own the lock at a time its a boolean variable indicating lock availability
Semaphore: it is integer variable accessed only through two atomic operation, manages a set of permits to control access to a limited resource
]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

[A deadlock is a situation in which a set of processes are blocked because each process is holding a resource and waiting for another resource acquired by sorge other process 
1- try-finally blocks: (this what i did to prevent deadlocks in my code) i add the lock() or acquire() calls in a try block and placing the unlock() or release() in the finally block, I ensured that a resource is always freed even if the thread crashes or an exception occurs This prevents a Hold and Wait condition

2- lock ordering : ensured that all threads request shared resources in a strictو consistent order rocess must acquire the CPU Semaphore before it can access the Counter Locks eliminated the "Circular Wait" condition]

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

[I implemented separate locks for each counter (fine-grained locking). I made this choice because the three counters (contextSwitchCount, completedProcessCount, and totalWaitingTime) are logically independent resources. By using separate locks, I ensured that a thread updating the context switch count does not block another thread that needs to update the total waiting time ,The main trade-off is that fine-grained locking increases the complexity of the code and requires more care to avoid deadlocks but it provides much better concurrency and higher throughput and a more efficient simulation compared to using a single( coarse-grained) lock ]

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: contextSwitchCount, completedProcessCount,totalWaitingTime

**Why they need protection**:They are shared across multiple process threads, Without protection, concurrent (read-modify-write) operations lead to Race Conditions, where updates are lost and final statistics become inaccurate. 

**Synchronization mechanism used**: 
ReentrantLock (Fine-grained approach with separate locks for each counter)
**Code snippet**:
```java
// Paste your implementation here

public static void incrementContextSwitch() {

        
        contextSwitchLock.lock();
        try {
            contextSwitchCount++;
        } finally {
            contextSwitchLock.unlock();
        }
    }


**Justification**: 
Using separate locks allows threads to update different counters simultaneously
---

### Critical Section #2: Execution Log

**What resource**: 
List<String> executionLog = new ArrayList<>()

**Why it needs protection**:

ArrayList is not thread-safe. If multiple threads attempt to call .add() at the same time, it can lead to data corruption andmissing log entries or a ConcurrentModificationException 

**Synchronization mechanism used**: 
ReentrantLock (logLock)

**Code snippet**:
```java
// Paste your implementation here
```
public static void logExecution(String message) {
        
        // RACE CONDITION: ArrayList is not thread-safe!

        logLock.lock();
        try{
           executionLog.add(message);
        }finally{
            logLock.unlock();
        }
}

**Justification**: 

The lock ensures Mutual Exclusion, meaning only one thread can append to the list at a time

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: control CPU Access,managing the availability of the CPU as a resource

**Number of permits and why**: 
1 permit This creates a Binary Semaphore
**Where implemented**: 
Inside the run() ,runToCompletion() methods of the Process class 
**Code snippet**:
```java
// Paste your implementation here
```
public void runToCompletion() {
       
        try{ SharedResources.cpuSemaphore.acquire();
        try {
           
        }  finally {
            SharedResources.cpuSemaphore.release();
        }} catch (InterruptedException e) {
            System.out.println(Colors.RED + "  ✗ " + name + " was interrupted." + Colors.RESET);
        } 
    }
**Effect on program behavior**:

It forces processes to execute sequentially rather than overlapping

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Verifying that the final counters (Context Switches, Waiting Time) are identical across multiple runs.

**Testing procedure**: 
Run the program 5 times using the same input file and the same Time Quantum

**Results**: 
(Total Context Switches: 20
Total Completed Processes: 11
Total Waiting Time: 361248ms
Average Waiting Time: 32840ms

Total Context Switches: 20
Total Completed Processes: 11
Total Waiting Time: 361779ms
Average Waiting Time: 32889ms

Total Context Switches: 20
Total Completed Processes: 11
Total Waiting Time: 360298ms
Average Waiting Time: 32754ms
)

**Why synchronization is necessary**: 
Without locks, if two threads finish at the same time, they might try to update the completedProcessCount together, and one update would be lost (Race Condition)

**Conclusion**: 

The locks are working perfectly to keep the data consistent

---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: I monitored the console while all processes were running and logging their status at the same time.

**Results**: The program finished all tasks without any red error messages or crashes.

**What this proves**: It proves that my logLock is protecting the shared List, so multiple threads don't break the list when they write to it simultaneously

---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**:Processes: 11  ,  Total Context Switches: 20

**Actual values**:
Total Context Switches: 20
Total Completed Processes: 11 

**Analysis**: The numbers match my manual calculation, which means the synchronization code didn't add any extra or wrong data

---

### Test 4: Different Scenarios
**Scenario tested**: [Using a very small Time Quantum]

**Purpose**: To create a lot of traffic and many context switches to see if the locks can handle the pressure

**Results**: Even with many quick switches, the program didn't freeze or give wrong totals

**What I learned**: I learned that synchronization makes the code strong and able to handle heavy multi-threading without errors

---

## Part 5: Reflection and Learning

### What I learned about synchronization:

[Working on this project made me realize how messy multi-threading can be without proper control. I learned that even a simple task like incrementing a counter can fail because threads overlap, a concept I now know as a Race Condition. The most important lesson for me was learning how to use try-finally blocks.it’s not just about locking a resource, but ensuring it’s released so the whole system doesn’t freeze.I also spent some time understanding why we use a Semaphore for the CPU specifically]

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: Banking Systems: Synchronization is critical when two people try to withdraw money from the same joint account at the exact same time. Without locks, both transactions might read the same initial balance and successfully withdraw money

**Example 2**Online flight Ticket Booking: When booking a seat for a flight  synchronization ensures that the same seat isn't sold to two different customers A lock is placed on the specific seat the moment a user starts the payment process, preventing others from racing to buy it simultaneously

---

### How I would explain synchronization to others:

[Think of synchronization as House Rules for a shared gaming room with only one laptop

The Problem (Race Condition): If everyone tries to grab the controller at the exact same time, the wires get tangled, the game glitches, and it crashes.

The Lock : This is like a Room Key. To play, you must take the key and lock the door.Others wait outside until you finish and return the key This ensures only one person (thread) changes the data at a time.

The Semaphore: This is like the Number of Chairs If there’s only one chair, only one person can sit and play. Even if others are in the room, they must wait for that chair to be free]

---

## Part 6: GitHub Repository Information

**Repository URL**: [https://github.com/MaisaFahad/OS-Assignment3-Maisa-ALAnbar]

**Number of commits**: 4 commits

**Commit messages**: 
1. Set my student Id 445052201
2. Add ReentrantLock to protect counter variables (contextSwitchCount, completedProcessCount, totalWaitingTime)
3.Add ReentrantLock to protect execution log (ArrayList- prevent ConcurrentModificationException) 
4. Add Semaphore to control concurrent CPU access (binary semaphore with 1 permit

---

## Summary

**Total time spent on assignment**: 
4 hours
**Key takeaways**: 
1. Data Safety is Priority
2. Resource Management
3. I realized that always releasing locks using finally blocks is the only way to ensure the system stays stable and avoids deadlocks.

**Most challenging aspect**: 
The hardest part was definitely debugging the Race Conditions. It was frustrating at first because the program would work fine once, then give wrong counter totals or crash the next time. Figuring out exactly where to put the lock() and unlock() calls to protect the ArrayList

**What I'm most proud of**: 

I am most proud of implementing Fine-grained locking for the counters. Instead of just using one big lock for everything, I managed to create separate locks for each resource. It made the code more organized and efficien

---

**End of Documentation**
