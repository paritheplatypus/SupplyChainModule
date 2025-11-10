# Chapter 0 – Environment Setup and Airflow Login

Before beginning the workflow modules, each student must ensure that their EC2 instance is running and that they can access the Airflow web interface with their own account.  
Follow these steps carefully.

---

## 🖥️ Step 1. Start Your EC2 Instance
1. Go to the **AWS Management Console** → **EC2 Dashboard** → **Instances**.  
2. Locate your assigned instance.  
3. If its **Instance state** shows **Stopped**, select it and click **Start instance**.  
4. Wait for the state to change to **Running**.
   
---

## 🐍 Step 2. Activate the Python Virtual Environment
Most setups include a preinstalled virtual environment named `mcd-env`.  
Activate it with:
```bash
source ~/mcd-env/bin/activate
```
You should see `(mcd-env)` appear at the beginning of your prompt.  
This ensures that all Airflow commands use the correct environment.

---

## 🗄️ Step 3. Initialize the Airflow Database
If this is your first time using Airflow, you must initialize the metadata database:
```bash
airflow db init
```
This step creates all necessary tables for users, connections, and DAG runs.

---

## 👤 Step 4. Create Your Airflow User
Each student must create a personal login account for the Airflow web UI.

Run the following command **inside the activated virtual environment**:

```bash
airflow users create     --username yourusername     --firstname YourName     --lastname Student     --role Admin     --email your_email@example.com     --password yourpassword
```

✅ Example:
```bash
airflow users create     --username alice     --firstname Alice     --lastname Johnson     --role Admin     --email alice@mizzou.edu     --password airflow123
```

---

## 🌐 Step 5. Log in to Airflow
1. Start the Airflow webserver (if not already running):
   ```bash
   module11-start-airflow.sh
   ```
   ```
3. Open your browser and navigate to:
   ```
   http://<your-ec2-public-dns>:8080
   ```
4. Log in using the credentials you just created.

---

## ✅ Verification
Once logged in:
- You should see the **Airflow Dashboard** with a list of available default DAGs.
- You can now start the workflow experiments described in Chapters 3–5.

---

**Note:** If you restart your EC2 instance later, you only need to:
1. Start the instance again.
2. Reactivate your virtual environment.
3. Restart the Airflow server.

You **do not** need to reinitialize the database or recreate the user unless it was deleted.
