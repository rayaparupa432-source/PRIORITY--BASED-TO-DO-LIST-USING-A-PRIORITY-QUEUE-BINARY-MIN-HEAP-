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
