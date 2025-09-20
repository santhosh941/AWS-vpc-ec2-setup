# AWS-vpc-ec2-setup

Step 1: Create VPC

Select a region (e.g., us-east-1).

Create a new VPC with a CIDR block of 172.16.0.0/16 (this provides 65536 IP addresses).

Step 2: Create Public Subnet

Inside the VPC, create a Public Subnet in us-east-1a.

Assign a smaller CIDR (e.g., 172.16.1.0/24).

Enable auto-assign public IP for this subnet.

Step 3: Create Private Subnet

Create a Private Subnet in a different AZ (e.g., us-east-1b).

Use CIDR 172.16.2.0/24.

Do not enable auto-assign public IP.

Step 4: Create Internet Gateway

Create an Internet Gateway (IGW).

Attach it to the VPC.

Step 5: Create Route Tables

Public Route Table:

Associate the Public Subnet with this route table.

Add a default route 0.0.0.0/0 → Internet Gateway.

Private Route Table:

Associate the Private Subnet with this route table.

No IGW route (only local VPC traffic).

Step 6: Launch EC2 in Public Subnet

Launch an EC2 instance in the Public Subnet.

Choose Amazon Linux 2.

Security Group:

Allow SSH (port 22) from your IP.

Allow HTTP (port 80) from anywhere.

Connect via SSH and install nginx:

sudo yum update -y
sudo amazon-linux-extras install nginx1 -y
sudo systemctl start nginx
sudo systemctl enable nginx

Verify by visiting the instance’s Public IP in a browser.

Step 7: Launch EC2 in Private Subnet

Launch another EC2 instance in the Private Subnet.

No public IP assigned.

Security Group:

Allow SSH only from the Public EC2 instance’s Security Group.

Connect to the private instance via the Public EC2 (bastion host):

# From your laptop
ssh -i key.pem ec2-user@<PUBLIC_INSTANCE_IP>


# From inside Public instance
ssh ec2-user@<PRIVATE_INSTANCE_PRIVATE_IP>

Now you can access the private instance through the public instance.
