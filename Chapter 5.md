# Chapter 5: Scaling Up – Multiple Jobs and Realistic Use Case

## 5.1 Overview

In this chapter, you will:
- Modify the DAG to run the optimizer multiple times with different random seeds
- Save output files with unique names
- (Optional) Upload output to S3
- Analyze and compare results

## 5.2 Update the DAG for Multiple Runs

Open the DAG:
```
nano ~/airflow/dags/supply_chain_sa_dag.py
```
Example structure:
```
for seed in [42, 99, 202]:
    run_task = BashOperator(
        task_id=f'run_optimizer_seed_{seed}',
        bash_command=f'python3 /home/ec2-user/supply_chain_module/main_SA.py --seed {seed}'
    )
```
Make sure `main_SA.py` accepts a `--seed` argument:
```
import argparse
parser = argparse.ArgumentParser()
parser.add_argument('--seed', type=int, default=123)
args = parser.parse_args()
```
## 5.3 Generate Unique Output Files

Update your export function:
```
filename = f"SA_results_seed_{seed}.xlsx"
export_SA_results(..., filename=filename)
```
## 5.4 (Optional) Upload to S3

If you want to upload results to S3:
```
upload_task = BashOperator(
    task_id=f'upload_results_seed_{seed}',
    bash_command=f'aws s3 cp /home/ec2-user/supply_chain_module/SA_results_seed_{seed}.xlsx s3://your-bucket/results/'
)
run_task >> upload_task
```
## 5.5 Analyze Results

Example:
```
import pandas as pd
for seed in [42, 99, 202]:
    df = pd.read_excel(f'SA_results_seed_{seed}.xlsx', sheet_name='Best_Objective')
    print(f"Seed {seed} Objective:", df.iloc[0,0])
```
## 5.6 Use Case: 3D Printing Job Scheduling

This approach could help:

- Assign jobs across multiple machines
- Reduce cost, delay, or material waste
- Optimize and log results daily with Airflow

## 5.7 Cleanup

When finished:
- Stop Airflow (optional):
```
pkill airflow
```
- Terminate your EC2 instance if done
- (Optional) Remove files from S3:
```
aws s3 rm s3://your-bucket/results/ --recursive
```
