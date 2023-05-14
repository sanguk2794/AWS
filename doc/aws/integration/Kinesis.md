## Kinesis
### 1. Kinesis Overview
- `Kinesis`는 공인 인증시험에서 제일 중요한 파트 중 하나이다.

- Makes it easy to collect, process, and analyze streaming data in real-time  
→ `Kinesis`를 활용하면 실시간 스트리밍 데이터를 손쉽게 수집하고 처리하여 분석할 수 있다.

- Ingest real-time data such as: Application logs, Metrics, Website clickstreams, IoT telemetry data…  
→ 실시간 데이터에는 애필르키에션 로그, 계측, 웹 사이트 클릭 스트림, `IoT` 원격 측정 데이터 등 어떤 것도 포함될 수 있다.

- 키네시스는 4가지 서비스로 구성되어 있다.
~~~
- Kinesis Data Streams: capture, process, and store data streams
→ Kinesis Data Streams은 데이터 스트림을 수집하여 처리하고 저장한다.

- Kinesis Data Firehose: load data streams into AWS data stores
→ Kinesis Data Firehose는 데이터 스트림을 AWS 내부나 외부의 데이터 저장소로 읽어들인다.

- Kinesis Data Analytics: analyze data streams with SQL or Apache Flink
→ Kinesis Data Analytics에서는 SQL이나 Apache Flink를 사용하여 데이터 스트림을 분석한다.

- Kinesis Video Streams: capture, process, and store video streams
→ 시험에는 나오지 않는다. 비디오 스트림을 수집하고 처리해 저장한다.
~~~

### 2. Kinesis Data Streams
- 시스템에서 큰 규모의 데이터 흐름을 다루는 서비스이다.
- 여러 개의 샤드로 구성되어 있고, 샤드는 `N`번까지 번호가 매겨진다. 샤드의 숫자는 프로비저닝이 필요하다.
- 데이터는 모든 샤드에 분배된다.

- 시나리오는 다음과 같다.
~~~
- 생산자가 데이터를 생성해 Kinesis Data Streams로 전송한다.
→ 생산자는 애플리케이션, 데스크톱, 휴대전화, SDK, KPL, Kinesis Agent 등 다양하다.

- 모든 생성자는 정확히 동일한 역할을 수행한다. 매우 낮은 수준에서 SDK에 의존하며, Kinesis Data Streams에 레코드를 전달한다.
- 레코드는 근본적으로 두 가지 요소로 구성된다. 파티션 키와 최대 1MB 크기의 데이터 블롭이 그것이다.

- 일단 데이터가 스트림에 들어가면 많은 소비자가 이 데이터를 사용하게 된다.
→ 소비자는 SDK, KCL에 의존하는 애플리케이션, Lambda, Kinesis Data Firehose, Kinesis Data Analytics 등 다양하다.

- 소비자가 받는 레코드는 파티션 키, 샤드에서 레코드의 위치를 나타내는 시퀀스 번호, 데이터 자체를 의미하는 데이터 블롭으로 이루어져 있다.
~~~

![image](https://user-images.githubusercontent.com/97398071/235569695-6fa3c5f1-44c4-4d40-9cbc-2e410205c4d3.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Retention between 1 day to 365 days  
→ 보존 기간은 1일부터 365일 사이로 설정할 수 있다.

- Ability to reprocess (replay) data  
→ 데이터를 다시 처리하거나 확인할 수 있다.

- Once data is inserted in Kinesis, it can’t be deleted (immutability)  
→ 데이터가 일단 `Kinesis`로 들어오면 삭제할 수 없다. 이러한 특성을 불변성이라고 한다.

- Data that shares the same partition goes to the same shard (ordering)  
→ 데이터 스트림으로 메세지를 전송하면 파티션 키가 추가되고, 파티션 키가 같은 메세지들은 같은 샤드로 들어가게 되어 키를 기반으로 데이터를 정렬할 수 있다.

- Producers: AWS SDK, Kinesis Producer Library (KPL), Kinesis Agent  
→ `AWS SDK`, `Kinesis Producer Library (KPL)`, `Kinesis Agent`를 생산자로 사용해 데이터를 전송할 수 있다.

- Consumers:
~~~
- Write your own: Kinesis Client Library (KCL), AWS SDK
→ Kinesis Client Library (KCL), AWS SDK를 사용하여 데이터를 소비할 수 있다.

- Managed: AWS Lambda, Kinesis Data Firehose, Kinesis Data Analytics
→ 아니면 AWS에서 관리하는 AWS Lambda, Kinesis Data Firehose, Kinesis Data Analytics를 활용할 수도 있다.
~~~

- `Kinesis Data Streams`로 레코드로 전송할 때 필요한 것이 `put-record`라는 `API`이다.  
→ 해당 `API` 호출시에는 스트림 이름, 파티션 키, 유저 데이터, 데이터 포맷을 설정한다.
~~~ shell
aws kinesis put-record --stream-name [stream name] --partition-key [key] --data [data] --cli-binary-format raw-in-base64-out
~~~

- 파티션 키를 공유하는 데이터는 같은 샤드로 이동한다. `Kinesis Data Streams`의 전송 지표는 `CloudWatch`에서 확인할 수 있다.

- 스트림 내의 레코드를 확인할 때에는 아래 커맨드를 사용한다. 여기서 샤드 아이디를 확인할 수 있다.
~~~ shell
# describe the stream
aws kinesis describe-stream --stream-name test
~~~

- 스트림 내의 레코드를 소비할 때에는 아래 커맨드를 사용한다. 스트림에서 읽어오기 위해서는 샤드 아이디가 필수이다. 
~~~ shell
# Consume some data
aws kinesis get-shard-iterator --stream-name test --shard-id shardId-000000000000 --shard-iterator-type TRIM_HORIZON
~~~

- 한 `Kinesis Data Stream`에 6개의 샤드가 프로비저닝되어 있습니다. 이 데이터 스트림은 보통 5MB/초의 속도로 데이터를 수신하며, 8MB/초의 속도로 데이터를 전송합니다. 이따금 트래픽이 2배까지 증가하여 `ProvisionedThroughputExceededException` 예외 처리가 발생합니다. 이 문제를 해결하려면 어떻게 해야 할까요?  
→ 더 많은 샤드 추가

#### 1. Kinesis Data Streams – Capacity Modes
- `Kinesis Data Streams`에는 프로비저닝 유형과, 온디맨드 유형의 두 가지 용량 유형이 있다.

- Provisioned mode:
~~~
- You choose the number of shards provisioned, scale manually or using API
→ 프로비저닝할 샤드 수를 정한다. 이 수는 수동으로 조정할 수 있다.

- Each shard gets 1MB/s in (or 1000 records per second)
→ 각 샤드는 초당 1MB 혹은 1천개의 레코드를 받아들인다.

- Each shard gets 2MB/s out (classic or enhanced fan-out consumer)
→ 출력의 경우 각 샤드는 초당 2MB를 내보낸다.

- You pay per shard provisioned per hour
→ 샤드를 프로비저닝할 경우 비용이 부가되므로 사전에 심사숙고해야한다.
~~~

- On-demand mode:
~~~
- No need to provision or manage the capacity
→ 프로비저닝을 하거나 용량을 관리할 필요가 없으며, 시간에 따라 언제든 용량이 조정된다.

- Default capacity provisioned (4 MB/s in or 4000 records per second)
→ 기본적으로 초당 4MB 혹은 4천 개의 레코드를 처리한다.

- Scales automatically based on observed throughput peak during the last 30 days
→ 이 용량은 지난 30일 동안 관측한 최대 처리량에 기반하여 자동으로 조정된다.

- Pay per stream per hour & data in/out per GB
→ 시간당 데이터량 단위에 따라 비용이 부가된다. 즉, 비용 산정 방식이 다르다. 
~~~

- 사전에 사용량을 예측할 수 없다면 온디맨드 유형을, 예측할 수 있다면 프로비저닝 유형을 사용하는 것이 낫다.

- 회사에서 `Amazon Kinesis Data Streams`을 사용해 클릭 스트림 데이터를 수집하여 분석 작업을 수행하고 있습니다. 며칠 뒤에 캠페인이 예정되어 있고 트래픽은 예측이 불가능하며 100배까지 증가할 수도 있습니다. 이런 경우에 적합한 `Kinesis Data Stream` 용량 모드는 무엇입니까?  
→ 온디맨드 모드

- 한 빅데이터 분석 업체가 `Kinesis Data Streams(KDS)`을 사용하여 농업 과학 기업의 필드 장치에서 `IoT` 데이터를 받아 처리하고 있습니다. 들어온 데이터 스트림은 다양한 컨슈머 애플리케이션에서 사용되고 있는데, 엔지니어들이 프로듀서와 데이터 스트림 컨슈머 사이에서 데이터 전달 속도의 성능 저하를 발견했습니다. 솔루션 아키텍트의 관점에서 주어진 사례의 성능 문제를 개선하기에 적합한 방식은 무엇입니까?  
→ `Kinesis Data Streams`의 `Enhanced Fanout` 기능을 활성화한다.  
`Amazon Kinesis Data Streams(KDS)`는 엄청난 규모로 확장할 수 있는 견고한 실시간 데이터 스트리밍 서비스입니다. `KDS`는 웹사이트 클릭스트림, 데이터베이스 이벤트 스트림, 재무 거래, 소셜미디어 피드, `IT` 로그, 위치추적 이벤트 등 수십만 가지 소스에서 초당 수 기가바이트에 이르는 데이터를 연속적으로 수집할 수 있습니다.
기본값으로 스트림 데이터를 소비하는 모든 애플리케이션 간에 2MB/초/샤드의 출력이 공유됩니다. 스트림으로부터 병렬로 데이터를 검색하는 다수의 컨슈머가 있다면 향상된 팬아웃 기능을 사용하셔야 합니다. 향상된 팬아웃 기능을 사용하여 개발자는 스트림 컨슈머를 등록하여 향상된 팬아웃 기능을 사용하고 샤드당 2MB/초라는 읽기 처리 속도로 수신할 수 있으며 이러한 처리 속도는 스트림 안의 샤드 개수에 맞춰 자동으로 스케일링됩니다.

#### 2. Kinesis Data Streams Security
- Control access / authorization using IAM policies  
→ `IAM` 정책을 통해 샤드 생성과 샤드 접근에 대한 권한을 제어할 수 있다.

- Encryption in flight using HTTPS endpoints  
→ `HTTPS`로 전송 중 데이터를 암호화할 수 있으며

- Encryption at rest using KMS  
→ `KMS`로 데이터를 암호화할 수 있다.

- You can implement encryption/decryption of data on client side (harder)  
→ 클라이언트 측에서 데이터를 암호화하거나 해독할 수 있다. 하지만 그만큼 보안이 강화된다.

- VPC Endpoints available for Kinesis to access within VPC  
→ `VPC` 엔드포인트를 사용할 수 있다. `Kinesis`에 인터넷을 거치지 않고 프라이빗 서브넷의 인스턴스에서 직접 손쉽게 접근할 수 있다.

- Monitor API calls using CloudTrail  
→ 모든 `API` 요청은 `CloudTrail`로 감시할 수 있다. 

![image](https://user-images.githubusercontent.com/97398071/235572977-1fde9c37-0ed2-46e7-b45a-c982a5f5e115.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. Kinesis Data Firehose
- 생산자로부터 데이터를 가져올 수 있는 유용한 서비스이다. 생산자는 `Kinesis Data Streams`의 생산자에 더해 `Kinesis Data Streams`, `CloudWatch`, `AWS Iot` 등이 포함된다.

- Fully Managed Service, no administration, automatic scaling, serverless  
→ `Kinesis Data Firehose`는 완전 관리형 서비스이다. 오토 스케일링을 지원하며 서버리스이므로 관리할 서버가 없다.

- 소비자는 다음과 같다. 반드시 외워야한다.
~~~
- AWS: Redshift / Amazon S3 / ElasticSearch - ElasticSearch는 이름이 바뀌었다. OpenSearch Service이다.
→ Redshift는 데이터 웨어하우스이다. 이 부분은 반드시 기억하자.

- Kinesis Data Firehose는 S3, Redshift, Elasticsearch 또는 Splunk에만 사용할 수 있다. Kinesis Data Firehose에서 나오는 데이터를 소비하는 개인 애플리케이션을 가질 수 없다.

- 3rd party partner: Splunk / MongoDB / DataDog / NewRelic / …
→ 서드파티 모듈에도 데이터를 송신할 수 있다.

- Custom: send to any HTTP endpoint
→ HTTP 엔드포인트가 있는 자체 API를 보유하고 있다면 이를 소비자로 설정할 수 있다.
~~~

- Pay for data going through Firehose  
→ 통신 데이터에 대해서만 비용을 지불하면 되므로 비용효율적이다.

- Near Real Time  
→ 거의 실시간이다. 실시간이 아니라 실시간에 가까운 서비스인 것이다. `Kinesis Data Firehose`는 버퍼 사이즈, 또는 `interval`을 조건으로 송신한다.

- 버퍼 사이즈, 버퍼 전송의 `interval`을 설정해 사용한다.

- Supports many data formats, conversions, transformations, compression  
→ 여러 데이터 형식과 데이터 전환, 변환, 압축을 지원한다.

- Supports custom data transformations using AWS Lambda  
→ 필요하다면 람다를 사용해 자체적으로 데이터를 변환할 수 있다.

- Can send failed or all data to a backup S3 bucket  
→ 실패한 데이터 혹은 모든 데이터를 백업 `S3` 버킷에 보낼 수 있다.

- 대량의 실시간 데이터를 생성하는 애플리케이션을 실행 중이며, 이 데이터를 `S3`와 `Redshift`로 로딩하려 합니다. 또한 이 데이터들은 목적지에 도달하기 전에 변환되어야 합니다. 이를 위해, 선택할 수 있는 가장 적절한 아키텍처는 무엇인가요?  
→ `Kinesis Data Streams` + `Kinesis Data Firehose`

### 4. Kinesis Data Streams vs Firehose
- Kinesis Data Streams
~~~
- Streaming service for ingest at scale
→ 데이터를 대규모로 수집할 때 쓰는 스트리밍 서비스이다.

- Write custom code (producer / consumer)
→ 생산자와 소비자 각각에 커스텀 코드를 사용할 수 있다.

- Real-time (~200 ms)
→ 실시간이다.

- Manage scaling (shard splitting / merging)
→ 용량을 직접 조정할 수 있어 샤드 분할이나 샤드 병합을 통해 용량이나 처리량을 늘릴 수 있다.

- Data storage for 1 to 365 days
→ 제공한 용량만큼 비용을 지불하며, 데이터는 1일부터 365일간 저장된다.

- Supports replay capability
→ 여러 소비자가 같은 스트림에서 읽어올 수 있고, 반복 기능도 지원된다.
~~~

- Kinesis Data Firehose
~~~
- Load streaming data into S3 / Redshift / ES / 3rd party / custom HTTP
→ 데이터를 대규모로 수집해 S3, Redshift, 서드파티, 자체 HTTP 서버로 스트리밍해주는 서비스이다.

- Fully managed
→ 완전 관리되며, 서버리스이다.

- Near real-time (buffer time min. 60 sec)
→ 실시간에 근접한 처리가 이루어진다. 실시간은 아니다.

- Automatic scaling
→ 오토 스케일링이 지원된다.

- No data storage
→ 데이터 스토리지가 없다.

- Doesn’t support replay capability
→ 데이터를 반복할 수 없다. 스토리지가 없기 때문이다.
~~~

### 5. Ordering data into Kinesis
- Imagine you have 100 trucks (truck_1, truck_2, … truck_100) on the road sending their GPS positions regularly into AWS.  
→ 도로에 트럭이 100개 있다고 가정한다. 이 트럭들은 각각의 `ID`가 있고, `GPS` 위치를 주기적으로 `AWS`에 전송한다.

- You want to consume the data in order for each truck, so that you can track their movement accurately.  
→ 각 트럭의 순서대로 데이터를 소비해서 트럭의 이동경로를 추적하고 그 경로를 순서대로 확인하려고 한다.

- How should you send that data into Kinesis?
- Answer: send using a “Partition Key” value of the “truck_id”  
→ 이 데이터를 키네시스에 전송하기 위해서는 파티션 키, 여기에서는 트럭 `ID`를 사용해야 한다.

- The same key will always go to the same shard  
→ 같은 키를 전달하면 언제나 동일한 샤드로 전달될 것이 보장되기 때문이다. 이렇게 재분배를 사용하기에 이 키를 파티션 키라고 하는 것이다.

![image](https://user-images.githubusercontent.com/97398071/235587794-ae8a70c4-c67d-4079-bb6c-59b600ba69c0.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 한 웹사이트 내에서 사용자들이 클릭하는 순서, 사용자들이 보내는 시간 및 탐색이 어디에서 시작되고 어떻게 종료되는지 등의 클릭스트림 데이터를 분석하고자 합니다. `Amazon Kinesis`를 사용하기로 했고, 웹사이트가 이러한 클릭스트림 데이터를 `Kinesis Data Stream`으로 전송하도록 구성한 상태입니다. `Kinesis Data Stream`으로 전송된 데이터를 확인하던 중, 데이터가 순서대로 정렬되어 있지 않으며, 한 개별 사용자로부터 온 데이터가 여러 샤드에 분산되어 있다는 것을 알게 되었습니다. 이 경우, 어떻게 문제를 해결해야 할까요?  
→ `Kinesis`로 보내지는 각 레코드에 사용자의 신원을 나타내는 파티션 키를 추가해야 함

### 6. Ordering data into SQS
- For SQS standard, there is no ordering.  
→ `SQS standard`에는 순서가 없다. 그렇기에 `FIFO`라는 선입선출 방식을 추가했다.

- For SQS FIFO, if you don’t use a Group ID, messages are consumed in the order they are sent, with only one consumer  
→ `FIFO`의 그룹 `ID`를 사용하지 않는 이상 모든 메세지가 소비되는 방식은 보내진 순서에 따르며 소비자는 하나만 존재한다.

- You want to scale the number of consumers, but you want messages to be “grouped” when they are related to each other  
→ 연관메세지를 그룹화한다면 그룹 `ID`를 사용할 수 있다. 

- Then you use a Group ID (similar to Partition Key in Kinesis)  
→ 그룹 `ID`는 `Kenesis`의 파티션 키와 그 사용법이 비슷하다.

![image](https://user-images.githubusercontent.com/97398071/235588625-86e4a85c-2d8c-4858-b0fe-e3cca3b6849c.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 7. Kinesis vs SQS ordering
- Let’s assume 100 trucks, 5 kinesis shards, 1 SQS FIFO
- Kinesis Data Streams:
~~~
- On average you’ll have 20 trucks per shard
→ 샤드당 평균 트럭이 20대가 된다.

- Trucks will have their data ordered within each shard
→ 트럭 데이터는 각 샤드에 순서대로 정렬된다.

- The maximum amount of consumers in parallel we can have is 5
→ 동시에 가질 수 있는 최대 소비자의 개수는 5개이다. 샤드가 5개이고 샤드마다 하나의 소비자가 필요하기 때문이다.

- Can receive up to 5 MB/s of data
→ 샤드가 5개이므로 초당 최대 5MB의 데이터를 수신할 수 있다.
~~~

- SQS FIFO
~~~
- You only have one SQS FIFO queue
→ 대기열은 오직 하나이다. 샤드 및 파티션을 정의할 필요 없이 오직 FIFO 대기열 하나뿐인 것이다.

- You will have 100 Group ID
→ 각 트럭 ID에 상응하는 그룹 ID를 100개 생성한다.

- You can have up to 100 Consumers (due to the 100 Group ID)
→ 그룹 ID가 100개이므로 소비자도 최대 100개가 될 수 있다. 각 소비자가 특정한 그룹 ID와 연결되기 때문이다.

- You have up to 300 messages per second (or 3000 if using batching)
→ 최대 초당 300개의 메세지를 처리 가능하다. 단, 배치를 사용하면 최대 3000개를 처리할 수 있다.
~~~

- 중요한 것은 경우에 따라 적절한 모델이 달라진다는 것이다.  
→`SQS FIFO`는 그룹 아이디 숫자에 따른 동적 소비자 수를 원할 때 사용하며, `Kinesis data streams`는 샤드 `ID`당 데이터를 정렬할 때 사용한다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---