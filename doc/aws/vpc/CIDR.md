## CIDR
### 1. Understanding CIDR – IPv4
- 사이더라고 불린다.

- Classless Inter-Domain Routing – a method for allocating IP addresses  
→ 클래스 없는 도메인 간 라우팅으로 `IP` 어드레스를 할당하는 방법이다.

- Used in Security Groups rules and AWS networking in general  
→ 보안 그룹 규칙과 네트워킹에서 사용된다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/8cad291b-97df-422f-8ea9-3093bbd4c77c)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- They help to define an IP address range:
~~~
- IP 주소의 범위를 정의할 수 있다.

- We’ve seen WW.XX.YY.ZZ/32 => one IP
→ 32로 끝나는 IP 주소는 하나의 IP 주소만이 범위가 된다.

- We’ve seen 0.0.0.0/0 => all IPs
→ 0.0.0.0/0이라면 모든 IP 주소가 범위가 된다.

- But we can define:192.168.0.0/26 =>192.168.0.0 – 192.168.0.63 (64 IP addresses)
→ 64개의 IP 주소를 나타낸다.
~~~

- A CIDR consists of two components  
→ 사이더의 구성 요소는 기본 `IP`와 서브넷 마스크의 2가지이다. 

- Base IP 
~~~
- Represents an IP contained in the range (XX.XX.XX.XX) 
→ 기본 IP는 범위에 포함되는 IP이다.

- Example: 10.0.0.0, 192.168.0.0, … 
→ 
~~~

- Subnet Mask 
~~~
- Defines how many bits can change in the IP 
→ 서브넷 마스크는 IP 주소에서 변경 가능한 비트의 개수를 정의한다.

- Example: /0, /24, /32 
- Can take two forms: 
- /8 ó 255.0.0.0 
- /16 ó 255.255.0.0 
- /24 ó 255.255.255.0 
- /32 ó 255.255.255.250
→ /8은 서브넷 마스크 255.0.0.0과 동일하고, /16은 255.255.0.0과 동일하다.
~~~

### 2. Understanding CIDR – Subnet Mask
- The Subnet Mask basically allows part of the underlying IP to get additional next values from the base IP
- `IP` 주소 `/32`는 2의 0승으로 하나의 `IP`만 허용된다. `IP` 주소 `/31`이라면 2의 1승으로 두 가지 `IP`가 허용된다.
- `IP`는 옥텟 네 개로 구성된다. `/32`는 옥텟 변경 불가를 뜻하고 `/24`는 마지막 옥텟 변경 가능을 뜻하며, `/16`은 두 개의 옥텟 변경 가능을 뜻한다. `/8`은 세 개의 옥텟 변경 가능을 뜻하고, `/0`은 전체 옥텟을 변경 가능하다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/ec6b0260-2bee-489a-9721-9fdf142c06bc)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Example
~~~
- 192.168.0.0/24 = … ?
→ 192.168.0.0 – 192.168.0.255 (256 IPs)

- 192.168.0.0/16 = … ?
→ 192.168.0.0 – 192.168.255.255 (65,536 IPs)

- 134.56.78.123/32 = … ?
→ Just 134.56.78.123

- 0.0.0.0/0
→ All IPs!
~~~

### 3. Public vs. Private IP (IPv4)
- The Internet Assigned Numbers Authority (IANA) established certain blocks of IPv4 addresses for the use of private (LAN) and public (Internet) addresses  
→ `Internet Assigned Numbers Authority, IANA`에서 구축한 `IPv4` 주소 블록은 로컬 네트워크나 공용 인터넷의 주소이다.

- Private IP can only allow certain values:
~~~
- 사설 IP는 특정 값만 허용한다.
- 10.0.0.0 – 10.255.255.255 (10.0.0.0/8) ç in big networks
→ 엄청나게 많은 값이 허용되기에 대형 네트워크에서 사용된다.

- 172.16.0.0 – 172.31.255.255 (172.16.0.0/12) ç AWS default VPC in that range
→ 계정을 생성할 때 AWS에서 제공하는 기본 VPC는 해당 네트워킹 공간에 포함된다.

- 192.168.0.0 – 192.168.255.255 (192.168.0.0/16) ç e.g., home networks
→ 192.168.0.0/16은 홈 네트워크를 나타낸다. 장치를 연결할 때 인터넷 라우터가 있으면 192로 시작하는 IP 주소를 흔히 사용한다.
~~~

- All the rest of the IP addresses on the Internet are Public  
→ 이외의 `IP` 주소는 모두 인터넷상에 있는 공용 `IP` 주소이다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---