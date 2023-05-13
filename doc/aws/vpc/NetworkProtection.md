## Network Protection
### 1. Network Protection on AWS
- Network Access Control Lists (NACLs)  
→ 네트워크 액세스를 제어할 수 있다. 서브넷 레벨에서 동작한다.

- Amazon VPC security groups  
→ 네트워크 액세스를 제어할 수 있다. 인스턴스 레벨에서 동작한다.

- AWS WAF (protect against malicious requests)  
→ 7계층에서 일어나는 일반적인 웹 취약점 공격으로부터 웹 애플리케이션을 보호한다.

- AWS Shield & AWS Shield Advanced  
→ `DDoS`로부터 웹 애플리케이션을 보호한다.

- AWS Firewall Manager (to manage them across accounts)  
→ `Firewall Manager`는 방화벽 규칙에 대한 중앙 집중식 보안 관리를 제공한다.

- But what if we want to protect in a sophisticated way our entire VPC?  
→ 전체 `VPC`를 보호하고 싶은 경우에는 `AWS Network Firewall`을 사용하면 된다.

### 2. AWS Network Firewall
- 이 방화벽은 `VPC` 수준에서 설정할 수 있고, 트래픽 필터링과 플로우 검사를 지원한다.

- Protect your entire Amazon VPC  
→ 전체 `VPC`를 방화벽으로 보호하는 서비스이다.

- From Layer 3 to Layer 7 protection  
→ `AWS Network Firewall`은 계층 3에서 계층 7까지의 보호를 수행한다.

- Any direction, you can inspect
~~~
- VPC to VPC traffic
→ VPC 간 트래픽을 검사한다.

- Outbound to internet
→ 아웃바운드 트래픽을 검사한다.

- Inbound from internet
→ 인바운드 트래픽을 검사한다.

- To / from Direct Connect & Site-to-Site VPN
→ 여기에는 Direct Connect나 Site to Site VPN 연결까지 포함된다.
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/e3a3cc01-a12f-426a-8268-482279a7f013)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Internally, the AWS Network Firewall uses the AWS Gateway Load Balancer  
→ 내부적으로 `Network Firewall`은 `AWS Gateway Load Balancer`를 사용한다.

- Rules can be centrally managed cross- account by AWS Firewall Manager to apply to many VPCs  
→ 이 규칙들 역시 중앙 집중식으로 관리되며 여러 계정과 `VPC`에 적용된다.

### 3. Network Firewall – Fine Grained Controls
- 네트워크 트래픽을 세부적으로 관리할 수 있다.

- Supports 1000s of rules
~~~
- VPC 수준에서 수천 개의 규칙을 지원한다. 

- IP & port - example: 10,000s of IPs filtering
→ IP와 포트로 필터링할 수 있다.

- Protocol – example: block the SMB protocol for outbound communications
→ 프로토콜로 필터링이 가능하다.

- Stateful domain list rule groups: only allow outbound traffic to *.mycorp.com or third-party software repo
→ 도메인별로도 필터링할 수 있어서 VPC의 아웃바운드 트래픽에 대해서 기업 도메인에만 액세스를 허용하거나 타사 소프트웨어 레포지토리에만 액세스를 허용하는 등의 설정이 가능하다.

- General pattern matching using regex
→ 정규식을 사용해 일반 패턴 매칭을 설정할 수 있다.
~~~

- Traffic filtering: Allow, drop, or alert for the traffic that matches the rules  
→ 트래픽 허용, 차단, 알림 등을 설정해서 설정한 규칙에 맞는 트래픽을 필터링할 수 있다.

- Active flow inspection to protect against network threats with intrusion-prevention capabilities (like Gateway Load Balancer, but all managed by AWS)  
→ 활성 플로우 검사를 통해 침입 방지 능력을 갖출 수 있다.

- Send logs of rule matches to Amazon S3, CloudWatch Logs, Kinesis Data Firehose  
→ 모든 규칙 일치는 `Amazon S3`와 `CloudWatch Logs` 및 `Kinesis Data Firehose`로 전송해 분석할 수 있다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---