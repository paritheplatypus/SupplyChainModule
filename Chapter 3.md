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
- `feasibility.py` and other helpers – enforce constraints
- Input files: `results69.xlsx` (and others)

## 3.3 Running the Optimizer via Airflow

Your optimizer is triggered via Airflow.

- Go to `http://<your-ec2-ip>:8080`
- Log in with username `admin` and password `admin123`
- Find the DAG named `supply_chain_simulated_annealing`
- Toggle it "on" and click the ▶️ button to trigger

Airflow will execute `main_SA.py`, and an output Excel file will be generated in the same directory.

## 3.4 Modify Optimization Parameters

To experiment with parameters:
```
nano ~/supply_chain_module/main_SA.py
```
Find these lines:
```
Tmin = 1
Tmax = 1000
cooling_rate = 0.99
max_iterations = 200
```
Try changing:

- Tmax from 1000 to 500
- max_iterations from 200 to 100

Save and trigger the DAG again from Airflow.

## 3.5 Add Logging to Track Progress

Inside `main_SA.py`, add:
```
if iteration % 25 == 0:
    print(f"[LOG] Iteration {iteration} — Current Best: {best_objective}")
```
Check the Airflow task logs to view progress.

## 3.6 Review the Excel Output

Output Excel files can be inspected via pandas:

```
import pandas as pd
df = pd.read_excel("SA_results123.xlsx", sheet_name="Best_Job_Completion_Time")
print(df.head())
```
