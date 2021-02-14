# AWS Solution Architect Associate 중요 개념 정리  
1) Global Accelerator vs CloudFront  
- Global Accelerator는 UDP 같은 Non-http에서. CloudFront는 http에서. 둘 다 Latency 빠르게 만드는 효과.  

2) EC2 Dedicated Host vs Dedicated Instance  
- Dedicated Host는 기존 서버 그대로 라이센스 들고 올 수 있음. Dedicated Instance는 한 HW 내에 같은 AWS 계정이지만 Dedicated Instance 아닌 다른 Instance도 가능.  
- Dedicated Instances that belong to different AWS accounts are physically isolated at a hardware level, even if those accounts are linked to a single-payer account. However, Dedicated Instances may share hardware with other instances from the same AWS account that are not Dedicated Instances.  
- A Dedicated Host is also a physical server that's dedicated for your use. With a Dedicated Host, you have visibility and control over how instances are placed on the server.  

3) Cognito User Pool + Application Load Balancer  
- Cognito를 이용한 3rd Party Authentication / ALB를 통해 securely authenticate users for accessing app.  

4) Cognito User Pool vs Cognito Identity Pool  
- User Pool : User Directory in AWS. 로그인을 일단 하면 User Pool에 directory profile 생김.   
- Identity Pool : Temporary AWS credentials to AWS services, such as Amazon S3 and DynamoDB.  
- Profile Info를 저장하려면 User Pool을 써야 됨!  
- Cognito User Pool과 CloudFront 다이렉트로 연결 못 함. Lambda@Edge를 따로 만들어줘야만 가능.  

5) EC2 Auto Scaling Group lifecycle hook  
- EC2가 Generate / Terminate 될 때 특정 action을 취하도록 설정 가능.  
- When an instance is paused, it remains in a wait state either until you complete the lifecycle action using the complete-lifecycle-action command or the CompleteLifecycleAction operation, or until the timeout period ends (one hour by default). For example, you could install or configure software on newly launched instances, or download log files from an instance before it terminates.  

6) EC2 Instance Meta Data vs User Data  
- Instance metadata is data about your instance that you can use to configure or manage the running instance. Instance metadata is divided into categories, for example, host name, events, and security groups.  
- You can also use instance metadata to access user data that you specified when launching your instance.  
- 결정적인 차이는 Launching Instance = User Data vs Running Instance = Meta Data.  
- User Data에 입력된 script는 root user previleges로 실행되는 게 default이며, 오직 boot cycle에만 실행.  

7) NAT Instance & NAT Gateway  
- 둘 다 목적은 VPC 내 Public Subnet에 존재하면서 Private Subnet의 Instance가 outbound IPv4 traffic과 연결되도록 하기 위함.  
- Security Group과 Associate할 수 있는 건 NAT Instance.  
- Port Forwading과 Bastion servers 지원하는 건 NAT Instance.  
- https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-comparison.html  

8) AWS Direct Connect  
- On-premise와 AWS로의 전용 네트워크.
- https://aws.amazon.com/ko/directconnect/  

9) Site-to-site VPN  
- VPN connection: A secure connection between your on-premises equipment and your VPCs.  
- You can enable access to your remote network from your VPC by creating an AWS Site-to-Site VPN (Site-to-Site VPN) connection, and configuring routing to pass traffic through the connection.  
- Low to modest BW.  
- A.K.A IPSec VPN.  
- cf) Direct Connect + VPN (https://docs.aws.amazon.com/whitepapers/latest/aws-vpc-connectivity-options/aws-direct-connect-vpn.html)  

10) AWS Database Migration Service & Schema Conversion Tool  
- Database Migration Service : Shutdown 없이 Migration 가능하고 homegeneous, heterogeneous DB 다 가능.  
- Schema Conversion Tool : Heterogeneous DB 간 source schema 맞추기.  
- cf) Basic Schema Copy : AWS Database Migration 내 기능. Table과 Primary Key만 복사해서 Test Migration 용도.  

11) VPC Endpoints  
- Interface Endpoints : Elastic Network Interface with a private IP address from the IP address range of your subnet that serves as an entry point for traffic destined to a supported service.  
- Gateway Endpoints : A gateway that you specify as a target for a route in your route table for traffic destined to a supported AWS service. The following AWS services are supported: Amazon S3 and DynamoDB.  

12) 4 configurations VPC supports
- https://docs.aws.amazon.com/vpc/latest/userguide/VPC_wizard.html
- VPC with a single public subnet : The configuration for this scenario includes a virtual private cloud (VPC) with a single public subnet, and an internet gateway to enable communication over the internet. We recommend this configuration if you need to run a single-tier, public-facing web application, such as a blog or a simple website.  
- VPC with public and private subnets (NAT) : The configuration for this scenario includes a virtual private cloud (VPC) with a public subnet and a private subnet. We recommend this scenario if you want to run a public-facing web application while maintaining back-end servers that aren't publicly accessible. A common example is a multi-tier website, with the web servers in a public subnet and the database servers in a private subnet. You can set up security and routing so that the web servers can communicate with the database servers.  
- VPC with public and private subnets and AWS Site-to-site VPN access : The configuration for this scenario includes a virtual private cloud (VPC) with a public subnet and a private subnet, and a virtual private gateway to enable communication with your network over an IPsec VPN tunnel. We recommend this scenario if you want to extend your network into the cloud and also directly access the Internet from your VPC. This scenario enables you to run a multi-tiered application with a scalable web front end in a public subnet and to house your data in a private subnet that is connected to your network by an IPsec AWS Site-to-Site VPN connection.  
- VPC with a private subnet only and AWS Site-to-site VPN access : The configuration for this scenario includes a virtual private cloud (VPC) with a single private subnet, and a virtual private gateway to enable communication with your network over an IPsec VPN tunnel. There is no Internet gateway to enable communication over the Internet. We recommend this scenario if you want to extend your network into the cloud using Amazon's infrastructure without exposing your network to the Internet.  

13) EC2 Launch configurations vs Launch Template  
- 둘은 매우 흡사. (ID of the Amazon Machine Image (AMI), the instance type, a key pair, one or more security groups, and a block device mapping)
- Launch Template만 가능 : you can provision capacity across multiple instance types using both On-Demand Instances and Spot Instances to achieve the desired scale, performance, and cost. Hence this is the correct option.  

14) VPC Peering vs VPC Sharing  
- VPC Sharing : multiple AWS accounts가 EC2나 RDS 같은 app을 shared and centrally-managed VPC에 만드는 것을 허용. 이를 위해 VPC owner는 one or more subnets를 same organization from AWS Organizations과 공유해야 함. 주의할 것은 VPC 자체를 공유하는 게 아니라 일부 subnet만 공유한다는 사실.  
- VPC Peering : A networking connection between two VPCs that enables you to route traffic between them using private IPv4 addresses or IPv6 addresses. Centrally managed VPC랑은 별 상관 없음.  

15) Amazon ElastiCache  
- IAM Auth는 ElastiCache랑 사용 못 함.  
- Memcached : 모델이 단순한 경우. 코어가 여러 개거나 스레드가 있는 경우. Scale out and in 필요한 경우.  
- Redis : Authentication 하는 경우. Replication, High Availability, and Cluster Sharding.  
16) AWS Transit Gateway  
- The AWS Transit Gateway allows customers to connect their Amazon VPCs and their on-premises networks to a single gateway. With AWS Transit Gateway, you only have to create and manage a single connection from the central gateway into each Amazon VPC, on-premises data center, or remote office across your network.  

17) AWS Redshift  
- Fully-managed petabyte-scale cloud-based data warehouse product designed for large scale data set storage and analysis.  
- Redshift Spectrum을 사용하면 Redshift table로 불러올 필요 없이 S3로부터 효과적으로 데이터 쿼리 가능.  
18) Tasks that require root user credentials  
- Change your account settings.  
- Restore IAM user permissions.  
- Activate IAM access to the Billing and Cost Management console.  
- Close your AWS account.  
- Configure an Amazon S3 bucket to enable MFA.  
- https://docs.aws.amazon.com/general/latest/gr/root-vs-iam.html#aws_tasks-that-require-root  

19) Amazon S3 Transfer Acceleration  
- 먼 거리의 클라이언트와 S3 Bucket 간 속도 증대 by CloudFront Edge  

20) Security Group  
- Security Group은 Stateful이다. Outbound traffic 모두 허락하며, 항상 permissive하고 Deny rule 못 만든다.  
- Default는 Allows all inbound traffic from other instances associated with the default security group. Allows all outbound traffic from the instance.  

21) AWS Storage Gateway  
- Hybrid cloud storage service that gives you on-premises access to virtually unlimited cloud storage.  
- Tape Gateway : Tape Gateway enables you to replace using physical tapes on-premises with virtual tapes in AWS without changing existing backup workflows.  
- Volume Gateway  : Volume Gateway to present cloud-based iSCSI block storage volumes to your on-premises applications. The Volume Gateway provides either a local cache or full volumes on-premises while also storing full copies of your volumes in the AWS cloud.  
- File Gateway : File Gateway offers SMB or NFS-based access to data in Amazon S3 with local caching. It can be used for on-premises applications, and for Amazon EC2-based applications that need file protocol access to S3 object storage.  

22) S3 One Zone-IA (Infrequent Access)  
- 20% cheaper than Standard IA(최소 3개의 Multiple AZ). 30일 후부터 S3 Standard에 있는 걸 One Zone-IA로 바꿀 수 있음.  
-  https://docs.aws.amazon.com/AmazonS3/latest/dev/lifecycle-transition-general-considerations.html  

23) Types of Access Control in S3  
- IAM Policies : AWS Account Level Control 안 됨. User Level Control 됨.
- ACLs (Access Control List) : AWS Account Level Control 됨. User Level Control 안 됨.
- S3 Bucket Policies : 둘 다 됨.  

24) Elastic Container Service (ECS)  
- Fully managed container orchestration service. ECS allows you to easily run, scale, and secure Docker container applications on AWS.  
- Launce Type은 EC2 or Fargate. EC2로 하면 Instances와 EBS에 따라 돈 내고, Fargate로 하면 vCPU와 Memory 사용량에 따라 돈 냄.  

25) AWS SQS - FIFO  
- 3,000 messages per second with batching 10 messages per operation(maximum), or 300 messages per second without batching.  
- Suffix should be .fifo.  
- Can't convert standard SQS into FIFO.  

26) AWS Auto Scaling Group 
- Rebalancing : 새로 만든 다음에 옛날 것 제거. (When rebalancing, Amazon EC2 Auto Scaling launches new instances before terminating the old ones, so that rebalancing does not compromise the performance or availability of your application.)  
- Fault Tolerance : 문제 있는 것 지운 다음에 새로 만든다. (Amazon EC2 Auto Scaling creates a new scaling activity for terminating the unhealthy instance and then terminates it. Later, another scaling activity launches a new instance to replace the terminated instance.)  

27) Private Hosted Zone  
- A container for records for a domain that you host in one or more Amazon virtual private clouds (VPCs). You create a hosted zone for a domain (such as example.com), and then you create records to tell Amazon Route 53 how you want traffic to be routed for that domain within and among your VPCs.  
- 이걸 쓰려면 두 개 풀어야 된다. enableDnsHostnames & enableDnsSupport  

28) AMI  
- A region에서 B region으로 AMI가 복사될 때는 B region에 snapshot이 자동으로 생성된다.  

29) AWS KMS  
- Deleting a customer master key (CMK) in AWS Key Management Service (AWS KMS) is destructive and potentially dangerous. Therefore, AWS KMS enforces a waiting period. To delete a CMK in AWS KMS you schedule key deletion. You can set the waiting period from a minimum of 7 days up to a maximum of 30 days. The default waiting period is 30 days. During the waiting period, the CMK status and key state is Pending deletion.  
- Server-side Encrpytion과 Client-side Encryption.  
- Server-side Encryption인 SSE-S3 / SSE-KMS / SSE-C  
- SSE-S3는 Unique Key로 Encrypt된다. AES-256 쓴다.  
- SSE-KMS는 CMK(Customer Master Keys)를 저장한다. Audit trail that shows when your CMK was used and by whom 제공한다.  
- SSE-C는 Customer-provided Keys다. 사용자가 Encrpytion Key 관리하고 S3는 Encryption만 한다.  

30) Elastic Fabric Adapter  
- A network interface for Amazon EC2 instances that enables customers to run applications requiring high levels of inter-node communications at scale on AWS.  

31) S3 Transfer Acceleration  
- There are no S3 data transfer charges when data is transferred in from the internet. Also with S3TA, you pay only for transfers that are accelerated.  

32) S3 Intelligent Tiering  
- The S3 Intelligent-Tiering storage class is designed to optimize costs by automatically moving data to the most cost-effective access tier, without performance impact or operational overhead. It works by storing objects in two access tiers: one tier that is optimized for frequent access and another lower-cost tier that is optimized for infrequent access.  
- Minimun storage duration : 30 days  

33) AWS Snowball Edge  
- It provides up to 80 TB of usable HDD storage, 40 vCPUs, 1 TB of SATA SSD storage, and up to 40 Gb network connectivity to address large scale data transfer and pre-processing use cases.  
- cf. SnowMobile : 10 PB  

34) Storage volume that can't be used as boot volumes  
- HDD는 boot volume 안 됨. SSD는 됨 (gp, io).
- Throughput Optimized HDD (st1)  
- Cold HDD (sc1)  
- https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html

35) Versioning on S3  
- Once you version-enable a bucket, it can never return to an unversioned state. You can, however, suspend versioning on that bucket.  

36) Amazon FSx for Windows Server & AWS Managed Microsoft AD  
- Amazon FSx for Windows File Server : Accessible over the industry-standard Service Message Block (SMB) protocol. It is built on Windows Server. Microsoft Active Directory (AD) integration. Microsoft’s Distributed File System (DFS).  
- AWS Managed Microsoft AD : enables your directory-aware workloads and AWS resources to use managed Active Directory in the AWS Cloud.  

37) Amazon FSx for Windows File Server & AWS Managed Microsoft AD
- Amazon FSx for Windows File Server : Accessible over the industry-standard Service Message Block (SMB) protocol. It is built on Windows Server. Microsoft Active Directory (AD) integration. Microsoft’s Distributed File System (DFS).
- AWS Managed Microsoft AD : Enables your directory-aware workloads and AWS resources to use managed Active Directory in the AWS Cloud. AWS Managed Microsoft AD is built on the actual Microsoft Active Directory and does not require you to synchronize or replicate data from your existing Active Directory to the cloud.  

38) Instance Store based EC2 Instance  
- Instance store provides temporary block-level storage for your instance. This storage is located on disks that are physically attached to the host computer.  
- Instance store is ideal for the temporary storage of information that changes frequently such as buffers, caches, scratch data, and other temporary content, or for data that is replicated across a fleet of instances, such as a load-balanced pool of web servers.  
- You can specify instance store volumes for an instance only when you launch it. You can't detach an instance store volume from one instance and attach it to a different instance.  
- https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html  

39) Programming Language supported by Lambda runtime  
- C#/.NET, Go, Java, Node.js, Python, Ruby  

40) Placement Groups  
- Cluster / Partition / Spread  
- You can have a maximum of 7 running instances per AZ per group.  

41) AWS Lambda  
- 1000 concurrent executions per AWS account per region. If your Amazon SNS message deliveries to AWS Lambda contribute to crossing these concurrency quotas, your Amazon SNS message deliveries will be throttled. You need to contact AWS support to raise the account limit.  

42) Prefix of S3  
- There are no limits to the number of prefixes in a bucket. You can increase your read or write performance by parallelizing reads.  
- Your application can achieve at least 3,500 PUT/COPY/POST/DELETE or 5,500 GET/HEAD requests per second per prefix in a bucket.  

43) API Gateway  
- API Gateway creates RESTful APIs : Are HTTP-based. / Enable stateless client-server communication. / Implement standard HTTP methods such as GET, POST, PUT, PATCH, and DELETE.
- API Gateway creates WebSocket APIs : Adhere to the WebSocket protocol, which enables stateful, full-duplex communication between client and server. Route incoming messages based on message content.  

44) WAF
- AWS WAF gives you control over how traffic reaches your applications by enabling you to create security rules that block common attack patterns, such as SQL injection or cross-site scripting, and rules that filter out specific traffic patterns you define.  
- Can set IP Match Conditions.  

45) Private IP range
- 192.168.0.0 - 192.168.255.255 (65,536 IP addresses) 
- 172.16.0.0 - 172.31.255.255 (1,048,576 IP addresses) 
- 10.0.0.0 - 10.255.255.255 (16,777,216 IP addresses) 

46) Default Security Group Rule
- Allow inbound traffic from network interfaces (and their associated instances) that are assigned to the same security group. 
- Allows all outbound traffic.  

47) Spot Block  
- Spot Instances with a defined duration

48) Bastion Host  
- Including bastion hosts in your VPC environment enables you to securely connect to your Linux instances without exposing your environment to the Internet. 
- Bastion Host는 Port 22로 SSH 쓴다. TCP 쓰는 Network Load Balancer가 알맞음.

49) Secrets Manager 
- AWS Secrets Manager helps you protect secrets needed to access your applications, services, and IT resources. The service enables you to easily rotate, manage, and retrieve database credentials, API keys, and other secrets throughout their lifecycle. 
- Users and applications retrieve secrets with a call to Secrets Manager APIs, eliminating the need to hardcode sensitive information in plain text. Secrets Manager offers secret rotation with built-in integration for Amazon RDS, Amazon Redshift, and Amazon DocumentDB.  
- 주의. SSM Parameter Store도 secret store 역할 가능하지만 rotating key manually.  

50) IAM 관련
- Service Control Policy (SCP) : AWS Organizations에 대한 규칙.  
- IAM Permission Boundary : IAM roles or users에만 적용 가능. IAM Groups에는 안 됨.  


