# Day 7: Change EC2 Instance Type

## Challenge Description
As part of optimizing resource utilization, the Nautilus DevOps team identified an underutilized EC2 instance. Our task is to change the instance type of the `devops-ec2` instance from `t2.micro` to `t2.nano`. We must ensure that the instance status checks are completed before making any changes and that the instance is in a running state after the modification. The operation should be performed in the `us-east-1` region.

## Key Concepts
*   **EC2 Instance States:** EC2 instances can be in various states (pending, running, stopping, stopped, shutting-down, terminated). Changing instance type requires the instance to be in the `stopped` state.
*   **Instance Type:** Defines the compute, memory, and networking capacity of an EC2 instance. `t2.nano` offers lower specifications and cost compared to `t2.micro`.
*   **Status Checks:** System status checks and instance status checks monitor the health of the EC2 instance and its underlying host. We need to wait for these to pass before modifying.
*   **AWS CLI:** Command Line Interface for managing AWS services.

## Solution

**Important Note:** Changing an instance type requires the instance to be in a `stopped` state, which will incur a brief period of downtime.

### Option 1: Using AWS Console
1.  We signed in to the AWS Management Console and ensured we were in the **us-east-1 (N. Virginia)** region.
2.  We navigated to the **EC2 Dashboard** and clicked on **Instances**.
3.  We located the instance named `devops-ec2`.
4.  We waited for the **Status check** column to show **2/2 checks passed**.
5.  We selected the `devops-ec2` instance.
6.  We clicked on **Instance state** and selected **Stop instance**. We then waited for the "Instance state" to change to **stopped**.
7.  With the instance still selected, we clicked **Actions** -> **Instance settings** -> **Change instance type**.
8.  In the "Change instance type" dialog, we selected `t2.nano` from the dropdown list and clicked **Apply**.
9.  After the type was changed, we selected the instance again, clicked **Instance state**, and selected **Start instance**.
10. We waited for the "Instance state" to return to **running**.

### Option 2: Using AWS CLI
Alternatively, we used the AWS CLI to perform these actions:

1.  **Get the Instance ID** for `devops-ec2`:
    ```bash
    INSTANCE_ID=$(aws ec2 describe-instances --filters "Name=tag:Name,Values=devops-ec2" --query "Reservations[*].Instances[*].InstanceId" --output text --region us-east-1)
    ```
2.  **Stop the instance** and wait for it to be fully stopped:
    ```bash
    aws ec2 stop-instances --instance-ids $INSTANCE_ID --region us-east-1
    aws ec2 wait instance-stopped --instance-ids $INSTANCE_ID --region us-east-1
    echo "Instance is stopped."
    ```
3.  **Change the instance type** to `t2.nano`:
    ```bash
    aws ec2 modify-instance-attribute --instance-id $INSTANCE_ID --instance-type "{"Value": "t2.nano"}" --region us-east-1
    ```
4.  **Start the instance** and wait for it to be running:
    ```bash
    aws ec2 start-instances --instance-ids $INSTANCE_ID --region us-east-1
    aws ec2 wait instance-running --instance-ids $INSTANCE_ID --region us-east-1
    echo "Instance is running."
    ```

## Verification
*   **Via Console:** In the EC2 Instances dashboard, we verified that the `devops-ec2` instance was in the **running** state and its **Instance type** was listed as `t2.nano`.
*   **Via CLI:** We used the following command to check the instance type and state:
    ```bash
    aws ec2 describe-instances --instance-ids $INSTANCE_ID --query "Reservations[*].Instances[*].[InstanceType, State.Name]" --output table --region us-east-1
    ```
    The output confirmed the instance type was `t2.nano` and the state was `running`.
