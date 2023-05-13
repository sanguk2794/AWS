## Transit Gateway
### 1. Transit Gateway
- `Transit` = 환승

- For having transitive peering between thousands of VPC and on-premises, hub-and-spoke (star) connection  
→ `Transit Gateway`를 사용하면 복수의 `VPC`와 온프레미스 데이터 센터 `Site-to-Site VPN`, `Direct Connect` 허브와 스포크 등을 피어링 없이 연결할 수 있다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/ae18c6b1-d61d-47d4-91e6-05e76ec66ebf)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Regional resource, can work cross-region  
→ 또한 리전 리소스이며 `cross-region`이 가능하다.

- Share cross-account using Resource Access Manager (RAM)  
→ `Transit Gateway`를 계정 간에 공유하려면 `Resource Access Manager`를 사용해야 한다.

- You can peer Transit Gateways across regions  
→ 리전 간 `Transit Gateway`를 피어링할 수 있다.

- Route Tables: limit which VPC can talk with other VPC  
→ `Transit Gateway`에 라우팅 테이블을 생성해서 연결간의 액세스를 제한할 수 있다. 트래픽 경로를 제어해서 네트워크 보안을 제공하는 것이다.

- Works with Direct Connect Gateway, VPN connections  
→ `Direct Connect Gateway` 및 `VPN connections`과 함께 작동한다.

- Supports IP Multicast (not supported by any other AWS service)  
→ 유일하게 `IP 멀티캐스트`를 지원하는 서비스이다. 만약 시험에 `IP 멀티캐스트`가 나오면 `Transit Gateway`를 떠올려야 한다.

### 2. Transit Gateway: Site-to-Site VPN ECMP
- ECMP = Equal-cost multi-path routing  
→ 등가 다중 경로 라우팅이다.

- Routing strategy to allow to forward a packet over multiple best path  
→ `ECMP`는 최적 경로를 통해 패킷을 전달하기 위한 라우팅 전략이다.

- Use case: create multiple Site-to-Site VPN connections to increase the bandwidth of your connection to AWS  
→ `Site-to-Site VPN` 연결을 많이 생성해서 `AWS`로의 연결 대역폭을 늘릴 때 사용한다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/4c053f60-7271-431b-a79f-667e06f494db)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. Transit Gateway: throughput with ECMP
- 가상 프라이빗 게이트웨이에 `VPN`을 연결하면 `VPC`마다 연결이 하나씩 생기고 연결 하나당 `1.25Gbps`으로 최대 처리량이 제한된다. 

- 가상 프라이빗 게이트웨이 대신에 `Transit Gateway`를 사용해 `VPN`을 연결하면 `Site-to-Site VPN` 하나가 여러 `VPC`를 사용할 수 있다. 동일한 `Transit Gateway`에 모두 전이적으로 연결되기 때문이다. 

- `Transit Gateway`에 `Site-to-Site VPN` 연결을 추가할 수 있다. 하나를 추가할 때마다 처리량이 `2.5Gbps` 늘어난다. 중요하다.  
→ 이렇게 설정하면 데이터가 `Transit Gateway`를 통과할 때 요금이 청구된다. 성능 최적화 비용이 추가되는 것이다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/118b5062-2c50-4917-9f82-29505d26cb98)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 4. Transit Gateway – Share Direct Connect between multiple accounts
- `Direct Connect 연결`을 여러 계정에서 공유할 때 `Transit Gateway`를 사용한다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/1cf2dc3c-5975-4b95-ba23-954cd83592ca)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---