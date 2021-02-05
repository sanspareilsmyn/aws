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





   
