# PRIORITY-BASED TO-DO LIST USING A PRIORITY QUEUE (BINARY MIN-HEAP)
# CHAPTER 1: INTRODUCTION
 
# 1.1 Background
Managing daily tasks becomes difficult when multiple deadlines overlap. Most simple to-do lists store entries in the exact order they were typed, forcing a user to read through the entire list just to figure out what needs immediate attention. In Data Structures and Algorithms (DSA), a **Priority Queue** implemented with a **Binary Min-Heap** solves this problem directly. By keeping the highestpriority task at the root of a complete binary tree, the system guarantees that the most urgent task is always ready at the front.
# 1.2 Problem Statement
Traditional to-do lists built using basic arrays or linked lists suffer from clear performance and usability drawbacks:
- Slow Search for Urgent Tasks: Finding the most urgent item in an unsorted array requires O(n) linear comparisons across every stored entry.
-	Costly Element Shifting: Keeping an array sorted by priority requires O(n) element shifts every time a new high-priority task is inserted.
-	Risk of Missed Deadlines: Without automatic urgency ordering, critical tasks easily get buried beneath low-priority notes.
# 1.3 Objectives
-	To study and analyse why linear data structures are inefficient for priority-based task scheduling.
-	To design and build a practical console-based To-Do List in **C** using a custom array-backed **Binary Min-Heap.**
-	To implement core modules for adding tasks, viewing the most urgent task, completing (extracting) the top task, displaying pending tasks, and saving records to a file.
-	To test the application for correctness, equal-priority tie-breaking, and O(log n) heap behavior.
-	To document the full design, C implementation, and future improvements clearly.
# CHAPTER 2: PROJECT PLANNING AND PROBLEM ANALYSIS
 
# 2.1 Initial Team Meeting (Individual Project Planning)
As per the CSE 2B DSA Mini Project guidelines, this work was planned and executed as an individual project ahead of the 30 November 2026 submission deadline. The objective was to implement Topic 3 ("To-do list with priorities: Use a priority queue (heap) so the most urgent task comes out first") in pure C using explicit array-based heap manipulation rather than external library shortcuts.

All phases—including designing the struct Task layout, coding heapifyUp() and heapifyDown(), validating edge cases, and uploading main.c to GitHub—were carried out independently using Visual Studio Code and the GCC C compiler.
# 2.2 Brainstorming and Requirement Identification
Before writing code, three standard data structures were compared for managing prioritized tasks:
| Operation | Unsorted Array | Sorted Array | Binary Min-Heap (Chosen) |
| :--- | :--- | :--- | :--- |
| Add a New Task (Enqueue) | O(1) | O(n) | *O(log n)* |
| View Most Urgent Task (Peek) | O(n) | O(1) | *O(1)* |
| Complete Urgent Task (Dequeue) | O(n) | O(1) | *O(log n)* |
| Rebuild Heap from Saved File | O(n) | O(n log n) | *O(n)* |

**Functional Requirements:**
-	Allow the user to enter a task title along with an integer priority from 1 (Highest Urgency) to 5 (Lowest Urgency).
-	Display the most urgent task immediately without removing it from the queue.
-	Complete and remove the most urgent task, automatically promoting the next most urgent task to the top.
-	Display all pending tasks in urgency order and save data persistently in tasks.txt.
-	
**Non-Functional Requirements:**
-	Logarithmic O(log n) time complexity for insertions and extractions with O(n) space.
-	First-In, First-Out (FIFO) tie-breaking when two tasks share the same priority number.
-	Safe input buffer handling so invalid characters do not cause infinite console loops.
# 2.3 Proposed Solution
The proposed system stores tasks in a fixed-capacity 1D array Task heap[MAX_TASKS] structured as a Complete Binary Tree (Min-Heap). Because a Min-Heap always keeps the smallest key at index 0, setting 1 as the highest urgency ensures the most critical task stays at the root. Each node also stores an incremental order number so tasks with identical priorities are served in the order they were added.
# 2.4 Project Schedule and Task Allocation
| Task / Activity | Assigned Member(s) | Planned Timeline | Status |
| :--- | :--- | :--- | :--- |
| Requirement analysis | Individual (Self) | Nov 18 – Nov 19, 2026 | Completed |
| System design | Individual (Self) | Nov 20 – Nov 21, 2026 | Completed |
| Development (C Coding) | Individual (Self) | Nov 21 – Nov 22, 2026 | Completed |
| Testing & Validation | Individual (Self) | Nov 22 – Nov 23, 2026 | Completed |
| Documentation & GitHub Upload | Individual (Self) | Nov 23 – Nov 25, 2026 | Completed |

# CHAPTER 3: REQUIREMENT ANALYSIS AND FEASIBILITY STUDY
**Hardware and Software Requirements**
| Category | Requirement / Specification |
| :--- | :--- |
| *Processor* | Intel Core i3 / AMD Ryzen 3 or higher |
| *RAM* | Minimum 4 GB |
| *Storage* | 100 MB free disk space |
| *Operating System* | Windows 10/11, Ubuntu Linux, or macOS |
| *Programming Language(s)* | C (ANSI C / C99 / C11 Standard) |
| *Frameworks / Libraries* | Standard C Libraries (<stdio.h>, <stdlib.h>, <string.h>) |
| *Database* | Pipe-delimited Text File Storage (tasks.txt) |
| *Development Tools* | Visual Studio Code (VS Code), GCC (gcc compiler), Git, GitHub |

**Feasibility Summary:** The project is technically straightforward using standard C arrays and file pointers, economically free since only open-source tools are required, and operationally simple for any user to run in a terminal.
# CHAPTER 4: SYSTEM DESIGN AND MODELLING
**4.1 System Architecture**

The program is divided into three clean functional layers:
1. **Console Interface Layer (main):** Displays the interactive menu, reads user choices, and cleans
standard input streams.
2. **Min-Heap Priority Engine:** Uses 0-based array index formulas—parent = (i - 1) / 2, 
left = 2 * i + 1, and right = 2 * i + 2—to maintain heap order through
heapifyUp() and heapifyDown().
3. **File Persistence Layer:** Uses C FILE* operations (fopen, fprintf, fgets) to save tasks to
tasks.txt and reload them on startup.
### Figure 4.1: System Architecture (3-Layer Design)

text
+-------------------------------------------------------+

|           1. Console UI Layer (main loop)             |
|          Menu, scanf/fgets Input Validation           |
+-------------------------------------------------------+
                           ^
                           |
                           v
+-------------------------------------------------------+

|             2. Min-Heap Priority Engine               |
|         Task heap[MAX_TASKS], heapifyUp/Down          |
+-------------------------------------------------------+
                           ^
                           |
                           v
+-------------------------------------------------------+

|              3. File Persistence Layer                |
|            FILE* (tasks.txt Read & Write)             |
+-------------------------------------------------------+
