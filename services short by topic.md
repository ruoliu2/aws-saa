## Data & Analytics

| Term               | Definition                                                   |
| ------------------ | ------------------------------------------------------------ |
| Kinesis            | data stream: stream data<br />data firehouse (managed service): collect, transform and store data (auto store data in s3 in parquet format) |
| Glue               | ELT, bookmark to keep track of process, easy for csv to parquet |
| EMR                | Big data query after Glue                                    |
| Dynamo             | fully managed, dynamo backup support backup to s3 glacier by default<br /><br />DynamoDB Accelerator (DAX): in mem cache, speed up RW <br />Dynamo stream: real time analysis <br />Dynamo On-Demand: unknown workloads, scale RW quick<br />Dynamo Provisioned Capacity Mode: known traffic<br />TTL: for data deletion |
| RDS                | can be enctypted creation or restoring from encrypted snapshot, has storage auto scaling<br /><br />RDS custom: allow access to DB OS<br />RDS proxy: allow pool and share connections, avoid high CPU memory by large # of connections<br />Provisioned IOPS SSD: consistent I/O for RDS MySQL<br />mysqldump utility: full export of db, cause app delay |
| Redshift           | big data (petabyte + scale)                                  |
| AWS Lake formation | data lake: manage fine-grained permissions for the data; ingest data into s3 data lake |
| DocumentDB         | compatible with MongoDB                                      |
| Quicksight         | visual for s3, dynamo, aurora, RDS, redshift ...             |
| Aurora             | 5x read speed compared to RDS (serverless), lifecycle/rentention only for 35 days, auto scale <br />Aurora cloning: quickly setting up test env using your prod data |
| elasticache        | distributed                                                  |

## Storage

| Term                         | Definition                                                   |
| ---------------------------- | ------------------------------------------------------------ |
| s3                           | s3 Cross-Region Replication (CRR): sync cross region<br />s3 Transfer Acceleration: speed up transfer into S3<br />s3 object lock: keep from deletion, modify<br />s3 legal hold: retention (not able to delete or modify) until removed<br />s3 GW (s3 VPC GW): allows direct access to S3 (not go through public internet)<br />s3 encryption: turn on default encrypt, use s3 inventory to run job<br />s3 lifecycle: delete previous versioning to delete older objects<br />S3 Object Lambda: transform data (hide sensitive info) and send to multi apps<br />S3 Storage Lens: analyze s3 access pattern, storage / activity trends<br />server side encrypyion header: ensure uploads are encrypted |
| EBS                          | Block storage, high IO (less than ec2 instance storage) and durable<br />EBS fast snapshot: clone EBS / fast AMI creation |
| EFS elastic File system      | available, durable (price > s3), concurrent access           |
| Amazon FSx (window or linux) | for windows, Lustre (linux high performance computing) + SSD (give 6 GBps performance)<br />supports SMB client, fully managed<br />Amazon FSx for NetApp ONTAP: for file share between linux and windows |
| AWS Backup                   | Backup schedule and retention policy (longer than retention 35 days)<br />ec2 instance as backup resource (will backup ebs too) |
| Storage GW                   | transfer to cloud, SMB<br />volume GW: provide on premise local access to file, iSCSI<br />cached GW: only recently visited store locally<br />tape GW: for media tape, iSCSI<br />FSx file GW: SMB / NFS <br />s3 file GW: SMB / NFS, config s3 lifecycle |

## Security

| Term                                    | Definition                                                   |
| --------------------------------------- | ------------------------------------------------------------ |
| Security                                | GuardDuty: monitor sus activity from AWS account<br />Inspector: OS vulnerability<br />Shield: against reflection attacks, SYN flood<br />Shield Advanced: against DDoS attacks (on EC2, ELB, NLB, CloudFront, Global Accelerator, and Route 53)<br />Macie: data privacy (sensitive info)<br />AWS Systems Patch Manager: system patch<br />Amazon Detective: collect and visualize the security event (after it happens) |
| Secrets Manager SSM                     | Store and auto-rotate credentials                            |
| IAM                                     | role: apps, services, used from other accounts (third party)<br />user: individual user<br />policy: users, roles, or groups<br />identity: can be authed, users, roles, or groups<br />identity providers: federated access using external identity systems<br />lambda can use AWS_IAM as auth type |
| Service Control Policies (SCPs)         | limit access to service and actions                          |
| AWS Network Firewall                    | url filtering                                                |
| firewall manager (WAF + OAI)            | simplifies manage AWS WAF (for sql injection and scripting attack), WAF geo match allow to allow from IP of geo location, (on CloudFront, ALB, API GW, AppSync) |
| ACL (access control list) (for VPC)     | allow block on IP, not for DDoS (default allow everything)   |
| Security Group                          | allow block on IP, in and out (virtual firewall) (default reject everything) |
| SSE - KMS (AWS Key Mangagement Service) | encryption/decryption (SSE, both SSE - S3 and SSE - KMS are multi regional) |
| SSE - S3                                | doesn't guarantee rotation in 365 days (varies, while KMS does, for simplicity use SSE-s3) |
| Client VPN                              | for secure access of files                                   |
| Amazon Cognito                          | auth / identity platform (not for internal user)             |
| Active Directory Connector              | integrate AD (no on premise) to other services               |

## Compute & Containers

| Term              | Definition                                                   |
| ----------------- | ------------------------------------------------------------ |
| lambda            | has running time cap 15 min<br />lambda@edge: auth, latency, url, image manipulation, security (block certain IP), (on cloudfront) |
| EC2               | EC2 Spot: tmp instance (a lot cheaper than on demand)<br />EC2 Reserved: for one year or longer, cheaper (doesn't provide capacity selection)<br /> EC2 On-Demand: hourly billed (cannot be interrupted, persistent storage etc)<br />EC2 instance store: tmp storage but max IO<br />M general, T CPU, R memory optimized |
| ECS               | container, microservices, scale w target tracking            |
| AWS Fargate       | Running containers without managing the underlying infra; works with Amazon ECS and EKS (serverless) |
| Elastic Beanstalk | easy deploy, switch between dev and prod (easy testing)<br />used to rehosting app (on premise Net. to AWS)<br />least amount of changes to the application |
| AMI               | (spot fleet) scale (overhead compared to fargate)<br />fast AMI creation: EBS fast snapshot |
| step function     | connecting lambda, microservices                             |

## Networking

| Term                                   | Definition                                                   |
| -------------------------------------- | ------------------------------------------------------------ |
| VPC                                    | Isolated section of AWS<br />GW VPC endpoint (+ AWS privateLink): between VPC and AWS services, doesn't go through public network, privateLink for connection<br />interface VPC endpoint: SSM, ec2, between two API services |
| Peering                                | Between VPC                                                  |
| Transit Gateway                        | Between VPC                                                  |
| GW LB                                  | Virtual appliance and traffic inspection                     |
| NAT GW (NAT instance obsolete)         | allow private subnet to have internet access, but reject non VPC traffic for private (needs to be in public subnet) |
| Internet GW                            | in public subnet, expose to public internet                  |
| Cloudfront                             | CDN (for cacheable), (any HTTP, video stream), (field-level encryption)<br />Cloudfront signed url / cookie: control access to specific content (s3 content) |
| Global Accelerator                     | accelerate UDP/TCP, IoT, Voice over IP (gaming)              |
| ELB                                    | Combination of NLB, ALB, and CLB (Classic)                   |
| ALB                                    | between apps services, SSL encrypt/decrypt, routing traffic in public subnet<br />session affinity: only store session data on one ec2 (not distributed) |
| NLB                                    | For TCP or UDP (support TLS, secyre data in transit)         |
| Route 53                               | needs ALB (not NLB), for DNS routing<br />weighted routing: weighted<br />multivalue routing: random (DNS return multiple) |
| OAI                                    | for Cloudfront, access s3 without directly using s3 url      |
| API GW                                 | RESTful                                                      |
| AWS ctrl tower + Service ctrl policies | deny internet access + grant internet access to one region   |
| CIDR block                             | prefix # (at the end) means how many bits are fixed, 32 - 1 IP address (not valid for VPC) |

## Application Services

| Term           | Definition                                     |
| -------------- | ---------------------------------------------- |
| AppFlow        | SaaS to S3                                     |
| Amazon Connect | customer support and engagement                |
| Pinpoint       | marketing, + kinesis for storage and analytics |

## Management

| Term                           | Definition                                                   |
| ------------------------------ | ------------------------------------------------------------ |
| Cloudwatch                     | Monitor and gather metrics, composite alarms for multi-metric alarm (reduce alarm noise)<br />Eventbridge<br />Cloudwatch application insights: auto discover and monitor<br />Cloudwatch logs: could be used for SSH/RDP<br />readonly: log + other access, share link<br />Cloudwatch stream: send metrics data (ec2 auto scale event) |
| CloudFormation                 | IaC (Infrastructure as Code)                                 |
| X-ray                          | Tracing requests, identifying errors and bottlenecks         |
| AWS Config                     | config change (without user info, while cloudtrail does)     |
| AWS Trusted Advisor            | advice on cost, security, performance optimization           |
| AWS Systems Manager            | speed, can apply to third parties<br />Parameter Store: doesnt offer auto rotation by itself<br />Session manager: replace SSH / provide secure access to ec2 |
| AWS Resource Groups Tag Editor | quickly query resource with tag                              |
| CloudTrail                     | record API calls, security analysis, (for logs)              |



## Migration

| Term                      | Definition                                                   |
| ------------------------- | ------------------------------------------------------------ |
| Data Transfer / Migration | DataSync + DMS (Database Migration Service): postgre - postgre<br />Schema Conversion Tool + DMS: oracle - postgre<br /><br />Data Sync: control bandwidth usage (NFS, SMB, s3, EFS, FSx) (not for rly low bandwidth)<br />Transfer: SFTP / FTP / FTPS, doesn't control bandwidth usage |
| Snow                      | snowball: for hundreds of TB data transfer <br /> snowcone: smaller, 8TB for one device (4-6 biz day to deliver) |

## Messaging

| Term | Definition                                                   |
| ---- | ------------------------------------------------------------ |
| SNS  | msg pub sub, retry would eventually discard after several retry |
| SES  | sending email (rich format like html)                        |
| SQS  | scale based on queue size / backlog per instance, increase visibility timeout<br />SQS Extended Client Library: host larger msg |

## Cost

| Term | Definition                                                                                                                            |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------- |
| cost | AWS budge: set budget and alert if exceed (no graph)<br />Cost Explorer: report by service, graph, 14 days granularity, 12-month data |



## Other

| Term            | Definition                                    |
| --------------- | --------------------------------------------- |
| important ports | 443: all ipv4 (https); 1443: ms mysql default |

