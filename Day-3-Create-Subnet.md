# Day 3: Create Subnet

## Challenge Description
The challenge requires us to create one subnet named `devops-subnet` under the default VPC in the `us-east-1` region.

## Key Concepts
*   **VPC (Virtual Private Cloud):** A logically isolated section of the AWS Cloud where you can launch AWS resources in a virtual network that you define.
*   **Subnet:** A range of IP addresses in your VPC.
*   **CIDR Block:** Classless Inter-Domain Routing is a method for allocating IP addresses and IP routing.
*   **AWS Management Console:** The web interface for managing AWS resources.

## Solution
We used the AWS Management Console to complete this challenge.

1.  **Log in to the Console:**
    *   We logged into the provided AWS Console URL using the supplied credentials.
    *   We ensured the region was set to `us-east-1` (N. Virginia).

2.  **Navigate to VPC Dashboard:**
    *   In the search bar, we typed "VPC" and selected the VPC service.

3.  **Create Subnet:**
    *   In the left navigation pane, we clicked on **Subnets**.
    *   We clicked the **Create subnet** button.
    *   **VPC ID:** We selected the default VPC from the dropdown list.
    *   **Subnet name:** We entered `devops-subnet`.
    *   **Availability Zone:** We left this as "No preference" (or selected a specific one like `us-east-1a`).
    *   **IPv4 CIDR block:** We entered a valid CIDR block that did not overlap with existing subnets (e.g., `172.31.96.0/20`).
    *   We clicked **Create subnet** to finish the process.

## Verification
1.  Navigate back to the **Subnets** dashboard in the VPC console.
2.  Filter the list by the tag Name: `devops-subnet`.
3.  We verified that the subnet exists and its state is **Available**.
