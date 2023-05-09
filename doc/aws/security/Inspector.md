## Amazon Inspector
### 1. Amazon Inspector
- Automated Security Assessments  
→ `Amazon Inspector`는 자동화된 보안 평가를 실행할 수 있는 서비스이다. 

- For EC2 instances
~~~
- Leveraging the AWS System Manager (SSM) agent
→ System Manager (SSM) agent를 활용하면 Amazon Inspector가 해당 EC2 인스턴스의 보안을 평가하기 시작한다.

- Analyze against unintended network accessibility
→ 의도되지 않은 네트워크 접근 가능성에 대해 분석한다.

- Analyze the running OS against known vulnerabilities
→ 실행 중인 운영 체제에서 알려진 취약점을 분석한다.
~~~

- For Container Images push to Amazon ECR
~~~
- Assessment of Container Images as they are pushed
→ Amazon Inspector는 도커 이미지 등 컨테이너 이미지를 Amazon ECR로 푸시할 때 실행된다. 컨테이너 이미지가 Amazon ECR로 푸시되면 Amazon Inspector가 알려진 취약점에 대해 검사한다.
~~~

- For Lambda Functions
~~~
- Identifies software vulnerabilities in function code and package dependencies
→ Amazon Inspector는 Lambda 함수가 배포될 때 함수 코드 및 패키지 종속성에서 소프트웨어 취약성을 분석한다.

- Assessment of functions as they are deployed
→ 함수가 배포될 때 평가된다.
~~~

- Reporting & integration with AWS Security Hub  
→ `Amazon Inspector`가 작업을 완료하면 결과를 `AWS` 보안 허브에 보고한다.

- Send findings to Amazon Event Bridge  
→ 또, 이러한 결과 및 결과 이벤트를 `Amazon Event Bridge`로 전송한다. 이를 통해 인프라에 있는 취약점을 모아서 확인할 수 있다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/437a6804-f3ed-4071-8ce4-6a4dc718f4ef)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. What does Amazon Inspector evaluate?
- `Amazon Inspector`가 평가하는 요소는 다음과 같다.

- Remember: only for EC2 instances, Container Images & Lambda functions  
→ `Amazon Inspector`는 실행 중인 `EC2` 인스턴스, `Amazon ECR`의 컨테이너 이미지, `Lambda` 함수에만 사용된다는 점을 기억해야 한다.

- Continuous scanning of the infrastructure, only when needed  
→ 필요한 경우에만 인프라를 지속적으로 스캔한다.

- Package vulnerabilities (EC2, ECR & Lambda) – database of CVE  
→ `EC2`, `ECR`, `Lambda`의 패키지 취약성을 평가한다.

- Network reachability (EC2)  
→ `EC2`의 네트워크 도달성을 평가한다.

- A risk score is associated with all vulnerabilities for prioritization  
→ 우선 순위 지정을 위한 위험 점수는 실행될 때마다 모든 취약성과 연결된다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---