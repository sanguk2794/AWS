## VPC Flow Logs
### 1. VPC Flow Logs
- Capture information about IP traffic going into your interfaces:
~~~
- VPC 흐름 로그는 인터페이스로 향하는 IP 트래픽 정보를 포착할 때 사용한다. VPC 계층, 서브넷 계층, ENI 계층의 세 가지 흐름 로그가 있다.
- VPC Flow Logs
- Subnet Flow Logs
- Elastic Network Interface (ENI) Flow Logs
~~~

- Helps to monitor & troubleshoot connectivity issues  
→ 흐름 로그는 연결 문제를 모니터링하고 트러블 슈팅하는 데 유용하다.

- Flow logs data can go to S3 / CloudWatch Logs  
→ 로그 데이터를 `Amazon S3`와 `CloudWatch Logs`에 전송할 수 있다.

- Captures network information from AWS managed interfaces too: ELB, RDS, ElastiCache, Redshift, WorkSpaces, NATGW, Transit Gateway…  
→ 관리형 인터페이스인 `ELB`, `RDS`, `ElastiCache`, `Redshift`, `Workspaces`, `NAT Gateway`, `Transit Gateway`의 정보를 포착한다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/1de678ea-157f-4e89-a9ae-71926de5e0b9)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. VPC Flow Logs Syntax
- `VPC`를 통과하는 네트워크 패킷의 메타데이터이다. 정해진 형식에 따른다.

- srcaddr & dstaddr – help identify problematic IP and identity problematic ports  
→ `srcaddr`과 `dstaddr`로는 문제가 있는 `IP`를 식별할 수 있다. 한 `IP`가 지속적으로 거부되면 여기에서 확인할 수 있다. 또, `IP`에 문제가 있거나 특정 `IP`가 공격받았을 때도 이 값으로 식별할 수 있다.

- Action – success or failure of the request due to Security Group / NACL  
→ `Action`은 요청이 수락되었는지 거부되었는지를 나타내며 보안 그룹이나 `NACL` 계층에서 요청이 성공했는지 실패했는지 알 수 있다.

- Can be used for analytics on usage patterns, or malicious behavior  
→ 이러한 `VPC` 흐름 로그는 사용량 패턴 분석이나 악성 행동 감지 포트 스캔 등에 사용할 수 있다.

- Query VPC flow logs using Athena on S3 or CloudWatch Logs Insights  
→ `S3`에서 `Athena`를 사용하거나, `CloudWatch Logs Insight`를 사용해 쿼리가 가능하다. 스트리밍 분석을 원할 때는 `CloudWatch Logs Insight`를 사용해야 한다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/ba6f59cc-265e-4765-be66-4b31059e858a)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. VPC Flow Logs –Troubleshoot SG & NACL issues
- 흐름 로그를 보안 그룹과 NACL 문제의 트러블 슈팅에 사용할 수 있다.

- 들어오는 요청에 대한 트러블 슈팅 시나리오는 다음과 같다.
~~~
- NACL과 서브넷에 요청이 들어온다. 이때 NACL은 무상태이고 보안 그룹은 상태를 유지한다.
- 인바운드 REJECT가 표시된 경우 EC2 외부에서 전송한 인바운드 요청이 거부되었다는 뜻으로 NACL이나 보안 그룹 중 하나가 요청을 거부하고 있다는 것을 의미한다.
- 인바운드 ACCEPT 아웃바운드 REJECT는 NACL 문제라는 의미이다. 보안 그룹은 상태 유지하므로 인바운드가 ACCEPT로 표시되면 자동으로 아웃바운드가 ACCEPT여야 하기 때문이다.
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/0d7e99f3-5186-49c2-9d65-d338e9d1b532)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 나가는 요청에 대한 트러블 슈팅 시나리오도 결이 같다. 아웃바운드 `REJECT`가 표시되면 `NACL`이나 보안 그룹 중 하나의 문제이지만, 아웃바운드 `ACCEPT` 인바운드 `REJECT`는 반드시 `NACL`가 원인이 된다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/e0e4fc00-b40d-46c8-9ddf-756daf282eaa)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 4. VPC Flow Logs – Architectures
- `CloudWatch Logs`로 전송된 흐름 로그는 `CloudWatch Contributor Insights`를 이용해 `VPC` 네트워크에 가장 많이 기여하는 상위의 `IP` 주소를 식별할 수 있다.
- `SSH`나 `RDP` 프로토콜에 관하여 지표 필터를 설정할 수 있다. 평소보다 `SSH`나 `RDP` 프로토콜 요청이 너무 많은 경우 `CloudWatch` 경보를 트리거하여 `Amazon SNS` 주제로 경보를 전송할 수 있다. 네트워크에 수상한 행동이 발생했을 수 있기 때문이다.
- 흐름 로그를 `S3` 버킷에 전송 및 저장해서 `Amazon Athena`를 이용하여 `SQL`로 `VPC` 흐름 로그를 분석할 수 있다. 흐름 로그에 대한 광범위한 분석이 필요할 때에는 `S3`가 좋은 선택이다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/93620f10-3aec-447f-8a1b-ff435141592c)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---