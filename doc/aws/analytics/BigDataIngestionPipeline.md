## Big Data Ingestion Pipeline
### 1. Big Data Ingestion Pipeline
- We want the ingestion pipeline to be fully serverless  
→ 사용자는 애플리케이션의 수집 파이프라인을 완전한 서버리스로써 `AWS`가 관리해 줄 것을 원한다.

- We want to collect data in real time  
→ 실시간으로 데이터를 수집하기를 원한다.

- We want to transform the data  
→ 데이터를 변형하기를 원한다.

- We want to query the transformed data using SQL  
→ 변형된 데이터를 `SQL`을 통해 요청하기를 원한다.

- The reports created using the queries should be in S3  
→ 이러한 쿼리를 사용해 생성한 보고서가 `S3`에 저장되면 좋을 것이다.

- We want to load that data into a warehouse and create dashboards  
→ 이후 데이터를 데이터 웨어하우스에 등재해 대시보드를 생성하고자 한다. 

- 대체로 수집이나 회수, 변형 혹은 쿼리 및 분석 과정에서 흔히 발생하는 빅데이터 문제를 처리하는 방법이다.
~~~
- IoT 디바이스가 있다. IoT 코어는 이 IoT 디바이스들의 정보를 수집하고 장치 관리를 돕는다.
- IoT 코어는 수집한 정보를 Kinesis Data Stream에 직접 전달한다. Kinesis Data Stream은 빅데이터가 실시간으로 Kinesis Data Firehose에 전송되도록 허용한다.
- Kinesis Data Firehose에는 람다 함수가 직접 연결되어 있다. 이 람다 함수를 통해 데이터의 변형을 돕는다.  
- Kinesis Data Firehose는 거의 실시간으로 S3 버킷에 데이터를 입력하고 오프로드한다. 이를 수집 버킷이라고 한다. 여러 장치에서 많은 데이터를 실시간으로 얻을 수 있는 파이프라인이 마련되었다.
- 데이터 수집 버킷을 통해 SQS 대기열을 작동시킬 수 있다. S3 버킷이 람다 함수를 직접 실행시킬 수 있으므로 이건 선택 사항이다. 이 SQS 대기열은 람다 함수를 실행한다.
- 람다가 Amazon Athena SQL 쿼리를 실행하면 Athena 쿼리는 수집 버킷에서 SQL 쿼리를 생성한다. 그리고 쿼리 결과를 레포트 S3 버킷에 저장한다.
- Amazon QuickSight를 사용하면 레포트 버킷에 저장된 결과를 직접 시각화할 수 있다. 또는 Redshift같은 데이터 웨어하우스에 데이터를 입력해 분석하기도 한다. Redshift는 서버리스가 아니다.
~~~

- 빅데이터 수집 파이프라인의 핵심은 실시간 데이터 수집과 변형, 람다와 `Redshift`를 이용한 데이터 웨어하우징, `QuickSight`를 활용한 시각화를 꼽을 수 있다.

![image](https://user-images.githubusercontent.com/97398071/235985486-d9dc4ac3-a3a1-42ca-b9f9-3ebf1834449b.png) 

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. Big Data Ingestion Pipeline discussion
- IoT Core allows you to harvest data from IoT devices  
→ `IoT Core`는 여러 `IoT` 장치에서 데이터 수집을 허용한다.

- Kinesis is great for real-time data collection  
→ `Kinesis`는 실시간 데이터 수집에 적합하다.

- Firehose helps with data delivery to S3 in near real-time (1 minute)  
→ `Firehose`는 실시간에 가깝게 `S3`로 데이터를 운반하도록 돕는다.

- Lambda can help Firehose with data transformations  
→ 람다는 `Firehose`를 도와 데이터를 변형한다.

- Amazon S3 can trigger notifications to SQS  
→  `Amazon S3`가 `SQS`, `SNS`, `Lambda`에 알림을 실행한다.

- Lambda can subscribe to SQS (we could have connecter S3 to Lambda)  
→ 람다는 `SQS`를 구독할 수 있다. 

- Athena is a serverless SQL service and results are stored in S3  
→ `Athena`의 실행 결과는 `S3`에 저장된다.

- The reporting bucket contains analyzed data and can be used by reporting tool such as AWS QuickSight, Redshift, etc…  
→ 보고 버킷은 분석된 데이터를 보관하기 위해 사용한다. 추가적인 시각화가 필요하다면 `QuickSight` 혹은 `Redshift` 등의 보고 도구를 사용하면 된다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
