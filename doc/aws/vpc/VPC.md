## VPC
### 1. Default VPC Walkthrough
- All new AWS accounts have a default VPC  
→ 새로 생성되는 `AWS` 계정은 모두 기본 `VPC`를 가지고 있고, 이를 바로 사용할 수 있다.

- New EC2 instances are launched into the default VPC if no subnet is specified  
→ 새로 생성하는 `EC2` 인스턴스는 서브셋을 지정하지 않으면 기본 `VPC`에 실행된다.

- Default VPC has Internet connectivity and all EC2 instances inside it have public IPv4 addresses  
→ 기본 `VPC`는 처음부터 인터넷에 연결되어 있다. 기본 `VPC` 내부에 `EC2` 인스턴스를 생성하면 이 인스턴스는 공용 `IPv4` 주소를 얻기 때문에 `EC2` 인스턴스를 생성하자마자 인터넷에 연결할 수 있다.

- We also get a public and a private IPv4 DNS names  
→ `EC2` 인스턴스를 위한 공용 및 사설 `IPv4 DNS` 이름도 얻을 수 있다.

- 기본 `VPC` 대신 프로덕션 계정에서 `VPC`를 직접 만드는 것이 추천된다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/9c7acf92-53dd-4cd9-94d4-489715743ce0)

### 2. VPC in AWS – IPv4
- VPC = Virtual Private Cloud  
→ `VPC`는 `Virtual Private Cloud`의 약자이다.

- You can have multiple VPCs in an AWS region (max. 5 per region – soft limit)  
→ 단일 `AWS` 리전에 여러 `VPC`를 가질 수 있다. 리전당 최대 5개까지 가능한데, 엄격하게 제한하지 않아서 늘릴 수 있다.

- Max. CIDR per VPC is 5, for each CIDR:
~~~
- VPC마다 할당 가능한 CIDR 수는 최대 다섯 개이다.
- Min. size is /28 (16 IP addresses)
→ 각 CIDR의 최소 크기는 /28으로 최소 16개의 IP 주소를 가진다.

- Max. size is /16 (65536 IP addresses)
→ 각 CIDR의 최대 크기는 /16으로 최대 65,536개의 IP 주소를 가진다.
~~~

- Because VPC is private, only the Private IPv4 ranges are allowed:
~~~
- VPC가 사설 리소스이기 때문에 사설 IPv4 범위만을 허용한다.
- 10.0.0.0 – 10.255.255.255 (10.0.0.0/8)
- 172.16.0.0 – 172.31.255.255 (172.16.0.0/12)
- 192.168.0.0 – 192.168.255.255 (192.168.0.0/16)
~~~

- Your VPC CIDR should NOT overlap with your other networks (e.g., corporate)  
→ 일반적인 규칙으로는 원하는 대로 할당할 수 있다. 하지만, `VPC CIDR`가 다른 `VPC`나 네트워크 혹은 기업 네트워크와 겹치지 않도록 주의해야 한다. 함께 연결하려면 `VPC` 혹은 `CIDR IP` 주소가 다른 네트워크의 `IP` 주소 범위와 겹치지 않아야 하기 때문이다.

### 3. VPC – Subnet (IPv4)
- 서브넷, 즉 서브네트워크는 네트워크 내부의 네트워크이다. 

- AWS reserves 5 IP addresses (first 4 & last 1) in each subnet  
→ 서브넷 범위 내에서 `AWS`가 처음 네 개와 마지막 한 개의 `IP` 주소를 예약한다. 총 5개를 예약하는 것이다.

- These 5 IP addresses are not available for use and can’t be assigned to an EC2 instance  
→ 이 5개의 예약된 `IP` 주소는 사용할 수 없으며 `EC2` 인스턴스에 할당할 수도 없다.

- Example: if CIDR block 10.0.0.0/24, then reserved IP addresses are:
~~~
- 10.0.0.0 – Network Address
→ 첫 번째 값은 네트워크 주소이다.

- 10.0.0.1 – reserved by AWS for the VPC router
→ 두 번째 값은 VPC의 라우터용이다.

- 10.0.0.2 – reserved by AWS for mapping to Amazon-provided DNS
→ 세 번째 값으로 Amazon 제공 DNS용이다.

- 10.0.0.3 – reserved by AWS for future use
→ 당장 사용되지 않지만 나중에 필요할지도 몰라서 예약해두는 부분이다.

- 10.0.0.255 – Network Broadcast Address. AWS does not support broadcast in a VPC, therefore the address is reserved
→ 네트워크의 브로드캐스트 주소이다. AWS는 VPC에서 브로드캐스트를 지원하지 않기 때문에 예약은 되지만 사용할 수 없다.
~~~

- Exam Tip, if you need 29 IP addresses for EC2 instances:
~~~
- You can’t choose a subnet of size /27 (32 IP addresses, 32 – 5 = 27 < 29)
→ EC2 인스턴스 서브넷에서 IP 주소 29개가 필요할 때 /27 서브넷은 사용할 수 없다. 최대 32개를 표현 가능한데 다섯 개를 제외하면 27개가 남기 때문이다.

- You need to choose a subnet of size /26 (64 IP addresses, 64 – 5 = 59 > 29)
→ 서브넷에서 IP 주소 29개가 필요할 경우에는 최소 /26 서브넷을 사용해야 한다.
~~~

### 4. Internet Gateway (IGW)
- `Internet Gateway`를 사용하면 서브넷에 인터넷 액세스를 연결할 수 있다.

- Allows resources (e.g., EC2 instances) in a VPC connect to the Internet  
→ `Internet Gateway`는 `VPC`의 리소스를 인터넷에 연결하도록 허용하는 `EC2` 인스턴스나 람다 함수 등을 일컫는다.

- It scales horizontally and is highly available and redundant  
→ 수평으로 확장되고 가용성과 중복성이 높다.

- Must be created separately from a VPC  
→ `VPC`와 별개로 생성해야 한다.

- One VPC can only be attached to one IGW and vice versa  
→ `VPC`는 `Internet Gateway` 하나에만 연결되며, 반대의 경우도 마찬가지이다.

- Internet Gateways on their own do not allow Internet access…  
→ `Internet Gateway`에 대한 설정만으로는 인터넷 액세스를 허용하지 않는다.

- Route tables must also be edited!  
→ 작동을 위해 라우팅 테이블을 수정해야 한다. 라우팅 테이블을 수정해서 `EC2` 인스턴스를 라우터에 연결하고 `Internet Gateway`에 연결하는 것으로 인터넷에 연결이 가능해진다.

- 기본 라우팅 테이블이 존재한다. 기본 라우팅 테이블은 테이블 연결이 없는 서브넷에 연결된다.

- 하나의 라우팅 테이블을 여러 개의 서브넷에 할당 가능하다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/f9012137-4b99-47a5-8ccc-12d189ee40bd)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 1. 라우팅 테이블
`IP` 네트워크를 통해 이동하는 데이터 패킷이 어디로 향하게 될지 결정하는 규칙이다. 주로 테이블 형태로 표시된다. 라우팅 테이블에는 패킷이 대상을 향한 최상의 경로를 따라 전달하는데 필요한 정보가 포함되어 있다.

---
#### ▶ Reference
- [라우팅 테이블 (Routing Table)](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=dreamxpeed&logNo=221671823623)
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---