# PRIORITY-BASED TO-DO LIST USING A PRIORITY QUEUE (BINARY MIN-HEAP)
# CHAPTER 1: INTRODUCTION
# 1.1 Background
Managing daily tasks becomes difficult when multiple deadlines overlap and all items are kept in a plain, unordered list. In most simple to-do apps, tasks are stored in the exact order they were typed, which means the user has to read through every single entry just to figure out what needs immediate attention. In Data Structures and Algorithms (DSA), a **Priority Queue** solves this exact problem by associating every item with a priority value so that the most urgent task is always served first.

The standard and most efficient way to implement a Priority Queue is by using a **Binary Min-Heap.** Unlike a regular queue that strictly follows First-In, First-Out (FIFO), a Min-Heap keeps the task with the smallest priority number (where Priority 1 stands for highest urgency) right at the root of the tree. This allows instant access to the most critical task without sorting the entire list every time a change is made.
# 1.2 Problem Statement
Building a task manager using basic linear data structures like an unsorted array or a sorted linked list creates noticeable performance drawbacks as the task list grows:
- **Slow Urgent-Task Lookup:** In an unsorted array or list, finding the most urgent task requires scanning all n items one by one, taking O(n) time.
- **Costly Reordering on Insertion:** Keeping an array permanently sorted by priority allows quick lookup, but inserting a new urgent task at the front requires shifting existing elements, which also takes O(n) time.
-	**Human Oversight:** Without automatic priority ordering, high-urgency assignments or bug fixes easily get buried under routine chores, leading to missed deadlines.
# 1.3 Objectives
- To study and compare linear array approaches against heap-based Priority Queues for task scheduling.
-	To design and build a working command-line To-Do List application in C++ using a custom Binary Min-Heap written from scratch.
-	To implement core operations including adding tasks with urgency levels (1 to 5), viewing the most urgent task, completing/removing the top task, and listing all pending tasks.
-	To ensure equal-priority tasks are handled fairly in arrival order and saved locally in a file so data is not lost when the program closes.
-	To verify correctness through structured test cases and document the complete design and complexity analysis
