# Chapter 2: Environment Preparation and Airflow Setup on AWS

## 2.1 Overview

In this chapter, you will:
- Launch the pre-configured EC2 instance with Simulated Annealing and Apache Airflow already installed
- Connect to the instance via SSH
- Explore the provided project directory and test the optimizer
- Start and access the Airflow web interface using Docker Compose

This prepares you for workflow automation in later chapters.

---

## 2.2 Launch Your EC2 Instance

Follow these steps to launch the cloud environment where your optimization and orchestration will run.

### 🔹 Step 1: Log into AWS
- Go to the [AWS Console](https://console.aws.amazon.com)
- Choose the **N. Virginia (us-east-1)** region (or as instructed)

### 🔹 Step 2: Launch an Instance
- Go to **EC2 > Instances > Launch Instance**
- Choose **AMI**: `cloud-sa-optimizer-ami` (provided by instructor or administrator)
- Select **Instance type**: `t2.micro` (Free Tier eligible)
- Use or create a **Key Pair** for SSH access
- Use or create a **Security Group**:
  - Inbound rule: Allow **port 22 (SSH)** from your IP
  - (Optional) Allow **port 8080** if you want to access Airflow UI from browser
- Name your instance `sa-optimizer`

Click **Launch Instance**.

---

## 2.3 Connect via SSH

Once your instance is running:

```bash
ssh -i /path/to/your-key.pem ec2-user@<your-ec2-public-ip>

<Replace <your-ec2-public-ip> with the public IPv4 address from the EC2 dashboard.>

## 2.4 Explore the Project Directory

On the EC2 instance:

```
cd ~cloud-batch-scheduling-sa
ls sa_core/
```

You should see Python files like:

- main_SA.py
- calculate_objective.py
- generate_neighbor.py
(etc.)

You should also see:
- requirements.txt
- docker-compose.yaml (for Airflow)
- dags/ folder (Airflow DAGs will go here)

## 2.5 Run the Optimizer Manually

Test that the SA code works:
```
python3 sa_core/main_SA.py
```

If successful, it will print logs and generate a file such as:
```
SA_results123.xlsx
```

Use <code>ls</code> to confirm:
```
ls *.xlsx
```








