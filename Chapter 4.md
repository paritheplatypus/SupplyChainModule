# Chapter 4: Hello World DAG – First Airflow Workflow

## 4.1 Overview

Now that you’ve successfully run the optimizer manually, it’s time to automate it using **Apache Airflow**.

In this chapter, you will:
- Create a basic DAG that runs the optimizer once
- Deploy the DAG to your Airflow instance
- Trigger it via the Airflow UI
- Explore logs, task status, and scheduling

This is your “Hello World” experience with Airflow.

---

## 4.2 What Is a DAG?

A DAG (Directed Acyclic Graph) is Airflow’s way of defining workflows. Each DAG is a Python script that describes:
- A set of **tasks**
- Their **dependencies**
- How and **when to run**

In our case:
- The task will be to run `main_SA.py`
- There will be one DAG file, with one task
- You’ll trigger it manually

---

## 4.3 Write Your First DAG

### Step 1: Create the DAG file

```bash
cd ~/cloud-batch-scheduling-sa/dags
nano sa_optimizer_dag.py
```

Paste the following code:

```
from airflow import DAG
from airflow.operators.bash import BashOperator
from airflow.utils.dates import days_ago

default_args = {
    'owner': 'airflow',
    'start_date': days_ago(1),
}

with DAG(
    dag_id='run_sa_optimizer',
    default_args=default_args,
    schedule_interval=None,  # Run manually for now
    catchup=False,
    description='Run Simulated Annealing optimizer once',
) as dag:

    run_optimizer = BashOperator(
        task_id='run_main_SA_script',
        bash_command='cd /home/ec2-user/cloud-batch-scheduling-sa && python3 sa_core/main_SA.py'
    )
```

Save and exit.

## 4.4 Restart Airflow to Load the DAG
If Airflow is already running via Docker Compose:

```
docker-compose restart
```

Or, if it was stopped:

```
docker-compose up -d
```

Wait 1–2 minutes for the DAG to appear.

## 4.5 Access the Airflow Web UI

In your browser, go to:
```
http://<your-ec2-public-ip>:8080
```

Log in:

Username: airflow
Password: airflow

## 4.6 Trigger the DAG
On the Airflow dashboard:

- Find <code>run_sa_optimizer</code>

- Click the toggle to “on”
- Click the “Play” ▶️ button to trigger the run
- Click on the task to:
- View its logs
- Confirm that main_SA.py was executed
- See that the output Excel file is generated again

$$ 4.7 Task: Modify the DAG Schedule
In <code>sa_optimizer_dag.py</code>, change the schedule interval to run every day:

```python
schedule_interval='@daily',
```
Restart Airflow again:

```python
docker-compose restart
```
> This will schedule the DAG to run automatically every 24 hours.

## 4.8 Task: Add a Dummy Task

Just for fun, let’s add a second task to echo a message after the script runs.

Add this under the first task:

```python
from airflow.operators.bash import BashOperator

final_message = BashOperator(
    task_id='echo_complete',
    bash_command='echo "SA run completed!"'
)

run_optimizer >> final_message
```
This will chain the two tasks.

Restart and trigger again to see both tasks run.
