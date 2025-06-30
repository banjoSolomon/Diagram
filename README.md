AWS Cloud Architecture Overview
This document describes the AWS cloud architecture based on the provided diagrams. It appears there are two distinct architectural configurations, possibly representing a "current" and a "proposed" or "version 1" and "version 2" setup. Both configurations aim to provide a secure, scalable, and highly available environment for a web application and its supporting services, including integration with an on-premise environment.

Architecture 1 (Top Diagram)
This architecture focuses on a multi-tier application deployed within a Virtual Private Cloud (VPC) across multiple Availability Zones for high availability.

Key Components:
Region: The entire infrastructure is deployed within a single AWS Region.

VPC (Virtual Private Cloud): The isolated network environment.

Internet Gateway: Allows communication between resources in the VPC and the internet.

Route 53: DNS web service, likely used for routing user requests to the WAF.

WAF (Web Application Firewall): Protects the web application from common web exploits.

Amplify: Likely used for hosting the frontend application.

Public Subnet:

NAT Gateway: Enables instances in private subnets to connect to the internet or other AWS services, while preventing the internet from initiating connections with those instances.

ALB (Application Load Balancer): Distributes incoming application traffic across multiple targets, such as EC2 instances.

EBS (Elastic Block Store): Provides block-level storage volumes for use with EC2 instances.

Private Subnet (within Availability Zones):

Auto Scaling Group: Manages the scaling of EC2 instances for services like IDM, Payment, Wallet, Notification, and Keycloak.

EC2 Instances: Hosts the application logic and services.

Security Groups: Act as virtual firewalls that control inbound and outbound traffic to instances.

Services:

IDM (Identity Management): Likely handles user authentication and authorization.

Payment: Processes payment transactions.

Wallet: Manages user wallets.

Notification: Handles sending notifications.

Keycloak: Open-source identity and access management solution.

MongoDB: NoSQL database, likely hosted within a private subnet or a dedicated service.

ECR (Elastic Container Registry): Docker container registry, used for storing and managing container images.

Secrets Manager: Stores and manages secrets like database credentials.

On-Premise Integration:

VPN (Virtual Private Network): Securely connects the AWS VPC to the on-premise environment.

Services (on-premise): Harbari, UP, ISW.

Availability Zone (on-premise): Implies redundancy within the on-premise setup.

Architecture 2 (Bottom Diagram)
This architecture introduces PostgreSQL and makes some structural changes, potentially optimizing for different database needs or adding new services.

Key Components (Differences and Additions from Architecture 1):
NAT Gateway Placement: The NAT Gateway is explicitly shown within the public subnet alongside NOW and Gateway, suggesting a slightly different network routing configuration.

Auto Scaling Group Services: The services within the Auto Scaling Group are IDM, Payment, Wallet, Notification, and Terminal. Keycloak is not explicitly shown here, and Terminal is a new addition.

CloudWatch: Added for monitoring and logging of AWS resources and applications.

PostgreSQL: A new relational database service is introduced in a separate private subnet, implying a need for relational data storage in addition to MongoDB.

MongoDB: Remains, potentially serving different data storage needs or migrating some data to PostgreSQL.

On-Premise Integration: ISW is explicitly shown as part of the on-premise setup, consistent with the top diagram.

Overall Goals (Common to Both Architectures):
High Availability: Deployment across multiple Availability Zones to ensure continuous operation in case of an AZ outage.

Scalability: Use of Auto Scaling Groups and load balancers to handle varying traffic loads.

Security: Implementation of WAF, Security Groups, and private subnets to protect resources and data.

Hybrid Cloud: Integration with on-premise infrastructure using VPN for seamless operations.

Managed Services: Utilization of AWS managed services (e.g., Route 53, WAF, ALB, ECR, Secrets Manager, CloudWatch) to reduce operational overhead.
