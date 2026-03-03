# aws-multi-tier-network-infra

I designed a custom VPC with a 10.0.0.0/16 CIDR block, providing a scalable foundation. To implement the Principle of Least Privilege, I segmented the network into two distinct tiers:

Public Subnet (10.0.10.0/24): Engineered for web-facing resources, associated with an Internet Gateway (IGW) via a custom Route Table.

Private Subnet (10.0.20.0/24): Isolated from the public internet to protect sensitive assets like databases. No route to the IGW exists here, ensuring a "dark" environment.

2. Routing Logic
The "GPS" of this project relies on custom Route Tables:

The Public Route Table contains a 0.0.0.0/0 route pointing to the Internet Gateway.

The Private Route Table remains local-only, preventing any unsolicited inbound traffic from the web.

3. Security Groups (Firewalls)
I configured a stateful Security Group for my test instance with the following rules:

SSH (Port 22): Allowed for administrative access.

ICMP (All IPv4): Enabled specifically to facilitate the "Ping" test and verify network reachability.

4. Verification: The "Ping" Test
To validate the architecture, I launched a t2.micro (Free Tier) EC2 instance into the Public Subnet. By successfully pinging the instance's Public IPv4 address from my local machine, I confirmed that:

The Internet Gateway was correctly attached.

The Route Table was correctly directing traffic.

The Security Group was properly filtering ICMP requests.

 Deployment Steps
VPC Setup: Created VPC with CIDR 10.0.0.0/16.

Subnetting: Defined Public (10.0.10.0/24) and Private (10.0.20.0/24) subnets.

Gateways: Created and attached an Internet Gateway.

Routing: Configured a Public Route Table to allow outbound 0.0.0.0/0 traffic via the IGW.

Compute: Launched an EC2 instance into the Public Subnet with a public IP.

Testing: Executed a ping command from the local terminal to verify end-to-end connectivity.
