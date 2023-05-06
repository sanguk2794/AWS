## AWS Config
### 1. AWS Config
- Helps with auditing and recording compliance of your AWS resources  
→ `AWS` 내 리소스에 대한 감사와 규정 준수 여부를 기록할 수 있게 해 주는 서비스이다.

- Helps record configurations and changes over time  
→ 설정된 규칙에 기반해 구성과 구성의 시간에 따른 변화를 기록할 수 있으며, 이를 통해 필요한 경우 인프라를 빠르게 롤백하고 문제점을 찾아낼 수 있다.

- Questions that can be solved by AWS Config:
~~~
- Is there unrestricted SSH access to my security groups?
→ 보안 그룹에 제한되지 않은 SSH 접근이 존재하는지

- Do my buckets have any public access?
→ 공용 액세스가 가능한 버킷이 존재하는지

- How has my ALB configuration changed over time?
→ 시간이 지나면서 변화한 ALB 구성이 있는지
~~~

- You can receive alerts (SNS notifications) for any changes  
→ 규칙의 규정 준수 여부와 관계없이 변화가 생길 때마다 SNS 알림을 받을 수 있다.

- AWS Config is a per-region service  
→ `Config`는 리전 스코프의 서비스이기 때문에 모든 리전별로 구성해야 한다.

- Can be aggregated across regions and accounts  
→ 데이터를 중앙화하기 위해 리전과 계정 간 데이터를 통합할 수 있다.

- Possibility of storing the configuration data into S3 (analyzed by Athena)  
→ 모든 리소스의 구성을 `S3`에 저장해 나중에 분석할 수 있다.

### 2. Config Rules
- Can use AWS managed config rules (over 75)  
→ `AWS managed Config`에는 총 75개의 규칙이 포함된다.

- Can make custom config rules (must be defined in AWS Lambda)
~~~
- 물론 커스텀 Config를 만드는 것도 가능하다. 이 때에는 람다를 이용해 스스로 규칙을 정의하게 된다.

- Ex: evaluate if each EBS disk is of type gp2
→ 각 EBS 디스크가 gp2 유형인지 평가

- Ex: evaluate if each EC2 instance is t2.micro
→ 개발 계정의 EC2 인스턴스가 t2.micro 유형인지 평가
~~~

- Rules can be evaluated / triggered:
~~~
- For each config change
→ 몇몇 규칙들은 구성이 변화할 때마다 평가되거나 트리거된다. 예를 들어 EBS 디스크에 새 구성이 생길 때마다 EBS 디스크의 유형을 평가하도록 할 수 있다.
~~~

- AWS Config Rules does not prevent actions from happening (no deny)  
→ `Config Rules`은 규정 준수를 위한 것이나 어떤 동작을 미리 예방하지는 못한다. 어떤 것도 차단할 수 없다. 하지만 구성의 개요와 리소스의 규정 준수 여부는 파악 가능하다.

- View compliance of a resource over time  
→ 리소스의 규정 준수 여부를 시간별로 확인할 수 있다.

- View configuration of a resource over time  
→ 리소스의 구성을 시간별로 확인할 수 있다. 이에는 누가, 언제 변경했는지 등이 포함된다.

- View CloudTrail API calls of a resource over time  
→ 이 정보를 `CloudTrail`과 연결해 리소스에 대한 `API` 호출을 확인할 수 있다. 그러므로 일어나는 일은 전부 파악할 수 있다.

### 3. Config Rules – Remediations
- Remediation - 복원, 교정 

- Automate remediation of non-compliant resources using SSM Automation Documents  
→ `Config` 내에서 행동을 차단할 수는 없지만 `SSM` 자동화 문서를 이용하면 규정을 준수하지 않는 리소스를 수정할 수 있다.

- IAM 액세스 키의 만료 여부를 모니터링한다고 가정할 때, 액세스 키의 만료를 통한 규칙 미준수는 예방할 수 없다. 하지만, 규정을 미준수할 때마다 작동하도록 트리거를 설정할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/236640986-8741aa9f-d042-4520-b68c-597f619ca49f.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Use AWS-Managed Automation Documents or create custom Automation Documents  
→ `AWS` 관리형 문서 또는 커스텀 자동화 문서를 만들어 규정을 준수하지 않는 리소스를 수정할 수 있다.
~~~
- Tip: you can create custom Automation Documents that invokes Lambda function
→ 만약 계속 스크립트를 하고 싶다면 람다 함수를 실행하는 문서를 생성해서 원하는 작업을 수행할 수 있다.
~~~

- You can set Remediation Retries if the resource is still non-compliant after auto- remediation  
→ 수정 작업은 재시도가 될 수 있다. 리소스를 자동 수정했음에도 여전히 규정을 미준수한다면 5번까지 재시도될 수 있다.

### 4. Config Rules – Notifications
- Use EventBridge to trigger notifications when AWS resources are non-compliant  
→ `EventBridge`를 사용해 리소스가 규정을 미준수했을 때마다 알림을 보낼 수 있다. 

![image](https://user-images.githubusercontent.com/97398071/236641011-7cd011ce-6c62-433f-ab6d-60ee1cf5248e.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Ability to send configuration changes and compliance state notifications to SNS (all events – use SNS Filtering or filter at client-side)  
→ 모든 구성 변경과 모든 리소스의 규정 준수 여부 알림을 `Config`에서 `SNS`로 보낼 수도 있다.

![image](https://user-images.githubusercontent.com/97398071/236641027-4d76ee76-0505-452b-b5d0-2ae9d40c509d.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---