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

                    | Copy heap to topTask|                            |
                    +-----------------------+                            |

                                |                                        |
                                v                                        |
                    +-----------------------+                            |

                    | Move heap[heapSize-1] |                            |
                    |   to heap & size--  |                            |
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

## 5.3 Database / File Handling Implementation
All tasks are saved inside tasks.txt using a simple line format: priority|order|title. Whenever the program starts, the loadFromFile() function reads each line back into the heap[] array. After loading, it runs Floyd's bottom-up heapify loop starting from (heapSize / 2) - 1 down to 0 so that the valid Min-Heap order is restored in $O(n)$ time.

## 5.4 Integration of Components
The main() function works as the central controller of the application. Once the saved tasks are loaded on startup, it displays a continuous 5-option menu for the user. Every time a task is added (enqueueTask) or completed (completeUrgent), the program immediately updates both the in-memory heap array and the tasks.txt file to keep data synced.

## 5.5 Source Code Repository & Project Structure

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
        getchar(); // clear newline

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
