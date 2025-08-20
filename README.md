# Supply Chain Module ([MizzouCloudDevOps](https://www.mizzouclouddevops.net/MizzouCloudDevOps/#!/home_page))

This module contains code authored by @parisa (Fatemah Pourdehghan Golneshini).

## Architecture Summary

main_SA.py  
│  
├── generate_data.py           --> Reads Excel data → creates 'data' dictionary  
├── initial_solution.py        --> Creates an initial feasible solution  
├── calculate_objective.py     --> Evaluates the cost of a solution  
├── generate_neighbor.py       --> Generates neighbor solutions using 14 operators (also runs all feasibility checks!)  
├── feasibility.py              └ All feasibility functions  
├── export_SA_results.py       --> Writes results to Excel  

## Purpose of the Module

The module teaches learners how to optimize a real-world supply chain scheduling problem (jobs → machines) using Simulated Annealing (SA), and then automate that optimization workflow with Apache Airflow on AWS.

### The scenario:
You’re in a factory with 3D printers/CNC machines. Each job has material, volume/weight constraints, due dates, and penalties (lateness/rejection).
The goal is to minimize total operational cost.

#### Structure

1. README.md
- Explains the architecture and core Python scripts:
- `generate_data.py`: Reads Excel → creates job/machine data
- `initial_solution.py`: Builds a feasible starting schedule
- `calculate_objective.py`: Computes total cost (production + lateness + rejection + unused capacity)
- `generate_neighbor.py`: Generates new candidate solutions with feasibility checks
- `feasibility.py`: Ensures constraints are respected
- `main_SA.py`: The main driver for the SA loop

2. Chapter 1: Overview & Getting Started
- Introduces the problem (job scheduling in manufacturing).
- Defines costs and constraints.
- Objective: minimize operational cost using SA.

3. Chapter 2: Environment Setup

1. Launch a pre-configured AWS EC2 instance (with Airflow + SA pre-installed).
2. Connect via SSH.
3. Explore project directory.
4. Start Airflow web UI (via Docker Compose).
> This sets up the cloud environment for automation.

4. Chapter 3: Running the SA Scheduler

- Learn how SA works internally.
- Manually run main_SA.py on EC2.
- Tweak parameters (temperature, cooling rate, iterations).
- Review Excel output with optimized schedules and costs.
- Helps students understand optimization before automation.

5. Chapter 4: Hello World Airflow DAG

- Introduces Airflow DAGs.
- Students run a provided DAG (supply_chain_sa_dag.py) that triggers main_SA.py.
- They explore the Airflow web UI and logs to see how workflows are orchestrated.

6. Chapter 5: Scaling Up

- Modify DAG to run optimizer multiple times with different seeds (to compare results).
- Save outputs under unique names.
- (Optional) Upload results to AWS S3.
- Analyze/compare outcomes across multiple runs for deeper learning.

> The module blends optimization (Simulated Annealing) with workflow automation (Airflow). Students start with manual job scheduling optimization → then move to automating workflows in Airflow → finally scale up experiments and analyze results.
