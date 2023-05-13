## VPC – Traffic Mirroring
### 1. VPC – Traffic Mirroring
- Allows you to capture and inspect network traffic in your VPC  
→ 보안 기능이다. `VPC`에서 네트워크 트래픽을 수집하고 검사하되 방해되지 않는 방식으로 실행하는 기능이다.

- Route the traffic to security appliances that you manage  
→ 관리 중인 보안 어플라이언스로 트래픽을 라우팅해야 한다.

- Capture the traffic
~~~
- From (Source) – ENIs
→ 수집하려는 트래픽이 있는 소스 ENI를 정의한다.

- To (Targets) – an ENI or a Network Load Balancer
→ 트래픽을 보낼 대상을 정의한다. 이는 ENI 또는 NLB가 될 수 있다.
~~~

- Capture all packets or capture the packets of your interest (optionally, truncate packets)  
→ 트래픽 미러링 기능을 사용하면 소스 `ENI`로 전송되는 트래픽이 타겟으로도 보내진다. 그리고 타겟에서는 트래픽 자체를 분석한다. 모든 정보가 아닌 일부 정보만을 취득하고 싶다면 필터를 사용할 수도 있다.

- Source and Target can be in the same VPC or different VPCs (VPC Peering)  
→ 소스와 타겟을 여러 개 설정할 수 있다. 이 때, 소스와 타겟이 동일한 `VPC` 내에 있어야 하지만, `VPC Peering`을 활성화했다면 다른 `VPC`에 있더라도 대상이 될 수 있다.

- Use cases: content inspection, threat monitoring, troubleshooting, …  
→ 콘텐츠 검사와 위협 모니터링 네트워킹 문제 해결 등에 사용된다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/ace08753-a404-4268-958f-e93db6ea6e35)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---