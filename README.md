# OS Simulator — CPU Scheduling, Banker's Algorithm & Page Replacement

A Java console application that simulates core Operating System concepts: CPU scheduling algorithms, deadlock avoidance (Banker's Algorithm), and page replacement strategies. Built as an educational tool to help students understand how these algorithms behave in practice.

## Features

### 1. CPU Scheduling Algorithms
- **First-Come, First-Served (FCFS)** — non-preemptive, executes processes in arrival order
- **Round Robin (RR)** — preemptive, each process gets a fixed time quantum
- **Shortest Job First — Non-Preemptive (SJF-NP)** — runs the shortest available burst to completion
- **Shortest Job First — Preemptive (SRTF)** — always runs the process with the shortest remaining time

Each algorithm reports, per process:
- Arrival Time, Burst Time
- Waiting Time, Turnaround Time

...plus average waiting time, average turnaround time, and a Gantt chart of execution order.

### 2. Banker's Algorithm
- Automatic Need Matrix calculation (`Need = Max - Allocation`)
- Safety state verification with safe sequence output
- Additional resource request handling (grant/deny based on resulting system safety)

### 3. Page Replacement Algorithms
- **FIFO** (First-In-First-Out)
- **Optimal**
- **LRU** (Least Recently Used)

Outputs page faults, hit ratio, and miss ratio for each algorithm side-by-side.

### Robust Input Handling
All user input is validated with `try-catch` blocks. Non-integer values, negative numbers, and mismatched array sizes are caught and the user is re-prompted instead of crashing the program.

## Requirements

- Java JDK 17 or newer
- Any Java IDE (NetBeans 16, IntelliJ, VS Code) or just a terminal with `javac`/`java`

## How to Run

```bash
# Compile
javac CompleteOSSimulator.java

# Run
java CompleteOSSimulator
```

Or open the project in your IDE of choice and run `CompleteOSSimulator.java` as the main class.

## Usage

On launch you'll see the main menu:

```
===== OS Simulator Menu =====
1. CPU Scheduling Algorithms
2. Banker's Algorithm
3. Page Replacement Algorithms
4. Exit
```

### CPU Scheduling
1. Enter the number of processes.
2. Choose whether to provide arrival times (defaults to 0 for all if skipped).
3. Enter burst times for each process.
4. Pick an algorithm (FCFS / Round Robin / SJF-NP / SJF-P). Round Robin will additionally ask for a quantum.
5. View the results table and Gantt chart. You can run multiple algorithms on the same process set without re-entering data.

**Example (FCFS):**
```
Process ID   Arrival Time   Burst Time   Waiting Time   Turnaround Time
1            0              10           0 ms           10 ms
2            3              4            7 ms           11 ms
3            6              5            8 ms           13 ms
4            9              2            10 ms          12 ms
Average Waiting Time: 6.25 ms
Average Turnaround Time: 11.50 ms

Gantt Chart:
| P1 | P2 | P3 | P4 |
0    10   14   19   21
```

### Banker's Algorithm
1. Enter number of processes and resource types.
2. Enter the Available resources vector.
3. Enter the Allocation matrix and Max matrix, row by row.
4. The program computes the Need matrix, checks if the system is in a safe state, and prints a safe sequence if one exists.
5. Optionally request additional resources for a process — the request is granted only if it keeps the system in a safe state.

### Page Replacement
1. Enter the frame size.
2. Enter the page reference string (space-separated).
3. The program runs FIFO, Optimal, and LRU on the same reference string and prints a comparison table of page faults, hit ratio, and miss ratio.

## Project Structure

```
CompleteOSSimulator.java   # Main class — menu system, input helpers, scheduling algorithms
class Process               # Holds id, arrival/burst/waiting/turnaround times, execution log
class BankerAlgorithm       # Need matrix, safety check, resource request handling
class PageReplacement       # FIFO / Optimal / LRU implementations
```

## Key Concepts Demonstrated

- **Convoy effect** in FCFS, where short processes wait behind long ones
- **Preemption** in Round Robin and SJF (Preemptive)
- **Starvation risk** in SJF when short jobs keep arriving
- **Deadlock avoidance** via the Banker's Algorithm safety check
- **Belady's anomaly potential** when comparing FIFO vs. Optimal/LRU page replacement

## Results Summary

- FCFS and SJF (Non-preemptive) produced similar results for the tested workload.
- Round Robin had higher average waiting time due to time-slicing overhead.
- SJF (Preemptive / SRTF) gave the lowest average waiting and turnaround times.
- For page replacement, Optimal consistently had the fewest page faults, followed by LRU, then FIFO.

## Extending the Simulator

- Add **Priority Scheduling** as a new algorithm.
- Extend the `Process` class with fields like `priority` or `ioTime`.
- Add more page replacement strategies (e.g., Second-Chance/Clock).
- Swap the console interface for a GUI (JavaFX/Swing) while reusing the existing logic classes.
