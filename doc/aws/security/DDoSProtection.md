## DDoS Protection
### 1. AWS Best Practices for DDoS Resiliency - Edge Location Mitigation (BP1, BP3)
- `mitigation` = 완화, `DDoS` 공격과 관련된 다양한 보호 방법은 중요하다.

- BP1 – CloudFront
~~~
- Web Application delivery at the edge
→ 웹 애플리케이션 전송이 엣지에서 일어난다.

- Protect from DDoS Common Attacks (SYN floods, UDP reflection…)
→ SYN Flood나 UDP 반사 공격과 같은 DDoS 일반 공격은 Shield 설정으로 막을 수 있다.
~~~

- BP1 – Global Accelerator
~~~
- Access your application from the edge
→ Global Accelerator를 사용하면 사용자는 엣지 로케이션을 통해 애플리케이션에 액세스하게 된다.

- Integration with Shield for DDoS protection
→ Global Accelerator를 Shield와 결합해 DDoS 방어에 사용할 수 있다.

- Helpful if your backend is not compatible with CloudFront
→ 백엔드가 CloudFront와 호환되지 않는 경우 유용하다. 어떤 백엔드를 사용하든 AWS 엣지 로케이션에 완전 분산이 가능하기 때문에 엣지 로케이션을 DDoS 공격으로부터 보호할 수 있다.
~~~

- BP3 – Route 53
~~~
- Domain Name Resolution at the edge
→ 엣지 로케이션에 도메인 이름 변환을 글로벌로 설정하면 DDoS 보호 매커니즘을 적용할 수 있다.
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/ff448632-9af1-49fe-982f-76ad904579aa)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. AWS Best Practices for DDoS Resiliency - Best practices for DDoS mitigation
- Infrastructure layer defense (BP1, BP3, BP6)
~~~
- 인프라 계층 방어는 BP1, BP3, BP6에 해당한다.

- Protect Amazon EC2 against high traffic
→ 높은 트래픽으로부터 Amazon EC2 인스턴스를 보호한다.

- That includes using Global Accelerator, Route 53, CloudFront, Elastic Load Balancing
→ CloudFront, Global Accelerator Route 53, ELB가 포함된다. 이 서비스들을 이용하면 EC2 인스턴스에 리퀘스트가 도달하기 전에 트래픽을 관리할 수 있다.
~~~

- Amazon EC2 with Auto Scaling(BP7)
~~~
- Helps scale in case of sudden traffic surges including a flash crowd or a DDoS attack
→ ASG에 많은 트래픽이 도달한다고 해도 자동으로 확장하여 애플리케이션에서 더 큰 로드를 수용할 수 있다.
~~~

- Elastic Load Balancing (BP6)
~~~
- Elastic Load Balancing scales with the traffic increases and will distribute the traffic to many EC2 instances
→ ELB가 여러 EC2 인스턴스 간의 트래픽을 자동으로 분산시킨다. 
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/6c410001-ef06-4e41-b11d-25c950426db2)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. AWS Best Practices for DDoS Resiliency - Application Layer Defense
- 애플리케이션 계층 방어의 예시로는 `BP1`과 `BP2`로 악성 요청을 감지 및 필터링하는 방식이 있다.

- Detect and filter malicious web requests (BP1, BP2)
~~~
- CloudFront cache static content and serve it from edge locations, protecting your backend
→ CloudFront는 정적 콘텐츠 전송 시 엣지 로케이션에서 전송함으로써 백엔드를 보호한다.

- AWS WAF is used on top of CloudFront and Application Load Balancer to filter and block requests based on request signatures
→ ALB나 CloudFront에 WAF를 설정하는 것으로 요청을 필터링 및 차단할 수 있다.

- WAF rate-based rules can automatically block the IPs of bad actors
→ WAF 속도 기반 규칙을 사용하면 악성 사용자의 IP를 자동으로 차단할 수 있다.

- Use managed rules on WAF to block attacks based on IP reputation, or block anonymous Ips
→ WAF에 여러 관리형 규칙을 사용하면 평판에 따라 IP를 차단하거나 익명 IP 등을 차단할 수 있다.

- CloudFront can block specific geographies
→ CloudFront로는 특정 지역을 차단할 수 있다.
~~~

- Shield Advanced (BP1, BP2, BP6)
~~~
- Shield Advanced automatic application layer DDoS mitigation automatically creates, evaluates and deploys AWS WAF rules to mitigate layer 7 attacks
→ Shield Advanced를 활성화하면 자동으로 WAF 규칙을 생성하여 L7 공격을 완화한다. 따라서 애플리케이션 계층 방어에 유용하다.
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/45ddfadd-06db-4fee-8ad3-d00035e01c25)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 4. AWS Best Practices for DDoS Resiliency - Attack surface reduction
- 공격 지점을 줄이기 위한 방법이다.

- Obfuscating AWS resources (BP1, BP4, BP6)
~~~
- Using CloudFront, API Gateway, Elastic Load Balancing to hide your backend resources (Lambda functions, EC2 instances)
→ CloudFront, API Gateway, ELB 중 하나를 사용하면 백엔드 리소스를 숨길 수 있다. 공격자는 이 리소스가 람다 함수인지, EC2 인스턴스인지 아니면 ECS 태스크인지 알 수 없는 것이다.
~~~

- Security groups and Network ACLs(BP5)
~~~
- Use security groups and NACLs to filter traffic based on specific IP at the subnet or ENI-level
→ 보안 그룹과 네트워크 ACL 등을 설정하여 특정 IP의 트래픽을 필터링할 수 있다.

- Elastic IP are protected by AWS Shield Advanced
→ Elastic IP도 AWS Shield Advanced로 보호할 수 있다.
~~~

- Protecting API endpoints (BP4)
~~~
- Hide EC2, Lambda, elsewhere
→

- Edge-optimized mode, or CloudFront + regional mode (more control for DDoS)
→ 엣지 최적화 모드를 사용할 경우 기본적으로 글로벌로 설정된다. CloudFront에 리전 모드를 더해 사용한다면 DDoS 보호에 관한 제어 기능이 더 강화된다.

- WAF + API Gateway: burst limits, headers filtering, use API keys
→ API Gateway에 WAF를 사용하는 경우 모든 HTTP 요청을 필터링할 수 있다.
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/443f5704-abb9-48f6-b492-96c2b8a7b9ee)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---