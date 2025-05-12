# Crew Assignment MILP

This project solves a crew scheduling problem using Mixed Integer Linear Programming (MILP). It is a typical example of **task assignment**, where we allocate workers to tasks under a set of real-world constraints. The model is implemented in Python and solved using IBM CPLEX.

## Problem Overview

We are given a set of work **orders** and a pool of **workers**. Each order:
- Requires a certain number of workers.
- Contributes a fixed benefit if completed.

Workers:
- Are paid based on the number of hours worked, following a piecewise linear cost structure.

### Objective

Maximize:
- Total benefit from completed orders  
Minus  
- Total worker cost

### Main Constraints

- A worker can perform **at most one task per shift**.
- A worker can work on **at most 5 days** out of a 6-day schedule.
- The **difference in workload** (number of tasks) between the most and least loaded workers is at most **8**.
- Some orders must be done **sequentially**.
- Some orders are **mutually exclusive** (cannot be assigned to the same worker consecutively).
- Some orders are **repetitive** (ideally not assigned to the same worker).
- Optional **soft constraints**:
  - Avoid assigning workers with **known conflicts** to the same order.
  - Avoid assigning **repetitive orders** to the same worker.

## Test Case Format

Input files are plain `.txt` files with the following structure:

1. Number of workers  
2. Number of orders  
3. One line per order:  
   `<order_id> <benefit> <required_workers>`  
4. Number of worker conflicts  
5. One line per conflict:  
   `<worker_1> <worker_2>`  
6. Number of order chains  
7. One line per pair:  
   `<order_before> <order_after>`  
8. Number of order conflicts  
9. One line per pair:  
   `<order_1> <order_2>`  
10. Number of repeated orders  
11. One line per pair:  
   `<order_1> <order_2>`

>  See `test_A.txt` for an example.
>
> ## Model Details

For a more detailed explanation of the assignment problem and the exact MILP model used, including all variables, constraints, and objective formulation, please refer to [`model_description.pdf`](./model_description.pdf) in this repository.



## Model explaination
In the file "model_description" you can read a detail about what the problem is about and the MILP problem.



