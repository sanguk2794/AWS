## Egress-only Internet Gateway
### 1. Egress-only Internet Gateway
- Used for IPv6 only (similar to a NAT Gateway but for IPv6)  
→ 송신 전용 인터넷 게이트웨이는 오직 `IPv6` 트래픽에서만 사용된다. `NAT Gateway`와 비슷하지만 `IPv6` 전용이라는 특징이 있다.

- Allows instances in your VPC outbound connections over IPv6 while preventing the internet to initiate an IPv6 connection to your instances  
→ `IPv6`을 통한 `VPC` 아웃바운드 연결을 허용하는 동시에 인터넷이 인스턴스에 대한 `IPv6` 연결을 시작하지 못하도록 한다.

- You must update the Route Tables  
→ 라우팅 테이블을 수정해야 한다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/2a2d9390-c3e6-4b07-92be-8e7c8e31badd)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. IPv6 Routing
- 공용 또는 사설 인스턴스를 인터넷과 연결하기 위해서는 다음과 과정을 거친다.
~~~
- IPv4의 경우에는 NAT Gateway를 사용한다. 서버는 NAT Gateway에 연결되고 NAT Gateway는 인터넷 Gateway에 연결되며 IPv4로 인터넷에 액세스하게 된다.
- IPv6의 경우에는 송신 전용 인터넷 게이트웨이를 사용한다. 송신 전용 인터넷 게이트웨이에 서버가 연결되고 IPv6로 인터넷에 액세스하게 된다. 
- 여기에서 공용 인스턴스와 사설 인스턴스는 라우팅 테이블의 설정이 달라진다.
~~~

- Public EC2 인스턴스를 인터넷과 연결하기 위해서는 다음과 같이 설정해야 한다.

| Destination             | Target |
|-------------------------|--------|
| 10.0.0.0/16             | local  |
| 2001:db8:1234:1a00::/56 | local  |
| 0.0.0.0/0               | igw-id |
| ::/0                    | igw-id |

- Private EC2 인스턴스를 인터넷과 연결하기 위해서는 다음과 같이 설정해야 한다.

| Destination             | Target         |
|-------------------------|----------------|
| 10.0.0.0/16             | local          |
| 2001:db8:1234:1a00::/56 | local          |
| 0.0.0.0/0               | nat-gateway-id |
| ::/0                    | eigw-id        |

![image](https://github.com/sanguk2794/AWS/assets/97398071/22ae7713-f1ae-4282-8e29-1ebafc88cadb)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---