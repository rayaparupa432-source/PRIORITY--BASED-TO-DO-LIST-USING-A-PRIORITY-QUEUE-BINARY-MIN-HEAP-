# PRIORITY-BASED TO-DO LIST USING A PRIORITY QUEUE (BINARY MIN-HEAP)
https://rayaparupa432-source.github.io/PRIORITY--BASED-TO-DO-LIST-USING-A-PRIORITY-QUEUE-BINARY-MIN-HEAP-/
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

All phases-including designing the struct Task layout, coding heapifyUp() and heapifyDown(), validating edge cases, and uploading main.c to GitHub-were carried out independently using Visual Studio Code and the GCC C compiler.
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
| Documentation & GitHub Upload | Individual (Self) | Nov 23 – Nov 26, 2026 | Completed |

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
2. **Min-Heap Priority Engine:** Uses 0-based array index formulas-parent = (i - 1) / 2, 
left = 2 * i + 1, and right = 2 * i + 2-to maintain heap order through
heapifyUp() and heapifyDown().
3. **File Persistence Layer:** Uses C FILE* operations (fopen, fprintf, fgets) to save tasks to
tasks.txt and reload them on startup.
<h3>Figure 4.1.a: System Architecture (3-Layer Design)</h3>

<pre>
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
</pre>

<h3>Figure 4.1.b: Array-Backed Binary Min-Heap Mapping (heap = Most Urgent)</h3>

<pre>
                 +-------------------------+

                 |    P1: Submit Report    |
                 |     (Index 0 - Root)    |
                 +-------------------------+
                       /             \
                      /               \
                     v                 v
      +---------------------+   +---------------------+

      | P2: Fix Memory Bug  |   |  P3: Prepare Viva   |
      |      (Index 1)      |   |      (Index 2)      |
      +---------------------+   +---------------------+
             /         \
            /           \
           v             v
+-----------------+ +-----------------+

| P4: Backup Files| | P5: Clean Notes |
|    (Index 3)    | |    (Index 4)    |
+-----------------+ +-----------------+
</pre>

## 4.2 Data Flow Diagram (DFD), Structure Design & Flowchart

### Context-Level DFD (Level 0)
In this system, the *User* interacts directly with the main process (0.0 Priority To-Do List Manager) by giving task details and selecting menu options. Based on the input, the manager updates the tasks inside the in-memory Min-Heap and keeps the records synced with the text file data store (D1: tasks.txt). Finally, it fetches and displays the highest-priority task back to the user.

<pre>
+----------+   Task Details & Menu Choices    +-------------------------------+

|          | -------------------------------> |                               |
|   User   |                                  |  0.0 Priority To-Do List      |
|          | <------------------------------- |      Manager (Main Process)   |
+----------+    Most Urgent Task & Status     +-------------------------------+
                                                        ^             |
                                    Reload Saved Tasks  |             | Sync & Save Tasks
                                                        |             v
                                              +-------------------------------+

                                              |         D1: tasks.txt         |
                                              +-------------------------------+
</pre>

### Data Structure Design (struct Task)
To store all details of a single task together, we defined a custom structure named Task. Each element in our heap array holds three fields:
* int priority: Stores the urgency level of the task from 1 (most urgent) to 5 (least urgent).
* int order: Acts as a simple FIFO counter to track insertion order. If two tasks have the same priority, the one added earlier gets executed first.
* char title: A character array to hold the name or description of the task.

### Operational Flowchart (Completing the Most Urgent Task)
When the user chooses to complete the top-priority task, the program follows these exact steps:
1. *Start* the operation and check if the heap is empty (heapSize == 0).
2. *If empty:* Print a warning message that no tasks are available and *End* the process.
3. *If not empty:* Save the root element (heap) into a temporary variable topTask.
4. Move the last task in the array (heap[heapSize - 1]) to the root position (heap) and reduce heapSize by 1.
5. Call heapifyDown(0) to restore the min-heap property.
6. Call saveToFile() to update tasks.txt, print the completed topTask on the screen, and *End*.

<pre>
                    +-----------------------+

                    |         Start         |
                    +-----------------------+
                                |
                                v
                    +-----------------------+       Yes      +-----------------------+

                    |   Is heapSize == 0 ?  | -------------> |  Print Empty Warning  |
                    +-----------------------+                +-----------------------+

                                |                                        |
                                | No                                     |
                                v                                        |
                    +-----------------------+                            |

                    | Copy heap to topTask|                              |
                    +-----------------------+                            |

                                |                                        |
                                v                                        |
                    +-----------------------+                            |

                    | Move heap[heapSize-1] |                            |
                    |   to heap & size--  |                              |
                    +-----------------------+                            |

                                |                                        |
                                v                                        |
                    +-----------------------+                            |

                    |  Run heapifyDown(0)   |                            |
                    +-----------------------+                            |

                                |                                        |
                                v                                        |
                    +-----------------------+                            |

                    |  Call saveToFile() &  |                            |
                    |    Print topTask      |                            |
                    +-----------------------+                            |

                                |                                        |
                                +--------------------+-------------------+
                                                     |
                                                     v
                                           +-------------------+

                                           |        End        |
                                           +-------------------+
</pre>

---

## 4.3 Algorithm / Logic

### Algorithm 1: Enqueue Task (enqueueTask) — Time Complexity: O(log n)
1. First, check if heapSize < MAX_TASKS. If the array is already full, print an overflow warning and return.
2. Create newTask with {priority, ++taskCounter, title}, place it at the end of the array (heap[heapSize]), and increment heapSize.
3. Call heapifyUp(heapSize - 1):
   * Keep checking while index > 0.
   * Compare heap[index] with its parent at heap[(index - 1) / 2].
   * If the current task is more urgent than its parent, swap both structures and update index to the parent's position; otherwise, stop the loop.

### Algorithm 2: Dequeue Most Urgent Task (completeUrgent) — Time Complexity: O(log n)
1. Check if heapSize == 0. If true, print an underflow message and return immediately.
2. Store topTask = heap, overwrite heap with the last element
3. 3. Call heapifyDown(0):
   * Find the left child (2 * index + 1) and right child (2 * index + 2).
   * Compare the current node with its children and swap it with the most urgent child if needed.
   * Repeat this process moving down the tree until the heap order is restored.
4. Save the updated heap back to tasks.txt and display the completed topTask to the user.
 # CHAPTER 5: IMPLEMENTATION

## 5.1 Development Environment
I wrote this project in standard C using Visual Studio Code and compiled it with the command gcc -Wall -O2 main.c -o todo. For version control and hosting the source code, I used Git and GitHub.

## 5.2 Database / File Handling Implementation
All tasks are saved inside tasks.txt using a simple line format: priority|order|title. Whenever the program starts, the loadFromFile() function reads each line back into the heap[] array. After loading, it runs Floyd's bottom-up heapify loop starting from (heapSize / 2) - 1 down to 0 so that the valid Min-Heap order is restored in $O(n)$ time.

## 5.3 Integration of Components
The main() function works as the central controller of the application. Once the saved tasks are loaded on startup, it displays a continuous 5-option menu for the user. Every time a task is added (enqueueTask) or completed (completeUrgent), the program immediately updates both the in-memory heap array and the tasks.txt file to keep data synced.

## 5.4 Source Code Repository & Project Structure

### Project Directory Structure
<pre>
dsa-priority-todo-c/
├── main.c          # Complete C source code with custom Binary Min-Heap
├── tasks.txt       # Auto-generated persistent task file
└── README.md       # Project documentation and build instructions
</pre>

<h3>Complete C Source Code (<code>main.c</code>)</h3>

<pre>
#include &lt;stdio.h&gt;
#include &lt;string.h&gt;

#define MAX_TASKS 100
#define MAX_TITLE 100
#define FILENAME "tasks.txt"

typedef struct {
    int priority;
    int order;
    char title[MAX_TITLE];
} Task;

Task heap[MAX_TASKS];
int heapSize = 0, taskCounter = 0;

int isMoreUrgent(Task a, Task b) {
    if (a.priority != b.priority)
        return a.priority &lt; b.priority;
    return a.order &lt; b.order;
}

void swapTasks(Task *a, Task *b) {
    Task temp = *a;
    *a = *b;
    *b = temp;
}

void heapifyUp(int index) {
    while (index &gt; 0 &amp;&amp; isMoreUrgent(heap[index], heap[(index - 1) / 2])) {
        swapTasks(&amp;heap[index], &amp;heap[(index - 1) / 2]);
        index = (index - 1) / 2;
    }
}

void heapifyDown(int index) {
    int smallest = index, left = 2 * index + 1, right = 2 * index + 2;
    if (left &lt; heapSize &amp;&amp; isMoreUrgent(heap[left], heap[smallest])) smallest = left;
    if (right &lt; heapSize &amp;&amp; isMoreUrgent(heap[right], heap[smallest])) smallest = right;
    if (smallest != index) {
        swapTasks(&amp;heap[index], &amp;heap[smallest]);
        heapifyDown(smallest);
    }
}

void saveToFile() {
    FILE *fp = fopen(FILENAME, "w");
    if (!fp) return;
    for (int i = 0; i &lt; heapSize; i++)
        fprintf(fp, "%d|%d|%s\n", heap[i].priority, heap[i].order, heap[i].title);
    fclose(fp);
}

void loadFromFile() {
    FILE *fp = fopen(FILENAME, "r");
    if (!fp) return;
    heapSize = 0;
    while (heapSize &lt; MAX_TASKS &amp;&amp; fscanf(fp, "%d|%d|%[^\n]\n", &amp;heap[heapSize].priority, &amp;heap[heapSize].order, heap[heapSize].title) == 3) {
        if (heap[heapSize].order &gt; taskCounter) taskCounter = heap[heapSize].order;
        heapSize++;
    }
    fclose(fp);
    for (int i = (heapSize / 2) - 1; i &gt;= 0; i--)
        heapifyDown(i);
}

void enqueueTask(int priority, char title[]) {
    if (heapSize &gt;= MAX_TASKS) {
        printf("Error: Task list is full!\n");
        return;
    }
    heap[heapSize].priority = priority;
    heap[heapSize].order = ++taskCounter;
    strcpy(heap[heapSize].title, title);
    heapifyUp(heapSize++);
    saveToFile();
    printf("Task added successfully!\n");
}

void viewUrgent() {
    if (heapSize == 0) printf("No pending tasks in the list.\n");
    else printf("Most Urgent Task -&gt; [Priority %d] %s\n", heap.priority, heap.title);
}

void completeUrgent() {
    if (heapSize == 0) {
        printf("No tasks available to complete.\n");
        return;
    }
    printf("Completed &amp; Removed -&gt; [Priority %d] %s\n", heap.priority, heap.title);
    heap = heap[--heapSize];
    heapifyDown(0);
    saveToFile();
}

void displayAllTasks() {
    if (heapSize == 0) {
        printf("The priority to-do list is currently empty.\n");
        return;
    }
    Task backupHeap[MAX_TASKS];
    int backupSize = heapSize;
    memcpy(backupHeap, heap, sizeof(heap));

    printf("\n--- Pending Tasks (Most Urgent First) ---\n");
    for (int rank = 1; heapSize &gt; 0; rank++) {
        printf("%d. [Priority %d] %s\n", rank, heap.priority, heap.title);
        heap = heap[--heapSize];
        heapifyDown(0);
    }
    memcpy(heap, backupHeap, sizeof(backupHeap));
    heapSize = backupSize;
}

int main() {
    loadFromFile();
    int choice, priority;
    char title[MAX_TITLE];

    while (1) {
        printf("\n=== DSA MINI PROJECT: PRIORITY TO-DO LIST ===\n");
        printf("1. Add Task\n2. View Most Urgent Task\n3. Complete Most Urgent Task\n4. Display All Pending Tasks\n5. Exit\n");
        printf("Enter your choice (1-5): ");
        scanf("%d", &amp;choice);
        getchar(); 

        if (choice == 1) {
            printf("Enter task title: ");
            fgets(title, sizeof(title), stdin);
            title[strcspn(title, "\n")] = '\0';

            printf("Enter priority (1-5): ");
            scanf("%d", &amp;priority);
            if (priority &lt; 1 || priority &gt; 5) printf("Invalid priority!\n");
            else enqueueTask(priority, title);
        } 
        else if (choice == 2) viewUrgent();
        else if (choice == 3) completeUrgent();
        else if (choice == 4) displayAllTasks();
        else if (choice == 5) {
            printf("Exiting program. All tasks saved to %s.\n", FILENAME);
            break;
        } 
        else printf("Invalid choice!\n");
    }
    return 0;
}
</pre>

# CHAPTER 6: TESTING AND VALIDATION

## 6.1 Test Cases and Results

| Test ID | Test Scenario | Input | Expected Output | Observed | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| *TC-01* | Out-of-order insertion (heapifyUp) | ("Buy notes", 4), ("Fix bug", 1), ("Lab file", 2) | "Fix bug" bubbles up to root heap | [Priority 1] Fix bug at root | Pass |
| *TC-02* | Peek most urgent task ($O(1)$) | Select Option 2 | Displays [Priority 1] Fix bug without removing | Displayed without removal | Pass |
| *TC-03* | Complete urgent task (heapifyDown) | Select Option 3 | Removes Priority 1; promotes Priority 2 to root | Lab file became new root | Pass |
| *TC-04* | Equal priority tie-breaking | Add ("Task A", 2) then ("Task B", 2) | "Task A" extracted before "Task B" (FIFO) | "Task A" dequeued first | Pass |
| *TC-05* | Invalid input & empty queue | Priority 7 or Dequeue on empty heap | Prints clean error message without crashing | Handled safely | Pass |

## 6.2 Error Handling and Validation
* *Input Buffer Cleanup:* After reading numeric menu choices with scanf("%d", ...), leftover newline characters are cleared using getchar() so fgets can safely read the next task title without skipping input.
* *Boundary & Bounds Checks:* The program rejects any priority value outside 1–5, prevents array overflow beyond MAX_TASKS, and blocks empty-heap extraction whenever heapSize == 0.

## 6.3 Testing Summary
All 5 test cases passed smoothly during GCC compilation with -Wall enabled (zero compiler warnings). Adding the monotonic order counter cleanly resolved tie-breaking ambiguity whenever multiple tasks shared the same priority level.

## 6.4 Sample Program Output

<pre>
=== DSA MINI PROJECT: PRIORITY TO-DO LIST ===
1. Add Task
2. View Most Urgent Task
3. Complete Most Urgent Task
4. Display All Pending Tasks
5. Exit
Enter your choice (1-5): 1
Enter task title: Study C
Enter priority (1-5): 3
Task added successfully!

=== DSA MINI PROJECT: PRIORITY TO-DO LIST ===
1. Add Task
2. View Most Urgent Task
3. Complete Most Urgent Task
4. Display All Pending Tasks
5. Exit
Enter your choice (1-5): 1
Enter task title: Submit Project
Enter priority (1-5): 1
Task added successfully!

=== DSA MINI PROJECT: PRIORITY TO-DO LIST ===
1. Add Task
2. View Most Urgent Task
3. Complete Most Urgent Task
4. Display All Pending Tasks
5. Exit
Enter your choice (1-5): 4

--- Pending Tasks (Most Urgent First) ---
1. [Priority 1] Submit Project
2. [Priority 3] Study C

=== DSA MINI PROJECT: PRIORITY TO-DO LIST ===
1. Add Task
2. View Most Urgent Task
3. Complete Most Urgent Task
4. Display All Pending Tasks
5. Exit
Enter your choice (1-5): 3
Completed &amp; Removed -&gt; [Priority 1] Submit Project

=== DSA MINI PROJECT: PRIORITY TO-DO LIST ===
1. Add Task
2. View Most Urgent Task
3. Complete Most Urgent Task
4. Display All Pending Tasks
5. Exit
Enter your choice (1-5): 5
Exiting program. All tasks saved to tasks.txt.
</pre>

### Output Screenshot
<img width="4080" height="3060" alt="1000202266" src="https://github.com/user-attachments/assets/24f2954c-df32-46ff-8141-91668ac25600" />
<img width="4080" height="3060" alt="1000202267" src="https://github.com/user-attachments/assets/bf988d16-0eee-4ef3-b8a7-5aa5d5844ae0" />

### Execution Video

# CHAPTER 7: CONCLUSION AND FUTURE SCOPE

## 7.1 Conclusion
Building this *Priority-Based To-Do List* clearly shows how an array-backed Binary Min-Heap Priority Queue in C works much faster than a regular linear list when managing urgent tasks. All the main goals of the project—including $O(\log n)$ insertion and deletion, $O(1)$ lookup for the most urgent task, $O(n)$ bottom-up file loading, FIFO tie-breaking, and permanent file storage—were successfully achieved. Currently, the main limitations are the fixed array limit (MAX_TASKS = 100) and the single-user terminal interface.

## 7.2 Future Scope
* *Dynamic Memory Allocation:* Replace the static array with dynamic memory reallocation (malloc / realloc) so the task list can grow automatically without any fixed size limit.
* *Automatic Priority Escalation:* Add due dates to tasks so their urgency level increases automatically as the deadline gets closer.
* *Visual Dashboard:* Build a simple visual or web interface that displays the live Binary Heap tree structure alongside the pending task list.

---

# REFERENCES

1. T. H. Cormen, C. E. Leiserson, R. L. Rivest, and C. Stein, Introduction to Algorithms, 4th ed. Cambridge, MA, USA: MIT Press, 2022.
2. E. Horowitz, S. Sahni, and S. Anderson-Freed, Fundamentals of Data Structures in C, 2nd ed. Silicon Press, 2008.
3. B. W. Kernighan and D. M. Ritchie, The C Programming Language, 2nd ed. Englewood Cliffs, NJ, USA: Prentice Hall, 1988.
