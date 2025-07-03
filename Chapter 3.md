# Chapter 3: Implementing and Running the SA Scheduler

## 3.1 Overview

In this chapter, you will:
- Understand the structure and flow of the Simulated Annealing (SA) optimizer
- Manually run the optimizer script on your EC2 instance
- Modify key optimization parameters to explore their impact
- Review the generated Excel output file and interpret the results

This chapter builds your understanding of what the SA script is doing before we automate it using Airflow in the next chapter.

## 3.2 Key Files and Folders

Inside your project directory (`~/supply_chain_module`), you'll find:

#### `sa_core/`
Contains all core logic for the SA optimizer:
- `main_SA.py` – main execution script
- `initial_solution.py` – generates starting job assignments
- `generate_neighbor.py` – produces nearby solutions
- `calculate_objective.py` – computes total cost of each solution
- `export_SA_results.py` – writes results to Excel
- `feasibility.py` and helpers – enforce constraints

#### `data/`
May contain Excel input files if needed (`results69.xlsx`).

#### `SA_results*.xlsx`
Created when `main_SA.py` is executed — this file contains output schedules and cost data.

## 3.3 Running the Optimizer Script

In your EC2 terminal:

```bash
cd ~/cloud-batch-scheduling-sa
python3 sa_core/main_SA.py
```

You should see console logs for:

- Initial solution cost
- Iterative improvements
- Final best objective value

After completion, check that a results file was created:

```
ls SA_results*.xlsx
```

## 3.4 Task: Modify Optimization Parameters
Open the script:
```
nano sa_core/main_SA.py
```
Find and edit these lines:
```
Tmin = 1
Tmax = 1000
cooling_rate = 0.99
max_iterations = 200
```

- Change Tmax from 1000 to 500
- Change max_iterations from 200 to 100
- Save and rerun the script

> Reflect: Does the output cost change significantly? How fast does the optimizer converge now?

## 3.5 Task: Add Logging for Objective Value
Still in <code>main_SA.py</code>, add this line inside the inner iteration loop:

```
if iteration % 25 == 0:
    print(f"[LOG] Iteration {iteration} — Current Best: {best_objective}")
```
This will help you track progress and convergence speed during execution.

## 3.6 Review the Excel Output
Transfer the output Excel file to your local machine (optional), or open it using pandas in Python:

```
import pandas as pd
df = pd.read_excel("SA_results123.xlsx", sheet_name="Best_Job_Completion_Time")
print(df.head())
```

Sheets include:

- Best_Job_Completion_Time
- Best_Batch_Volume, Best_Batch_Height
- Best_Objective, Best_Rejection_Cost, etc.
