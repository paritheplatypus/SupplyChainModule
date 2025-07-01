## Chapter 2: Environment Preparation and Local Execution

### 2.1 Launch Pre-configured AMI

Go to AWS Console > EC2 > Launch Instance

Select the custom AMI named cloud-sa-optimizer-ami

Choose t2.micro (free tier eligible)

Create/use a key pair (download .pem)

Allow SSH access (port 22) via security group

Name the instance sa-optimizer

### 2.2 Connect to the Instance

<code>ssh -i /path/to/key.pem ec2-user@<your-ec2-ip></code>

### 2.3 Verify Environment

The AMI already includes:

- Python 3.10
- Required Python libraries (<code>numpy</code>, <code>pandas</code>, <code>matplotlib</code>, <code>openpyxl</code>)
- Code base and test data in <code>/home/ec2-user/cloud-batch-scheduling-sa</code>

Navigate to the working directory and run:
'''
cd ~/cloud-batch-scheduling-sa
python3 sa_core/main_SA.py
'''
