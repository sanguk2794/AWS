## Amazon Athena
### 1. Amazon Athena
- Serverless query service to analyze data stored in Amazon S3  
→ `Amazon S3` 버킷에 저장된 데이터 분석에 사용하는 서버리스 쿼리 서비스이다.

- Uses standard SQL language to query the files (built on Presto)  
→ 데이터 분석을 위해서는 표준 `SQL` 언어로 파일을 쿼리해야 한다. `Amazon Athena`는 `SQL` 언어를 사용하는 `Presto` 엔진에 빌드되기 때문이다.

- Supports CSV, JSON, ORC, Avro, and Parquet  
→ `Amazon Athena`는 `CSV`, `JSON`, `ORC`, `Avro`, `Parquet` 등 다양한 형식을 지원한다.

- Pricing: $5.00 per TB of data scanned  
→ 가격은 스캔된 데이터의 `TB` 당 고정 가격을 지불한다. 전체 서비스가 서버리스여서 데이터베이스를 프로비저닝 할 필요가 없다.

- Commonly used with Amazon Quicksight for reporting/dashboards  
→ `Amazon Athena`는 `Amazon QuickSight`라는 도구와 함께 사용하는 일이 많다. 이를 통해 보고서와 대시보드를 생성할 수 있다.

- Use cases: Business intelligence / analytics / reporting, analyze & query VPC Flow Logs, ELB Logs, CloudTrail trails, etc...  
→ `Amazon Athena`의 사용 사례에는 임시 쿼리 수행, `BI` 분석 및 보고, `AWS` 서비스의 로그 쿼리 및 분석 등이 있다. 로그에는 `VPC Flow Logs`, `ELB Logs`, `CloudTrail trails` 등이 해당된다.

- Exam Tip: analyze data in S3 using serverless SQL, use Athena  
→ 서버리스 `SQL` 엔진을 사용한 `Amazon S3` 데이터 분석이 나오면 `Amazon Athena`를 떠올리면 된다.

![image](https://user-images.githubusercontent.com/97398071/235933334-e090bd58-8510-4db1-8119-4da0a70ae6f8.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- `S3` 버킷에 저장된 수많은 로그 파일이 저장되어 있습니다. 가능하다면 서버리스로 로그를 필터링하는 간단한 분석 작업을 수행하여 허용되지 않은 작업을 시도한 사용자를 찾으려고 합니다. 이런 경우에 사용할 수 있는 `AWS` 서비스는 무엇입니까?  
→ `Amazon Athena`


### 2. Amazon Athena – Performance Improvement
- `Athena`의 성능은 향상 가능하다. 이 부분이 출제될 수 있다.
- 비용을 지불할 때 스캔된 데이터의 TB당 가격을 지불하므로 데이터를 적게 스캔할 수 있도록 해야한다.
- Use columnar data for cost-savings (less scan)
~~~
- 첫 번째 방법은 열 기반 데이터 유형을 사용하는 것이다. 열 기반 데이터 유형을 사용하면 필요한 열만 스캔하므로 비용을 절감할 수 있다.

- Apache Parquet or ORC is recommended
→ 그렇기 때문에 Apache Parquet 또는 ORC가 추천된다.

- Huge performance improvement
→ 이 형식들을 사용하면 성능이 크게 향상된다.

- Use Glue to convert your data your Parquet or ORC
→ 파일을 Apache Parquet나 ORC 형식으로 가져오려면 Glue 등 ETL 서비스를 사용해야 한다. Glue는 CSV와 Parquet간의 데이터를 변환하는데 매우 유용하다.
~~~

- Compress data for smaller retrievals (bzip2, gzip, lz4, snappy, zlip, zstd…)  
→ 두 번째 방법은 압축이다. 데이터를 압축하면 더 적게 검색할 수 있다.

- Partition datasets in S3 for easy querying on virtual columns
~~~
- 데이터 세트를 분할할 수 있다. S3 버킷에 있는 전체 경로를 슬래시로 분할해 S3 내부의 데이터를 정리하고 분할할 수 있다.
- 데이터를 쿼리할 때 S3의 어떤 경로로 데이터를 스캔할지 정확히 알 수 있다.
- s3://yourBucket/pathToTable
                 /<PARTITION_COLUMN_NAME>=<VALUE>
                   /<PARTITION_COLUMN_NAME>=<VALUE>
                     /<PARTITION_COLUMN_NAME>=<VALUE>
                       /etc…
~~~

- Example: s3://athena-examples/flight/parquet/year=1991/month=1/day=1/  
→ `S3`에서 데이터를 조회할 때 특정 연월일 데이터를 정확히 어떤 폴더에서 가져와야 하는지 알 수 있어 데이터의 서브셋만 복구하면 된다. 

- Use larger files (> 128 MB) to minimize overhead  
→ 큰 파일을 사용해서 오버헤드를 최소화하면 성능을 향상시킬 수 있다. `S3`에 작은 파일이 너무 많으면 `Athena`는 큰 파일이 있을 때보다 성능이 떨어진다. 파일이 클 수록 스캔과 검색이 쉬우므로 `128MB` 이상의 파일을 사용해야 한다.

### 3. Amazon Athena – Federated Query
- `Amazon Athena`의 다른 기능으로 연합 쿼리가 있다.

- Allows you to run SQL queries across data stored in relational, non-relational, object, and custom data sources (AWS or on-premises)  
→ `Athena`는 `S3` 뿐만 아니라 어떤 곳의 데이터도 쿼리할 수 있다. 관계형 데이터베이스나 비관계형 데이터베이스, 객체, 사용자 지정 데이터 원본이 이에 포함된다.

- Uses Data Source Connectors that run on AWS Lambda to run Federated Queries (e.g., CloudWatch Logs, DynamoDB, RDS, …)  
→ `Data Source Connectors`는 `S3` 이외의 데이터 소스에서 데이터를 조회할 때 사용된다. 이 데이터의 원본 커넥터는 람다 함수이며 다른 서비스에서 연합 쿼리를 실행한다. `CloudWatch Logs`, `DynamoDB`, `RDS` 등 서비스에서 지원되며 매우 강력하다.

- Store the results back in Amazon S3  
→ 쿼리 결과는 사후 분석을 위해 `Amazon S3` 버킷에 저장할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/235933923-ced44246-8797-467e-9853-8d1603f59535.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
