# Day 5: Create GP3 Volume

## Challenge Description
The Nautilus DevOps team is proceeding with the incremental migration to AWS. For this task, we are required to create an EBS volume with the following specifications:
*   **Name:** `xfusion-volume`
*   **Type:** `gp3`
*   **Size:** `2 GiB`
*   **Region:** `us-east-1`

## Key Concepts
*   **Amazon EBS (Elastic Block Store):** Scalable, high-performance block-storage services designed for use with Amazon EC2.
*   **GP3 Volumes:** General Purpose SSD volumes that provide a balance of price and performance, allowing independent provisioning of IOPS and throughput.
*   **Tagging:** Assigning metadata to resources for organization and tracking.

## Solution

### Option 1: Using AWS Console
1.  We signed in to the AWS Management Console and selected the `us-east-1` region.
2.  We navigated to the **EC2 Dashboard** and selected **Volumes** under the *Elastic Block Store* section.
3.  We clicked **Create volume**.
4.  We configured the volume with the following settings:
    *   **Volume type:** `General Purpose SSD (gp3)`
    *   **Size:** `2` GiB
    *   **Availability Zone:** `us-east-1a` (or any available zone in the region)
5.  We added a tag:
    *   **Key:** `Name`
    *   **Value:** `xfusion-volume`
6.  We clicked **Create volume**.

### Option 2: Using AWS CLI
Alternatively, we used the AWS CLI to create the volume:

```bash
aws ec2 create-volume \
    --volume-type gp3 \
    --size 2 \
    --availability-zone us-east-1a \
    --tag-specifications 'ResourceType=volume,Tags=[{Key=Name,Value=xfusion-volume}]' \
    --region us-east-1
```

## Verification
We verified the volume creation by listing the volumes filtered by the name tag:

```bash
aws ec2 describe-volumes --filters "Name=tag:Name,Values=xfusion-volume" --region us-east-1
```
The output showed the volume `xfusion-volume` in the `available` state.
