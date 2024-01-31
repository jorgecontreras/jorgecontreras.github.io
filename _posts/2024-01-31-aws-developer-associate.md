# AWS Developer Associate

- [AWS Developer Associate](#aws-developer-associate)
  - [EC2](#ec2)
    - [Instance types](#instance-types)
  - [EBS and EFS](#ebs-and-efs)
  - [Elastic Load Balancer](#elastic-load-balancer)
    - [ALB](#alb)

## EC2
EC2 is a fundamental building block for many cloud-based applications and services

### Instance types

EC2 instances are optimized for different use cases: 
- General purpose
- Compute optimized
- Memory optimized
- Storage optimized
- Accelerated computing
- High performance computing
  
[more...](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html#AvailableInstanceTypes)

## EBS and EFS

| Feature                            | Amazon EFS                             | Amazon EBS                               |
|------------------------------------|----------------------------------------|------------------------------------------|
| **Use Case**                       | Shared file storage across instances   | Block-level storage for individual instances |
| **Access Method**                   | NFS protocol                           | Raw block device attached to an instance |
| **Scalability**                     | Scales automatically with storage needs | Requires manual adjustment or additional volumes |
| **Durability and Availability**     | Redundantly stored across AZs          | Durability tied to specific volume type and infrastructure |
| **Typical Applications**            | Content management, web serving, big data | Boot volumes, Databases, transactional workloads         |

[more...](https://aws.amazon.com/efs/when-to-choose-efs/)


**scalability vs high availability (HA)**:
```
scalability is about handling growth and maintaining performance as the workload increases, high availability is about ensuring consistent uptime and reliability of the system, even during failures or maintenance events. 
```

## Elastic Load Balancer

Load balancers are servers that forward traffic to multiple servers downstream:

- single point of access
- distribute load
- health checks on instances

**Type of load balancers on AWS**:

- Classic Load Balancer (CLB) _deprecated_
- Application Load Balancer (ALB)
- Network LB (NLB)
- Gateway LB (GWLB)

### ALB

Routing based on:
- URL
- hostname
- query string params

Great for microservices and containers

These load balancers can route to multiple target groups:
- EC2 instances
- ECS tasks
- Lambda functions
- IP Addresses



