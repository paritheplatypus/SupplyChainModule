# 1.1 Purpose of the Module

In this step, we will understand the Simulated Annealing (SA) algorithm with Q-learning-based adaptive operator selection used for Supply Chain Optimization.

We will explore how SA minimizes total cost in supply chain batch scheduling and how operators modify solutions.

---

# 1.2 References to Guide Module Work

- [Simulated Annealing](https://en.wikipedia.org/wiki/Simulated_annealing)
- [Q-learning](https://en.wikipedia.org/wiki/Q-learning)
- [Airflow](https://airflow.apache.org/)
- [Supply Chain Optimization Techniques](https://www.sciencedirect.com/science/article/pii/S0925527316302425)

---

# 1.3 Overview of the Algorithm

The SA optimization module aims to minimize **total production cost**, including:

- Production cost
- Lateness cost
- Rejection cost
- Unused capacity cost

### Algorithm Workflow:

1. Generate Initial Feasible Solution
2. Evaluate Objective Function (Cost)
3. Iteratively:
    - Select operator using Q-learning Q-table
    - Generate Neighbor Solution using operator
    - Evaluate Objective Function
    - Accept/reject based on SA acceptance rule
    - Update best solution

### Q-learning Operators:

- 14 operators including swap, move, split/merge, reject, insert
- Q-table adaptively updates operator selection probabilities

---

# 1.4 Goal of This Module

You will automate this optimization process via **Airflow**, enabling:

- Scheduled optimization runs
- Dynamic parameter tuning
- Workflow orchestration
- Result visualization and tracking
