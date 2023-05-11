## NAT
### 1. NAT Instance (outdated, but still at the exam)
- NAT = Network Address Translation
- Allows EC2 instances in private subnets to connect to the Internet  
→ `NAT` 인스턴스는 사설 서브넷 `EC2` 인스턴스를 인터넷에 연결할 때 사용된다.

- Must be launched in a public subnet  
→ `NAT` 인스턴스는 공용 서브넷에서 실행되어야 한다.

- Must disable EC2 setting: Source / destination Check  
→ `Source / destination Check`는 반드시 비활성화해야 한다.

- Must have Elastic IP attached to it  
→ `NAT` 인스턴스에는 고정된 탄력적 `IP`가 연결되어야 한다.

- Route Tables must be configured to route traffic from private subnets to the NAT Instance  
→ 사설 서브넷에서 `NAT` 인스턴스로 트래픽을 라우팅하도록 루트 테이블을 구성해야 한다.

- 인스턴스의 `IP`는 공용 서버로 액세스하는데 `NAT` 인스턴스를 통과해야 한다.

- `NAT` 인스턴스가 하는 일은 네트워크 패킷의 재작성이다. 소스 `IP`인 `Source` 정보를 사설 서브넷의 인스턴스 `IP`에서 공용 서브넷의 `NAT Instance` 주소로 변경한다.

- 네트워크 패킷의 `IP`가 재작성되므로 `Source / destination Check`는 `EC2` 인스턴스에서 반드시 비활성화해야 한다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/b518a7c2-e68e-46b2-b387-4c6c4c99d07a)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. NAT Instance – Comments
- Pre-configured Amazon Linux AMI is available
~~~
- Reached the end of standard support on December 31, 2020
→ NAT 인스턴스 설정을 위해 사전 구성된 Amazon Linux AMI를 사용할 수 있었지만 2020년 12월 31일로 표준 지원이 종료되었다. 현재는 NAT Gateway를 권장한다.
~~~

- Not highly available / resilient setup out of the box
~~~
- You need to create an ASG in multi-AZ + resilient user-data script
→ NAT 인스턴스는 가용성이 높지 않고 초기화 설정으로 복원할 수 없어서 여러 AZ에 ASG를 생성해야 하고 복원되는 사용자 데이터 스크립트가 필요하기에 꽤 복잡하다.
~~~

- Internet traffic bandwidth depends on EC2 instance type
→ 작은 인스턴스와 큰 인스턴스를 비교했을 때 작은 인스턴스의 대역폭이 더 작다.

- You must manage Security Groups & rules:
~~~
- 보안 그룹과 규칙을 관리해야 한다.

- Inbound:
- Allow HTTP / HTTPS traffic coming from Private Subnets
- Allow SSH from your home network (access is provided through Internet Gateway)
→ 인바운드에서는 사설 서브넷의 HTTP/HTTPS 트래픽과 홈 네트워크의 SSH 트래픽을 허용해야 한다.

- Outbound:
- Allow HTTP / HTTPS traffic to the Internet
→ 아웃바운드에서는 HTTP/HTTPS 트래픽이 나가도록 허용해야 한다.
~~~

- `NAT Instance`는 사라질 서비스이다. 기본적으로 시험에는 `NAT` 인스턴스와 `NAT Gateway`를 고르는 문제가 나온다.

### 3.NAT Gateway
- AWS-managed NAT, higher bandwidth, high availability, no administration  
→ `AWS`의 관리형 `NAT` 인스턴스이다. 높은 대역폭과 높은 가용성을 가지고 있다. 관리할 필요가 없다.

- Pay per hour for usage and bandwidth  
→ 사용량 및 `NAT Gateway`의 대역폭에 따라 금액이 청구된다.

- NATGW is created in a specific Availability Zone, uses an Elastic IP  
→ `NAT Gateway`는 특정 `AZ`에 생성되며 `Elastic IP`를 사용한다.

- Can’t be used by EC2 instance in the same subnet (only from other subnets)  
→ 동일한 서브넷의 `EC2` 인스턴스에서 사용할 수 없다. 다른 서브넷에서 액세스할 때만 `NAT Gateway`를 사용한다.

- Requires an IGW (Private Subnet => NATGW => IGW)  
→ `NAT Gateway` 설정을 위해서는 `Internet Gateway`가 필요하다.

- 5 Gbps of bandwidth with automatic scaling up to 45 Gbps
- No Security Groups to manage / required  
→ 보안 그룹을 관리할 필요가 없다. 연결을 하기 위해 어떤 포트를 활성화해야 되는지 생각할 필요가 없다.

### 4. NAT Gateway with High Availability
- NAT Gateway is resilient within a single Availability Zone  
→ `NAT Gateway`는 단일 `AZ` 내에서 복원 가능하다.

- Must create multiple NAT Gateways in multiple AZs for fault-tolerance  
→ `AZ`가 중지될 경우를 위해 여러 `AZ`에 `NAT Gateway`를 생성해야 한다. 여러 가용 영역에 리소스가 있고 `NAT` 게이트웨이 하나를 공유하는 경우, `NAT` 게이트웨이의 가용 영역이 다운되면 다른 가용 영역의 리소스도 인터넷에 액세스할 수 없게 되기 때문이다. 이를 통해 고가용성을 달성할 수 있다. 중요하다.

- There is no cross-AZ failover needed because if an AZ goes down it doesn't need NAT  
→ `AZ`가 다운된 경우 `NAT`이 필요하지 않으므로 `AZ`의 실패 처리가 필요하지 않다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/7c4d2193-4be8-4fcc-85d2-e9ce1b184e0d)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 5. NAT Gateway vs. NAT Instance

![image](https://github.com/sanguk2794/AWS/assets/97398071/f5be75b6-4a9c-4cf2-9a9a-b65aefc58ec8)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---