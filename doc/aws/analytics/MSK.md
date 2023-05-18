## Amazon MSK
### 1. Amazon Managed Streaming for Apache Kafka (Amazon MSK)
- `Amazon MSK`는 `Apache Kafka`용 관리형 스트리밍 서비스이다.

- Alternative to Amazon Kinesis  
→ `Kafka`는 `Amazon Kinesis`의 대안이다. 두 서비스 모두 데이터를 스트리밍한다.

- Fully managed Apache Kafka on AWS
~~~
- MSK는 AWS 완전 관리형 kafka 클러스터 서비스이다.

- Allow you to create, update, delete clusters
→ 그때그때 클러스터를 생성하고 수정하며 삭제한다.

- MSK creates & manages Kafka brokers nodes & Zookeeper nodes for you
→ MSK는 클러스터 내 브로커 노드와 Zookeeper 브로커 노드를 생성 및 관리한다.

- Deploy the MSK cluster in your VPC, multi-AZ (up to 3 for HA)
→ 고가용성을 위해 VPC의 클러스터를 최대 세 개의 다중 AZ에 배포한다.

- Automatic recovery from common Apache Kafka failures
→ 일반 kafka 장애를 자동 복구하는 기능이 있다.

- Data is stored on EBS volumes for as long as you want
→ EBS 볼륨에 데이터를 저장할 수도 있다.
~~~

- MSK Serverless
~~~
- Run Apache Kafka on MSK without managing the capacity
→ MSK에서 Apache Kafka를 실행하지만 서버 프로비저닝이나 용량 관리가 필요하지 않다.

- MSK automatically provisions resources and scales compute & storage
→ MSK가 리소스를 자동으로 프로비저닝하고 컴퓨팅과 스토리지를 스케일링한다.
~~~

### 2. Apache Kafka at a high level
- `Apache Kafka`는 데이터를 스트리밍하는 방식이다. `kafka` 클러스터는 여러 브로커로 구성되고, 데이터를 생산하는 생산자는 `Kinesis` ,`IoT`, `RDS` 등의 데이터를 클러스터에 주입한다.
- `kafka` 주제로 데이터를 전송하면 해당 데이터는 다른 브로커로 완전 복제된다. kafka 주제는 실시간으로 데이터를 스트리밍하고 소비자는 데이터를 소비하기 위해 주제를 폴링한다.
- 소비자는 데이터를 원하는 대로 처리하거나 `EMR`, `S3`, `SageMaker`, `Kinesis`, `RDS` 등의 대상에 송신한다.

![image](https://user-images.githubusercontent.com/97398071/235982550-35bbb544-a528-4dd6-a442-6f5ae8218394.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. Kinesis Data Streams vs. Amazon MSK
- Kinesis Data Streams
~~~
- 1 MB message size limit
→ 1MB의 메세지 크기 제한이 있다.

- Data Streams with Shards
→ 샤드로 데이터를 스트리밍한다.

- Shard Splitting & Merging
→ 용량을 늘릴때는 샤드 분할을, 축소할 때는 샤드 병합을 수행한다.

- TLS In-flight encryption
→ TLS 전송 중 암호화 기능을 지원한다.

- KMS at-rest encryption
→ KMS를 활용한 저장 데이터 암호화가 가능하다.
~~~

- Amazon MSK
~~~
- 1MB default, configure for higher (ex: 10MB)
→ 1MB가 기본이고, 메세지 크기를 최대 10MB까지 설정할 수 있다.

- Kafka Topics with Partitions
→ 파티션을 이용한 Kafka topic을 사용해 스트리밍한다. 비슷하다.

- Can only add partitions to a topic
→ 파티션 추가를 통한 topic 확장만 가능하다. 파티션의 제거는 불가능하다.

- PLAINTEXT or TLS In-flight Encryption
→ 평문과 TLS 전송 중 암호화 기능이 있다.

- KMS at-rest encryption
→ KMS를 활용한 저장 데이터 암호화가 가능하다.
~~~

### 4. Amazon MSK Consumers
- `MSK`에 데이터를 생산하려면 `kafka` 생산자를 생성해야 한다.
- `MSK`의 데이터를 소비하는 방법은 여러가지이다. `Apache Flink`용 `Kinesis Data Analytics`를 사용해 `Apache Flink` 앱을 실행하고 `MSK` 클러스터의 데이터를 읽어 오면 된다.
- `AWS Glue`로 `ETL` 작업을 스트리밍해도 된다.
- `MSK`를 이벤트 소스로 두고 람다 함수와 함께 사용할 수 있다.
- 자체 `kafka` 소비자를 생성해 원하는 플랫폼에서 실행할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/235982814-c22d357f-e8eb-44c6-82c1-e22687f6fc4f.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---