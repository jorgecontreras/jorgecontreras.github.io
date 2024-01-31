# AWS Developer Associate

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