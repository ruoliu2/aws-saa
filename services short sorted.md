| Term                                       | Definition                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| ACL (access control list) (for VPC) | allow block on IP, not for DDoS                                  |
| ALB                              | between apps services, SSL encry decry, routing traffic in public subnet<br />session affinity: only store session data on one ec2 (not distributed) |
| AMI + spot fleet                 | scale (overhead compared to fargate)                             |
| API GW                           | restful                                                          |
| AWS Backup                       | Backup schedule and retention policy (longer than rentention 35 days) |
| AWS Config                       | config change                                                    |
| AWS Fargate                      | Running containers without managing the underlying infra; works with Amazon ECS and EKS (serverless) |
| AWS Lake formation               | data lake: manage fine-grained permissions for the data          |
| AWS Resource Groups Tag Editor   | quickly query resource with tag                                  |
| AWS Systems Manager              | speed, can apply to third parties<br /><br />Parameter Store: IAM role w param read access, decrypt access from KMS, attach to EC2<br />Session manager: SSH |
| AWS Systems Patch Manager        | System patch                                                     |
| AWS ctrl tower + Service ctrl policies | deny internet access + grant internet access to one region       |
| Amazon Cognito                   | identity platform                                                |
| Amazon Connect                   | customer support and engagement                                  |
| Amazon FSx (window or linux)     | for windows, Lustre (linux high performance computing)<br />supports SMB client, fully managed |
| Amazon Macie                     | Data privacy/security service                                    |
| AppFlow                          | SaaS to S3                                                       |
| Aurora                           | 5x read speed compared to RDS (serverless), lifecycle/rentention only for 35 days<br />Aurora cloning: quickly setting up test env using your prod data |
| CloudFormation                   | IaC (Infrastructure as Code)                                     |
| CloudTrail                       | record API calls, security analysis, (for logs)                  |
| Cloudfront                       | CDN (for cacheble), (any HTTP, video stream), (field-level encryption)<br /><br />Cloudfront signed: ctrl access to specific content (s3 content) |
| Cloudwatch                       | Monitor and gather metrics, composite alarms for multi metric alarm (reduce alarm noise)<br />Eventbridge<br />Cloudwatch application insights: auto discover and monnitor<br />Cloudwatch logs: could be used for SSH/RDP<br />readonly: log + other access, share link |
| Data Migration                   | DataSync + DMS (Database Migration Service): postgre - postgre<br />Schema Conversion Tool + DMS: oracle - postgre |
| DocumentDB                       | compatible with mongoDB                                          |
| Dynamo                           | fully managed<br /><br />DynamoDB Accelerator (DAX): in mem cache, speed up RW <br />Dynamo stream: real time analysis <br />Dynamo On-Demand: unknown workloads, scale RW quick<br />Dynamo Provisioned Capacity Mode: known traffic<br />TTL: for data deletion |
| EBS                              | Block storage, high IO (less than ec2 instance storage) and durable<br />EBS fast snapshot: clone EBS |
| EC2                              | EC2 Spot: tmp instance (a lot cheaper than on demand)<br />EC2 Reserved: for one year or longer, cheaper (doesnt provide capacity selection)<br /> EC2 On-Demand: hourly billed (cannot be interrupted, persistent storage etc)<br />EC2 instance store: tmp storage but max IO<br />M general, T CPU, R memory optimized |
| ECS                              | container, microservices                                         |
| EFS elastic File system          | available, durable (price > s3), concurrent access               |
| ELB                              | Combination of NLB, ALB, and CLB (Classic)                       |
| EMR                              | Big data query after Glue                                        |
| Elastic Beanstalk                | easy deploy, switch between dev and prod (easy testing)<br />used to rehosting app (on premise Net. to AWS)<br />least amount of changes to the application |
| GW VPC endpoint                  | for s3 direct access (VPC endpoint is better than IP address)    |
| GWLB                             | Virtual appliance and traffic inspection                         |
| Global Accelerator               | accelerate UDP/TCP, IoT, Voice over IP                           |
| Glue                             | ELT, bookmark to keep track of process, easy for csv to parquet  |
| GuardDuty                        | monitor suspicious activity from AWS account                     |
| IAM                              | role: apps, services, used from other accounts (third party) <br />user: individual user<br />policy: users, roles, or groups<br />identity: can be authed, users, roles, or groups<br />identity providers: federated access using external identity systems<br />lambda can use AWS_IAM as auth type |
| IAM role                         | has no auth feat                                                 |
| Inspector                        | OS vulnerability                                                 |
| Internet GW                      | in public subnet, expose to public internet                      |
| Kinesis                          | Stream process                                                   |
| NAT GW (NAT instance obsolete)   | allow private subnet to have internet access, but reject non VPC traffic for private (needs to be in public subnet) |
| NLB                              | For TCP or UDP                                                   |
| OAI                              | for cloudfront, access s3 without directly using s3 url          |
| Peering                          | Between VPC                                                      |
| Pinpoint                         | marketing, + kinesis for storage and analitics                   |
| Provisioned IOPS SSD             | High I/O for RDS MySQL                                           |
| Quicksight                       | visual for s3, dynamo, aurora, RDS, redshift ...                 |
| RDS                              | can be enctypted creation or restoring from encrypted snapshot<br /><br />RDS custom: allow access to DB OS<br />RDS proxy: allow pool and share connections, avoid high CPU memory by large # of connections<br />Provisioned IOPS SSD: consistent I/O for RDS MySQL |
| Redshift                         | big data (petabyte + scale)                                      |
| SES                              | sending email                                                    |
| SFTP / FTP (secure file transfer protocol) | aws transfer                                                     |
| SNS                              | msg pub sub, retry would eventually discard after several retry  |
| SQS                              | scale based on queue size / backlog per instance, increase visibility timeout |
| SSE - KMS (AWS Key Mangagement Service) | encryption/decryption (SSE, both SSE - S3 and SSE - KMS are multi regional) |
| SSE - S3                         | doesnt guarantee rotation in 365 days (varies, while KMS does, for simplicity use SSE-s3) |
| Secrets Manager SSM              | Store and auto-rotate credentials                                |
| Security Group                   | allow block on IP, in and out (virtual firewall)                 |
| Service Control Policies (SCPs)  | limit access to service and actions                              |
| Shield                           | Protection against reflection attacks, SYN flood                 |
| Shield Advanced                  | Protection against DDoS attacks (on EC2, ELB, NLB, CloudFront, Global Accelerator, and Route 53) |
| Snowball device                  | for hunderds of TB data transfer                                 |
| Transit Gateway                  | Between VPC                                                      |
| VPC                              | Isolated section of AWS                                          |
| VPC endpoint + AWS privateLink   | between vpc and aws services doesnt go thro public network, privatelink for connect |
| X-ray                            | Tracing requests, identifying errors and bottlenecks             |
| cost                             | AWS budge: set budget and alert if exceed (no graph)<br />Cost Explorer: report by service, graph, 14 days granularity, 12 month data |
| elasticache                      | distributed                                                      |
| firewall manager (WAF + OAI)     | simplifies manage AWS WAF (for sql injection and scripting attack), WAF geo match allow to allow from IP of geo location, (on CloudFront, ALB, API GW, AppSync) |
| important ports                  | 443: all ipv4 (https); 1443: ms mysql default                    |
| interface VPC endpoint           | for SSM, ec2                                                     |
| lambda                           | has time cap 15 min                                              |
| mysqldump utility                | full export of db, cause app delay                               |
| route 53                         | needs ALB (not NLB), for DNS routing<br /><br />weighted routing: weighted<br />multivalue routing: random |
| s3                               | s3 Cross-Region Replication (CRR): sync cross region<br />s3 Transfer Acceleration: speed up transfer into S3<br />s3 object lock: keep from deletion, modify<br />s3 legal hold: retention (not able to delete or modify) until removed<br />s3 File GW: for on-premise to access S3 (over NFS or SMB), config lifecycle<br />s3 GW (s3 VPC GW): allows direct access to S3 (not go through public internet)<br />s3 encryption: turn on default encrypt, use s3 inventory to run job<br />s3 lifecycle: delete previous versioning to delete older objects |