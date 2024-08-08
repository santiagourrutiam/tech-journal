---
title: Comandos Escenciales para trabar con instancias EC2 via AWS CLI
date: 2028-08-08
category: aws labs
---

1. **Listing EC2 Instances**:
   ```bash
   aws ec2 describe-instances
   ```
   This command lists all the EC2 instances in your AWS account.

2. **Launching an EC2 Instance**:
   ```bash
   aws ec2 run-instances --image-id AMI_ID --count 1 --instance-type t2.micro --key-name my-key-pair
   ```
   Replace `AMI_ID` with the ID of the Amazon Machine Image (AMI) you want to use, and `my-key-pair` with the name of your key pair.

3. **Stopping an EC2 Instance**:
   ```bash
   aws ec2 stop-instances --instance-ids INSTANCE_ID
   ```
   Replace `INSTANCE_ID` with the ID of the instance you want to stop.

4. **Starting an EC2 Instance**:
   ```bash
   aws ec2 start-instances --instance-ids INSTANCE_ID
   ```
   Replace `INSTANCE_ID` with the ID of the instance you want to start.

5. **Terminating an EC2 Instance**:
   ```bash
   aws ec2 terminate-instances --instance-ids INSTANCE_ID
   ```
   Replace `INSTANCE_ID` with the ID of the instance you want to terminate.

6. **Creating a Security Group**:
   ```bash
   aws ec2 create-security-group --group-name my-security-group --description "My security group"
   ```
   This creates a new security group with the name "my-security-group".

7. **Adding a Rule to a Security Group**:
   ```bash
   aws ec2 authorize-security-group-ingress --group-id SECURITY_GROUP_ID --protocol tcp --port 22 --cidr 0.0.0.0/0
   ```
   Replace `SECURITY_GROUP_ID` with the ID of the security group, and the port number and CIDR block as needed.

These are just a few of the basic AWS EC2 commands to get you started. As you become more familiar with EC2, you can explore additional commands and options to manage your instances more effectively.