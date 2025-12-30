# Day 9: Enable Termination Protection for EC2 Instance

## Challenge Description
The Nautilus DevOps team created an EC2 instance named `nautilus-ec2` in the `us-east-1` region but forgot to enable termination protection. We need to enable this feature to prevent accidental termination of the instance.

## Key Concepts
*   **Termination Protection:** An EC2 instance attribute that stops the instance from being terminated using the AWS Console or API. It must be disabled before the instance can be terminated.
*   **AWS Management Console (UI):** Web-based interface to manage AWS resources.
*   **AWS CLI:** Command-line interface to manage AWS resources programmatically.

## Solution

We can enable termination protection using either the AWS Management Console or the AWS CLI.

### Option 1: AWS Management Console (UI)
1.  Log in to the **AWS Management Console**.
2.  Navigate to the **EC2 Dashboard**.
3.  Click on **Instances** in the left sidebar.
4.  Find and select the instance named `nautilus-ec2` in the `us-east-1` region.
5.  Click on the **Actions** dropdown menu at the top.
6.  Navigate to **Instance settings** > **Change termination protection**.
7.  Check the **Enable** box.
8.  Click **Save**.

### Option 2: AWS CLI

#### 1. Identify the Instance ID
First, we retrieve the `InstanceId` of the instance named `nautilus-ec2`.

```bash
aws ec2 describe-instances \
    --region us-east-1 \
    --filters "Name=tag:Name,Values=nautilus-ec2" \
    --query "Reservations[*].Instances[*].InstanceId" \
    --output text
```

#### 2. Enable Termination Protection
Using the retrieved `InstanceId` (replace `<INSTANCE_ID>` with the actual ID), we enable the `disableApiTermination` attribute.

```bash
aws ec2 modify-instance-attribute \
    --region us-east-1 \
    --instance-id <INSTANCE_ID> \
    --disable-api-termination "Value=true"
```

## Verification

To confirm that termination protection is enabled via CLI:

```bash
aws ec2 describe-instance-attribute \
    --region us-east-1 \
    --instance-id <INSTANCE_ID> \
    --attribute disableApiTermination
```

The output should confirm the setting with `"Value": true`:

```json
{
    "InstanceId": "<INSTANCE_ID>",
    "DisableApiTermination": {
        "Value": true
    }
}
```
