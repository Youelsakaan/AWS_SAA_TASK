# AWS_SAA_TASK
Architecture and Workflow Explanation – Scalable Web Application on AWS

I designed and implemented a scalable and highly available web application architecture on AWS
Architecture Overview:

I created a custom VPC that has two Availability Zones to ensure high availability. Each AZ contains both public and private subnets, and the components are:

Public Subnets:

Application Load Balancer (ALB): Accepts incoming HTTP/HTTPS requests and forwards them to EC2 instances in private subnets
Bastion Host: Used for secure administrative SSH access to the private EC2 instances
NAT Gateway: Allows EC2 instances in private subnets to access the internet for software updates
Internet Gateway: Provides internet connectivity to public subnets

Private Subnets:

EC2 Instances: Host the web application backend and are part of an Auto Scaling Group(I placed the EC2 instances inside private subnets to enhance the overall security of the architecture, This setup ensures that the instances are not directly exposed to the internet, and can only receive traffic forwarded by the Application Load Balancer in the public subnets, Access to these instances is only possible through a bastion host, which is placed in the public subnet)
Auto Scaling Group (ASG): Automatically scales the number of EC2 instances up or down based on defined CloudWatch metrics
Amazon RDS (Multi-AZ): Provides a highly available managed relational database with automated failover

Other Services:
Amazon CloudWatch: Collects logs and performance metrics from EC2 and RDS
Amazon SNS: Sends alert notifications based on CloudWatch alarms
AWS IAM Roles: Used to assign permissions to EC2 instances(I assigned an IAM role to the EC2 instances, This role allows the instances to interact with AWS service  CloudWatch (for logging and monitoring))


Workflow Explanation:
User Access:
End-users connect to the application through the Application Load Balancer (ALB), which routes traffic to the EC2 instances across multiple Availability Zones
Web Tier:
The ALB forwards requests to the EC2 instances inside private subnets. These instances are responsible for handling the main business logic and web responses
Database Tier:
The application interacts with an Amazon RDS (Multi-AZ) instance to store and retrieve data. The RDS configuration ensures automatic failover for high availability
Auto Scaling:
Based on traffic and resource usage (such as CPU or memory), the Auto Scaling Group adjusts the number of running EC2 instances automatically
Monitoring & Alerts:
CloudWatch collects metrics and logs from EC2 and RDS. If thresholds are breached, SNS sends notifications to administrators
Admin Access:
For maintenance or manual troubleshooting, I access the EC2 instances via a bastion host, using SSH with limited IP access

