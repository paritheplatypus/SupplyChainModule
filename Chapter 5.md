# Chapter 5: The Newsvendor Model – Single‑Period Inventory Under Uncertainty

The **Newsvendor** model captures a prototypical single‑period inventory decision: how much to order prior to uncertain demand. The decision balances the **underage cost** (lost margin or goodwill when demand exceeds stock) against the **overage cost** (holding or spoilage when inventory remains). Despite its simplicity, the model is foundational because it formalizes decision‑making under stochastic demand with asymmetric costs, offering a clear analytic solution and a lens through which more elaborate multi‑period and multi‑item models can be understood.

Let \( Q \) denote the order quantity and \( D \) the random demand.  
For unit selling price \( p \), purchase cost \( c \), and salvage value \( v \) (with \( p > c \ge v \)),  
the marginal **underage cost** is \( c_u = p - c \) and the marginal **overage cost** is \( c_o = c - v \).  

The optimal policy satisfies the **critical fractile** condition:
\[
F_D(Q^*) = \frac{c_u}{c_u + c_o}
\]
where \( F_D \) is the cumulative distribution function (CDF) of demand.  

Under Normal demand with mean \( \mu \) and standard deviation \( \sigma \),  
the optimal order is:
\[
Q^* = \mu + z^* \sigma
\]
with
\[
z^* = \Phi^{-1}\!\left( \frac{c_u}{c_u + c_o} \right)
\]

The elegance of this result belies practical complications:  
demand distributions are rarely known, costs can be time-varying,  
and capacity or multi-product interactions introduce coupling that invalidates the single-item, single-period assumptions.

This chapter uses the classical model to illustrate how analytic prescriptions and empirical calibration coexist in practice. We provide a small, auditable Python utility that computes \(Q^*\) under Normal demand and can be embedded in a pipeline for scenario analysis, sensitivity studies, or teaching demonstrations. Extensions—non‑parametric estimates of \(F_D\), multi‑period smoothing, and service‑level constraints—follow the same logic but require additional data and modeling assumptions.

## Create the Newsvendor DAG (no external script required)
```bash
DAGS_DIR="$(airflow config get-value core dags_folder 2>/dev/null || echo ${AIRFLOW_HOME:-$HOME/airflow}/dags)"
tee "$DAGS_DIR/newsvendor_dag.py" >/dev/null <<'PY'
from datetime import timedelta
import os, json, pendulum
from statistics import NormalDist
from airflow import DAG
from airflow.operators.python import PythonOperator

RESULTS_DIR = "/opt/airflow/results"
os.makedirs(RESULTS_DIR, exist_ok=True)

def compute_newsvendor(**_):
    mu = float(os.environ.get("NV_MU", "100"))
    sigma = float(os.environ.get("NV_SIGMA", "20"))
    cu = float(os.environ.get("NV_CU", "5"))
    co = float(os.environ.get("NV_CO", "2"))

    p = cu/(cu+co)
    z = NormalDist().inv_cdf(p)
    q_star = mu + z * sigma

    result = {
        "mu": mu, "sigma": sigma, "cu": cu, "co": co,
        "critical_fractile": p, "z": z, "q_star": round(q_star, 2)
    }
    path = os.path.join(RESULTS_DIR, "newsvendor_result.json")
    with open(path, "w") as f:
        json.dump(result, f, indent=4)
    print(f"Saved Newsvendor result to: {path}\n{json.dumps(result, indent=4)}")

default_args = {
    "owner": "airflow",
    "retries": 0,
    "retry_delay": timedelta(minutes=1),
}

with DAG(
    dag_id="newsvendor_model",
    start_date=pendulum.datetime(2024, 10, 1, tz="UTC"),
    schedule=None,
    catchup=False,
    default_args=default_args,
    tags=["chapter5", "inventory", "stochastic"],
) as dag:
    compute = PythonOperator(task_id="compute_newsvendor", python_callable=compute_newsvendor)
PY

airflow dags list | grep newsvendor_model || true
airflow tasks test newsvendor_model compute_newsvendor 2024-10-01
cat /opt/airflow/results/newsvendor_result.json
```
