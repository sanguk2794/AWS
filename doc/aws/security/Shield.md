## AWS Shield
### 1. AWS Shield: protect from DDoS attack
- `AWS Shield`는 `DDoS` 공격을 보호하기 위한 서비스이다.

- DDoS: Distributed Denial of Service – many requests at the same time  
→ `DDoS, Distributed Denial of Service`는 분산 서비스 거부 공격이라는 뜻으로 인프라에 동시에 엄청난 양의 요청이 유입되는 공격이다. 그 목적은 인프라에 과부하를 일으켜 실제 사용자들에게 서비스를 제공할 수 없게 만드는 것에 있다.

- AWS Shield Standard:
~~~
- Free service that is activated for every AWS customer
→ 이 서비스는 모든 AWS 고객에게 무료로 활성화되어 있는 서비스이다.

- Provides protection from attacks such as SYN/UDP Floods, Reflection attacks and other layer 3/layer 4 attacks
→ SYN/UDP Floods, 반사 공격, L3/L4 공격으로부터 서버를 보호해 준다.
~~~

- AWS Shield Advanced:
~~~
- AWS Shield Advanced는 고급 보호가 필요한 고객을 위한 서비스이다.
- Optional DDoS mitigation service ($3,000 per month per organization)
→ 선택적으로 제공되는 디도스 완화 서비스이다.

- Protect against more sophisticated attack on Amazon EC2, Elastic Load Balancing (ELB), Amazon CloudFront, AWS Global Accelerator, and Route 53
→ Amazon EC2, ELB Amazon CloudFront, AWS Global Accelerator, Route 53를 향한 더욱 정교한 디도스 공격을 방어해 준다.

- 24/7 access to AWS DDoS response team (DRP)
→ AWS 디도스 대응 팀이 항시 대기하고 있어 공격을 받았을 때 지원을 받을 수 있다

- Protect against higher fees during usage spikes due to DDoS
→ 디도스 공격으로 인한 요금 상승을 AWS Shield Advanced로 방지할 수 있다.

- Shield Advanced automatic application layer DDoS mitigation automatically creates, evaluates and deploys AWS WAF rules to mitigate layer 7 attacks
→ 자동 애플리케이션 계층 디도스 완화를 제공한다. 자동으로 WAF 규칙을 생성, 평가, 배포함으로써 L7 공격을 완화할 수 있다.
~~~

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---