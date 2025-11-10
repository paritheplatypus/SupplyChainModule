# Chapter 3: Hello, Airflow!

Simulated Annealing (SA) is a probabilistic metaheuristic inspired by the physics of annealing in solids, wherein a material is heated and then cooled slowly to allow particles to settle into a low-energy crystalline configuration. In optimization, the analogy is made precise: solutions correspond to micro-states of the system, the objective function to energy, and a temperature parameter governs the probability of accepting moves that worsen the objective value. At high “temperatures,” the algorithm explores broadly—even accepting inferior solutions with non‑negligible probability—so it can escape local minima. As the temperature decays according to a cooling schedule, exploration is gradually curtailed and the search concentrates in promising regions of the landscape.

The method’s power lies in its robust balance between exploration and exploitation. Unlike greedy descent or exact enumeration, SA tolerates uphill moves early in the search, making it well‑suited to discrete, non‑convex, and combinatorial problems that characterize real systems. Decades of literature report successful deployments across traveling‑salesman tours, VLSI placement, facility layout, and scheduling. Convergence properties depend critically on the **temperature schedule** (initial temperature, cooling rate), **neighborhood structure** (how candidate solutions are perturbed), and **acceptance rule** (typically the Metropolis criterion). With appropriate tuning, SA reliably produces high‑quality solutions for problem sizes that are intractable for exact methods.

In this chapter, we will not only formalize the algorithmic components—state representation, move operators, acceptance probabilities—but also connect them to practical engineering choices. Students will see how cooling schedules influence convergence speed and solution quality, how to design neighborhoods that reflect structural constraints, and how to implement SA within a modern workflow manager (Apache Airflow) so that experiments are reproducible and auditable. The following “Hello, Airflow” DAG ensures your environment is correctly configured before we proceed to a scheduling application in Chapter 4.

## Create a minimal DAG that also writes to a file
```bash
DAGS_DIR="$(airflow config get-value core dags_folder 2>/dev/null || echo ${AIRFLOW_HOME:-$HOME/airflow}/dags)"
tee "$DAGS_DIR/hello_world.py" >/dev/null <<'PY'
from datetime import timedelta
import pendulum, os

from airflow import DAG
from airflow.operators.bash import BashOperator
from airflow.operators.python import PythonOperator

RESULTS_DIR = "/opt/airflow/results"
os.makedirs(RESULTS_DIR, exist_ok=True)

def write_file(**_):
    path = os.path.join(RESULTS_DIR, "hello_world.txt")
    with open(path, "w") as f:
        f.write("Hello, Airflow!\n")
    print(f"Wrote: {path}")

default_args = {
    "owner": "airflow",
    "retries": 0,
    "retry_delay": timedelta(minutes=1),
}

with DAG(
    dag_id="hello_world",
    start_date=pendulum.datetime(2024, 10, 1, tz="UTC"),
    schedule=None,            # manual trigger
    catchup=False,
    default_args=default_args,
    tags=["chapter3", "sanity"],
) as dag:
    hello = BashOperator(
        task_id="hello",
        bash_command="echo 'Hello, Airflow!'"
    )
    to_file = PythonOperator(task_id="to_file", python_callable=write_file)
    hello >> to_file
PY
airflow dags list | grep hello_world || true
airflow tasks test hello_world hello 2024-10-01
airflow tasks test hello_world to_file 2024-10-01
cat /opt/airflow/results/hello_world.txt
```
