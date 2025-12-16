# Day 1: Create Key Pair

## Challenge Description
The Nautilus DevOps team is migrating parts of their infrastructure to AWS and needs to create an EC2 key pair. The key pair must be named `datacenter-kp`, be of type `rsa`, and be created in the `us-east-1` region.

## Key Concepts
*   **AWS EC2 Key Pairs**: These are SSH key pairs used to securely connect to EC2 instances. They consist of a public key that AWS stores and a private key that we keep.
*   **RSA Key Type**: A widely used type of cryptographic key for secure communications and digital signatures.
*   **AWS Management Console**: A web-based interface for managing AWS services.

## Solution
We created the key pair using the AWS Management Console by following these steps:
1.  Logged into the AWS Management Console.
2.  Navigated to the EC2 Dashboard.
3.  Under "Network & Security," we selected "Key Pairs."
4.  Clicked on "Create key pair."
5.  Provided `datacenter-kp` as the "Name."
6.  Selected `rsa` as the "Key pair type."
7.  Ensured the region was set to `us-east-1`.
8.  Clicked "Create key pair." Since we used the UI, the private key was not automatically downloaded or saved on our local system.

## Verification
To verify the key pair was successfully created, we can:
1.  Log into the AWS Management Console.
2.  Go to the EC2 Dashboard.
3.  In the navigation pane, under "Network & Security," select "Key Pairs."
4.  We should see an entry for `datacenter-kp` with the "Key pair type" listed as `rsa`.
