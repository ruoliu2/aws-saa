###### 1. data from global to one s3 -> S3 Transfer Acceleration on the destination S3 bucket

S3 Cross-Region Replication for replication not for data transfer

EBS is for block storage for ec2



###### 2. running simple on-demand (ad-hoc ) sql query in s3 -> athena

redshift need to load data and run sql

cloudwatch is not for running ad-hoc sql

Glue catagorize + EMR spark, too much overhead



###### 3. s3 access control for org -> PrincipalOrgID global condition key

Condition keys in AWS provide granular control over actions. Useful with AWS Organizations:

\- aws:PrincipalOrgID simplifies specifying the Principal element in a resource-based policy. It provides an alternative to listing all account IDs for all AWS accounts in an organization. Instead, specify the organization ID in the Condition element.

\- aws:PrincipalOrgPaths matches members of a specific organization root, OU, or its children. The condition key returns true when the principal making the request is in the specified organization path. A path represents the structure of an AWS Organizations entity.

dont use OU and PrincipalOrgPaths cuz more overhead cuz need to list each OU path



###### 4. EC2 in VPC, needs to access the S3 bucket without connectivity to the internet -> gateway VPC endpoint

private VPC for cloudwatch, but cloudwatch log takes up to 12 hours to s3 bucket, only need ec2 to s3

instance profile just grant access but doesnt help ec2 connect to s3 privately

API gateway w private link to s3, api gateway forward request to aws lambda, ec2, elastic load balancing, dynamo, kinesis, or https based endpoint, not s3



###### 5. 2 EBS instances with 2 EC2, concurrent access -> EFS

ebs doesnt support cross az (availability zone), EFS does

EFS, keyword **concurrent access**



###### 6. NFS transfer to s3, 70TB no longer growing, as soon as possible minimum bandwidth -> snowball edge

snowball edge, copy to device, then send back to s3

IAM & AWS CLI to copy locally to s3, take too long, too much bandwidth

file gateway use 70TB internet bandwidth, and internet speed is max 1GBps, slow and use bandwidth

direct connect, up to 10 GBps speed, 15.5 hours, but takes a week to set up and 70TB peer to peer bandwidth between on premises env and AWS



###### 7. msg consumption, traffic peak, decouple services -> SNS & SQS

SNS & SQS, max 3k msg per second. **keyword: decouple**, One pattern that allows that decoupling is the fan-out pattern, which sets an SNS topic with many SQS queues that process the message

kinesis for data analytics, not for msg to microservices

ec2 auto scale wont scale w msg quantity

KDS Kinesis Data Streams w single shard for large # of msg wont work, cant handle load

 

###### 8. SQS for job destination, auto scale based on queue size

coordinates jobs across multiple nodes

SQS could be used as destination for jobs (jobs are collected then acted upon compute units)

cloudtrail and eventBridge are typically not used for destination for jobs

cloudtrail for log API activity

eventBridge is event bus 

auto scale on queue size make sense, while schedule auto scaling doesnt make sense



###### 9. provide file lifecycle, without reducing latency to recently accessed file -> S3 file gateway

S3 file gateway, life cycle to transfer to glacier deep: allow cache recently visited file, also allow access to s3 thro standard file protocol

dataSync sync to AWS, Amazon FSx extend storage, doesnt provide life cycle or latency

each pc  install utility to access to s3, life cycle to transfer to glacier deep: utility is vague



###### 10. ordering of processing order -> SQS FIFO queue



###### 11. store credential, least overhead -> AWS Secrets Manager, automatic rotation

Secrets Manager parameter store doesnt support auto rotation



###### 12. domain route53, s3, ALB; reduce latency for dynamic and static -> cloudfont

Create an Amazon CloudFront distribution that has the S3 bucket and the ALB as origins. Configure Route 53 to route traffic to the CloudFront distribution.

CloudFront  cache both static and dynamic api calls 

Global Accelerator  

non HTTP use case like gaming (UDP), IoT (MQTT), or Voice over IP

HTTP use cases: require static IP addresses and deterministic, fast regional failover



###### 13. rotate credentials for its Amazon RDS for MySQL databases across multiple AWS Regions, least operational overhead -> AWS Secrets Manager

Secure string parameter only apply for Parameter Store



###### 14. read req >> write, auto scale -> aurora 

aurora read performance 5x over RDS mysql, high availability A-Z deployment



###### 15. traffic inspection and traffic filtering for the production VPC on AWS -> AWS Network Firewall 

Amazon GuardDuty for threat detection service

Traffic Mirroring for replicate and send a copy of network traffic from a VPC to another VPC or on-premises (run local) location

AWS Firewall Manager for manage firewalls across accounts



###### 16. visualization on s3 and RDS postgre  -> quicksight

Keywords: 

- Data lake on AWS. 
- Consists of data in Amazon S3 and Amazon RDS for PostgreSQL. 

QuickSight only support users and groups, not IAM. We use users and groups to view the QuickSight dashboard 



###### 17. ec2 instance s3 access -> IAM role

ec2 -> IAM roles

IAM policy-> IAM user or group

IAM group -> group together IAM users and policies  

IAM user -> represent a person or service that interacts with AWS resources, not to grant access to resources.



###### 18. durable, stateless, automatic processing -> SQS w s3 notification / SQS lambda

in memory file, voilate stateless

ec2 monitor voilate stateless

eventbridge, not auto process



###### 19. vpc, add, third party firewall for inspection before web server -> gateway load balancer

GWLB easy for virtual appliance and traffic inspection

Network Load Balancer NLB, needs handling of traffic

Application LB ALB, route http traffic, higher overhead compared to NLB, not suitable for deep packet inspection

transit gateway, moderate to high overhead



###### 20. modification of clone data of EBS doent affect original, minimize time -> EBS fast snapshot

Keywords: 

- Modifications to the cloned data must not affect the production environment. 
- Minimize the time that is required to clone the production data into the test environment.

fast snapshot restore works



create new EBS, cannot restore

restore snapshot takes too long

multi attach would afffect original



###### 21. website 24 hour sale, handle high load on peak traffic -> dynamoDB, API gateway, lambda, s3 + cloudfront  (all infinitely scalable)

API GW lambda dynamo all fully managed

only s3 doesnt provide million of request capacity

ec2 and ALB and RDS, would require more operational overhead

EKS more operational overhead for container, RDS also overhead



###### 22. resiliency, unpredictable pattern of access -> s3 intelligence tiering

unpredictable pattern

resilience requirement

-> 

S3 Standard, S3 Intelligent-Tiering, S3 Standard-IA, S3 Glacier Instant Retrieval, S3 Glacier Flexible Retrieval, and S3 Glacier Deep Archive redundantly store objects on multiple devices across a minimum of three Availability Zones in an AWS Region

S3 one zone doenst meet resilience



###### 23. s3 one month then not accessed -> s3 standard to glacier deep

intelligence  tiering not designed for long term storage, also cost more

s3 standard to s3 standard IA/s3 one zone IA, not lowest cost



###### 24. vertical scaling of ec2, graph investigation of last 2 month -> AWS Cost Explorer

cost explorer -> 14 days hourly granularity, 12 months data

graph from aws billing and cost management 

aws budgets report no graph

aws cost and usage report -> s3 -> quicksight, too much overhead



###### 25. api GW + lambda + aurora, load data in db, need increase lambda quote, min config effort -> two lambda SQS

ec2 JDBC doesnt improve perfomance

dynamo accelerator for read not for write

two lambda  + SNS for sending notification

two lambda + SQS, integrate with SQS



###### 26. review s3 doesnt have unauthorized config change -> AWS config with appropriate rules

key word: config change -> aws config 

aws trusted adviser for recommendations

inspector for software vulnerabilities and network exposure



###### 27. cloudwatch dashboard Product Manager access, no AWS account -> dashboard w email access, share link

cloudwatch watchreadonly access would give log access and other access too



###### 28. SSO for all accounts; manage users groups on on premise microsoft active directory -> all AWS service needs to look up in on premise domains, then two way trust between on premise and AWS managed microsoft AD domains



###### 29. voice over internet protocol that use UDP. route user to lowerst latency, auto failover between regions, ec2 auto scale-> global accelerator

cloudfront edge location for cache content

global accelerator for optimal pathway to nearest regional endpoint, for both http and non http

NLB + global accelerator, NLB as endpoints for global accelerator

ALB for microservice, lambda targets, web app w http and https

NLB for TCP, UDP, ultra low latency, static ip, VPC endpoints



###### 30. RDS mysql 48h monthly test w performance insights -> snapshot, terminate db instance and restore snapshot when required

stop db still pays for storage, more than snapshot



###### 31. all aws resources config with tags -> aws config



###### 32. html, client-side js, css, image, cost effective -> s3 



###### 33. thousands of user, near real time solution, rm sensitive data before storing, low latency read -> firehouse / kinesis 

firehouse doesnt support dynamo, currrently Amazon S3, Amazon Redshift, Amazon OpenSearch Service, Splunk, Datadog, NewRelic, Dynatrace, Sumologic, LogicMonitor, MongoDB, and HTTP End Point

kinesis is near real time

dynamodb streams for logs, not for real time

s3 files not suitable for db



###### 34. config change of aws, history of api calls -> AWS config, cloudtrail

cloudwatch monitor and gather metrics

cloudtrail for record api calls



###### 35. third party for DNS, protect against ddos -> shield advanced

gaurdDuty for accounts to monitor sus activity

inspector for OS vulnerability

shield with route 53, not for ddos, also not third party

shield advanced for ddos



###### 36. s3 two regoin KMS, least overhead -> KMS server side encryption, replication

customer managed key



###### 37. ec2 instances, remote secure administer -> IAM role + AWS system manger session manger for SSH



###### 38. s3 route53, reduce latency -> cloudfront



###### 39. RDS 10 mil row, constant update, insert takes too long -> Provisioned IOPS SSD

Provisioned IOPS SSD: consistent and predictable I/O performance for the Amazon RDS MySQL

memory optimized instance: storage performance

burstable performance instance: cpu usage



###### 40. 1TB alert, 14 days available for immediate, archive older -> kinesis data firehouse -> s3 + lifecycle to s3 glacier



###### 41. saas to s3 -> appflow

appflow: saas to s3



###### 42. single VPC, ec2 in several subnets across multiple zones, upload and download from s3, avoid regional data transfer charges -> gateway VPC for s3

Gateway VPC: allows direct access to S3 without going through public internet

NAT GW doesnt solve data transfer between region

ec2 dedicated host, doesnt solve data transfer



###### 43. sensitive data to s3, bandwidth limitation, min impact on internet connect with internal user -> AWS Direct Connect

AWS Direct Connect: network connection from data center to AWS that doesnt go through public internet

VPN + VPC GW, still use bandwidth

snowball transfer daily not long term



###### 44. protect s3 from deletion -> versioning / cross-region replication / MFA

MFA (multi factor auth) when deletion



###### 45. SNS + lambda, network could fail lambda -> SQS



###### 46. SFTP 200GB, contain Personal II, need alert if happens -> Amazon Macie

Amazon Macie: data privacy/security service



###### 47. some specifc availability zone in specific region ec2 instance with capacity req, for a upcoming event -> On-Demand Capacity Reservation that specifies the Region and three Availability Zones needed

reserved instances doesnt provide capacity reservation



###### 48. store catalog highly available  and durable -> Amazon EFS (elastic file system)

EFS durable and available 

elastiCache, ec2 is not durable



###### 49. store file random instant access in a year, after 1 year delay acceptable -> intelligent tier then glacier flexible retrival, s3 query with athena, s3 glacier query with glacier select

query s3 meta data less effective

RDS for query is more complex than athena



###### 50. 1k EC2, third party software patch -> AWS Systems Manager run custome patch

AWS Systems Patch Manager for system patch not for third party



###### 51. shipping stats from api, html to email -> SES + 

SES (html format)

EventBridge (cloudwatch events) schedule events invokes lambda

glue for large dataset and complex transform

sns doesnt fulfill html format



###### 52. app output file, migrate to AWS, data store in file system structure, scales, highly available -> EFS

EFS scales and highly available

s3 is object store instead of file system

EBS doesnt scale



###### 53. accessible immediate for 1 year, retain for 9, cant delete during 10 y, resiliency -> standard to glacier deep, s3 object lock

s3 object lock: keep from deletion

s3 object lock in governance: allow user w special permission to do anything



###### 54. windows file share on 2 ecs, sync data betwen them, want highly available and durable -> s3 

EFS not supported for windows instances

s3 cannot be mounted directly on windows

s3 file GW doenst apply since company alr on aws

Amazon FSx for Windows File Server: fully managed Microsoft Windows file servers



###### 55. dedicated subnet for db, only allow private subnet to access -> security allows inbound from private, attach to db

route table that excluding public subnet doesn't fully configure the traffic flow 

security groups don't have deny rules 

peering is mostly between VPCs



###### 56. API GW, want url with domain name, third party can use https -> 

1. regional API GW endpoint (allow the company to create an endpoint that is specific to a region)

2. associate API GW endpoint w domain (allow use domain name for API GW url)

3. import public certificate with domain name into AWS certificate manager (ACM) in same region (allow company to use https)
4. attach certificate to API GW APIs (allow company to use certificate for API GW url)

5. config route 53 to route traffic to API GW endpoint (use route 53 to route traffic to API GW url with domain)



###### 57. uploaded image doenst have NSFW -> Amazon Rekognition + human review

Amazon Rekognition: image vid analysis, NSFW detection

Amazon Comprehend: txt 

Amazon SageMaker: need to build and train custom model

AWS Fargate: need effort and large dataset of image



###### 58. container for scale and availability, maintance of critical app instead managed underlying infra -> ECS w Fargate



###### 59. 30TB click stream data -> kinesis data firehouse + s3 data lake + redshift

data pipeline w EMR not suitable since its not for stream, plus is not recommended anymore



###### 60. ALB w two listener, want forward all req, all use https -> listener rule redirect http to https

ALB ACL for control inbound and outbound traffic at subnet level, not listener level

replace http w https, wouldnt redirect traffic to https listener

SNI for getting correct SSL w domain name



###### 61. ec2 RDS, credentials rotate -> secrete manger auto rotate

 AWS system maner param store, doesnt native support auto rotate, more overhead



###### 62. ALB, SSL by external CA, need renew every year -> ACM import SSL, apply to ALB, evenbridge to send notification when expiring soon, rotate certificate manually

only AWS certificate can use manged renewal



###### 63. document pdf (5MB) to jpg, needs original and converted, scalable, cost effective -> s3 put event + lambda

elastic beanstalk + ec2 + ebs could work, but expensive

Elastic Beanstalk + EC2 and EFS, expensive

s3 + lambda cost effective



###### 64. windows file server, migrate to AWS, daily, min latency, vpn connection -> FXs + FSx file GW (low latency)

user and app interact with data each day, min latency



###### 65. API GW + lambda, upload pdf and jpg for PHI protected health info -> amazon textract + amazon comprehend medical



###### 66. generate large # of 5MB file - s3, store 4 years before delete, immediate access always, freq access in 30 days, rarely after -> s3 standard + standard infrequent access 30 days after, then delete



###### 67. ec2 SQS - RDS, occasional dupliate, SQS doesnt have duplicate, want process once -> changeMessageVisibility API, increase visibility timeout

multi consumer, if visibility timeout isnt set then other consumer would pick up job

RcvMessage API is for waiting for new msg coming in queue



###### 68. on premise to aws, highly available, low latency to aws region, min cost, accept slower traffic if primary connection fails -> direct connect + vpn connection if primary fails

two direct connect will be expensive



###### 69. ALB + ec2 auto scale + aurora postgres single availability zone, want high availability min downtime and min loss of data -> auto scale to use multi AZ, db multi AZ + RDS proxy (auto route traffic to healthy db instance)

different region doesnt help



###### 70. http NLB + ec2 auto scale, NLB not detecting http error, manual restart of ec2, improve availability without writing custome code -> replace NLB w ALB, enable http health check by supplying url of company app, config auto scale action to replace unhealthy instances

supplying url, load balancer can assess the returned response



###### 71. dynamo, data corruption - recovery point objective (RPO) 15 min and recovery time objective (RTO) 1 hour -> dynamo point-in-time recovery

cant take snapshot of dynamo since serverless



###### 72. frequent download upload file to s3, same aws region, increase data transfer cost -> s3 VPC GW



###### 73. public subnet app ec2, private bastion ec2, on premise - company internet - bastion host - app servers, security group of ec2 allow access -> 

security group of bastion host that only allows inbound access from external IP range for company

security group of app instance that allows inbound SSH access from private IP address of bastion host



###### 74. public web ec2, private db ms mysql ec2-> web allow inbound on 443 from 0.0.0.0/0 (allow inbound https from any IPv4), db allow inbound on port 1433 from web tier (allow inbound on port 1433 ms sql default)



###### 75. app tier restful between, txn are dropping when one tier overloaded -> API GW + lambda + SQS

"scale up when communication failures are detected", scaling shouldnt be based on failures!



###### 76. 10TB json data on storage area network on premise data, migrate to s3 near real time, secure transfer -> AWS DataSync + direct connect

AWS DataSync: data transfer

AWS DMS (data migration service) for db



###### 77. real time data ingestion, API, transform data stream, storage -> kinesis + lambda + s3



###### 78. user txn dynamo retain 7 years, op efficient -> AWS backup 

AWS backup: backup schedule and retention policy



###### 79. morning no traffic, evening unpredictable traffic night, dynamo,  -> dynamo on demand capacity mode

On-demand mode: unknown workloads, unpredictable traffic, pay as u go

provisioned capacity mode is for known traffic



###### 80. AMI (amazon machine image) backed by EBS encrypted w KMS, share AMI w Amazon MSP partner -> change launchPermission of AMI, key policy to allow MSP partner to use

change key policy to trust a new key could jeopardize security of data

instead allow MSP to use the existing key



###### 81. loosely coupled, durable, processor application stateless, process run in parallel, auto scale -> SQS, AMI, auto scale based on queue size



###### 82. ELB with certificate in ACM, notify expiraion in 30 days -> Eventbridge + SNS / AWS config + SNS

eventbridge w lambda +  SNS, lambda is not necessary here

AWS config has certificate check



###### 83. on premise server in US, backend must retain in US, optimize site loading for europe -> cloudfront point to on premise servers

cloudfront CDN

s3 wouldnt provide server side processsing capacities

route53 geoproximity routing policy direct traffic to on premise: still doesnt leverage edge locations and caching



###### 84. ec2 30% peak hours, 10% non-peak, prod ec2 24h / day, dev test ec2 run for 8h /day, auto stop dev and test ec2 when not in use ->  reserved instance for prod, on demand for dev and test

spot: tmp instance

spot block: guaranteed duration (1-6 hours) fixed price (no longer available )

reserved: purchased for one year or longer, lower price

on demand: hourly billed



###### 85. doc upload cannot be modified or delete after store -> s3 object lock

only s3 object lock prevent modification and deletion

EFS read only and s3 ACL read only both doesnt prevent modify and deletion



###### 86. several server frequent RDS Multi AZ DB, secure method connect between web server and db, rotate user credentials -> secrets manger

secrets manager SSM: store and auto rotate credentials

KMS encryption good, but storing on web server bad practice



###### 87. upgrade aurora db, lamdba fn sometimes fail to establish connection, loss data -> SQS



###### 88. data s3, share data w europe mkt firm that has s3, data tranfer low cost -> s3 cross region replication 

requester pays feat: the data rcv pays for data transfer instead of initiator



###### 89. s3 for confidential doc, bucket policy restrict audit team IAM credential, stop accidental deletion -> versioning + MFA delete feat



###### 90. SQL RDS single AZ DB, random query daily to record # of new movies, report final total during biz hours, inadequate performance for dev task when script is running -> read replica

elasti cache for read common data



###### 91. ec2 VPC - s3 to store and read, security, no traffic across internet -> s3 GW endpoint



###### 92. secure access ec2 in VPC to s3 w sensitive info -> s3 GW + bucket policy VPC only 

nat instance obsolete now



###### 93. MySQL - AWS, heavy read, pull data 4 hours for staging env, user exp delay, then use staging db once done -> aurora + replica for prod + db cloning for create staging db on demand

Aurora cloning: quickly setting up test env using your prod data

mysqldump utility full export, cause app delay

read replica + standby for staging: doenst allow continue using staging db without delay, standby for failover in case of prod instance failure, not intented to use as staging



###### 94. upload s3, data transform - json, file process as fast as possible after uploading, demand vary for file # volume -> SQS + lambda + dynamo (allow json)

EMR: big data query

SQS + ec2: (ec2 more overhead than lambda)

eventbridge + kinesis: real time large vol of data



###### 95. RDS Mysql - seperate read from write -> read replica, same compute and storage resources



###### 96. EC2 policy, basically read



###### 97. windows share file, AWS, highly available, Active directory for access control -> Amazon FSx, active directory



###### 98. s3 upload, SQS, lambda, invoking lambda more than once - duplicate email -> visibility time out



###### 99. Lustre clients to access data, shared storage for on premises data center -> amazon FSx for lustre fs, attach to origin server, connect app server to fs

Lustre only available as FSx



###### 100. download certificate ec2, encrypt and decrypt, near real time, highly available storage after encryption -> AWS KMS + S3

EBS not highly available 



###### 101. three AZ, each public, private, all private needs access to internet GW, 

NAT GW (NAT instance obsolete), allow private to get internet access but dont allow inbound from internet



###### 102. data transfer -> dataSync, install in on premise data center, create suitable location config



###### 103. glue ETL, xml prevent from processing old data -> job bookmark



###### 104. ddos, windows on ec2 -> shield advanced + cloudfront



###### 105. aws lambda permission -> resource based, lambda : invokeFunction + service : events.amazonaws.com as principal

event based : lambda needs other aws resourcce

resource based: allow aws service to perform action on that resource



###### 106. encryption key rotation -> KMS + auto rotate



###### 107. store and retrieve data pt -> api GW + kinesis data analytics



###### 108. RDS, needs data deletion send to multi target system (fan out) -> SQS (FIFO not required)



###### 109. unchangable + only specific user can delete -> object lock + legal hold (like retention (not able to delete or modify) until removed)



###### 110. resize image + store : slow performance, need reduce coupling -> upload s3 + s3 event w kinesis data analytics (able to store)

presigned url for tmp storage



###### 111. msg - activeMQ on ec2 - ec2  - mysql on ec2, highly available -> amazon MQ + eds w multi AZ



###### 112. on premise server cannot handle -> aws fargate the least op overhead



###### 113. 50TB data to aws, no avail network bandwidth + transform job weekly -> snowball + glue

snowcone 14TB



###### 114. upload photo, meta for adding frame, ec2 + dynamo, need scale -> aws lambda + s3 + dynamo



###### 115. data s3, ec2 public subnet access s3 thro public internet - need private route -> move ec2 to private + vpc endpoint

NAT for outbound internet connectivity for private subnet

security group for outbound traffic 



###### 116. update 4 times a year, no dynamic content, highly avail -> cloudfront + s3 static website



###### 117. app logs into cloudwatch now change into opensearch instead in near-real time ->  cloudwatch logs subscription into opensearch (near real time)



###### 118. ec2 in multi AZ - access to 900TB txt doc, scale -> s3

cost s3 < EFS 



###### 119. API GW for multi AZ prevent SQL injection & cross site scripting -> AWS firewall manager (simplifies admin of AWS WAF)



###### 120. ec2 behind NLB in US west, add ec2 in eu and NLB, route -> global accelerator

can only use ALB for route 53



###### 121. RDS need encryption -> add encryption by encrypting snapshot, then restore db from the snapshot



###### 122. key management -> KMS



###### 123. SSL on two ec2, encry decry bottleneck -> SSL into ACM, ALB to use SSL from ACM



###### 124. 60 min job -> ec2 spot instance

lambda has 15min time cap



###### 125. 2 tier website, LB send traffic to EC2, RDS, both shouldnt expose to public internet, ec2 need internet access for third party payment -> two public and private, two nats across 2 AZ, ALB in public subnet (routing traffic in public), RDS multi AZ, ec2 auto scale



###### 126. s3 standard, keep 25 years, 2 year available -> s3 lifecycle policy



###### 127. 20TB storage w max IO, 300TB for durable, 900TB for archievev -> ec2 instance store, s3, s3 glacier



###### 128. stateless, tolerate disruption, run container on cloud -> EKS + spot instance



###### 129. linux postgre -> aurora + fargate (both serverless)



###### 130. ec2 across multi AZ + ALB, cpu near 40% best performance -> target tracking policy to scale



###### 131. s3 + cloudfront, dont want direct access s3 url -> OAI to cloudfront, config s3 to grant only OAI read permission



###### 132. download, scale, global -> cloudfront + s3



###### 133. disaster recovery for DB, upgrade DB, need access to DB OS -> oracle - RDS custom (has access to OS) + read replica (for DR)



###### 134. s3 exisiting and new data, encryption and replication -> S3 Cross region replication, SSE - KMS / SSE - S3



###### 135. aws connect to VPC private, restricted to target service -> VPC endpoint for service, AWS privatelink



###### 137. miss email from root user distribution list w a few admin, set up aws account alternative contact



###### 138. rabbitMQ, on ec2, consumer on another ec2, postgre another ec2, all in single AZ -high avail -> amazon MQ, muti AZ ec2, muti AZ RDS



###### 140. ec2, can interupt; front end run on fargate, lambda for api layer, api layer predictable for 1 year -> spot instance for data ingestion layer, 1 year compute saving plan for front end and api layer

ec2 saving for ec2 only, 72%

compute saving for fargate ec2, 66%



###### 141. cloudfront for low latency



###### 159. block unauthorized request -> api key shared with user only + WAF rule 

IAM doesnt have authorization



###### 179. system manager param store, secure param for db password -> allow param store read access, decrypt access for KMS, and attach to db ec2































































