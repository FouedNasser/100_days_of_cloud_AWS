# Day 6: Launch EC2 Instance

## Challenge Description
The Nautilus DevOps team requires an EC2 instance to be created with specific configurations as part of their AWS migration strategy. We need to create an EC2 instance named `nautilus-ec2`, using an Amazon Linux AMI, an instance type of `t2.micro`, a new RSA key pair named `nautilus-kp`, and attaching the default security group. The instance must be launched in the `us-east-1` region.

## Key Concepts
*   **EC2 (Elastic Compute Cloud):** Amazon's web service that provides resizable compute capacity in the cloud.
*   **AMI (Amazon Machine Image):** A template that contains a software configuration (operating system, application server, and applications) to launch an EC2 instance.
*   **Instance Type:** Specifies the hardware configuration of an EC2 instance, including CPU, memory, storage, and networking capacity. `t2.micro` is part of the Burstable Performance Instances family.
*   **Key Pair:** A set of security credentials used to prove our identity when connecting to an EC2 instance.
*   **Security Group:** Acts as a virtual firewall for our instance to control inbound and outbound traffic.
*   **AWS CLI:** Command Line Interface for managing AWS services.

## Solution

### Option 1: Using AWS Console
1.  We signed in to the AWS Management Console and ensured we were in the **us-east-1 (N. Virginia)** region.
2.  We navigated to the **EC2 Dashboard** and clicked on **Launch instance**.
3.  For **Name and tags**, we entered `nautilus-ec2` in the "Name" field.
4.  For **Application and OS Images (Amazon Machine Image)**, we selected an **Amazon Linux** AMI (e.g., Amazon Linux 2023 AMI).
5.  For **Instance type**, we selected `t2.micro`.
6.  For **Key pair (login)**, we clicked **Create new key pair**, named it `nautilus-kp`, selected `RSA` for Key pair type, and `.pem` for Private key file format, then clicked **Create key pair** and saved the file.
7.  For **Network settings**, we clicked **Edit**, then under **Firewall (security groups)**, we selected **Select existing security group** and chose the `default` security group.
8.  Finally, we reviewed the settings and clicked **Launch instance**.

### Option 2: Using AWS CLI
Alternatively, we used the AWS CLI to create the key pair and launch the instance:

1.  **Create and save the key pair**:
    ```bash
    aws ec2 create-key-pair \
      --key-name nautilus-kp \
      --query 'KeyMaterial' \
      --output text > nautilus-kp.pem
    
    # Set correct permissions for the key
    chmod 400 nautilus-kp.pem
    ```
2.  **Get the latest Amazon Linux 2 AMI ID**:
    ```bash
    AMI_ID=$(aws ssm get-parameters --names /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2 --region us-east-1 --query 'Parameters[0].[Value]' --output text)
    ```
3.  **Get the default Security Group ID**:
    ```bash
    SG_ID=$(aws ec2 describe-security-groups --group-names default --query "SecurityGroups[0].GroupId" --output text --region us-east-1)
    ```
4.  **Launch the EC2 instance**:
    ```bash
    aws ec2 run-instances \
      --image-id $AMI_ID \
      --instance-type t2.micro \
      --key-name nautilus-kp \
      --security-group-ids $SG_ID \
      --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=nautilus-ec2}]' \
      --region us-east-1
    ```

## Verification
*   **Via Console:** We verified the creation in the EC2 Instances dashboard, confirming that `nautilus-ec2` was in the `Running` state.
*   **Via CLI:** We used the following command to check the instance status:
    ```bash
    aws ec2 describe-instances --filters "Name=tag:Name,Values=nautilus-ec2" "Name=instance-state-name,Values=running" --region us-east-1
    ```
    The output confirmed the `nautilus-ec2` instance was running.
