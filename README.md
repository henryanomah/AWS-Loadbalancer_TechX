
# Tech X Assignment: Load Balancing on AWS

## Objective
Demonstrate practical understanding of AWS EC2 instances, Application Load Balancer (ALB) routing, and basic service reliability through failover testing.

## Architecture & Resources Used
* **Compute:** 2x Amazon EC2 `t3.micro` instances
* **Web Server:** Apache (`httpd`) installed via EC2 User Data
* **Networking:** Application Load Balancer (ALB) spanning multiple Availability Zones
* **Health Checks:** Target Group configured to perform HTTP health checks on port 80

## Setup Steps
1. **Provisioned EC2 Instances:** Launched two EC2 `t3.micro` instances in the `us-east-2` region. Allowed incoming HTTP traffic in the security group.
2. **Automated Server Setup:** Used bash scripts in the EC2 "User Data" field to automatically update the OS, install Apache, start the service, and write a custom `index.html` file to distinguish the servers ("Server 1" and "Server 2").
3. **Configured Target Group:** Created a target group registering both EC2 instances and set up an HTTP health check on the root path (`/`).
4. **Deployed Load Balancer:** Created an Internet-facing Application Load Balancer (ALB), attached a security group allowing inbound traffic from `0.0.0.0/0` on port 80, and routed the listener to the target group.

## Reliability / Failover Test
To verify the load balancer's failover capability:
1. Accessed the ALB DNS URL in a browser to confirm traffic was actively routing and alternating between Server 1 and Server 2.
2. Manually stopped the "Server 1" EC2 instance via the AWS Console.
3. Reloaded the ALB URL and verified that the load balancer recognized the failed health check on Server 1 and successfully routed 100% of the traffic to Server 2 without downtime.
