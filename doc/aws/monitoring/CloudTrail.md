## CloudTrail
### 1. AWS CloudTrail
- Provides governance, compliance and audit for your AWS Account  
→ `CloudTrail`는 `AWS` 계정의 거버넌스, 감사 및 규정 준수를 도와준다.

- CloudTrail is enabled by default!  
→ `CloudTrail`는 기본적으로 활성화 상태이다.

- 기본적으로 `CloudTrail` 이벤트 로그 파일은 `Amazon S3` 서버 측 암호화를 사용해 암호화된다.

- Get an history of events / API calls made within your AWS Account by:
~~~
- Console
- SDK
- CLI
- AWS Services
→ 콘솔, SDK, CLI 뿐만 아니라 기타 AWS 서비스에서 발생한 모든 이벤트 및 API 호출 기록을 확인할 수 있다.
~~~

- Can put logs from CloudTrail into CloudWatch Logs or S3  
→ `CloudTrail`의 로그를 `Amazon S3`로 옮길 수 있다.

- A trail can be applied to All Regions (default) or a single Region.  
→ 전체 또는 단일 리전에 적용되는 트레일을 생성해 모든 리전에 걸친 이벤트 기록을 한 곳으로 모을 수 있다.

- If a resource is deleted in AWS, investigate CloudTrail first!  
→ `CloudTrail`을 사용하면 누군가가 `AWS`에서 무언가를 삭제했을 때 대처할 수 있다. 예를 들어 `EC2` 인스턴스가 종료되었을 경우 누가 했는지 알고 싶다면 `CloudTrail`을 확인하면 된다. `CloudTrail`에 해당 `API` 호출이 남아 있으므로 누가 언제 문제를 일으켰는지 알아낼 수 있는 것이다.

- 모든 이벤트를 90일 이상 보관하려면 `CloudWatch Logs` 또는 `S3` 버킷으로 전송하면 된다.

![image](https://user-images.githubusercontent.com/97398071/236638712-82a4c7df-ab4d-447b-8741-9961bd34e325.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. CloudTrail Events
- `CloudTrail`에는 세 종류의 이벤트가 있다.

#### 1. Management Events
- Operations that are performed on resources in your AWS account  
→ `AWS` 계정의 리소스에서 수행되는 작업이다. 인스턴스를 제거하거나 새 `IAM` 역할을 생성하는 것 등이 포함된다.

- Examples:
~~~
- Configuring security (IAM AttachRolePolicy)
→ 보안 설정을 구성하면 IAM AttachRolePolicy라는 API를 사용하게 된다. 이 것이 CloudTrail에 표시된다.

- Configuring rules for routing data (Amazon EC2 CreateSubnet)
→ 서브넷을 생성하면 표시된다.

- Setting up logging (AWS CloudTrail CreateTrail)
→ 로깅을 설정해도 표시된다.
~~~

- By default, trails are configured to log management events.  
→ `CloudTrail`은 기본적으로 관리 이벤트를 기록하도록 되어 있다.

- Can separate Read Events (that don’t modify resources) from Write Events (that may modify resources)  
→ 관리 이벤트는 두 종류로 구분할 수 있다. 리소스를 수정하지 않는 읽기 이벤트와 리소스를 수정할 수 있는 쓰기 이벤트이다. 쓰기 이벤트의 중요도가 높은데, 그 이유는 `AWS` 인프라를 망칠 수 있기 때문이다.

#### 2. Data Events
- 데이터 이벤트는 `S3` 버킷이나 `Lambda` 함수에서 일어나는 이벤트이다.

- By default, data events are not logged (because high volume operations)  
→ 데이터 이벤트는 고볼륨 작업이므로 기본적으로 로깅되지 않는다.

- Amazon S3 object-level activity (ex: GetObject, DeleteObject, PutObject): can separate Read and Write Events  
→ `GetObject`, `DeleteObject`, `PutObject`와 같은 `Amazon S3`의 객체 수준 작업은 S3 버킷에서 빈번히 발생하는 이벤트이므로 기본적으로 로깅되지 않는다.

- AWS Lambda function execution activity (the Invoke API)  
→ `Invoke API`를 사용할 때마다 `Lambda` 함수가 몇 번 활용되었는지 확인할 수 있다. 이 역시 함수가 많이 실행되면 볼륨이 커질 수 있다.

#### 3. CloudTrail Insights Events
- Enable CloudTrail Insights to detect unusual activity in your account:
~~~
- 모든 유형의 서비스에 걸쳐 너무 많은 관리 이벤트가 있고 계정에서 다수의 API가 매우 빠르게 발생해서 무엇이 이상하거나 특이한지 파악하기 어려울 때 사용한다. 
→ 비용을 지불하고 CloudTrail Insights를 활성화하면 이벤트를 분석해서 계정 내의 특이한 활동을 탐지해준다.

- inaccurate resource provisioning
→ 부정확한 리소스 프로비저닝

- hitting service limits
→ 서비스 한도 도달

- Bursts of AWS IAM actions
→ AWS IAM 작업 버스트

- Gaps in periodic maintenance activity
→ 주기적 작업 관리의 부재
~~~

- CloudTrail Insights analyzes normal management events to create a baseline  
→ `CloudTrail`이 정상적인 관리 활동을 분석해서 기준선을 생성한 다음 무언가를 변경하거나 변경하려고 시도하는 모든 쓰기 유형의 이벤트를 지속적으로 분석하여 특이한 패턴을 탐지한다.

- 관리 이벤트를 지속적으로 분석해서 무언가를 탐지하면 인사이트 이벤트를 생성한다. 그리고 이 인사이드 이벤트는 `CloudTrail` 콘솔에 표시된다. 이를 원한다면 `Amazon S3`에 전송할 수도 있다.
- 변경 사항을 추적하고 리소스에 발생한 내역을 저장할 수 있지만 엄격한 규정 준수를 시행해야 할 때는 적절하지 않다.

![image](https://user-images.githubusercontent.com/97398071/236638776-92269a8d-79a8-4382-8bc9-28c19c39d344.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. CloudTrail Events Retention
- Events are stored for 90 days in CloudTrail  
→ 이벤트는 `CloudTrail` 내부에 90일 간 저장되고 이후에는 삭제된다.

- To keep events beyond this period, log them to S3 and use Athena  
→ 기본 시간 이상으로 이벤트를 보존하려면 `S3`로 전송해서 `S3`에 로그를 기록하고 `Athena`를 사용해 분석하면 된다.

![image](https://user-images.githubusercontent.com/97398071/236638828-2b2908ec-0564-47b6-b83b-a24887a8a269.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 4. Amazon EventBridge – Intercept API Calls
- `CloudTrail` 통합 중에 `API` 호출을 가로채는 `Amazon EventBridge`와의 통합은 중요하다.

- 사용자가 테이블 삭제 `API` 호출을 사용해서 `DynamoDB`의 테이블을 삭제할 때마다 `SNS` 알림을 받고 싶다고 가정한다.
~~~
- AWS에서 API 호출을 실행할 때마다 API 호출 자체가 CloudTrail에 로깅된다.
- 그리고 모든 API 호출은 Amazon EventBridge에 이벤트로 기록된다.
- 여기서 특정 테이블 삭제 API 호출을 찾아 규칙을 생성한다.
- 규칙의 대상으로 SNS를 설정해 메세지를 송신할 수 있다.
~~~

![image](https://user-images.githubusercontent.com/97398071/236638855-5857c91d-23bf-436d-8cdc-9c7fea6f5d94.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---