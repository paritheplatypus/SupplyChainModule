# Chapter 1: Overview

## 1.1 Purpose of the Lab

In this lab, you will learn how to solve a **real-world job scheduling problem** using a powerful optimization technique called **Simulated Annealing (SA)**. Imagine a factory with a set of 3D printers or CNC machines — you need to assign manufacturing jobs to those machines in the most efficient way possible.

Each job has:
- A material requirement
- Volume and weight constraints
- A due date (for delivery)
- A production cost, lateness penalty, and rejection cost

Your goal is to **minimize the total operational cost** by choosing smart job-to-machine assignments.

To automate and scale this process, we will deploy our scheduling algorithm in the **cloud** using:
- **AWS EC2** as the compute platform
- **Apache Airflow** to orchestrate and automate the optimization



## 1.2 Prerequisites

> fill in prerequisites

By the end of this module, you will:
- Understand how Simulated Annealing can be applied to job scheduling
- Launch a pre-configured EC2 instance with all dependencies installed
- Run and modify Python-based scheduling code
- Automate and monitor scheduling runs using Apache Airflow DAGs
- Analyze output cost breakdown and compare scheduling strategies

# References to guide lab work

# Overview

> rest is not needed


## 1.3 Problem Statement: Why Optimize Scheduling?

Consider a 3D printing lab with limited machine capacity and many incoming jobs. Simply assigning jobs in the order they arrive may lead to:
- Some machines being overloaded
- Delayed deliveries
- Unused material or space
- Higher operational costs

By optimizing:
- You assign jobs more intelligently
- You reduce waste and penalties
- You deliver faster and cheaper

This lab simulates exactly this — but in a **controlled, cloud-based environment**.



## 1.4 What Is Simulated Annealing?

**Simulated Annealing (SA)** is a probabilistic technique for finding a near-optimal solution to difficult optimization problems.

It works by:
- Starting with a random solution
- Making small changes (neighbors)
- Occasionally accepting worse solutions (to escape local optima)
- Gradually reducing the chance of accepting bad solutions (cooling)

In our case, SA will attempt to minimize a cost function that includes:
- Production cost
- Lateness penalties
- Rejection costs
- Unused capacity



## 1.5 What Is a DAG in Airflow?

In **Apache Airflow**, a DAG (Directed Acyclic Graph) is a workflow definition that tells Airflow:
- What tasks to run
- In what order
- How often

You will write a **simple DAG** to:
- Run your Python optimizer script (`main_SA.py`)
- Archive or push output files
- Schedule runs periodically



## 1.6 Tools and Technologies

| Tool               | Role                                 |
|--------------------|--------------------------------------|
| Python 3           | Runs the optimization logic          |
| Simulated Annealing| Custom scheduling algorithm          |
| AWS EC2            | Runs all code in the cloud (AMI provided) |
| Apache Airflow     | Automates and schedules workflow runs |
| Excel              | Stores final job schedule and cost breakdowns |



## 1.7 Pre-requisites

Before starting, make sure you:
- Have access to the provided AWS **AMI** (`cloud-sa-optimizer-ami`)
- Can launch and SSH into EC2 instances
- Have a basic understanding of:
  - Python scripting
  - Terminal commands (Linux)
  - AWS EC2 interface
- Are familiar with GitHub (recommended, not required)



## 1.8 Module Deliverables

You will complete and demonstrate:
- One **manual run** of the scheduling optimizer on EC2
- One **Airflow DAG run** using a Hello World workflow
- One **scaled-up run** using multiple seeds or parameter configurations
- Submission of:
  - Your final output Excel file
  - One modified Python script or DAG
  - A short reflection or answers to provided questions
