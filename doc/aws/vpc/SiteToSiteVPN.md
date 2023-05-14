## AWS Site-to-Site VPN
### 1. AWS Site-to-Site VPN
- 온프레미스 데이터 센터를 `AWS`와 비공개로 연결하기 위해서 해야 하는 일은 다음과 같다.
~~~
- 기업은 고객 게이트웨이를 갖춰야 한다.
- VPC는 VPN 게이트웨이를 갖춰야 한다.
- AWS 콘솔에서 고객 게이트웨이와 VGW 정보를 설정한다.
- 공용 인터넷을 통해 사설 Site-to-Site VPN을 연결한다. VPN 연결은 공용 인터넷을 사용하긴 하지만 암호화가 적용된다.
~~~

- Virtual Private Gateway (VGW)
~~~
- VPN concentrator on the AWS side of the VPN connection
→ 가상 프라이빗 게이트웨이는 VPN 연결에서 AWS 측에 있는 VPN 집선기이다.

- VGW is created and attached to the VPC from which you want to create the Site-to-Site VPN connection
→ VGW는 생성되면 Site-to-Site VPN 연결을 생성하려는 VPC에 연결된다.

- Possibility to customize the ASN (Autonomous System Number)
→ ASN을 지정할 수 있다.
~~~

- Customer Gateway (CGW)
~~~
- Software application or physical device on customer side of the VPN connection
→ 고객 게이트웨이는 소프트웨어 혹은 물리적 장치이다. VPN 연결에서 고객 데이터센터에 있는 VPN 집선기이다.
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/3ed6d10f-7411-4c83-9af6-f43fb2b09c63)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. Site-to-Site VPN Connections
- Customer Gateway Device (On-premises)
~~~
- Public Internet-routable IP address for your Customer Gateway device
→ 고객 게이트웨이가 공용이라면 인터넷 라우팅이 가능한 IP 주소가 고객 게이트웨이 장치에 있다. 이 IP 주소를 사용해서 VGW와 CGW를 연결하게 된다.

- If it’s behind a NAT device that’s enabled for NAT traversal (NAT-T), use the public IP address of the NAT device
→ 고객 게이트웨이가 사설 IP를 가질 수도 있다. 이런 경우는 대부분 NAT 장치 뒤에 숨겨져 있고, NAT 장치의 IP 주소를 사용해서 VGW와 CGW를 연결하게 된다.
~~~

- Important step: enable Route Propagation for the Virtual Private Gateway in the route table that is associated with your subnets  
→ 서브넷의 `VPC`에서 라우트 전파를 활성화해야 `Site-to-Site VPN` 연결이 실제로 작동한다.

- If you need to ping your EC2 instances from on-premises, make sure you add the ICMP protocol on the inbound of your security groups  
→ 온프레미스에서 `EC2` 인스턴스 상태를 진단할 때 보안 그룹 인바운드 `ICMP` 프로토콜이 활성화됐는지 확인해야 한다. 이건 보안 그룹 문제이지만 `Site-to-Site VPN` 내용과 섞어 놓으면 헷갈리기 쉬우므로 기억해 두면 도움이 된다.

### 3. AWS VPN CloudHub
- Provide secure communication between multiple sites, if you have multiple VPN connections  
→ `CloudHub`는 서로 다른 위치 간의 기본 또는 보조 네트워크 연결에 사용되며 여러 `VPN`을 연결할 때 안전한 소통을 보장한다. 

- Low-cost hub-and-spoke model for primary or secondary network connectivity between different locations (VPN only)  
→ 비용이 적게 드는 허브 앤 스포크 모델을 사용한다. 

- It’s a VPN connection so it goes over the public Internet  
→ `VPN` 연결이므로 모든 트래픽이 공용 인터넷을 통과한다. 사설 네트워크로는 연결되지 않는다.

- To set it up, connect multiple VPN connections on the same VGW, setup dynamic routing and configure route tables  
→ 설치 방법은 간단하다. 가상 프라이빗 게이트웨이 하나에 `Site-to-Site VPN` 연결을 여러 개 만들어 동적 라우팅을 활성화하고 라우팅 테이블을 구성하면 된다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/8bda620a-c494-4a0a-ba21-ed50962402fe)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---