## VPC Section Summary
### 1. VPC Section Summary
- CIDR – IP Range
→ `CIDR`는 `IP`의 범위이다.

- VPC – Virtual Private Cloud => we define a list of IPv4 & IPv6 CIDR  
→ `VPC`는 가상 사설 클라우드로 `IPv4`와 `IPv6`용으로 작동한다.

- Subnets – tied to an AZ, we define a CIDR  
→ 서브넷은 `CIDR`가 정의된 `AZ`에 연결되며 공용 및 프라이빗 서브넷이 있다.

- Internet Gateway – at the VPC level, provide IPv4 & IPv6 Internet Access  
→ 공용 서브넷을 구축할 때 인터넷 게이트웨이를 연결해서 공용 서브넷에서 인터넷 게이트웨이로 경로를 만들어야 한다. 활성화되면 `IPv4` 및 `IPv6`에 인터넷 액세스를 제공한다.

- Route Tables – must be edited to add routes from subnets to the IGW, VPC Peering Connections, VPC Endpoints, …  
→ 라우팅 테이블은 대체 게이트웨이나 `VPC` 피어링 연결, `VPC` 엔드 포인트로 향하는 라우트를 갖도록 편집하고 네트워크가 `VPC` 내부에서 작동하도록 돕는다.

- Bastion Host – public EC2 instance to SSH into, that has SSH connectivity to EC2 instances in private subnets  
→ `Bastion Host`는 공용 `EC2` 인스턴스로 `SSH`를 수행하며 프라이빗 서브넷의 다른 `EC2`로 `SSH` 연결을 할 수 있다.

- NAT Instances – gives Internet access to EC2 instances in private subnets. Old, must be setup in a public subnet, disable Source / Destination check flag  
→ `NAT` 인스턴스는 `EC2` 인스턴스이며 사설 및 공용 서브넷에 배포되어 프라이빗 서브넷의 `EC2` 인스턴스에 인터넷 액세스를 제공한다. 더는 사용하지 않는다.

- NAT Gateway – managed by AWS, provides scalable Internet access to private EC2 instances, IPv4 only  
→ 지금은 `NAT Instances` 대신 `NAT Gateway`를 사용한다. 성능이 훨씬 좋고 `AWS`에서 관리한다. 사설 `EC2` 인스턴스에 확장 가능한 인터넷 액세스를 제공한다. `IPv4`에서만 작동한다.

- Private DNS + Route 53 – enable DNS Resolution + DNS Hostnames (VPC)  
→ `Route 53`를 통해 사설 `DNS`를 쓰려면 `DNS` 변환과 `DNS` 호스트 이름 설정을 `VPC`에서 활성화해야 한다.

- NACL – stateless, subnet rules for inbound and outbound, don’t forget Ephemeral Ports  
→ `NACL`은 방화벽 규칙이다. 인바운드와 아웃바운드 액세스를 서브넷 레벨에서 정의한다. 임시 포트 `NACL`은 무상태이므로 인바운드와 아웃바운드 규칙이 항상 평가된다.

- Security Groups – stateful, operate at the EC2 instance level  
→ 보안 그룹 또한 방화벽 규칙이다. 보안 그룹 규칙은 EC2 인스턴스 레벨에서 적용된다. 보안 그룹 규칙은 상태가 유지되므로 인바운드 허용 시 아웃바운드도 바로 허용되고 반대의 경우에도 바로 허용된다.

- Reachability Analyzer – perform network connectivity testing between AWS resources  
→ `VPC Reachability Analyzer`를 사용하면 여러 `AWS` 리소스 사이에서 네트워크 연결을 시험하고 디버깅을 수행할 수 있다.

- VPC Peering – connect two VPCs with non overlapping CIDR, non-transitive  
→ `VPC` 피어링은 두 `VPC`를 연결할 때 유용한데 `CIDR`가 겹치지 않는 경우에만 가능하다. `VPC` 피어링 연결은 비전이적이므로 `VPC` 세 개를 연결하려면 `VPC` 피어링 연결도 세 개가 필요하다.

- VPC Endpoints – provide private access to AWS Services (S3, DynamoDB, CloudFormation, SSM) within a VPC  
→ `VPC` 엔드 포인트는 `AWS` 서비스에 사설 액세스를 제공한다. `Amazon S3`, `DynamoDB CloudFormation`, `SSM` 등 `VPC` 내 모든 서비스와 통합 가능하다. `Amazon S3`와 `DynamoDB`는 게이트웨이 엔드 포인트를 사용하고 나머지는 모두 인터페이스 엔드 포인트를 사용한다.

- VPC Flow Logs – can be setup at the VPC / Subnet / ENI Level, for ACCEPT and REJECT traffic, helps identifying attacks, analyze using Athena or CloudWatch Logs Insights  
→ `VPC` 내 모든 패킷과 관련해 일정 레벨의 메타데이터를 얻을 수 있다. 여기에는 `ACCEPT` 및 `REJECT` 트래픽 정보가 포함된다. `VPC` 플로우 로그는 `VPC` 서브넷이나 `ENI` 레벨에서 생성할 수 있으며 `Amazon S3`로 전송할 수 있다. 이 정보를 `Athena`나 이나 `Cloudwatch Logs Insights`로 분석할 수 있다.

- Site-to-Site VPN – setup a Customer Gateway on DC, a Virtual Private Gateway on VPC, and site-to-site VPN over public Internet  
→ `VPC`를 데이터 센터에 연결하려면 두 가지 옵션이 있다. 먼저 `Site-to-Site VPN`은 공용 인터넷을 지나는 `VPN` 연결로 `AWS`에는 가상 프라이빗 게이트웨이를, 데이터 센터에는 고객 게이트웨이를 생성하고 나서 `VPN` 연결을 구축한다.

- AWS VPN CloudHub – hub-and-spoke VPN model to connect your sites  
→ 가상 프라이빗 게이트웨이를 사용해 `VPC` 연결을 여러 개 생성하기 위해서는 `VPN CloudHub`를 활용해야 한다. 허브와 스포크 간 `VPN` 모델로 사이트에 연결한다.

- Direct Connect – setup a Virtual Private Gateway on VPC, and establish a direct private connection to an AWS Direct Connect Location  
→ 데이터 센터에 연결하는 또 다른 방법은 `Direct Connect`를 사용해 완전히 비공개로 연결하는 것이다. 공용 인터넷을 통과하지 않으며 구축에 시간이 걸린다. 데이터 센터를 `Direct Connect` 로케이션과 연결해 줘야 작동한다. `AWS` 사설망을 사용하므로 더 안전하고 아니라 연결도 안정적입니다.

- Direct Connect Gateway – setup a Direct Connect to many VPCs in different AWS regions  
→ `Direct Connect Gateway`는 다양한 `AWS` 리전의 수많은 `VPC`에 `Direct Connect`를 설정할 때 사용한다.

- AWS PrivateLink / VPC Endpoint Services:
~~~
- Connect services privately from your service VPC to customers VPC
→ PrivateLink 또는 VPC 엔드 포인트를 사용하면 고객 VPC에 만든 자체 VPC의 내부 서비스에 비공개로 연결할 수 있다. 

- Doesn’t need VPC Peering, public Internet, NAT Gateway, Route Tables
→ 장점을 꼽자면 `VPC` 피어링, 공용 인터넷 `NAT Gateway`, 라우팅 테이블의 설정이 필요없다는 것이다.

- Must be used with Network Load Balancer & ENI
→ 네트워크 로드 밸런서, ENI와 함께 사용되어야 한다.
~~~

- Transit Gateway – transitive peering connections for VPC, VPN & DX  
→ `Transit Gateway`는 `VPC`와 `VPN Direct Connect`를 위한 전이적 피어링 연결이다. 트래픽은 모두 `Transit Gateway`를 거쳐 연결된다.

- Traffic Mirroring – copy network traffic from ENIs for further analysis  
→ 트래픽 미러링이란 추가 분석을 위해 `ENI` 등에서 네트워크 트래픽을 복사하는 것이다.

- Egress-only Internet Gateway – like a NAT Gateway, but for IPv6  
→ `Egress-only Internet Gateway`는 `NAT Gateway`와 비슷하지만 `IPv6` 트래픽 전용이다.

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---