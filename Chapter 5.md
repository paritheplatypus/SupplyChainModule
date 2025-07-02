# Chapter 5: Scaling Up – Multiple Jobs and Realistic Use Case

## 5.1 Overview

Now that you’ve successfully run a single SA optimization with Airflow, it’s time to scale up.

In this chapter, you will:
- Modify the DAG to run **multiple optimizer jobs** with different parameters
- Save result files with unique names
- (Optional) Upload output to an **S3 bucket**
- Analyze and compare the output results
- Connect the project to a **realistic use case**, such as managing 3D printing operations

## 5.2 Update the DAG for Multiple Seeds

Let’s run the optimizer **3 times**, each with a different random seed.

#### Step 1: Open the DAG

```bash
cd ~/cloud-batch-scheduling-sa/dags
nano sa_optimizer_dag.py
```

#### Step 2: Replace the Bash task with a loop
Here’s how to define multiple BashOperator tasks dynamically:

```python
from airflow import DAG
from airflow.operators.bash import BashOperator
from airflow.utils.dates import days_ago

default_args = {
    'owner': 'airflow',
    'start_date': days_ago(1),
}

with DAG(
    dag_id='run_sa_optimizer_scaled',
    default_args=default_args,
    schedule_interval=None,
    catchup=False,
    description='Run SA optimizer with multiple seeds',
) as dag:

    for seed in [42, 99, 202]:
        run_task = BashOperator(
            task_id=f'run_optimizer_seed_{seed}',
            bash_command=f'cd /home/ec2-user/cloud-batch-scheduling-sa && python3 sa_core/main_SA.py --seed {seed}'
        )
```
> Note: Your main_SA.py must be updated to accept a --seed command-line argument. If it doesn’t already, this is a good mini coding task!

## 5.3 Save Results with Unique Filenames
Update <code>main_SA.py</code> to generate results like <code>SA_results_seed_42.xlsx</code>.

In main_SA.py, add support for seed parsing:

```python
import argparse
parser = argparse.ArgumentParser()
parser.add_argument('--seed', type=int, default=123)
args = parser.parse_args()
seed = args.seed
Update the Excel output filename:

```python
export_SA_results(..., filename=f"SA_results_seed_{seed}.xlsx")
```

## 5.4 (Optional) Upload Output to S3
If your EC2 instance has S3 access (via IAM role or AWS CLI config), you can add this task to the DAG:

```
upload_task = BashOperator(
    task_id=f'upload_results_seed_{seed}',
    bash_command=f'aws s3 cp /home/ec2-user/cloud-batch-scheduling-sa/SA_results_seed_{seed}.xlsx s3://your-bucket/results/'
)

run_task >> upload_task
```

Make sure:

- The S3 bucket exists
- You replace your-bucket with your actual bucket name

## 5.5 Task: Compare Output Results
Use pandas to read and compare objective values:

```
import pandas as pd

for seed in [42, 99, 202]:
    df = pd.read_excel(f'SA_results_seed_{seed}.xlsx', sheet_name='Best_Objective')
    print(f"Seed {seed} Objective:", df.iloc[0,0])
```
> Reflection Prompt: Which seed gave the lowest cost? Why might this be?

## 5.6 Use Case: 3D Printing Shop Scheduling
Imagine this system being used to:
- Assign 3D print jobs to different machines
- Account for material types, weights, and deadlines
- Reduce turnaround time and material waste

By automating this with Airflow, the shop can:
- Optimize jobs every night
- Archive results for reporting
- Scale up as volume grows

## 5.7 Clean Up Resources
When done:

- Stop Airflow:

```
docker-compose down
```

- Terminate your EC2 instance from the AWS Console.

- (Optional) Remove files from S3:

```
aws s3 rm s3://your-bucket/results/ --recursive
```
