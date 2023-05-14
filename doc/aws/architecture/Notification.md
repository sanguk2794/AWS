## Event Process
### 1. Lambda, SNS & SQS

- `SQS` + `Lambda`
~~~
- SQS와 Lambda를 사용해서 이벤트 처리를 할 수 있다.
- 이벤트가 SQS 대기열에 삽입되고 Lambda 서비스가 SQS 대기열을 폴링한다. 문제가 발생하면 해당 메시지를 다시 SQS 대기열에 입력하고 폴링을 재시도한다.
- 다섯 번의 재시도 후 메세지에 중대한 문제가 있다고 판단하고 DLQ, 데드 레터 대기열로 보내도록 SQS를 설정할 수 있다.
~~~

- `SQS FIFO` + `Lambda`
~~~
- `FIFO`이므로 한 메시지를 처리하지 못 하면 차단이 발생하여 처리가 끝나지 않고 결국 전체 대기열 처리가 차단된다.
- 다섯 번의 재시도 후 메세지에 중대한 문제가 있다고 판단하고 DLQ, 데드 레터 대기열로 보내도록 SQS를 설정할 수 있다.
~~~

- `SNS` + `Lambda`
~~~
- 메시지는 비동기식으로 Lambda에 전송된다.
- 이 구조에서는 처리하지 못하는 메시지가 발생하더라도 람다 함수 내부적으로만 재시도를 수행한다.
- 재시도는 총 세 번까지 하며 메시지가 제대로 처리되지 않으면 해당 메시지를 제거하거나 DLQ로 보내도록 구성할 수 있지만, 기본적으로 SQS를 활용하는 것이 추천된다.
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/8c6e3f49-8e8e-426c-8b2f-7a5f0c87fd8c)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- `S3` 버킷으로 업로드되는 객체를 처리하는 서버리스 애플리케이션으로 작업하고 있습니다. `S3` 버킷에 `S3` 이벤트를 구성해 객체가 업로드될 때마다 `Lambda` 함수가 호출되도록 만들었습니다. 프로세싱되지 않는 이벤트는 추후 처리를 위해 데드 레터 대기열(`DLQ`)로 전송시키려 합니다. 이런 경우, 다음 중 어떤 `AWS` 서비스를 선택해야 할까요?
→ `Lambda`

### 2. Fan Out Pattern: deliver to multiple SQS
- 팬아웃 패턴은 다중 `SQS` 대기열에 데이터를 전송하는 방식이다.

- `SDK`와 다중 `SQS`를 연결하는 것은 안정성이 떨어진다. 2번째 `SQS` 대기열에 메세지를 전송한 후 애플리케이션에 문제가 발생했다면 3번째 `SQS` 대기열에는 메세지가 제대로 전송되지 않는다.

- `SDK`와 다중 `SQS` 대기열 사이에 `SNS`를 둠으로써 안정성을 높일 수 있다.
→ `SQS` 대기열은 `SNS` 주제의 구독자가 되고 `SNS` 주제에 메시지를 전송할 때마다 메시지가 모든 `SQS` 대기열에 전달되기 때문이다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/4da26b3e-38d1-4479-9fa0-5dfefd1aba9f)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. S3 Event Notifications
- S3:ObjectCreated, S3:ObjectRemoved, S3:ObjectRestore, S3:Replication…  
→ Amazon S3 버킷이 특정 이벤트에만 반응하도록 설정할 수 있다. 객체 생성, 삭제 복원, 복제본 생성 등 이벤트를 선택해서 알림을 보낼 수 있다.

- Object name filtering possible (*.jpg)  
→ 객체 이름별로 필터링이 가능하다.

- Use case: generate thumbnails of images uploaded to S3  
→ 사용 사례로는 `Amazon S3`에 업로드된 이미지의 섬네일을 생성하는 경우가 있다.

- Can create as many “S3 events” as desired  
→ 이때 `S3` 이벤트는 원하는 만큼 생성할 수 있다.

- S3 event notifications typically deliver events in seconds but can sometimes take a minute or longer  
→ 이벤트 알림은 보통 수초 내로 전송된다. 하지만, 가끔 몇 분 이상이 소요될 수도 있다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/f0e46763-82ff-4166-9e78-7888e49171ce)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 1. S3 Event Notifications with Amazon EventBridge
- `Amazon S3` 버킷에서 일어난 모든 이벤트를 `Amazon EventBridge`로 전송하는 방식이다. 규칙을 설정하여 18개 이상의 `AWS` 서비스로 전달할 수 있다.

- Advanced filtering options with JSON rules (metadata, object size, name...)  
→ `JSON` 규칙의 고급 필터링 옵션을 사용하여 메타데이터, 객체 크기, 이름 등으로 필터링할 수 있다.

- Multiple Destinations – ex Step Functions, Kinesis Streams / Firehose…  
→ 또 여러 대상에 이벤트를 한 번에 보낼 수 있다. `Step Functions`, `Kinesis Streams / Firehose` 등이 대상이 될 수 있다.

- EventBridge Capabilities – Archive, Replay Events, Reliable delivery  
→ 아카이빙, 이벤트 재생, 그리고 안전한 이벤트 전송 등 추가 기능을 제공한다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/819c4f1e-f525-40fd-aa35-2538b3cf6f2e)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Amazon EventBridge – Intercept API Calls
~~~
- Amazon EventBridge에서 모든 API 호출을 인터셉트하고자 할 때에는 CloudTrail를 통합해 사용한다.
- 가령 사용자가 DynamoDB의 테이블을 삭제하고자 DeleteTable API을 호출하는 이벤트가 발생했다면 이 API 호출은 CloudTrail에 로그된다.
- 이 로그가 Amazon EventBridge의 이벤트를 트리거하도록 등록하면 경보를 생성하여 Amazon SNS에 전송할 수 있다.
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/1eee6041-95f6-4866-b39b-e1949d3c6cea)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 2. API Gateway – AWS Service Integration Kinesis Data Streams example
- `API Gateway`에 클라이언트가 요청을 보내면 `API Gateway`가 `Kinesis Data Stream`에 메시지를 전송한다.
- 해당 메세지는 `Kinesis Data Firehose`로 이동하고 최종적으로는 `Amazon S3`에 저장된다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/2cd662f8-08c2-4a8c-8e64-57f026b1ab5e)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---