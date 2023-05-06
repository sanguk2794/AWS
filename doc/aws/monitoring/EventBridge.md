## Amazon EventBridge
### 1. Amazon EventBridge
- Schedule: Cron jobs (scheduled scripts)  
→ 클라우드에서 `CRON` 작업을 예약할 수 있다. 예를 들어 스크립트를 예약할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/236635752-e98d4b4e-44a0-4789-9fc6-d3cf4e0d5fe2.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Event Pattern: Event rules to react to a service doing something  
→ 이벤트 패턴에 반응할 수 있다. 특정 작업을 수행하는 서비스에 반응하는 것이다.

![image](https://user-images.githubusercontent.com/97398071/236635776-d4094f4d-dbb9-4950-921b-6821c895e49c.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Trigger Lambda functions, send SQS/SNS messages…  
→ 대상이 다양하다면 `SQS`, `SNS`를 활용해 메세지를 보내도록 구현할 수 있다.

- `Amazon EventBridge`가 중앙에 위치하고, `Amazon EventBridge`로 전송할 다양한 소스가 있다. 소스로부터의 이벤트에는 필터링이 가능하다.
~~~
- 시작, 중단, 종료 - EC2 Instance
- 빌드 실패 - CodeBuild
- 객체 업로드 이벤트 발생 - S3 Event
- 보안 문제 - Trusted Advisor
- AWS 계정에서 생성된 모든 API 호출을 가로챌 때 사용 - CloudTrail
- 일정 시간마다 스케줄링 - CRON
~~~

- `Amazon EventBridge`를 통해 할 수 있는 일은 다음과 같다.
~~~
- Lambda 함수를 트리거하는 일정을 예약
- AWS Batch에서 배치 작업을 예약
- Amazon ECS에서 ECS 작업을 실행
- SQS, SNS나 Kinesis Data Streams로 메시지 송신
- 단계 함수를 실행
- CodePipeline으로 CI/CD 파이프라인을 시작
- CodeBuild에서 빌드를 실행
~~~

![image](https://user-images.githubusercontent.com/97398071/236635866-f7a7bb6a-aafc-4452-b9b9-26e04431bf3e.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- `Amazon EventBridge`는 이벤트를 기본 이벤트 버스로 전송하는 `AWS` 서비스를 말한다. 이벤트 버스는 이벤트를 수신하는 파이프라인이다.

- Event buses can be accessed by other AWS accounts using Resource-based Policies  
→ 리소스 기반 정책을 사용해 다른 계정의 이벤트 버스에 액세스할 수 있다.

- You can archive events (all/filter) sent to an event bus (indefinitely or set period)  
→ 이벤트 아카이빙이 가능하다. 이벤트를 아카이빙할 때는 보존 기간을 무기한 또는 일정 기간으로 설정할 수 있다. 

- Ability to replay archived events  
→ 아카이브된 이벤트를 재생할 수 있다. 디버깅에 아주 유용하다.

### 2. Amazon EventBridge – Schema Registry
- `EventBridge`는 여러 곳에서 이벤트를 받기 때문에 이벤트가 어떻게 생겼는지 파악해야 한다. 스키마는 전송되는 이벤트의 구조를 정의한다. 

- 스키마 레지스트리에서 스키마를 확인할 수 있다.

- EventBridge can analyze the events in your bus and infer the schema  
→ `Amazon EventBridge`는 버스의 이벤트를 분석하고 스키마를 추론하는 능력이 있다.

- The Schema Registry allows you to generate code for your application, that will know in advance how data is structured in the event bus  
→ `Schema Registry`의 스키마를 사용하면 애플리케이션의 코드 생성할 수 있고 이벤트 버스의 데이터가 어떻게 정형화되는지 알 수 있다.

- Schema can be versioned  
→ 스키마의 버저닝이 가능하다.

### 3. Amazon EventBridge – Resource-based Policy
- 설정하지 않으면 이벤트 버스의 소유자만 이벤트 버스에 이벤트를 전송할 수 있다.

- Manage permissions for a specific Event Bus  
→ 특정 이벤트 버스의 권한을 관리할 수 있다.

- Example: allow/deny events from another AWS account or AWS region  
→ 예를 들면 특정 이벤트 버스가 다른 리전이나 다른 계정의 이벤트를 허용하거나 거부하도록 설정할 수 있다.

- Use case: aggregate all events from your AWS Organization in a single AWS account or AWS region  
→ 여러 계정의 모음인 `AWS Organizations`의 중앙에 이벤트 버스를 두고 모든 이벤트를 수신할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/236636546-02c1107b-b652-4ac6-9b0a-8e7f0a82d4a2.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---