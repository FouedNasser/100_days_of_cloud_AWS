# Day 2: Create Security Group

## Challenge Description
The Nautilus DevOps team is migrating infrastructure to AWS. For this task, we need to create a security group named `nautilus-sg` in the default VPC in the `us-east-1` region. The security group should have the description "Security group for Nautilus App Servers" and allow inbound traffic for HTTP (port 80) and SSH (port 22) from any source (`0.0.0.0/0`).

## Key Concepts
*   **Security Groups:** Virtual firewalls for EC2 instances to control inbound and outbound traffic.
*   **Inbound Rules:** Rules that define allowed incoming traffic.
*   **VPC (Virtual Private Cloud):** A logically isolated section of the AWS Cloud.
*   **CIDR Notation:** Used to specify IP address ranges (e.g., `0.0.0.0/0` for all IPs).

## Solution
To complete this challenge using the AWS Management Console, we followed these steps:

1.  Logged into the AWS Management Console and ensured the region was set to **us-east-1 (N. Virginia)**.
2.  Navigated to the **VPC** service.
3.  In the left navigation pane, selected **Security Groups**.
4.  Clicked on **Create security group**.
5.  Entered the following details:
    *   **Security group name:** `nautilus-sg`
    *   **Description:** `Security group for Nautilus App Servers`
    *   **VPC:** Selected the default VPC.
6.  Under **Inbound rules**, we added two rules:
    *   **Type:** HTTP | **Port range:** 80 | **Source:** 0.0.0.0/0
    *   **Type:** SSH | **Port range:** 22 | **Source:** 0.0.0.0/0
7.  Clicked **Create security group** to finish.

## Verification
1.  In the **Security Groups** dashboard, we searched for `nautilus-sg`.
2.  Selected the security group and checked the **Inbound rules** tab.
3.  Verified that both HTTP (80) and SSH (22) rules exist with the source `0.0.0.0/0`.
