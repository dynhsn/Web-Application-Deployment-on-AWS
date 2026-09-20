# Web App Deployment on AWS

Project is a hands-on cloud infrastructure learning exercise showcasing the manual provisioning and configuration of a multi-tiered, fault tolerant web application environment on Amazon Web Services (AWS).

By manually configuring the network, security perimeters, compute nodes and load balancers this project demonstrates foundational AWS concepts and systems administration best practices.

first, let’s understand,

## what is VPC?

The full form of VPC is a virtual private network. you can think in this way that you have your own personal area or a little corner on the internet within AWS that you can use for running your applications on the internet. That personal area is called VPC. This VPC keeps your data separate from other users within the AWS. You don’t need to worry about interference from others.
You can manage your space and create different areas in that little corner for different purposes. those different areas are called Subnets. You are the one who will decide who can come to your place by using some rules that are called security groups.
You can think that you have your own house in the cloud AWS and you can create many rooms in that house and you are the one who will decide who will decide who can come to your house. In this scenario. the house is VPC, rooms are subnets and rules are security groups who will secure your house.
hope you understand this, now let’s move to the next part,

## Architecture

See the project's architecture diagram below:
![Project Architecture](architecture-diagram.png)

The application is deployed across two availability zones in the `ap-south-1` region and features a hardened networking security perimeter isolating the public internet from the compute layer.

### Network Infrastructure
Custom VPC (`AWS-Project-vpc`) with CIDR `10.0.0.0/16` serving as the private corporate network for the application:
• Public subnets spanning across `ap-south-1a` and `ap-south-1b` hosting the bastion, load balancers and internet gateway
• Private subnets spanning across `ap-south-1a` and `ap-south-1b` housing the application instances
• Internet gateway attached to the VPC to provide internet access to the public subnets
• NAT gateway configured with an associated elastic IP to enable internet-bound traffic from private subnet instances for yum updates
• S3 Gateway endpoint configured for optimized access to AWS S3

### Scaling, Compute and Security

• Auto scaled application using Amazon Elastic Compute Cloud (EC2) instances launched from launch template `AWS-Project-ASG` with RHEL 9 OS image
• Instances deployed to private subnets behind a network load balancer spanning across availability zones with a desired capacity of 2 and max of 4
• Security groups controlling traffic flow to and from instances

• Bastion jump server instance launched into public subnet to enable secure administrative access to the application instances
• Security group for bastion host allowing SSH access from anywhere while restricting direct SSH access to application instances
• Security group for application allowing traffic on Port 8000 from the bastion host and HTTP traffic on Port 80 from the Application Load Balancer (ALB)

### TrafficRouting and Load Balancing
Internet facing Application Load Balancer (`AWS-Project-ALB`) spanning across public subnets listening on HTTP Port 80:
• Target group `AWS-Project_TG` configured to forward traffic on Port 80 to application instances on Port 8000 with regular health checks
• Bastion host jump server allowing secure administrative access to the application servers
### Application
The application serves a responsive styled Introduction to DevOps web curriculum:
Web application is hosted on a simple Python HTTP server daemon:
```bash
sudo python -m http.server 8000
```
Serving the custom `index.html` assets from the private instance.
The application was verified to be accessible over HTTP with 200 OK response from the public ALB DNS endpoint.
## Tools and Technologies
This project demonstrates proficiency in the following tools and technologies:
AWS Infrastructure as Code Traffic Management Zero Direct Public Exposure Linux System Administration
VPCs Elastic Load Balancing Bastion Hosts RHEL Package Management
CIDR, Subnets, Internet Gateway, Route Tables, VPC Endpoints Target Groups Security Groups Network ACLs Shell Scripting, Vi, Yum

- ---

## Author

**Dayyan Hasan**
- **LinkedIn:** [Dayyan Hasan](https://www.linkedin.com/in/dayyanhasan57)
- **Medium:** [Dayyan Hasan](https://medium.com/@dayyanhasan)
- **GitHub:** [Dayyan Hasan](https://github.com/dynhsn)

- ---

## 📄 License

Open for use in coursework, portfolios, and teaching material. Attribution appreciated.
