# Day 4: Allocate Elastic IP

## Challenge Description
The Nautilus DevOps team is migrating infrastructure to AWS in incremental steps. For this task, we need to allocate an Elastic IP address and name it `nautilus-eip` in the `us-east-1` region.

## Key Concepts
*   **Elastic IP (EIP):** A static IPv4 address designed for dynamic cloud computing.
*   **AWS CLI:** Command Line Interface to interact with AWS services.
*   **Tagging:** Assigning metadata (Key-Value pairs) to AWS resources for identification and management.

## Solution

### Option 1: Using AWS Console
1.  We signed in to the AWS Management Console and selected the `us-east-1` region.
2.  We navigated to **VPC** > **Elastic IPs**.
3.  We clicked **Allocate Elastic IP address**.
4.  In the allocation settings, we added a tag with Key: `Name` and Value: `nautilus-eip`.
5.  We clicked **Allocate**.

### Option 2: Using AWS CLI
Alternatively, we used the AWS CLI to allocate and tag the IP:

1.  **Allocate the Elastic IP:**
    ```bash
    aws ec2 allocate-address --domain vpc --region us-east-1
    ```
    *(We noted the `AllocationId` from the output)*

2.  **Tag the Elastic IP:**
    ```bash
    aws ec2 create-tags --resources <AllocationId> --tags Key=Name,Value=nautilus-eip --region us-east-1
    ```

## Verification
We verified the creation by listing the addresses filtered by the tag:
```bash
aws ec2 describe-addresses --filters "Name=tag:Name,Values=nautilus-eip" --region us-east-1
```
The output confirmed the existence of the Elastic IP with the correct name tag.
