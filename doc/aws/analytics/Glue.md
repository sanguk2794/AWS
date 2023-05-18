## AWS Glue
### 1. AWS Glue
- Managed extract, transform, and load (ETL) service  
→ `Glue`는 추출과 변환, 로드 서비스를 관리하는 `ETL` 서비스이다.

- Useful to prepare and transform data for analytics  
→ 분석을 위해 데이터를 준비하고 변환하는데 매우 유용하다.

- Fully serverless service  
→ 서버리스 서비스이다.

- `S3` 버킷이나 `Amazon RDS`의 데이터를 데이터 웨어하우스인 `Redshift`에 로드할 때 `Glue`를 사용해 데이터 소스에서 추출한 데이터를 원하는대로 변형할 수 있다.
- 변형 이후의 최종 출력 데이터를 `Redshift Data Warehouse`에 로드할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/235962880-a3c128d6-4ea4-4f53-8539-dfc9113a6da7.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. AWS Glue – Convert data into Parquet format
- 데이터를 `Parquet` 형식으로 변환할 수 있다. `Parquet`은 열 기반의 데이터 형식이므로 `Athena` 같은 서비스와 함께 사용하면 효과적이다.

- `Glue` 서비스를 사용해 `Athena`의 성능을 향상시키는 시나리오는 다음과 같다.
~~~
- S3의 CSV 파일을 Glue 서비스를 이용해 Parquet 형식으로 변환한 다음 출력 S3 버킷으로 내보낸다.
- 이를 Amazon Athena에 송신하면 Amazon Athena가 파일을 훨씬 더 잘 분석한다.
- 이 전체 과정을 자동화할 수 있다. 파일이 S3 버킷에 삽입될 때마다 람다 함수 또는 EventBridge로 이벤트 알림을 보내 Glue ETL 작업을 트리거하는 것이다.
~~~

![image](https://user-images.githubusercontent.com/97398071/235962999-18438a35-a418-4fff-8c95-0ce5c9807dcc.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. Glue Data Catalog: catalog of datasets
- `Glue Data Catalog`는 데이터 세트의 카탈로그이다.
- `Glue Data Catalog`는 `Glue` 데이터 크롤러를 실행해 `S3`, `RDS`, `DynamoDB` 또는 호환 가능한 온프레미스 `JDBC` 데이터베이스에 연결한다.
- `Glue` 데이터 크롤러는 데이터베이스를 크롤링하고 데이터베이스의 테이블 열, 데이터 형식 등의 모든 메타 데이터를 `Glue Data Catalog`에 기록한다.  
- 따라서 `ETL`을 수행하기 위한 `Glue` 작업에 활용될 모든 데이터베이스, 테이블 메타 데이터를 가지게 된다.
- `Amazon Athena`는 데이터와 스키마를 검색할 때 백그라운드에서 `Glue Data Catalog`를 활용한다. 이는 `Amazon Redshift Spectrum`, `Amazon EMR`도 마찬가지이다.

![image](https://user-images.githubusercontent.com/97398071/235963137-206b7be1-4a3e-4192-a95a-f1ac08d90581.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 4. Glue – things to know at a high-level
- Glue Job Bookmarks: prevent re-processing old data  
→ `Glue Job Bookmarks`를 사용하면 새로운 `ETL` 작업을 실행할 때 이전 데이터를 재처리하는 것을 방지할 수 있다. 

- Glue Elastic Views:
~~~
- Combine and replicate data across multiple data stores using SQL
→ SQL을 사용해 여러 데이터 스토어의 데이터를 결합하고 복제한다.

- No custom code, Glue monitors for changes in the source data, serverless
→ 커스텀 코드를 지원하지 않으며 Glue가 원본 데이터의 변경 사항을 모니터링한다. 서버리스이다.

- Leverages a “virtual table” (materialized view)
→ 또 여러 데이터 스토어에 분산된 구체화된 뷰인 가상 테이블을 생성할 수 있다.
~~~

- Glue DataBrew: clean and normalize data using pre-built transformation  
→ `Glue DataBrew`는 사전 빌드된 변환을 사용해 데이터를 정리하고 정규화한다.

- Glue Studio: new GUI to create, run and monitor ETL jobs in Glue  
→ `Glue Studio`는 `Glue`에서 `ETL` 작업을 생성, 실행 및 모니터링할 때 사용한다.

- Glue Streaming ETL (built on Apache Spark Structured Streaming): compatible with Kinesis Data Streaming, Kafka, MSK (managed Kafka)  
→ `Glue Streaming ETL`은 `Apache Spark Structured Streaming` 위에 빌드되며 `ETL` 작업을 배치 작업이 아니라 스트리밍 작업으로 실행할 수 있다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---