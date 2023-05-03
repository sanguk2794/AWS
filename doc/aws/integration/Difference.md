## SQS vs SNS vs Kinesis
### 1. SQS vs SNS vs Kinesis
#### 1. SQS
- Consumer “pull data”  
→ 소비자가 데이터를 요청해서 메세지를 가져오는 `pull data model`이다.

- Data is deleted after being consumed  
→ 데이터를 처리한 후 소비자가 대기열에서 삭제해서 다른 소비자가 읽을 수 없도록 처리해야 한다.

- Can have as many workers(consumers) as we want  
→ 작업자나 소비자 수의 제한이 없다. 작업자와 소비자가 함께 소비하고 대기열에서 삭제하기 때문이다.

- No need to provision throughput  
→ 관리형 서비스이므로 처리량을 프로비저닝할 필요가 없다.

- Ordering guarantees only on FIFO queues  
→ 순서를 보장하려면 `FIFO` 대기열을 활성화해야 한다.

- Individual message delay capability  
→ 메세지를 숨겨 일정 시간 후에 대기열에 나타나도록 할 수 있다.

#### 2. SNS
- Push data to many subscribers  
→ 다수에 구독자에게 데이터를 푸시하면 구독자는 메세지의 복사본을 받게 된다.

- Up to 12,500,000 subscribers  
→ 주제별로 12,500,000명의 구독자가 가능하다.

- Data is not persisted (lost if not delivered)  
→ 데이터가 한 번 `SNS`에 전송되면 지속되지 않는다. 제대로 전달되지 않으면 데이터를 잃을 가능성이 있는 것이다.

- Up to 100,000 topics  
→ 게시 / 구독 모델은 최대 10만 개의 주제로 확장이 가능하다.

- No need to provision throughput  
→ 처리량을 프로비저닝하지 않아도 괜찮다.

- Integrates with SQS for fan- out architecture pattern  
→ 원한다면 `SQS`와 결합할 수 있다. 이를 `fan-out architecture pattern`이라고 한다.

- FIFO capability for SQS FIFO  
→ `SNS FIFO`의 주제를 `SQS FIFO` 대기열과 결합할 수 있다.

#### 3. Kinesis
- Standard: pull data  
→ 두 가지 소비모드가 있다. 표준 모드는 소비자가 `Kinesis`로부터 데이터를 가져온다.

- 2 MB per shard  
→ 샤드 당 `2MB/s`의 속도를 지원한다.

- Enhanced-fan out: push data  
→ 향상된 `fan out` 소비 매커니즘에서는 `Kinesis`가 소비자에게 데이터를 푸시한다.

- 2 MB per shard per consumer  
→ 소비자 당 `2MB/s`의 속도를 지원한다.

- Possibility to replay data  
→ 데이터가 지속되기 때문에 데이터를 다시 재생할 수 있다.

- Meant for real-time big data, analytics and ETL  
→ 그렇기에 실시간 빅데이터 분석, `ETL`등에 활용된다.

- Ordering at the shard level  
→ 스트림마다 원하는 샤드 양을 지정해야 한다.

- Data expires after X days  
→ 샤드를 직접 확장해서 데이터가 언제 만료될 지 정한다. 1 ~ 365일 사이이다.

- Provisioned mode or on-demand capacity mod  
→ 용량 모드에는 두 가지가 있다. 프로비저닝 용량 모드는 원하는 샤드 양을 미리 지정하며, 온디맨드 용량 모드에서는 샤드 수가 자동으로 지정된다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---