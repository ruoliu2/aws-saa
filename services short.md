| Term                              | Definition                                                        |
|-----------------------------------|-------------------------------------------------------------------|
| Kinesis                           | Stream process                                                    |
| Glue                              | ELT, bookmark to keep track of process |
| EMR                               | Big data query after Glue                                         |
| s3 Cross-Region Replication (CRR)  | Sync cross region                                                 |
| s3 Transfer Acceleration           | Speed up transfer into S3                                         |
| s3 Object Lock                    | Keep from deletion                                               |
| s3 legal hold | retention (not able to delete or modify) until removed |
| s3 File GW                         | For on-premise to access S3 with standard file protocol           |
| s3 GW (s3 VPC GW)                  | Allows direct access to S3 without going through public internet  |
| EBS                               | Block storage, high IO (less than ec2 instance storage) and durable |
| EFS elastic File system            | Available, durable (price > s3)                                   |
| Dynamo                             | Fully managed                                                     |
| DynamoDB Accelerator (DAX) | in mem cache, speed up RW |
| Cloudfront                         | CDN (for cacheble), (any HTTP, video stream), (field-level encryption) |
| Cloudfront signed | ctrl access to specific content (s3 content) |
| Global Accelerator | accelerate UDP/TCP, IoT, Voice over IP |
| CloudFormation                     | IaC (Infrastructure as Code)                                      |
| VPC                                | Isolated section of AWS                                            |
| Peering                            | Between VPC                                                       |
| Redshift                           | Big data                                                           |
| RDS | can be enctypted creation or restoring from encrypted snapshot |
| RDS custom | allow access to DB OS |
| RDS proxy | allow pool and share connections, avoid high CPU memory by large # of connections |
| Aurora                             | 5x read speed compared to RDS (serverless), lifecycle/rentention only for 35 days |
| Aurora cloning | quickly setting up test env using your prod data |
| CloudTrail                         | Record API calls, security analysis                                |
| Cloudwatch (Evenbridge is Cloudwatch events) | Monitor and gather metrics, composite alarms for multi metric alarm (reduce alarm noise) |
| X-ray                              | Tracing requests, identifying errors and bottlenecks               |
| ALB                                | between apps services, SSL encry decry, routing traffic in public subnet |
| NLB                                | For TCP or UDP                                                    |
| ELB                                | Combination of NLB, ALB, and CLB (Classic)                         |
| GuardDuty                          | monitor suspicious activity from AWS account                 |
| Inspector                          | OS vulnerability                                                   |
| Shield                             | Protection against reflection attacks, SYN flood                  |
| Shield Advanced                    | Protection against DDoS attacks                                  |
| Transit Gateway                    | Between VPC                                                        |
| GWLB                               | Virtual appliance and traffic inspection                           |
| AppFlow                            | SaaS to S3                                                          |
| Provisioned IOPS SSD               | High I/O for RDS MySQL                                             |
| Amazon Macie                       | Data privacy/security service                                      |
| AWS Systems Patch Manager          | System patch                                                       |
| AWS Systems Manager                | speed, can apply to third parties                                 |
| AWS Systems Manager Parameter Store | IAM role: param read access, decrypt access from KMS, attach to EC2 |
| Amazon FSx (window or linux) | for windows, Lustre (linux high performance computing) |
| AWS Fargate                        | Running containers without managing the underlying infra; works with Amazon ECS and EKS (serverless) |
| AWS DataSync                       | Data transfer                                                      |
| AWS DMS (Data Migration Service)   | Data migration for databases                                      |
| AWS Backup                         | Backup schedule and retention policy (longer than rentention 35 days) |
| Dynamo On-Demand Mode              | Unknown workloads, unpredictable traffic, pay-as-you-go           |
| Dynamo Provisioned Capacity Mode   | Known traffic                                                      |
| lambda | has time cap 15 min |
| EC2 Spot                           | Temporary instance (a lot cheaper than on demand) |
| EC2 Reserved                       | Purchased for one year or longer, lower price                     |
| EC2 On-Demand                      | Hourly billed                                                      |
| EC2 instance store | tmp storage but max IO |
| ECS | container, microservices |
| Secrets Manager SSM                | Store and auto-rotate credentials                                  |
| mysqldump utility | full export of db, cause app delay |
| important ports | 443: all ipv4; 1443: ms mysql default |
| AWS Key Mangagement Service (AWS KMS) | encryption/decryption (SSE, both SSE - S3 and SSE - KMS are multi regional) |
| NAT GW (NAT instance obsolete) | allow private subnet to have internet access, but reject non VPC traffic for private (needs to be in public subnet) |
| Internet GW | in public subnet, expose to public internet |
| VPC endpoint + AWS privateLink | between vpc and aws services doesnt go thro public network, privatelink for connect |
| OAI | access s3 without directly using s3 url |
| firewall manager (WAF + OAI) | simplifies manage AWS WAF (for sql injection and scripting attack), WAF geo match allow to allow from IP of geo location |
| route 53 | needs ALB (not NLB), for DNS routing |
| AWS DB migration service | for replication between on premise and aws db, on premise db must be online |
| AMI + spot fleet | scale (overhead compared to fargate) |
| SNS | retry would eventually discard after several retry |
| AWS ctrl tower + Service ctrl  policies | deny internet access + grant internet access to one region |
| IAM role | has no auth feat |
| Service Control Policies (SCPs) | limit access to service and actions |
| ACL (for VPC) | allow block on IP, not for DDoS |
| Security Group | allow block on IP, in and out (virtual firewall) |
| IAM role policy | role for ec2 (aws service), policy for users, roles, or groups |
|  |  |
|  |  |