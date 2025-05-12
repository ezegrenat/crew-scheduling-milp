# Crew Assignment MILP

This project solves a crew scheduling problem using Mixed Integer Linear Programming (MILP). It is a typical example of **task assignment using MILP**, where we allocate workers to tasks under a set of real-world constraints. The model is implemented in Python and solved using IBM CPLEX.

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

> 📄 See `test_A.txt` for an example.


## Variables Used

Let the following variables be defined:

- \( W_{xj} \): Number of hours worked by worker \( j \) in salary band \( x \).
- \( Y_{xj} \in \{0,1\} \): 1 if worker \( j \) works within salary band \( x \); 0 otherwise.
- \( O_{ik} \in \{0,1\} \): 1 if task \( i \) is scheduled in shift \( k \); 0 otherwise.
- \( A_{ij} \in \{0,1\} \): 1 if worker \( j \) is assigned to task \( i \); 0 otherwise.
- \( X_{kj} \in \{0,1\} \): 1 if worker \( j \) works during shift \( k \); 0 otherwise.
- \( V_{dj} \in \{0,1\} \): 1 if worker \( j \) works on day \( d \); 0 otherwise.
- \( \delta_{ikj} \in \{0,1\} \): 1 if worker \( j \) performs task \( i \) during shift \( k \); 0 otherwise.
- \( H_i \in \{0,1\} \): 1 if task \( i \) is completed; 0 otherwise.
- \( \lambda_{ijj'} \in \{0,1\} \): 1 if workers \( j \) and \( j' \) (who have a conflict) are both assigned to task \( i \); 0 otherwise.
- \( \theta_{ii'j} \in \{0,1\} \): 1 if worker \( j \) performs both repetitive tasks \( i \) and \( i' \); 0 otherwise.
- \( \texttt{max\_work} \in \mathbb{Z}_{\geq 0} \): Maximum number of shifts assigned to any worker.
- \( \texttt{min\_work} \in \mathbb{Z}_{\geq 0} \): Minimum number of shifts assigned to any worker.

These variables define the assignment, scheduling, salary tracking, and conflict restrictions required for the optimization model.



