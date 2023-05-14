## Amazon GuardDuty
### 1. Amazon GuardDuty
- Intelligent Threat discovery to protect your AWS Account  
→ `Amazon GuardDuty`는 `AWS` 계정을 보호하는 지능형 위협 탐지 서비스이다.

- Uses Machine Learning algorithms, anomaly detection, 3rd party data  
→ 머신 러닝 알고리즘을 사용하여 이상 탐지하고, 타사 데이터를 이용해 계정에 대한 공격을 탐지한다.

- One click to enable (30 days trial), no need to install software  
→ 클릭 한 번으로 활성화가 가능하다. 소프트웨어를 설치할 필요 없이 백엔드에서 작동된다.

- Input data includes:
~~~
- Amazon GuardDuty는 여러 소스에서 입력 데이터를 얻는다. 소스를 외워야 한다.
- CloudTrail Events Logs – unusual API calls, unauthorized deployments
- CloudTrail Management Events – create VPC subnet, create trail, …
- CloudTrail S3 Data Events – get object, list objects, delete object, …
→ CloudTrail Events Logs 로그의 입력 데이터로 비정상적인 API의 호출과 무단 배포등을 탐지한다.

- VPC Flow Logs – unusual internal traffic, unusual IP address
→ VPC Flow Logs를 통해 비정상적인 인터넷 트래픽과 IP 주소를 찾는다.

- DNS Logs – compromised EC2 instances sending encoded data within DNS queries
→ DNS Logs를 통해 DNS 쿼리에서 인코딩된 데이터를 전송할 EC2 인스턴스가 손상되었는지 확인한다.

- Kubernetes Audit Logs – suspicious activities and potential EKS cluster compromises
→ Kubernetes 감사 로그를 확인하여 의심스러운 활동 및 잠재적인 EKS 클러스터 손상을 감지한다.
~~~

- Can setup CloudWatch Event rules to be notified in case of findings  
→ `CloudWatch` 이벤트 규칙을 설정하여 탐색 결과에 대한 알림을 받을 수 있다.

- CloudWatch Event rules can target AWS Lambda or SNS  
→ `CloudWatch` 이벤트 규칙은 `AWS Lambda` 또는 `SNS`를 대상으로 지정 가능하다.

- Can protect against CryptoCurrency attacks (has a dedicated “finding” for it)  
→ `GuardDuty`로 암호화폐 공격을 보호할 수 있다. 관련된 전용 탐지가 있기 때문이다. 시험에 나올 수 있는 내용이다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/a9e945a4-78f6-4fc7-ba35-64740fcacdd9)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 요약하면 `GuardDuty`는 `VPC Flow Logs`, `CloudTrail Events Logs`, `DNS Logs`, `Kubernetes Audit Logs`를 모두 `GuardDuty`로 불러온다. 그리고 `CloudWatch` 이벤트 규칙 덕분에 `Lambda` 함수나 `SNS` 주제 알림을 받을 수 있다.

- 한 금융 서비스 회사에서 자사의 `Amazon S3` 버킷에 저장된 민감 데이터를 식별하려고 합니다. 또한 회사는 `S3`에 저장된 모든 데이터를 모니터링하여 악의적인 시도로부터 보호하고자 합니다. 솔루션 아키텍트의 관점에서 다음 중 주어진 요구 사항을 해결하는 데 도움이 될 만한 솔루션은 무엇입니까?  
→ `Amazon GuardDuty`를 사용해 S3에 저장된 데이터에 대한 악의적인 시도를 모니터링하고, `Amazon Macie`를 사용해 S3에 저장된 민감 데이터를 식별한다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---