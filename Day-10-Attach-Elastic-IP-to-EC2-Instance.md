# Day 10: Attach Elastic IP to EC2 Instance

## Challenge Description
The Nautilus DevOps team requirements involve attaching an existing Elastic IP named `datacenter-ec2-eip` to an existing EC2 instance named `datacenter-ec2` in the `us-east-1` region. This ensures the instance retains a static public IP address.

## Key Concepts
*   **Elastic IP (EIP):** A static IPv4 address designed for dynamic cloud computing.
*   **EC2 Instance Association:** Linking an EIP to a specific EC2 instance to make it reachable from the internet via a fixed address.
*   **AWS CLI:** Using the command line to manage AWS resources.

## Solution

### Method 1: AWS Management Console (UI)

1.  We logged in to the **AWS Management Console** and navigated to the **EC2 Dashboard**.
2.  In the left navigation pane, under **Network & Security**, we selected **Elastic IPs**.
3.  We found and selected the Elastic IP named `datacenter-ec2-eip`.
4.  We clicked on the **Actions** button and selected **Associate Elastic IP address**.
5.  In the **Resource type** section, we ensured **Instance** was selected.
6.  In the **Instance** dropdown, we searched for and selected the instance named `datacenter-ec2`.
7.  We clicked **Associate** at the bottom of the page.

### Method 2: AWS CLI

1.  **Get the Instance ID:**
    We retrieved the Instance ID for `datacenter-ec2`:
    ```bash
    aws ec2 describe-instances \
      --filters "Name=tag:Name,Values=datacenter-ec2" \
      --query "Reservations[*].Instances[*].InstanceId" \
      --output text
    ```

2.  **Get the Allocation ID:**
    We retrieved the Allocation ID for the Elastic IP `datacenter-ec2-eip`:
    ```bash
    aws ec2 describe-addresses \
      --filters "Name=tag:Name,Values=datacenter-ec2-eip" \
      --query "Addresses[*].AllocationId" \
      --output text
    ```

3.  **Associate the Elastic IP:**
    We associated the address using the IDs retrieved above:
    ```bash
    aws ec2 associate-address \
      --instance-id <INSTANCE_ID> \
      --allocation-id <ALLOCATION_ID>
    ```

## Verification

To verify the association was successful, we checked the instance details to confirm the Public IP matched the Elastic IP:

```bash
aws ec2 describe-instances \
  --instance-ids <INSTANCE_ID> \
  --query "Reservations[*].Instances[*].PublicIpAddress" \
  --output text
```

Alternatively, in the AWS Console, we checked the **Elastic IPs** section to see that the `datacenter-ec2-eip` was associated with the `datacenter-ec2` instance.
