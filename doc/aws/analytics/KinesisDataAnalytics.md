## Kinesis Data Analytics
### 1. Kinesis Data Analytics (SQL application)
- 실시간 분석 처리에 사용한다.

- Add reference data from Amazon S3 to enrich streaming data  
→ `S3` 버킷의 참조 데이터를 조인해 사용할 수 있다.

- Fully managed, no servers to provision  
→ 완전 관리형 서비스이므로 서버를 프로비저닝하지 않는다.

- Automatic scaling  
→ 오토 스케일링이 가능하다.

- `Kinesis Data Analytics`는 `SQL application`용과 `Apache Flink`용의 두 가지 종류로 나뉘어진다.

- Pay for actual consumption rate  
→ `Kinesis Data Analytics`에 전송된 데이터만큼 비용을 지불한다.

- Real-time analytics on Kinesis Data Streams & Firehose using SQL  
→ `Kinesis Data Streams` 및 `Kinesis Data Firehose`에 대한 실시간 분석이 가능하다. 분석에는 `SQL`을 활용한다.

- Target
→ 분석 데이터를 `Kinesis Data Streams` 또는 `Kinesis Data Firehose`에 전송할 수 있다. 각각의 사용 사례가 존재한다.
~~~
- Kinesis Data Streams: create streams out of the real-time analytics queries
→ 실시간 분석 쿼리를 Kinesis Data Streams에 전송할 수 있다. 람다 또는 애플리케이션을 활용한 실시간 처리가 가능하다.

- Kinesis Data Firehose: send analytics query results to destinations
- 분석 쿼리의 결과를 Kinesis Data Firehose에 전송할 수 있다. S3, Redshift, OpenSearch 등 서비스에 결과를 전송할 수 있다.
~~~

- Use cases:
~~~
- Time-series analytics
→ 시계열 분석

- Real-time dashboards
→ 실시간 대시보드

- Real-time metrics
→ 실시간 지표
~~~

![image](https://user-images.githubusercontent.com/97398071/235977958-0a81bc34-2cd6-4072-b342-cbc2acd5a70f.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. Kinesis Data Analytics for Apache Flink
- `Kinesis Data Analytics`에서 `Apache Flink`를 사용할 수 있다.

- Use Flink (Java, Scala or SQL) to process and analyze streaming data  
→ `Apache Flink`를 사용하면 `Java`, `Scala`, `SQL`로 애플리케이션을 작성해 스트리밍 데이터를 처리, 분석할 수 있다. 

- `Flink`는 코드로 작성해야 하는 특별한 애플리케이션이다. `Flink` 애플리케이션을 `Kinesis Data Analytics`의 `Flink` 전용 클러스터에서 실행할 수 있다.

- `Apache Flink`를 사용해 두 개의 메인 데이터 소스인 `Kinesis Data Analytics`나 `Amazon MSK`의 데이터를 읽을 수 있다.

![image](https://user-images.githubusercontent.com/97398071/235972231-58201557-a3a2-4c67-94eb-2fda21d3ae9e.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Run any Apache Flink application on a managed cluster on AWS
~~~
- AWS의 관리형 클러스터에서 Apache Flink 애플리케이션을 실행할 수 있다. Apache Flink는 표준 SQL보다 훨씬 강력하다.
→ 고급 쿼리 능력이 필요할 때나 Kinesis Data Streams, MSK 등의 서비스로부터 스트리밍 데이터를 읽는 능력이 필요할 때 Kinesis Data Analytics를 사용한다.

- provisioning compute resources, parallel computation, automatic scaling
→ 이 서비스를 사용하면 컴퓨팅 리소스를 자동 프로비저닝할 수 있으며 병렬 연산과 오토 스케일링이 지원된다.

- application backups (implemented as checkpoints and snapshots)
→ 체크포인트와 스냅샷으로 구현되는 애플리케이션 백업이 가능하다.

- Use any Apache Flink programming features
→ Apache Flink 프로그래밍 기능을 사용할 수 있다.

- Flink does not read from Firehose (use Kinesis Analytics for SQL instead)
→ Apache Flink는 Kinesis Data Firehose의 데이터는 읽지 못한다.
~~~

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
