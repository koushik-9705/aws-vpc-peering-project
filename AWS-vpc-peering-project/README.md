AWS VPC PEERING — CROSS-VPC PRIVATE COMMUNICATION

Project: EWB AWS Internship
Cloud Platform: Amazon Web Services (AWS)
Region: Asia Pacific (Mumbai) — ap-south-1
Service Category: Networking & Security
Project Type: Hands-on Cloud Infrastructure Project


1. PROJECT OVERVIEW

This project demonstrates how to establish private network connectivity between two Amazon Virtual Private Clouds (VPCs) using AWS VPC Peering.

Two VPCs were configured with separate IPv4 CIDR blocks. Route tables and security group rules were configured to support communication between private IP addresses of EC2 instances in the two VPCs. The project also covered SSH connectivity, private IP communication, route configuration, and connectivity testing from the EC2 command line.


2. PROBLEM STATEMENT

AWS resources deployed in separate VPCs are isolated by default. Applications running in different VPCs may need to communicate privately without relying on public IP addresses for communication between those networks.

The objective of this project was to establish a VPC peering connection, configure the required routes and security rules, and validate private communication between EC2 instances.


3. PROJECT OBJECTIVES

- Create and configure two VPCs with distinct CIDR ranges.
- Configure the required subnets.
- Establish a VPC peering connection between the VPCs.
- Update the relevant route tables to direct traffic to the peer network through the peering connection.
- Configure security group rules for required SSH and ICMP traffic.
- Access EC2 instances and test private IP connectivity.
- Document the implementation and observed results with screenshots.


4. ARCHITECTURE AND CONFIGURATION

VPC A — ewb-vpc-a

VPC CIDR: 10.10.0.0/16
Public subnet: 10.10.1.0/24
Private subnet: 10.10.2.0/24
Bastion instance: ewb-bastion
Application instance: ewb-app-a
Application A private IP: 10.10.2.10
VPC ID: vpc-0baaa64b599a9564a
Public route table: ewb-rt-a-public
Route table ID: rtb-03bb7a349ca805781


VPC B — ewb-vpc-b

VPC CIDR: 10.20.0.0/16
Private subnet: 10.20.1.0/24
Application instance: ewb-app-b
Application B private IP: 10.20.1.10
VPC ID: vpc-0edfc5a72b7f3fa9e


VPC PEERING CONNECTION

Connection name: ewb-a-b-peer
Connection ID: pcx-00d88e5dafce78b7e
Region: ap-south-1
Observed status during testing: Active

VPC peering provides private, one-to-one network connectivity between the VPCs. It does not automatically configure routes or permit traffic. The appropriate route-table entries and security group rules are also required.


5. IMPLEMENTATION STEPS

STEP 1 — CONFIGURE THE VPCs

Created two VPCs with separate IPv4 CIDR blocks:

VPC A: 10.10.0.0/16
VPC B: 10.20.0.0/16

The separate address ranges allow the networks to be routed independently.


STEP 2 — CONFIGURE SUBNETS AND EC2 INSTANCES

Configured the required subnets and EC2 instances for testing. The environment included a bastion instance and application instances in the two VPCs.

Bastion: ewb-bastion
Application A: ewb-app-a
Application B: ewb-app-b

The bastion instance was used as the SSH entry point for private-instance access.


STEP 3 — CREATE THE VPC PEERING CONNECTION

Created a VPC peering connection named ewb-a-b-peer between the two VPCs in the Mumbai region.

The recorded connection status was Active.


STEP 4 — CONFIGURE ROUTE TABLES

Configured the relevant route tables so traffic could be directed to the other VPC's CIDR block through the peering connection.

VPC A:
Destination: 10.20.0.0/16
Route target: VPC peering connection

VPC B:
Destination: 10.10.0.0/16
Route target: VPC peering connection

The VPC A public route table also contained the VPC-local route and a default route through its internet gateway. Refer to the project evidence for the recorded route-table configuration.


STEP 5 — CONFIGURE SECURITY GROUPS

Configured security groups for the traffic required by the lab, including SSH access and ICMP connectivity testing.

Security group sources should be restricted to the appropriate trusted IP address or network CIDR. Avoid exposing SSH to the entire internet unless there is a specific, temporary requirement.


STEP 6 — CONNECT USING SSH

Used SSH to access the bastion instance and connect to private application instances. A proxy-style SSH connection was used for private-instance access.

Private key files must remain local and must never be committed to a public GitHub repository.


6. CONNECTIVITY TESTING AND RESULTS

The project evidence records a successful ICMP connectivity test from Application A to Application B.

Source instance: Application A — 10.10.2.10
Destination instance: Application B — 10.20.1.10
Packets transmitted: 4
Packets received: 4
Packet loss: 0%
Minimum round-trip time: Approximately 0.169 ms
Average round-trip time: Approximately 0.263 ms
Maximum round-trip time: Approximately 0.482 ms

The successful ping demonstrates ICMP reachability for the tested path and configuration at the time of testing. It does not, by itself, verify every application protocol or prove that all security controls are configured correctly.


7. TROUBLESHOOTING CONSIDERATIONS

If instances cannot communicate across a VPC peering connection, check:

1. The peering connection is active.
2. The VPCs use non-overlapping CIDR blocks.
3. Each relevant route table contains a route to the other VPC's CIDR.
4. Security groups permit the required source, protocol, and port.
5. Network ACLs permit the required inbound and outbound traffic.
6. The destination instance and its operating system allow the traffic being tested.
7. SSH key permissions, usernames, and proxy settings are correct.

A failed ping does not always mean the peering connection is broken. ICMP may be blocked by a security group, network ACL, or operating-system firewall.


8. KEY LEARNING OUTCOMES

Through this project, I practised:

- AWS VPC architecture and IPv4 CIDR planning.
- VPC peering connection configuration.
- Route-table configuration for private network communication.
- EC2 instance access using SSH.
- Bastion-style access to private instances.
- Security group rules for SSH and ICMP.
- Connectivity verification using terminal commands.
- Troubleshooting network reachability and documenting cloud infrastructure.


9. LIMITATIONS AND FUTURE IMPROVEMENTS

- The setup was created as a hands-on learning environment, not a production-ready enterprise network.
- Public IP addresses and instance states shown in evidence reflect the environment at the time of testing and may change.
- Additional validation would be required for application-level connectivity, monitoring, high availability, and production security.
- Future improvements could include AWS Systems Manager Session Manager, tighter security group rules, centralized logging, infrastructure as code, and automated connectivity checks.
- Network architecture and address ranges should be reviewed before using a configuration in production.


10. EVIDENCE AND SCREENSHOTS

The evidence directory should contain the original screenshots used in the project report. The index below follows the report's figure numbering and describes what each piece of evidence shows.

A. ARCHITECTURE AND VPC CONFIGURATION

Figure 1 — VPC Peering Architecture Diagram

Diagram created from the project screenshots to illustrate the two VPCs and their connection.

Figure 2 — VPC List

AWS VPC console in ap-south-1, showing ewb-vpc-a (10.10.0.0/16) and ewb-vpc-b (10.20.0.0/16) as Available. The default VPC is also visible.

Figure 3 — VPC A Details

VPC name: ewb-vpc-a
VPC ID: vpc-0baaa64b599a9564a
CIDR: 10.10.0.0/16
Status: Available
DNS resolution: Enabled
DNS hostnames: Disabled
Main route table: rtb-033be1caa1aa2f5b9

Figure 4 — VPC B Details

VPC name: ewb-vpc-b
VPC ID: vpc-0edfc5a72b7f3fa9e
CIDR: 10.20.0.0/16
Status: Available
DNS resolution: Enabled
DNS hostnames: Disabled
Main route table: rtb-0f9ab50c83d64617f

Figure 5 — Subnets List

Shows project subnets ewb-a-private (10.10.2.0/24), ewb-a-public (10.10.1.0/24), and ewb-b-private (10.20.1.0/24). The default-VPC subnets visible in the list are not part of this project.

Figure 6 — VPC A Private Subnet

Subnet name: ewb-a-private
Subnet ID: subnet-07d7cd4a11c071c32
CIDR: 10.10.2.0/24
Availability Zone: ap-south-1a
Auto-assign public IPv4: Disabled
Route table: ewb-rt-a-private

Figure 7 — VPC A Public Subnet

Subnet name: ewb-a-public
Subnet ID: subnet-01c584803255d3401
CIDR: 10.10.1.0/24
Availability Zone: ap-south-1a
Auto-assign public IPv4: Disabled
Route table: ewb-rt-a-public

Figure 8 — VPC B Private Subnet

Subnet name: ewb-b-private
Subnet ID: subnet-0a29f322b78b51473
CIDR: 10.20.1.0/24
Availability Zone: ap-south-1a
Auto-assign public IPv4: Disabled
Route table: ewb-rt-b-private


B. ROUTE TABLES AND VPC PEERING

Figure 9 — VPC A Public Route Table

Route table name: ewb-rt-a-public
Route table ID: rtb-03bb7a349ca805781

Shows the following routes:

- 0.0.0.0/0 to internet gateway igw-0fafab354c6908f0d
- 10.10.0.0/16 local
- 10.20.0.0/16 through the peering connection

All routes were shown as Active in the recorded screenshot.

Figure 10 — VPC A Private Route Table

Route table name: ewb-rt-a-private
Route table ID: rtb-04b5d108613636f58

Shows 10.10.0.0/16 local and 10.20.0.0/16 through the peering connection. Both were shown as Active.

Figure 11 — VPC B Private Route Table

Route table name: ewb-rt-b-private
Route table ID: rtb-0f2c292903f88f77e

Shows 10.10.0.0/16 through the peering connection and 10.20.0.0/16 local. Both were shown as Active.

Figure 12 — Private Subnet Association

Subnet associations tab for ewb-rt-a-private, showing the explicit association with ewb-a-private (subnet-07d7cd4a11c071c32, 10.10.2.0/24).

Figure 13 — VPC Peering Connection

Connection name: ewb-a-b-peer
Connection ID: pcx-00d88e5dafce78b7e
Status: Active

Shows requester VPC vpc-0baaa64b599a9564a (ewb-vpc-a) and accepter VPC vpc-0edfc5a72b7f3fa9e (ewb-vpc-b), in the same account.


C. SECURITY GROUP EVIDENCE

Figure 14 — Bastion Inbound Rules

Security group: sg-0ef812dc16c705a9d

The edit-inbound-rules view shows SSH (TCP port 22) from 157.50.125.69/32.

Figure 15 — Application Security Group Inbound Rules

Security group: sg-0e267dfedbcba142f

The edit-inbound-rules view shows All ICMP - IPv4 from 10.20.1.10/32 and SSH from 10.10.1.10/32.

Figure 16 — Application Security Group Inbound Rules

Security group: sg-02ab0878c5cbfeaf9

The edit-inbound-rules view shows All ICMP - IPv4 from 10.10.2.10/32 and SSH from 10.10.1.10/32.

Evidence limitation: These security-group screenshots show the edit-inbound-rules pages. The report does not treat them as proof that the displayed rules were saved. The instance screenshots do not clearly show the application security-group attachment, so that detail is marked as not captured in the report.


D. EC2 INSTANCE DETAILS AND CONNECTIVITY TESTS

Figure 17 — Bastion Instance

Instance name: ewb-bastion
Instance ID: i-07a957f5791d232bc
State: Running at capture time
Instance type: t3.micro
Private IP: 10.10.1.10
Public IP: 43.205.111.8
IAM role: EWB-Bastion-SSM-Role
Subnet: ewb-a-public

Figure 18 — Application A Instance

Instance name: ewb-app-a
Instance ID: i-09a5f9baa26eaafb8
State: Running at capture time
Instance type: t3.micro
Private IP: 10.10.2.10
Public IPv4 address: None
Subnet: ewb-a-private

Figure 19 — Application B Instance

Instance name: ewb-app-b
Instance ID: i-01695738463794277
State: Running at capture time
Instance type: t3.micro
Private IP: 10.20.1.10
Public IPv4 address: None
VPC: ewb-vpc-b
Subnet: ewb-b-private

Figure 20 — SSH Through the Bastion

Laptop PowerShell output showing ssh-add -l and an SSH command using agent forwarding (-A) and ProxyJump (-J) through 43.205.111.8 to ec2-user@10.10.2.10.

The prompt ip-10-10-2-10 confirms access to ewb-app-a. The screenshot then begins the command ping -c 4 10.20.1.10.

Figure 21 — Successful Private-IP Ping

From ewb-app-a, ping -c 4 10.20.1.10 shows:

Packets transmitted: 4
Packets received: 4
Packet loss: 0%
Round-trip time: Approximately 0.169/0.263/0.482 ms (minimum/average/maximum)

The report does not include evidence of a reverse-direction ping, an SSH login to ewb-app-b, or a separately captured controlled ICMP failure and recovery. Do not claim those tests were completed.


E. CLEANUP EVIDENCE

Figures 22–32 document cleanup after the lab. They show the state recorded in the AWS console at capture time; they are not evidence that the project resources are still running.

Figure 22 — Instance Termination

ewb-app-a (i-09a5f9baa26eaafb8), ewb-app-b (i-01695738463794277), and ewb-bastion (i-07a957f5791d232bc) all show Terminated. Each was a t3.micro in ap-south-1a.

Figure 23 — No Running Project Instances

With the instance-state filter set to Running, the EC2 list shows “No matching instances found”.

Figure 24 — Elastic IP Cleanup

The Elastic IP console shows “No Elastic IP addresses found in this Region”.

Figure 25 — Peering Connection Deleted

ewb-a-b-peer, connection ID pcx-00d88e5dafce78b7e, shows status Deleted. The requester and accepter are the two project VPCs in ap-south-1.

Figure 26 — Route Cleanup

ewb-rt-a-public (rtb-03bb7a349ca805781) shows the remaining default route 0.0.0.0/0 to igw-0fafab354c6908f0d and local route 10.10.0.0/16. The peering route is no longer present.

Figure 27 — Subnet Cleanup

The subnet list shows three subnets belonging to the default VPC, with CIDRs 172.31.0.0/20, 172.31.16.0/20, and 172.31.32.0/20.

Figure 28 — Security Group Cleanup

Only the default security group sg-0d26796701fecad03 is listed for VPC vpc-084a7dc8cd5639b2a.

Figure 29 — Route Table Cleanup

The route-table list shows one route table, rtb-0bdbe0017e92d4163, marked Main: Yes.

Figure 30 — Internet Gateway Cleanup

Only internet gateway igw-0c57f103f7de3e92d, attached to default VPC vpc-084a7dc8cd5639b2a, is listed. The project gateway igw-0fafab354c6908f0d is not listed.

Figure 31 — VPC Cleanup

The VPC list shows only default VPC vpc-084a7dc8cd5639b2a. Neither project VPC is listed.

Figure 32 — Billing Home Screenshot

Shows month-to-date cost of $11.91, last month $0.00, forecast “Data unavailable”, Budgets “Setup required”, and two cost anomalies detected.

Review this screenshot for account or billing information before making the repository public.


EVIDENCE FILE NAMING

The descriptions above follow the report's figure numbers. Rename or map the actual image files to descriptive names if useful, for example:

- 01-architecture-diagram.png
- 02-vpc-list.png
- 09-vpc-a-public-route-table.png
- 13-vpc-peering-active.png
- 17-bastion-instance.png
- 21-successful-private-ip-ping.png
- 22-instance-termination.png

These are suggested names only; include only images you actually have. The cleanup screenshots were supplied without original filenames, so identify them by their content rather than inventing original filenames.


11. REPOSITORY STRUCTURE

aws-vpc-peering-project/
|
|-- README.md
|-- .gitignore
|-- report/
|   |-- AWS-VPC-Peering-Report.docx
|   |-- AWS-VPC-Peering-Report.pdf
|
|-- architecture/
|   |-- vpc-peering-architecture.png
|
|-- evidence/
    |-- 01-vpc-configuration.png
    |-- 02-subnet-configuration.png
    |-- 03-route-tables.png
    |-- 04-vpc-peering-status.png
    |-- 05-security-groups.png
    |-- 06-ec2-instances.png
    |-- 07-connectivity-test.png

The filenames above illustrate the intended structure. The repository should contain the actual files available for the project.


12. SECURITY AND CLEANUP

This repository is intended for project documentation and learning. Do not commit private keys, AWS credentials, access tokens, passwords, or .env files containing secrets. Review screenshots and documents for sensitive account information before publishing.

AWS resources should be monitored and removed when no longer needed to avoid unnecessary charges. Do not assume resources are still running if they have already been cleaned up.


CONCLUSION

This project provided practical experience in connecting separate AWS VPCs through VPC peering. By configuring routes and security group rules and testing private IP connectivity between EC2 instances, I practised AWS networking fundamentals, access control, troubleshooting, and cloud infrastructure documentation.
