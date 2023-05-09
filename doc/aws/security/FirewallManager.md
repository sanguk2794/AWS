## AWS Firewall Manager
### 1. AWS Firewall Manager
- `Firewall Manager`는 방화벽 규칙에 대한 중앙 집중식 보안 관리를 제공한다.
 
- Manage rules in all accounts of an AWS Organization  
→ `AWS Organizations`의 모든 계정의 방화벽 규칙을 관리하는 서비스이다. 여러 계정의 규칙을 동시에 관리할 수 있다.

- Security policy: common set of security rules
~~~
- 보안 규칙의 집합인 보안 정책을 설정할 수 있다.

- WAF rules (Application Load Balancer, API Gateways, CloudFront)
→ 보안 정책에 ALB, API Gateway CloudFront 등에 적용되는 웹 애플리케이션 방화벽(WAF) 규칙이 포함된다.

- AWS Shield Advanced (ALB, CLB, NLB, Elastic IP, CloudFront)
→ 보안 정책에 ALB, CLB, NLB, 엘라스틱 IP CloudFront를 위한 AWS Shield 어드밴스드 규칙이 포함된다.

- Security Groups for EC2, Application Load Balancer and ENI resources in VPC
→ 보안 정책에 EC2, ALB, VPC의 ENI 리소스를 위한 보안 그룹 정책이 포함된다.

- AWS Network Firewall (VPC Level)
→ 보안 정책에 VPC 수준의 AWS Network Firewall이 포함된다.

- Amazon Route 53 Resolver DNS Firewall
→ 보안 정책에 Amazon Route 53 Resolver DNS Firewall이 포함된다.

- Policies are created at the region level
→ 정책은 리전 수준에서 생성되며 조직에 등록된 모든 계정에 적용된다.
~~~

- Rules are applied to new resources as they are created (good for compliance) across all and future accounts in your Organization  
→ 조직에서 `ALB`에 대한 `WAF` 규칙을 생성한 다음 새 `ALB`를 생성하는 경우 `AWS Firewall Manager`는 새 `ALB`도 같은 규칙을 적용해 준다. 이것이 `AWS Firewall Manager`의 장점이다.

### 2. WAF vs. Firewall Manager vs. Shield
- WAF, Shield and Firewall Manager are used together for comprehensive protection  
→ `WAF`, `Shield`, `Firewall Manager` 모두 포괄적인 계정 보호를 위한 서비스이다.

- Define your Web ACL rules in WAF  
→ `WAF`는 웹 `ACL, Access Control List` 규칙을 정의한다. 

- For granular protection of your resources, WAF alone is the correct choice  
→ 리소스별 보호를 구성할 때 `WAF`가 적절하다.

- If you want to use AWS WAF across accounts, accelerate WAF configuration, automate the protection of new resources, use Firewall Manager with AWS WAF  
→ 여러 계정이 `WAF`를 사용하는 상황에서 `WAF` 구성을 가속하고 새 리소스 보호를 자동화하려면 `Firewall Manager`로 `WAF` 규칙을 관리하면 된다. `Firewall Manager`는 이러한 규칙들을 모든 계정과 모든 리소스에 자동으로 적용해 주기 때문이다.

- Shield Advanced adds additional features on top of AWS WAF, such as dedicated support from the Shield Response Team (SRT) and advanced reporting.  
→ `Shield Advanced`는 `DDoS` 공격으로부터 고객을 보호한다. 또, `AWS` 디도스 대응 팀이 항시 대기하고 있어 공격을 받았을 때 지원을 받을 수 있다. 또, `WAF` 규칙을 자동 생성해준다.

- If you’re prone to frequent DDoS attacks, consider purchasing Shield Advanced  
→ 디도스 공격을 자주 받는다면 `Shield Advanced`를 사용하는 것이 좋은 선택지가 될 수 있다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---