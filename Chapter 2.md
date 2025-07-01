## Chapter 2: Environment Preparation and Local Execution

### 2.1 Launch Pre-configured AMI

Go to AWS Console > EC2 > Launch Instance

Select the custom AMI named cloud-sa-optimizer-ami

Choose t2.micro (free tier eligible)

Create/use a key pair (download .pem)

Allow SSH access (port 22) via security group

Name the instance sa-optimizer

### 2.2 Connect to the Instance

```
ssh -i /path/to/key.pem ec2-user@<your-ec2-ip>
```

### 2.3 Verify Environment

The AMI already includes:

- Python 3.10
- Required Python libraries (<code>numpy</code>, <code>pandas</code>, <code>matplotlib</code>, <code>openpyxl</code>)
- Code base and test data in <code>/home/ec2-user/cloud-batch-scheduling-sa</code>

Navigate to the working directory and run:
```
cd ~/cloud-batch-scheduling-sa  
python3 sa_core/main_SA.py
```

### 2.4 Task: Modify Initial Parameters

Open main_SA.py and locate the following block:
```
Tmin = 1
Tmax = 1000
cooling_rate = 0.99
```

Change Tmax to 500 and re-run the script. Observe changes in objective values and runtime in the output.

**Reflection Prompt**: What do you notice about the solution quality and runtime when Tmax is reduced?
