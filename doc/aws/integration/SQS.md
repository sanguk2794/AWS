## SQS
### 1. Amazon SQS - What’s a queue?
- `SQS`를 활용하는 문제는 매우 자주 등장한다.
 
- `SQS, Simple Queue Service`의 핵심은 대기열이다. 대기열에는 메세지가 포함된다.

- `SQS`의 대기열에 메세지를 보내는 주체를 `생산자, Producer`라고 한다. 생산자는 한 개일 수도, 그 이상일수도 있다.

- `SQS`의 대기열의 메세지를 수신하는 대상을 `소비자, Consumer`라고 한다. 소비자는 대기열에서 메세지를 폴링하는데, 이는 소비자 앞으로 온 메세지가 있는지를 물어보는 것이다.

- 즉, `SQS`는 생산자와 소비자를 분리하는 버퍼 역할을 수행한다. 

- 분리나 급격히 증가한 로드 혹은 시간초과 문제를 해결하기 위한 신속한 스케일링이 필요하다면 `SQS`를 사용해야 한다.

![image](https://user-images.githubusercontent.com/97398071/235358781-1b546fec-9916-4bb3-9107-33e72a8c198d.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- `SQS`의 유형에는 `Standard`와 `FIFO`가 있다.

- `Amazon SQS` → `Queues` → `Create queue`에서 `SQS`를 생성할 수 있다.

### 2. Amazon SQS – Standard Queue
- Oldest offering (over 10 years old)  
→ `SQS`는 가장 오래된 서비스 중 하나이다.

- Fully managed service, used to decouple applications  
→ 완전 관리형 서비스이며 애플리케이션을 분리하기 위해 사용한다. 시험에서 `분리, decouple`을 확인하면 `SQS`라고 생각하면 된다.

- Attributes:
~~~
- Unlimited throughput, unlimited number of messages in queue
→ 처리량이 무한하다. 원하는 만큼의 메세지를 보낼 수 있고, 이를 받는 대기열은 메세지 수의 제한이 없다.
 
- Default retention of messages: 4 days, maximum of 14 days
→ 각 메세지는 수명이 짧다. 기본값으로 4일 동안 대기열에 남아있고, 대기열에 있을 수 있는 최대 시간은 14일이다.

- Low latency (<10 ms on publish and receive)
→ 지연 시간이 짧다.

- Limitation of 256KB per message sent
→ SQS 메세지는 작아야 한다. SQS 메세지의 최대 사이즈는 256KB이다.
~~~

- Can have duplicate messages (at least once delivery, occasionally)  
→ 중복 메세지가 있을 수 있다. 같은 메세지가 두 번 전송되는 경우가 있으므로 주의가 필요하다. 이를 `적어도 한 번의 전송, at least once delivery`이라고 한다.

- `Purge queue`를 입력하면 대기열 안의 모든 메세지를 삭제할 수 있다.
→ 개발 환경에서는 굉장히 편리하다. 단, 프로덕션 환경에서는 절대로 사용해서는 안 된다.

### 3. SQS – Producing Messages
- Produced to SQS using the SDK (SendMessage API)  
→ 생산자는 `SDK`를 사용하여 `SQS`에 메세지를 보내야 한다. 이 `API`를 `SendMessage API`라고 한다.

- The message is persisted in SQS until a consumer deletes it  
→ 메세지가 작성되면 소비자는 해당 메세지를 읽고 삭제할 때까지 `SQS` 대기열에 유지된다. 메세지가 삭제되었다는 것은 메세지가 처리됐다는 뜻이다.

- Message retention: default 4 days, up to 14 days  
→ 기본값으로 4일 동안 대기열에 남아있고, 대기열에 있을 수 있는 최대 시간은 14일이다.

- Example: send an order to be processed
~~~
- Order id
- Customer id
- Any attributes you want
~~~

![image](https://user-images.githubusercontent.com/97398071/235359304-c9cea2c3-6c1d-44d3-b2b7-d78c31e35844.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- SQS standard: unlimited throughput  
→ `SQS`는 무제한 처리량을 자랑한다.

### 4. SQS – Consuming Messages
- Consumers (running on EC2 instances, servers, or AWS Lambda)…  
→ `EC2 instances`, 온프레미스 서버, `AWS Lambda` 등을 통해 실행할 수 있다.

- Poll SQS for messages (receive up to 10 messages at a time)  
→ 소비자는 메세지를 폴링한다. 자신 앞으로 온 메세지가 있는지 확인하는 과정이다.

- Process the messages (example: insert the message into an RDS database)  
→ 소비자는 메세지를 처리할 책임이 있다.

- Delete the messages using the DeleteMessage API  
→ 메세지를 삭제할 때 사용하는 `API`가 `DeleteMessage API`이다. 그러면 다른 소비자들은 이 메세지를 볼 수 없다.

![image](https://user-images.githubusercontent.com/97398071/235359328-0a5ea7c1-bb07-4437-913c-bb93ba4620fb.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 1. SQS – Multiple EC2 Instances Consumers
- Consumers receive and process messages in parallel  
→ 메세지를 동시에 수신하고 처리할 소비자를 여러 개 가질 수 있다.

![image](https://user-images.githubusercontent.com/97398071/235359627-5c7ac55b-8721-4c7e-96b8-28fcc21895ba.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- At least once delivery  
→ 만일 처리 속도가 너무 느리면 다른 소비자에게 메세지가 수신된다. 그래서 적어도 한 번은 전송이 된다고 하는 것이다.

- Best-effort message ordering  
→ 최선의 노력으로 메세지 순서를 지정하는 이유이다.

- Consumers delete messages after processing them  
→ 소비자는 메세지를 처리하면 메세지를 삭제해야한다. 그렇지 않으면 다른 소비자에게 이 메세지가 전송된다.

- We can scale consumers horizontally to improve throughput of processing  
→ 더 많은 메세지를 처리해야 한다면 소비자를 추가하고 수평 확장을 수행함으로써 처리량을 개선할 수 있다.

### 5. SQS with Auto Scaling Group (ASG)
- `SQS`의 대기열의 길이를 `ASG`의 지표로 활용할 수 있다. 이 지표를 `ApproximateNumberOfMessages`라고 한다.
- `ApproximateNumberOfMessages`를 지표로 `CloudWatch` 알람을 설정해 `ASG`의 용량을 증감시킬 수 있는 것이다.

![image](https://user-images.githubusercontent.com/97398071/235359770-c622111c-49bf-4230-b52c-41b44ece1d7f.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 시나리오는 다음과 같다.
~~~
- SQS 대기열과 ASG가 있을 때 ASG 내의 EC2 인스턴스가 SQS 내의 메세지를 폴링한다.
- 이 때 ASG의 스케일링 지표를 메세지 대기열의 사이즈로 설정할 수 있다.  ApproximateNumberOfMessages로 대기열에 몇 개의 메세지가 남았는지 확인할 수 있기 때문이다.
- 이를 지표로 CloudWatch 알람을 설정한다. 이 알람을 기준으로 ASG가 스케일링을 실행한다.
~~~

![image](https://user-images.githubusercontent.com/97398071/235362616-9bace8d5-9769-438b-b1c2-0232e74f6a81.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- `SQS`를 버퍼로 사용할 수 있다. 애플리케이션의 중요 트랜잭션의 버퍼로 사용하는 것이다. 이렇게 구성하면 유실되는 요청이 단 하나도 없다. 중요하다.

![image](https://user-images.githubusercontent.com/97398071/235363036-9af3911e-f8fa-4b26-8df3-f6989e5d527c.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 6. SQS decouple Scenario
- 비디오를 처리하는 프론트엔드 애플리케이션이 있다고 가정하자. 이 처리를 프론트엔드에서 직접 처리하면 프론트 처리가 느려질 수 있다.

- 이를 분리해서 처리하기 위해 프론트엔드 애플리케이션은 `SQS`의 대기열로 메세지를 전송하도록 구현한다.

- 그리고 백엔드 애플리케이션은 해당 메세지를 수신하여 실제 비디오 작업을 수행하고 `S3`에 저장한다.

![image](https://user-images.githubusercontent.com/97398071/235359941-0a130633-2947-4c02-9021-5b42809a8892.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 7. Amazon SQS - Security
- Encryption:
~~~
- In-flight encryption using HTTPS API
→ HTTPS API를 사용해 메세지를 보내고 생성함으로써 전송 중 암호화를 수행한다.
 
- At-rest encryption using KMS keys
→ KMS 키를 사용해 암호화를 수행할 수 있다.

- Client-side encryption if the client wants to perform encryption/decryption itself
→ 원한다면 클라이언트 사이드의 암호화도 가능하다. 이는 클라이언트가 자체적으로 암호화와 복호화를 수행해야함을 의미한다.
~~~

- Access Controls: IAM policies to regulate access to the SQS API  
→ 액세스 제어를 위한 `IAM` 정책은 `SQS API`에 대한 액세스를 규제할 수 있다.

- SQS Access Policies (similar to S3 bucket policies)  
→ S3 버킷 정책과 유사하다. 동일한 목적을 가지고 있기 때문이다.
~~~
- Useful for cross-account access to SQS queues
→ SQS 대기열에 대한 교차 계정 액세스를 수행할 수 있다.

- Useful for allowing other services (SNS, S3…) to write to an SQS queue
→ 다른 서비스가 SQS 대기열에 메세지를 추가할 수 있도록 허용할 때 매우 유용하다.
~~~

### 8. SQS – Message Visibility Timeout
- After a message is polled by a consumer, it becomes invisible to other consumers  
→ 소비자가 메세지를 폴링하면 그 메세지는 다른 소비자들에게 보이지 않게 된다.

- By default, the “message visibility timeout” is 30 seconds  
→ 메세지 가시성 시간초과의 기본값은 30초이며, 0초 ~ 12시간까지 자유롭게 설정할 수 있다. 

- That means the message has 30 seconds to be processed  
→ 가시성 시간 초과 기간 내라면 폴링한 메세지가 다른 소비자들에게 보이지 않는다. 이 말은 30초 이내에 이 메세지가 처리되어야 한다는 의미이다.

- After the message visibility timeout is over, the message is “visible” in SQS  
→ 하지만, 가시성 시간 초과 시간이 경과되고 메세지가 삭제되지 않았다면 메세지는 대기열에 다시 들어간다.
그러면 다른 소비자 또는 동일한 소비자가 `ReceiveMessage API`를 호출했을 때 이전의 그 메세지를 받게 된다.

![image](https://user-images.githubusercontent.com/97398071/235361280-af0f033c-b003-4d57-9fd3-ce5f68b18a57.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- If a message is not processed within the visibility timeout, it will be processed twice  
→ 가시성 시간 초과 기간 내에 메세지를 처리하지 않으면 메세지가 두 번 처리될 수 있다.

- A consumer could call the ChangeMessageVisibility API to get more time  
→ `ChangeMessageVisibility API`가 있다. 소비자가 메세지를 처리하는데 시간이 더 필요하다는 것을 알 경우에 해당 메세지를 두 번 처리하고 싶지 않다면 소비자는 해당 `API`를 호출하여 `SQS`에 알려야 한다.

- If visibility timeout is high (hours), and consumer crashes, re-processing will take time  
→ 기본값을 매우 높은 값으로 설정하면 처리가 지연될 수 있다.

- If visibility timeout is too low (seconds), we may get duplicates  
→ 기본값을 매우 낮은 값으로 설정하면 처리할 시간이 충분하지 않아 다른 소비자가 메세지를 메세지를 여러번 읽을 것이며, 중복 처리될 수 있다.

- 그러므로 메세지 가시성 시간 초과 기간은 애플리케이션에 합당하도록 설정되어야 한다.

### 9. Amazon SQS - Long Polling
- When a consumer requests messages from the queue, it can optionally “wait” for messages to arrive if there are none in the queue  
→ 소비자가 대기열에 메세지를 요청할 때 대기열에 아무것도 없다면 메세지를 기다리게 된다.

- This is called Long Polling
→ 이를 롱 폴링이라고 한다.

- LongPolling decreases the number of API calls made to SQS while increasing the efficiency and reducing latency of your application  
→ 롱 폴링을 사용하는 이유는 두 가지이다. 첫 번째 이유는 지연시간을 낮추기 위해서고, 두 번째 이유는 `SQS`로 보내는 `API` 호출 숫자를 줄이기 위해서이다.

- The wait time can be between 1 sec to 20 sec (20 sec preferable)  
→ 롱 폴링은 1초부터 20초 사이로 설정이 가능하다. 길면 길수록 좋다.

- Long Polling is preferable to Short Polling  
→ 롱 폴링이 숏 폴링보다 선호된다.

- Long polling can be enabled at the queue level or at the API level using WaitTimeSeconds  
→ 롱 폴링을 구성하는 방법에는 두 가지가 있다. 첫 번째 방법은 대기열 레벨에서 롱 폴링을 활성화하는 것이며, 두 번째 방법은 `WaitTimeSeconds`를 지정함으로써 소비자가 스스로 롱 폴링을 하도록 하는 것이다.

- `SQS` 대기열에 대한 `API` 호출 수를 최적화하고 지연 시간을 줄이는 법을 묻는다면 롱 폴링에 대한 문제이다.

![image](https://user-images.githubusercontent.com/97398071/235362155-1474fb16-593d-4823-a7e8-692d6f8364bf.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 10. Amazon SQS – FIFO Queue
- FIFO = First In First Out (ordering of messages in the queue)  
→ `FIFO`이므로 대기열에 첫 번째로 도착한 메세지가 대기열이 떠날 때도 첫 번째가 된다. 그렇기에 표준 대기열보다 메세지의 처리 순서가 더 확실히 보장된다.

![image](https://user-images.githubusercontent.com/97398071/235362279-54fdcb8a-1769-4bca-9f2d-104d4068ee32.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Limited throughput: 300 msg/s without batching, 3000 msg/s with  
→ 순서를 확실히 보장하기 때문이 이 `SQS` 대기열의 처리량에는 제한이 존재한다.

- Exactly-once send capability (by removing duplicates)  
→ 중복을 제거하는 `SQS FIFO` 대기열의 기능으로 인해 정확히 한 번 메세지를 보낸다.

- Messages are processed in order by the consumer  
→ 메세지가 순서대로 처리된다는 것을 확신할 수 있다.

- 처리 순서를 유지할 필요가 있을 때, `FIFO`를 사용한다. 이 때에는 너무 많은 `SQS` 메세지를 보내지 않도록 처리량에 제한이 필요할 수 있다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
