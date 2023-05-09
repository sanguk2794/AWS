## WAF
### 1. AWS WAF – Web Application Firewall
- Protects your web applications from common web exploits (Layer 7)  
→ `AWS WAF`는 7계층에서 일어나는 일반적인 웹 취약점 공격으로부터 웹 애플리케이션을 보호한다. `Amazon API Gateway`, `CloudFront`, `ALB`를 위한 서비스이다.

- Layer 7 is HTTP (vs Layer 4 is TCP/UDP)  
→ 7계층은 `HTTP`이므로, `HTTP`의 취약점 공격을 막아주는 것이다.

- Deploy on
~~~
- Application Load Balancer
- API Gateway
- CloudFront
- AppSync GraphQL API
- Cognito User Pool
→ WAF의 배포가 어디에 되는지 꼭 기억해야 한다. 시험에서 속이기 위해 NLB에 배포한다는 틀린 문장이 나올 수 있다.
~~~

- Define Web ACL (Web Access Control List) Rules:
~~~
- 서비스에 `WAF`를 배포한 이후에는 `ACL` 규칙을 정의해야 한다.

- IP Set: up to 10,000 IP addresses – use multiple Rules for more IPs
→ 여기에서 규칙은 IP 주소를 기반으로 구성된다. IP 세트를 정의할 수 있는데 각 IP 세트는 최대 10000개의 IP 주소를 가질 수 있다. 더 많은 IP 주소가 필요하면 규칙을 여러개 생성하면 된다.

- HTTP headers, HTTP body, or URI strings Protects from common attack - SQL injection and Cross-Site Scripting (XSS)
→ HTTP 헤더와 바디에 기반해 필터링을 수행하거나 URI 문자열을 조건으로 두어 SQL Injection과 Cross-Site Scripting 등의 일반적인 공격을 차단할 수 있다.

- Size constraints, geo-match (block countries)
→ 용량에 제한을 걸어 요청이 최대 2MB를 넘지 않게 설정하거나 지역 일치 조건을 두어 특정 국가를 허용 또는 차단할 수 있다.

- Rate-based rules (to count occurrences of events) – for DDoS protection
→ 속도 기반 규칙을 설정하면 IP 당 요청 수를 측정하여 디도스 공격을 막을 수도 있다.
~~~

- Web ACL are Regional except for CloudFront  
→ 웹 `ACL`은 기본적으로 리전 단위로 적용된다. 단, `CloudFront`는 글로벌로 정의된다.

- A rule group is a reusable set of rules that you can add to a web ACL  
→ 규칙 그룹이 존재한다. 이는 여러 웹 `ACL`에 추가할 수 있는 재사용 가능한 규칙 모음이다.

### 2. WAF – Fixed IP while using WAF with a Load Balancer
- 애플리케이션에 고정 `IP`를 사용하면서 로드 밸런서와 함께 `WAF`를 사용하고 싶다고 가정한다.

- WAF does not support the Network Load Balancer (Layer 4)  
→ `WAF`는 `NLB`를 지원하지 않는다. 

- We can use Global Accelerator for fixed IP and WAF on the ALB  
→ 따라서 `WAF`를 제공하려면 `ALB`가 있어야 한다. 하지만 `ALB`는 고정 IP가 없는데, 이 문제는 `AWS Global Accelerator`로 고정 `IP`를 할당받은 다음 `ALB`에서 `WAF`를 활성화하면 해결할 수 있다.

- 아키텍처는 다음과 같다.
~~~
- ALB와 EC2 인스턴스가 있는 리전이 하나 존재한다. 
- Global Accelerator를 ALB 앞에 두고 애플리케이션에서 사용할 고정 IP를 얻는다.
- WAF와 ACL을 연결하고, WAF를 ALB와 연결한다.
~~~ 

![image](https://user-images.githubusercontent.com/97398071/236870603-4e50501f-379d-444a-beff-baa0f4f4209a.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---