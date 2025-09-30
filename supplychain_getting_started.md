# Purpose of the Lab

In this module, you will learn how to deploy a Supply Chain Optimization module using Simulated Annealing (SA) and Q-learning-based adaptive operator selection on an Airflow-based Cloud DevOps platform.

You will automate the optimization pipeline to process input data (Excel files) and generate optimal job-to-batch and machine assignments to minimize supply chain costs.

---

# Goals / Outcomes

- Understand the SA + Q-learning optimization process
- Set up an Airflow pipeline to orchestrate optimization runs
- Configure Airflow to run the `main_SA.py` script with various Excel inputs
- Export optimization results and visualize them
- Learn how to schedule and track optimization runs via Airflow UI

---

# Before you begin... Reference Links

> add descriptions under each link
- [Airflow](https://airflow.apache.org/) - Workflow orchestration platform
- [Simulated Annealing](https://en.wikipedia.org/wiki/Simulated_annealing) - Metaheuristic optimization
- [Q-learning](https://en.wikipedia.org/wiki/Q-learning) - Reinforcement Learning
- [Supply Chain Optimization](https://www.sciencedirect.com/science/article/pii/S0925527316302425) - Example research reference

---

# Before You Begin...

- Python 3.8+
- Airflow installed (we will guide you)
- Your supply chain optimization code provided (`main_SA.py` and supporting modules)
- Example Excel data file (provided in this lab)

---

# Guideline to Complete Module

- Step 1: Understand SA + Q-learning Algorithm
- Step 2: Set up Python environment
- Step 3: Install and configure Airflow
- Step 4: Build Airflow DAG for the optimization
- Step 5: Run example optimization and analyze results
- Step 6: Optional - Automate parameter tuning and experiment tracking

> add medals and stuff here
> remove the stuff before this (guideline... and add the actual guidelines instead)
