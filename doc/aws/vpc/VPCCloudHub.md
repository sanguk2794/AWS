## AWS VPN CloudHub
### 1. AWS VPN CloudHub
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