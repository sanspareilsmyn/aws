# AWS Solution Architect Associate 중요 개념 정리  
1) Global Accelerator vs CloudFront  
- Global Accelerator는 UDP 같은 Non-http에서. CloudFront는 http에서. 둘 다 Latency 빠르게 만드는 효과.
2) EC2 Dedicated Host vs Dedicated Instance
- Dedicated Host는 기존 서버 그대로 라이센스 들고 올 수 있음. Dedicated Instance는 한 HW 내에 같은 AWS 계정이지만 Dedicated Instance 아닌 다른 Instance도 가능.  
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



