# Chapter 4: Hello World DAG – First Airflow Workflow (Revised)

## 4.1 Overview

In this chapter, you will:

- Understand how Airflow DAGs orchestrate jobs
- Use the provided DAG to trigger main_SA.py
- Explore the Airflow interface and logs

## 4.2 What Is a DAG?

A DAG (Directed Acyclic Graph) defines workflows in Airflow. Each DAG is a Python file that tells Airflow:

- What tasks to run
- In what order
- On what schedule

For your project:

- The DAG is located at `~/airflow/dags/supply_chain_sa_dag.py`
- It runs the script: `/home/ec2-user/supply_chain_module/main_SA.py`

## 4.3 Trigger Your DAG

Go to:
```
http://<your-ec2-ip>:8080
```
Steps:

1. Log in (`admin` / `admin123`)
2. Find `supply_chain_simulated_annealing`
3. Toggle it on
4. Click ▶️ to trigger

## 4.4 Explore Logs and Outputs

Click on the task name to view:
- Console output
- Objective value logs (if added)
- Confirmation that `main_SA.py` executed successfully

## 4.5 Schedule the DAG (Optional)

To run the optimizer daily:
- Edit `supply_chain_sa_dag.py`
```
schedule_interval='@daily'
```
- Restart Airflow:

```
pkill airflow
airflow scheduler &
airflow webserver --port 8080 &
```
