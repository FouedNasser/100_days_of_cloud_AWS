# Day 8: Enable Stop Protection for EC2 Instance

## Challenge Description
As part of a migration, we need to enable stop protection for an existing EC2 instance named `nautilus-ec2` located in the `us-east-1` region. This ensures the instance cannot be stopped via the AWS API or Console accidentally.

## Key Concepts
*   **EC2 Instance Attributes:** Properties of an EC2 instance that can be modified, such as termination protection or stop protection.
*   **Stop Protection (`disableApiStop`):** An attribute that, when enabled, prevents the instance from being stopped (it can still be terminated if termination protection is not enabled).
*   **AWS CLI:** Using the command line to query and modify AWS resources.

## Solution

### 1. Identify the Instance ID
First, we retrieve the `InstanceId` of the instance named `nautilus-ec2`.

```bash
aws ec2 describe-instances \
    --region us-east-1 \
    --filters "Name=tag:Name,Values=nautilus-ec2" \
    --query "Reservations[*].Instances[*].InstanceId" \
    --output text
```

### 2. Enable Stop Protection
Using the retrieved `InstanceId` (replace `<INSTANCE_ID>` with the actual ID), we enable the `disableApiStop` attribute.

```bash
aws ec2 modify-instance-attribute \
    --region us-east-1 \
    --instance-id <INSTANCE_ID> \
    --disable-api-stop "Value=true"
```

## Verification

To confirm that stop protection is enabled, we check the instance attribute:

```bash
aws ec2 describe-instance-attribute \
    --region us-east-1 \
    --instance-id <INSTANCE_ID> \
    --attribute disableApiStop
```

The output should confirm the setting:

```json
{
    "InstanceId": "<INSTANCE_ID>",
    "DisableApiStop": {
        "Value": true
    }
}
```
