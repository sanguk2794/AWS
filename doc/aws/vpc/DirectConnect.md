## Direct Connect (DX)
### 1. Direct Connect (DX)
- Provides a dedicated private connection from a remote network to your VPC  
→ `Direct Connect`는 원격 네트워크에서 `VPC`로의 전용 프라이빗 연결을 제공한다.

- Dedicated connection must be setup between your DC and AWS Direct Connect locations  
→ `Direct Connect`를 사용할 때는 전용 연결을 생성해야 하고 `AWS Direct Connect` 로케이션을 사용한다.

- You need to setup a Virtual Private Gateway on your VPC  
→ `VPC`에 가상 프라이빗 게이트웨이를 설치해야 온프레미스 데이터 센터와 `AWS` 간 연결이 가능하다.

- Access public resources (S3) and private (EC2) on same connection  
→ `Amazon S3` 등의 퍼블릭 리소스와 `EC2` 인스턴스 등의 프라이빗 리소스에 퍼블릭 및 프라이빗 `VIF, Virtual InterFace`를 사용해 액세스할 수 있다.

- Use Cases:
~~~
- Increase bandwidth throughput - working with large data sets – lower cost
→ 큰 데이터 세트를 처리할 때 속도가 빨라진다. 퍼블릭 인터넷을 거치지 않기 때문이다. 또 프라이빗 연결을 사용하므로 비용도 절약된다.

- More consistent network experience - applications using real-time data feeds
→ 퍼블릭 인터넷 연결에 문제가 발생해도 Direct Connect를 사용하면 연결 상태를 유지할 수 있다.

- Hybrid Environments (on prem + cloud)
→ 하이브리드 환경을 지원한다. 이는 온프레미스 데이터 센터와 클라우드가 연결되어 있기 때문이다.
~~~

- Supports both IPv4 and IPv6  
→ `IPv4`와 `IPv6`를 모두 지원한다.

- 모든 과정을 수동으로 연결해야 하므로 설치가 오래 걸린다. 하지만 퍼블릭 인터넷을 전혀 지나지 않고 전부 비공개로 연결이 가능하다.

- `AWS` 내 퍼블릭 서비스, 가령 `Amazon Glacier` 또는 `Amazon S3`에 연결할 수도 있는데 이 때는 퍼블릭 가상 인터페이스를 설치해야 한다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/171bdb95-e0ea-4431-a0b2-4a41eea45b8f)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. Direct Connect Gateway
- If you want to setup a Direct Connect to one or more VPC in many different regions (same account), you must use a Direct Connect Gateway  
→ 하나 이상의 `VPC`와 연결하고 싶다면 `Direct Connect Gateway`를 사용해야 한다.

- 온프레미스 데이터 센터를 두 개의 `VPC`에 연결하기 위한 시나리오이다.
~~~
- Direct Connect Connection를 생성한 뒤 온프레미스 데이터 센터를 Direct Connect Gateway에 연결한다.
- 각각의 Private VPC, Direct Connect Connection를 VIF를 사용해 Direct Connect Gateway에 연결한다.
- 설정을 통해 여러 VPC와 여러 리전을 연결할 수 있다.
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/175e02de-f842-42d4-b122-c3ef8abe5b57)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. Direct Connect – Connection Types
- Dedicated Connections: 1Gbps,10 Gbps and 100 Gbps capacity
~~~
- Physical ethernet port dedicated to a customer
→ 고객은 물리적 이더넷 포트를 할당받는다.

- Request made to AWS first, then completed by AWS Direct Connect Partners
→ AWS에 요청을 보내면 AWS Direct Connect 파트너가 처리를 완료해준다.
~~~

- Hosted Connections: 50Mbps, 500 Mbps, to 10 Gbps
~~~
- Connection requests are made via AWS Direct Connect Partners
→ AWS에 요청을 보내면 AWS Direct Connect 파트너가 처리를 완료해준다.

- Capacity can be added or removed on demand
→ 필요하면 언제든지 용량을 추가하거나 제거할 수 있다. 전용 연결보다 유연하다.

- 1, 2, 5, 10 Gbps available at select AWS Direct Connect Partners
→ 선택한 로케이션에서 초당 1, 2, 5, 10Gbp 이용이 가능하다.
~~~

- Lead times are often longer than 1 month to establish a new connection  
→ 전용 연결이든 호스팅 연결이든 새 연결을 만들기 위해 한 달이 넘게 걸릴 수 있다. 일주일 안에 빠르게 데이터를 전송하고 싶은 경우 `Direct Connect`는 절대 답이 될 수 없다.

### 4. Direct Connect – Encryption
- Data in transit is not encrypted but is private  
→ `Direct Connect`에는 암호화 기능이 없어서 데이터가 암호화되지는 않는다. 하지만 프라이빗 연결이므로 보안을 유지할 수 있다.

- AWS Direct Connect + VPN provides an IPsec-encrypted private connection  
→ 만약 암호화를 원한다면 `AWS Direct Connect`와 함께 `VPN`을 설치해야 한다. `IPsec`으로 암호화된 프라이빗 연결이 가능하다.

- Good for an extra level of security, but slightly more complex to put in place  
→ 추가적인 보안을 확보할 수 있지만 구현이 복잡하다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/2c1db084-a6a2-45dc-88f2-2cb22de63f20)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 5. Direct Connect - Resiliency
- `Resiliency` = 복원력

- 핵심 워크로드의 복원력을 높이기 위해서 여러 `Direct Connect`를 설치하는 것이 좋다.

- 각 `Direct Connect` 로케이션에 독립적인 연결을 두 개씩 수립하면 복원력을 최대로 만들 수 있다.  
→ 생성 가능한 연결은 최대 두 개이다. 최대라는 단어를 사용해 문제로 등장할 수 있다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/2c7732ce-0b90-4736-aa7e-467ef6eaf12f)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 6. Site-to-Site VPN connection as a backup
- 회사 데이터 센터를 `Direct Connect`를 통해 `VPC`에 연결할 때의 시나리오이다.
~~~
- Direct Connect를 통해 VPC에 연결한다. 기본 방법이며, 비용도 많이 들고 가끔은 Direct Connect 연결에 문제가 발생할 수도 있다.
- 복원을 위해 또 다른 Direct Connect로 보조 연결을 할 수 있지만 이는 상당히 많은 비용이 소모된다.
- 또는 Site to Site VPN을 백업 연결을 두어 기본 연결에 문제가 발생했을 때 사용하도록 설정할 수 있다. Site-to-Site VPN을 통해 퍼블릭 인터넷에 연결할 수 있으므로 안정성이 높아진다.
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/fe3db549-61af-4520-915f-fed285127006)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---