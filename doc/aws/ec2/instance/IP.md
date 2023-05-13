## IP
### 1. Private vs Public IP
- Networking has two sorts of IPs. IPv4 and IPv6
~~~
- 네트워크에는 두 종류의 `IPv4`와 `IPv6`, 두 종류의 IP가 있다.
- IPv4: 1.160.10.240
- IPv6: 3ffe:1900:4545:3:200:f8ff:fe21:67cf
~~~

- IPv4 is still the most common format used online.  
→ 가장 대중적으로 사용되는 것은 `IPv4`이다.

- IPv6 is newer and solves problems for the Internet of Things (IoT).  
→ `IPv6`는 `사물인터넷, IoT`에 주로 사용된다.

#### 1. IPv4
`IPv4`는 인터넷 프로토콜의 4번째 판이며 전 세계적으로 사용된 첫 번째 인터넷 프로토콜이다. `IPv4`는 인터넷에 연결된 각 장치 간 정보를 주고받을 수 있는 주소를 나타낸다. 이 인터넷 프로토콜은 약 43억 개의 고유한 `IP` 주소를 가질 수 있는 32비트 숫자의 형태이다.

#### 2. IPv6
인터넷은 `IPv4` 프로토콜로 구축되어 왔으나 32비트라는 제한된 주소 공간으로 인해 국가별로 할당된 주소가 거의 소진되고 있다는 문제점에 부딛혔다. 이에 대한 대안이 `IPv6` 프로토콜이다. 이 프로토콜은 `RFC`를 통해서 국제 표준으로써 확정되었고, 현재 휴대폰 및 컴퓨터에 할당되어 적용되고 있다.

### 2.Private vs Public IP (IPv4)
- `Public IP`가 있으면 인터넷 전역에 대한 액세스가 가능하고 `Private IP`만 있다면 사설 네트워크 내에서만 액세스가 가능하다.

![image](https://user-images.githubusercontent.com/97398071/232199600-79da65d0-5003-40a5-a1d7-c539685f6322.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 1. Public IP
- `Public IP`가 있으면 기기가 인터넷상에서 식별될 수 있다. 그렇기 때문에 각 `Public IP`는 전체 웹에서 유일해야 한다. `IP`를 통해 실제 `Geo-location` 정보를 파악할 수 있다.

#### 2. Private IP
- `Private IP`는 사설 네트워크 안에서만 유일함이 보장된다. 

- Machines connect to WWW using a NAT + internet gateway (a proxy)  
→ `NAT` 장치와 프록시 역할을 할 인터넷 게이트웨이를 통해 인터넷에 연결된다.

- Only a specified range of IPs can be used as private IP  
→ 지정된 범위의 `IP`만을 `Private IP`로 이용할 수 있다.

#### 3. Private vs Public IP (IPv4)
- By default, your EC2 machine comes with  
→ 내부 `AWS` 네트워크에서는 `Private IP`를, `WWW`에는 `Public IP`를 사용한다.
~~~
A private IP for the internal AWS Network
A public IP, for the WWW.
~~~

- When we are doing SSH into our EC2 machines:  
→ `EC2` 기기에 `SSH`를 통해 접근할 때에는 `Private IP`를 사용할 수 없다. `VPN`을 통해 사설망에 쓰지 않는 이상 같은 네트워크에 있는 것이 아니기 때문이다.
~~~
We can’t use a private IP, because we are not in the same network
We can only use the public IP.
~~~

- If your machine is stopped and then started, the public IP can change  
→ 인스턴스를 멈췄다가 재기동하면 `Public IP`는 바뀔 수 있다.

### 2. Elastic IPs
- When you stop and then start an EC2 instance, it can change its public IP  
→ `EC2` 인스턴스를 시작하고 종료할 때, 공용 `IP`를 바꿀 수 있다.

- If you need to have a fixed public IP for your instance, you need an Elastic IP  
→ 어떤 이유에서든 인스턴스에 고정된 `Public IP`를 사용하는 경우 이 탄력적 `IP`가 필요하다.

- An Elastic IP is a public IPv4 IP you own as long as you don’t delete it  
→ `Elastic IP`는 공용 `IPv4`이며, 삭제하지 않는 한 계속 가지고 있게 된다. 물론 이 값은 한번에 한 인스턴스에만 사용할 수 있다.

- With an Elastic IP address, you can mask the failure of an instance or software by rapidly remapping the address to another instance in your account.  
→ `IP Address`가 탄력적이면 한 인스턴스에서 다른 인스턴스로 빠르게 이동함으로써 인스턴스 또는 소프트웨어 오류를 마스킹할 때 사용할 수 있다.
하지만, `탄력적 IP`는 기본적으로 계정당 5개만 설정할 수 있기 때문에 위와 같이 사용할 일은 사실 드물다. 단, 이 개수를 늘려 줄 것을 `AWS`측에 요청할 수는 있다.

- Overall, try to avoid using Elastic IP  
→ 결론적으로, `Elastic IP`는 사용하지 않는 것이 좋다.
~~~
They often reflect poor architectural decisions
→ 탄력적 아이피는 매우 좋지 않은 구조적 결정으로 종종 언급된다.

Instead, use a random public IP and register a DNS name to it
→ 대신 임의의 Public IP를 써서 DNS 이름을 할당하는 것이 좋다. DNS는 Route 53을 통해 설정 가능하다.

- Or, as we’ll see later, use a Load Balancer and don’t use a public IP
→ 로드 밸런서를 사용하면 Public IP를 전혀 사용하지 않을수도 있다. 이 것이 AWS에서 선택할 수 있는 최상의 패턴이다.
~~~

#### 1. Allocate Elastic IP
![image](https://user-images.githubusercontent.com/97398071/232225064-c815647c-80fa-4c7f-b5fb-7d3e43b150d0.png)

- `Elastic IP`를 생성하면 연결이 유지되는 한 `Elastic IP`도 계속 유지하게 된다. 즉 `Elastic IP`를 가지고 있는데 사용하지 않으면 요금이 부과된다. 그러니 가능한 한 빠르게 연결하는 것이 좋다.

![image](https://user-images.githubusercontent.com/97398071/232225087-0eb045e9-411d-4a22-9bb9-c9044b122c27.png)

- `Associate Elastic IP address`를 통해 어떤 `Instance` 또는 `Network interface`에 `Elastic IP`를 지정할 것인지 선택할 수 있다. 

### 3. What is IPv6?
- IPv4 designed to provide 4.3 Billion addresses (they’ll be exhausted soon)  
→ `IPv4`가 처음 만들어질 당시 주소 43억 개를 제공하도록 설계되었다. 그리고 이 주소는 머지않아 소진되었다.

- IPv6 is the successor of IPv4  
→ `IPv6` 프로토콜은 32비트라는 제한된 주소 공간으로 인해 국가별로 할당된 주소가 거의 소진되고 있다는 문제점에 부딛힌 `IPv4`에 대한 대안으로 등장했다.

- IPv6 is designed to provide 3.4 × 10<sup>38</sup> unique IP addresses  
→ 3.4 × 10<sup>38</sup>의 고유 `IP` 주소를 제공한다.

- Every IPv6 address is public and Internet-routable (no private range)  
→ 모든 `IPv6` 주소는 공용이고 인터넷 라우팅이 가능하다. `IPv6`에는 사설 범위가 없다.

- Format → x.x.x.x.x.x.x.x (x is hexadecimal, range can be from 0000 to ffff)  
→ 형식은 16진수 8자리이다.

- Examples:
~~~
- 2001:db8:3333:4444:5555:6666:7777:8888
- 2001:db8:3333:4444:cccc:dddd:eeee:ffff
- :: → all 8 segments are zero
- 2001:db8:: → the last 6 segments are zero
- ::1234:5678 → the first 6 segments are zero
- 2001:db8::1234:5678 → the middle 4 segments are zero
~~~

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
