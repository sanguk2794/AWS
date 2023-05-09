## AWS Macie
### 1. AWS Macie
- Amazon Macie is a fully managed data security and data privacy service that uses machine learning and pattern matching to discover and protect your sensitive data in AWS.  
→ `Macie`는 완전 관리형의 데이터 보안 및 데이터 프라이버시 서비스이다. 머신 러닝과 패턴 일치를 사용하여 `AWS`의 민감한 정보를 검색하고 보호한다.

- Macie helps identify and alert you to sensitive data, such as personally identifiable information (PII)  
→ 구체적으로 말하면 민감한 데이터를 경고한다. 민감한 데이터에는 `개인 식별 정보, PII`가 포함된다.

- 민감한 데이터를 경고하는 시나리오는 다음과 같다.
~~~
- PII 데이터가 S3 버킷에 존재한다고 가정한다. Macie는 S3 버킷 내의 데이터를 분석한다.
- 분석한 데이터를 토대로 PII로 분류되는 데이터를 검색해 CloudWatch 이벤트나 EventBridge로 검색 결과를 전송한다.
- 이후 CloudWatch 이벤트와 통합해서 SNS나 람다 함수를 실행시킬 수 있다.
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/32fc4979-e023-4be0-9d54-a7c20a737fcd)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---