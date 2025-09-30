# Chapter 2: Preparation

## 2.1 Overview

In this chapter, you will:
- Launch the pre-configured EC2 instance with Simulated Annealing and Apache Airflow already installed
- Connect to the instance via SSH
- Explore the provided project directory and test the optimizer
- Start and access the Airflow web interface using Docker Compose

This prepares you for workflow automation in later chapters.

## 2.2 Launch Your EC2 Instance

Follow these steps to launch the cloud environment where your optimization and orchestration will run.

### Step 1: Log into AWS
- Go to the [AWS Console](https://console.aws.amazon.com)
- Choose the **N. Virginia (us-east-1)** region (or as instructed)

### Step 2: Launch an Instance
- Go to **EC2 > AMIs**
- Choose **AMI**: `AirflowAMI`
- Select **Instance type**: `t2.micro` (Free Tier eligible)
- Use or create a **Key Pair** for SSH access
- Use or create a **Security Group**:
  - Inbound rule: Allow **port 22 (SSH)** from your IP
  - (Optional) Allow **port 8080** if you want to access Airflow UI from browser
- Name your instance anything you'd like

Click **Launch Instance**.



## 2.3 Connect via SSH

Once your instance is running:

```bash
ssh -i /path/to/your-key.pem ec2-user@<your-ec2-public-ip>
````
> Replace <your-ec2-public-ip> with the public IPv4 address from the EC2 dashboard.

## 2.4 Explore the Project Directory

Navigate to the project folder:

```
cd ~/supply_chain_module
ls
```

You should see files like:

- <code>main_SA.py</code>
- <code>generate_data.py</code>, <code>initial_solution.py</code>, etc.
- Input Excel files (e.g., <code>results69.xlsx</code>)

## 2.5 Start the Airflow Services

Start the scheduler and webserver:

```
airflow scheduler &
airflow webserver --port 8080 &
```

In your browser, go to:

```
http://<your-ec2-public-ip>:8080
```

Log in using:

Username: <code>admin</code>
Password: <code>admin123</code>

You are now ready to use the Airflow UI to trigger the optimizer.








