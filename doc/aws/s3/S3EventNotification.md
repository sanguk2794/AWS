## S3 Event Notifications
### 1. S3 Event Notifications
- S3:ObjectCreated, S3:ObjectRemoved, S3:ObjectRestore, S3:Replication…  
→ `S3`에서는 이벤트가 발생한다. 이 이벤트란 객체의 생성, 삭제, 복원, 복제 중 하나의 사건이 발생한 상황을 의미한다.

- Use case: generate thumbnails of images uploaded to S3  
→ `Amazon S3`에서 발생하는 특정 이벤트에 자동으로 반응하게 할 수 있다. 사진의 섬네일을 생성하고자 한다면 이에 대한 알람을 만들어서 몇 가지 수신지로 보낼 수 있다.

- Can create as many “S3 events” as desired  
→ `S3` 이벤트를 생성하고 원하는 대상으로 보낼 수 있다. 대상에는 `SNS`, `SQS`, `Lambda Function`, `EventBridge`가 포함된다.

- S3 event notifications typically deliver events in seconds but can sometimes take a minute or longer  
→ 이벤트가 대상에 전달되는 시간은 일반적으로 몇 초밖에 안 걸리지만 가끔은 1분 이상 걸릴 수도 있다.

![image](https://user-images.githubusercontent.com/97398071/235161881-2c241801-bd6c-46f9-b784-df173bea87ab.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- `Properties` → `Event notifications`에서 설정 가능하다. 이벤트 알림을 생성하거나 `Amazon EventBridge` 통합을 활성화할 수 있다.

- 이 때 `S3`가 대상에 메시지를 게시할 수 있도록 권한을 제공해야 한다.  
→ `SQS`에 이벤트 알림을 보낸다고 가정할 때, 생성한 `SQS`의 액세스 정책을 `S3`에서의 접근을 허용하도록 변경해야 한다.

#### 1. S3 Event Notifications with Amazon EventBridge
- 이벤트 알람이 새로운 기능으로 `Amazon EventBridge`와 통합되었다. 이벤트 종류와 상관없이 모든 이벤트는 `Amazon EventBridge`로 모이게 된다.
- `Amazon EventBridge`에서 설정한 규칙을 통해 18개가 넘는 `AWS` 서비스에 이벤트 알림을 보낼 수 있다. `S3 이벤트 알림` 기능을 크게 향상시켜 준다.

- Advanced filtering options with JSON rules (metadata, object size, name...)  
→ `Amazon EventBridge`가 있으면 고급 필터링 옵션을 이전보다 훨씬 더 많이 사용할 수 있다.

- Multiple Destinations – ex Step Functions, Kinesis Streams / Firehose…  
→ 동시에 여러 수신지로 전송할 수 있다.

- EventBridge Capabilities – Archive, Replay Events, Reliable delivery  
→ `Amazon EventBridge`에서 제공하는 기능을 사용할 수도 있다. 이벤트를 보관하거나 재생할 수 있고 보다 안정적으로 전송할 수 있다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---