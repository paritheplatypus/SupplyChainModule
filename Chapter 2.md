## Chapter 2: Environment Preparation and Local Execution

### 2.1 Launch Pre-configured AMI

Go to AWS Console > EC2 > Launch Instance

Select the custom AMI named cloud-sa-optimizer-ami

Choose t2.micro (free tier eligible)

Create/use a key pair (download .pem)

Allow SSH access (port 22) via security group

Name the instance sa-optimizer

### 2.2 Connect to the Instance

<code>ssh -i /path/to/key.pem ec2-user@<your-ec2-ip><code>

