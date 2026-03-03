# AWS VPC Network Architecture Project

## 1. VPC Architecture Design

I designed a custom VPC with a `10.0.0.0/16` CIDR block, providing a scalable and flexible networking foundation.

To implement the **Principle of Least Privilege**, I segmented the network into two distinct tiers:

### Public Subnet (`10.0.10.0/24`)
- Designed for web-facing resources
- Associated with an Internet Gateway (IGW) via a custom Route Table
- Allows controlled inbound and outbound internet access

### Private Subnet (`10.0.20.0/24`)
- Isolated from the public internet
- Intended for sensitive resources such as databases
- No route to the Internet Gateway
- Maintains a secure "dark" environment

---

## 2. Routing Logic

The routing layer acts as the "GPS" of the architecture through custom Route Tables.

### Public Route Table
- Contains route: `0.0.0.0/0 → Internet Gateway`
- Enables outbound internet access for public subnet resources

### Private Route Table
- Local routing only
- No route to Internet Gateway
- Prevents unsolicited inbound internet traffic

---

## 3. Security Groups (Stateful Firewall)

A stateful Security Group was configured for the EC2 test instance with the following rules:

- **SSH (Port 22)**: Allowed for administrative access
- **ICMP (All IPv4)**: Enabled to allow ping testing and verify connectivity

---

## 4. Verification – Connectivity Testing

To validate the architecture:

- Launched a `t2.micro` (AWS Free Tier) EC2 instance into the Public Subnet
- Assigned a public IPv4 address
- Executed a ping command from the local machine to the instance’s public IP

Successful results confirmed:

- Internet Gateway was correctly attached
- Route Table was properly directing traffic
- Security Group correctly allowed ICMP traffic

---

# Deployment Steps

1. **VPC Setup**
   - Created VPC with CIDR `10.0.0.0/16`

2. **Subnetting**
   - Created Public Subnet: `10.0.10.0/24`
   - Created Private Subnet: `10.0.20.0/24`

3. **Internet Gateway**
   - Created and attached IGW to the VPC

4. **Routing Configuration**
   - Created Public Route Table
   - Added route `0.0.0.0/0 → IGW`
   - Associated Public Subnet with Public Route Table

5. **Compute Layer**
   - Launched EC2 instance in Public Subnet
   - Assigned public IP address
   - Attached configured Security Group

6. **Testing**
   - Executed `ping <public-ip>` from local terminal
   - Verified end-to-end connectivity

