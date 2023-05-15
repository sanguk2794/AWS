## Amazon SNS
### 1. Amazon SNS
- SNS = Simple Notification Service

- What if you want to send one message to many receivers?  
→ 메세지 하나를 여러 수신자에게 보낸다고 가정할 때
~~~
- Direct integration
→ 직접 통합, 서비스가 생성될 때마다 통합을 생성하고 작성해야하므로 불편할 수 있다.

- Pub / Sub
→ 게시, 구독을 통해 구매 서비스가 SNS 주제를 게시할 수 있다.
~~~

![image](https://user-images.githubusercontent.com/97398071/235562388-f7259822-2508-4cb0-907b-68fd488906b6.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- The “event producer” only sends message to one SNS topic  
→ 이벤트 생산자는 한 `SNS` 주제에 대해서만 주제를 생성한다.

- As many “event receivers” (subscriptions) as we want to listen to the SNS topic notifications  
→ 수신자는 해당 주제와 관련되는 `SNS` 알림을 받으려는 구독자들로 제한된다.

- Each subscriber to the topic will get all the messages (note: new feature to filter messages)  
→ 구독자는 해당 주제로 전송된 메세지를 모두 받는다.

- Up to 12,500,000 subscriptions per topic  
→ 주제별 최대 12,500,000 명의 구독자가 존재할 수 있다.

- 100,000 topics limit  
→ 계정당 100,000 개의 주제를 가질 수 있다.

- Emails, SMS & Mobile Notification, HTTP(S) Endpoints, SQS, Lambda, Kinesis Data Firehose... 

![image](https://user-images.githubusercontent.com/97398071/235562492-18bbc849-0972-456d-a1a1-61b754f9b834.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Many AWS services can send data directly to SNS for notifications  
→ `SNS`는 많은 아마존 서비스에서 데이터를 수신하기도 한다.

- CloudWatch Alarms, ASG, CloudFormation, AWS Budgets, S3 Bucket, AWS DMS, Lambda, DynamoDB, RDS Events...  
→ 기억하지 않아도 된다.

![image](https://user-images.githubusercontent.com/97398071/235563062-daf70215-7c89-45d0-a100-b9b821d2bd93.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. Amazon SNS – How to publish
- Topic Publish (using the SDK) 
~~~
- Create a topic 
→ 메세지를 개시하기 위해서 먼저 주제를 생성한다.

- Create a subscription (or many) 
→ 하나 또는 여러 개의 구독을 생성한다.

- Publish to the top
→ SNS 주제에 게시한다.
~~~

- Direct Publish (for mobile apps SDK) 
~~~
- Create a platform application 
→ 플랫폼 애플리케이션을 생성한다.

- Create a platform endpoint 
→ 플랫폼 엔드 포인트를 생성한다.

- Publish to the platform endpoint 
→ 플랫폼 엔드 포인트에 게시한다.

- Works with Google GCM, Apple APNS, Amazon ADM…
→ 수신 대상은 Google GCM, Apple APNS, Amazon ADM등이 있다.
~~~

### 3. Amazon SNS – Security
- 보안 측면에서 보면 `SQS`와 거의 동일하다.

- Encryption:
~~~
- In-flight encryption using HTTPS API
→ 전송 중 암호화를 지원한다.

- At-rest encryption using KMS keys
→ KMS 키를 사용한 저장 데이터 암호화를 지원한다.
 
- Client-side encryption if the client wants to perform encryption/decryption itself
→ 클라이언트가 SNS에 암호화된 메시지를 보내려는 경우를 위한 클라이언트 측 암호화가 있다. 이 때의 암호화는 클라이언트 몫이다.
~~~

- Access Controls: IAM policies to regulate access to the SNS API  
→ `IAM` 정책이 중심이 된다. 모든 `SNS API`가 `IAM` 정책으로 규제되기 때문이다.

- SNS Access Policies (similar to S3 bucket policies)  
→ `S3` 버킷 정책과 매우 흡사하다.
~~~
- Useful for cross-account access to SNS topics
→ SNS 주제에 대해 교차 계정 액세스 권한을 가질 수 있다.

- Useful for allowing other services ( S3…) to write to an SNS topic
→ S3 이벤트 같은 서비스가 SNS 주제를 작성할 수 있도록 허용할 때 매우 유용하다.
~~~

### 4. SNS + SQS: Fan Out
- SNS + SQS 팬아웃 패턴이라고 한다.  
→ 메세지를 여러 SQS 대기열로 보내고 싶을 때 모든 SQS 대기열에 개별적으로 메세지를 보내면 문제가 발생할 수 있다. 예를 들면 애플리케이션이 중간에 비정상적으로 종료될 수 있다.

- Push once in SNS, receive in all SQS queues that are subscribers  
→ 원하는 만큼의 `SQS` 대기열에 `SNS` 주제를 구독하게 하는 방식이다. 이 대기열들은 구독자로써 `SNS`로 들어오는 모든 메세지를 송신할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/235564703-43294b4a-2427-4446-8f49-763970aa573d.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Fully decoupled, no data loss  
→ 완전히 분리된 모델이기에 데이터 손실을 걱정하지 않아도 괜찮다.

- SQS allows for: data persistence, delayed processing and retries of work  
→ `SQS`로 작업을 다시 시도할 수 있다.

- Make sure your SQS queue access policy allows for SNS to write  
→ `SQS` 액세스 정책에서 `SNS` 주제가 `SQS` 쓰기 작업을 할 수 있도록 허용해야 한다.

- Cross-Region Delivery: works with SQS Queues in other regions  
→ 리전 간 전달도 가능하다. 즉 보안상 허용된다면 한 리전의 `SNS` 주제에서 다른 리전의 `SQS` 대기열로 메세지를 전송할 수 있다.

### 5. Application: S3 Events to multiple queues
- If you want to send the same S3 event to many SQS queues, use fan-out  
→ 여러 대기열에 동일한 `S3` 알림을 보내고 싶을 때 팬아웃 패턴을 사용해야 한다.

![image](https://user-images.githubusercontent.com/97398071/235565097-a68f5e1d-8fcf-4bc5-9526-4421724af1e6.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 6. Application: SNS to Amazon S3 through Kinesis Data Firehose
- SNS can send to Kinesis and therefore we can have the following solutions architecture:  
→ 다른 방법으로 `Kinesis Data Firehose, KDF`를 통해 `SNS`에서 `Amazon S3`로 데이터를 전송할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/235565549-303e44ad-7f3c-4d2b-bf51-f2f5e12a84bf.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 7. Amazon SNS – FIFO Topic
- FIFO = First In First Out (ordering of messages in the topic)  
→ `SNS`에는 주제의 메세지 순서를 지정하는 `FIFO` 기능이 있다.

- Similar features as SQS FIFO:
~~~
- Ordering by Message Group ID (all messages in the same group are ordered)
→ SQS의 FIFO와 유사하다. 메세지 그룹 ID에 따라 순서를 매긴다.

- Deduplication using a Deduplication ID or Content Based Deduplication
→ 중복 제거 ID를 활용하거나 내용을 비교하여 중복 데이터를 제거한다.
~~~

- Can only have SQS FIFO queues as subscribers  
→ `SQS FIFO` 대기열을 `FIFO SNS` 주제의 구독자로 설정한다.

- Limited throughput (same throughput as SQS FIFO)  
→ 처리량은 제한적이며 `SQS FIFO` 대기열과 동일하다.

![image](https://user-images.githubusercontent.com/97398071/235566218-d2d9e554-d74d-4ce5-b42c-ed6d412440e5.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)
 
- In case you need fan out + ordering + deduplication  
→ `FIFO`에서 펜아웃 패턴을 사용하기 위해서는 순서, 중복 제거가 필요하다.

![image](https://user-images.githubusercontent.com/97398071/235566381-78311fda-39ab-42ea-8e28-469c056236f7.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 8. SNS – Message Filtering
- 팬아웃 패턴과 관련하여 정말 편리한 `SNS`의 기능 중 하나는 `SNS`에서 메세지 필터링이 가능하다는 것이다.

- JSON policy used to filter messages sent to SNS topic’s subscriptions  
→ 이는 `SNS` 주제를 구독할 때 전송되는 메세지를 필터링하는데 사용되는 `JSON` 정책이다.

- If a subscription doesn’t have a filter policy, it receives every message  
→ 구독에 어떤 필터링 정책도 없다면 모든 메세지를 받아들이며, 이것이 기본 동작이다.

![image](https://user-images.githubusercontent.com/97398071/235567051-5ee0a72e-66f3-424e-9f1a-18f594f73061.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---