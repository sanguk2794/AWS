## DMS 
### 1. DMS – Database Migration Service
- 온프레미스 시스템에서 `AWS` 클라우드로 데이터베이스를 마이그레이션 하고 싶다고 가정했을 때 데이터베이스 마이그레이션 서비스로 `DMS`를 사용해야 한다.

- Quickly and securely migrate databases to AWS, resilient, self healing  
→ 빠르고 안전한 데이터베이스 마이그레이션 서비스이다. 복원성이 좋고 자가 복구가 된다는 장점이 있다.

- The source database remains available during the migration  
→ 마이그레이션을 하는 과정에서도 소스 데이터베이스를 사용 가능하다.

- Supports:
~~~
- Homogeneous migrations: ex Oracle to Oracle
→ 동종 마이그레이션을 지원한다. 온프레미스 Oracle에서 RDS Oracle로 데이터의 마이그레이션이 가능하다.

- Heterogeneous migrations: ex Microsoft SQL Server to Aurora
→ 이기종 마이그레이션을 지원한다. 예를 들면 Microsoft SQL Server에서 Aurora로 마이그레이션이 가능하다.
~~~

- Continuous Data Replication using CDC  
→ `CDC, 변경 데이터 캡처`를 사용한 지속적 데이터 복제를 지원한다.

- You must create an EC2 instance to perform the replication tasks  
→ 마지막으로 `DMS`를 사용하려면 `EC2` 인스턴스를 생성해서 복제를 처리하도록 해야한다.

- 데이터 마이그레이션의 시나리오는 다음과 같다.
~~~
- 소스 데이터베이스는 온프레미스에 있고 RDS로 데이터를 마이그레이션하고자 한다.
- DMS 소프트웨어가 있는 EC2 인스턴스를 실행한다. DMS가 배포되어 있는 EC2 인스턴스는 소스 데이터베이스로부터 데이터를 지속적으로 풀한다.
- 대상 RDS 데이터베이스로 데이터를 마이그레이션 한다.
~~~

- `AWS Database Migration Service`를 사용하면 가동 중지 시간이 거의 없이 데이터베이스를 `AWS`로 마이그레이션할 수 있다.

### 2. DMS Sources and Targets
#### 1. SOURCES
- On-Premise and EC2 instances databases: Oracle, MS SQL Server, MySQL, MariaDB, PostgreSQL, MongoDB, SAP, DB2  
→ 온프레미스 데이터베이스 인스턴스가 포함된다.

- Azure: Azure SQL Database  
→ 마이크로소프트 `Azure SQL`의 마이그레이션 또한 가능하다.
 
- Amazon RDS: all including Aurora  
→ `RDS`는 오로라와 `Amazon S3`를 포함한 모든 데이터베이스가 포함된다. `S3`로 데이터를 전송할 때 파일은 `.csv` 형태로 마이그레이션된다.

- Amazon S3

#### 2. TARGETS
- On-Premise and EC2 instances databases: Oracle, MS SQL Server, MySQL, MariaDB, PostgreSQL, SAP  
→ 온프레미스 데이터베이스 인스턴스가 포함된다.

- Amazon RDS
- Amazon Redshift
- Amazon DynamoDB
- Amazon S3
- ElasticSearch Service
- Kinesis Data Streams
- DocumentDB

### 3. AWS Schema Conversion Tool (SCT)
- 소스 데이터베이스와 대상 데이터베이스의 엔진이 다른 경우에는 `Schema Conversion Tool, SCT`를 사용해야 한다.

- Convert your Database’s Schema from one engine to another  
→ 데이터베이스의 스키마를 다른 엔진으로 변환해 준다. 핵심은 소스 데이터베이스가 대상 데이터베이스와 다른 엔진을 쓴다는 것이다.

- Example OLTP: (SQL Server or Oracle) to MySQL, PostgreSQL, Aurora  
→ `OLTP`를 사용한다면 `SQL` 서버나 `Oracle`을 `MySQL`, 또는 `PostgreSQL`, 오로라로 마이그레이션 가능하다.

- Example OLAP: (Teradata or Oracle) to Amazon Redshift  
→ `OLAP`에 사용되는 `Amazon Redshift`로의 변환도 가능하다.

- You do not need to use SCT if you are migrating the same DB engine
~~~
- 시험을 위해 알아야 할 것은 같은 엔진을 쓰는 데이터베이스의 마이그레이션에는 SCT가 필요없다는 것이다.
- Ex: On-Premise PostgreSQL => RDS PostgreSQL
- The DB engine is still PostgreSQL (RDS is the platform)
~~~

- 데이터의 마이그레이션에는 어느 정도 시간이 걸린다. 그러므로 긴급하게 온프레미스 `RDS`를 `AWS`로 마이그레이션한다면 `SCT`를 사용하는 것이 적절하지 않다.

### 4. DMS - Continuous Replication
- `DMS`에 지속적인 복제를 설정할 수 있다.

- 소스로 `Oracle` 데이터베이스, 대상으로 `MySQL DB`의 `Amazon RDS` 데이터베이스를 사용한다고 가정할 때의 시나리오이다.
~~~
- 이형의 데이터베이스이므로 SCT가 필요하다. 온프레미스에 AWS SCT를 설치해서 서버를 구축하는 것이 최적이다.
- MySQL을 실행하는 Amazon RDS 데이터베이스에 맞도록 스키마 변환을 수행한다.
- 그러면 DMS 복제 인스턴스를 설정해 풀로드(full load)와 CDC를 사용할 수 있고 지속적 복제가 생긴다.
~~~

- CDC = Change Data Capture

![image](https://github.com/sanguk2794/AWS/assets/97398071/34809fd9-42f0-463d-b72b-85fa6709dbf2)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 5. RDS & Aurora MySQL Migrations
- RDS MySQL to Aurora MySQL
~~~
- Option 1: DB Snapshots from RDS MySQL restored as MySQL Aurora DB
→ RDS MySQL 데이터베이스의 스냅샷을 생성해서 MySQL Aurora 데이터베이스에서 복원할 수 있다. 이 때에는 다운 타임이 발생한다.

- Option 2: Create an Aurora Read Replica from your RDS MySQL, and when the replication lag is 0, promote it as its own DB cluster (can take time and cost $)
→ RDS MySQL에서 Amazon Aurora의 읽기 전용 복제본을 만들 수 있다. 복제본의 지연이 0이 되면 Aurora 복제본이 MySQL과 완전히 일치하다는 것이다. 이때 복제본을 데이터베이스 클러스터로 승격시키면 된다.
이 방법은 데이터베이스 스냅샷보다는 시간이 많이 걸리고 복제본 생성과 관련한 네트워크 비용도 발생할 수 있다.
~~~

- External MySQL to Aurora MySQL
~~~
- Option 1:
- Use Percona XtraBackup to create a file backup in Amazon S3
- Create an Aurora MySQL DB from Amazon S3
→ Percona XtraBackup 기능을 사용하여 백업할 수 있다. 백업 파일을 생성하여 Amazon S3에 두면 Amazon Aurora의 기능을 사용해서 새로운 Aurora MySQL DB 클러스터로 백업 파일을 가져올 수 있다.

- Option 2:
- Create an Aurora MySQL DB
- Use the mysqldump utility to migrate MySQL into Aurora (slower than S3 method)
→ `mysqldump` 기능을 `MySQL` 데이터베이스에서 실행하여 `Amazon Aurora` 데이터베이스로 출력값을 전송할 수 있다. 이 경우에는 시간이 많이 드는 대신 Amazon S3를 사용하지 않는다.
~~~

- Use DMS if both databases are up and running  
→ `Amazon DMS`를 이용해서 두 데이터베이스가 가동되는 채로 데이터베이스 간 지속적인 마이그레이션을 수행하는 방법도 있다.

- RDS PostgreSQL to Aurora PostgreSQL
~~~
- Option 1: DB Snapshots from RDS PostgreSQL restored as PostgreSQL Aurora DB
→ RDS PostgresSQL 데이터베이스의 스냅샷을 생성해서 PostgresSQL Aurora 데이터베이스에서 복원할 수 있다. 이 때에는 다운 타임이 발생한다.

- Option 2: Create an Aurora Read Replica from your RDS PostgreSQL, and when the replication lag is 0, promote it as its own DB cluster (can take time and cost $)
→ RDS PostgresSQL에서 Amazon Aurora의 읽기 전용 복제본을 만들 수 있다. 복제본의 지연이 0이 되면 Aurora 복제본이 PostgresSQL과 완전히 일치하다는 것이다. 이때 복제본을 데이터베이스 클러스터로 승격시키면 된다.
이 방법은 데이터베이스 스냅샷보다는 시간이 많이 걸리고 복제본 생성과 관련한 네트워크 비용도 발생할 수 있다.
~~~

- External PostgreSQL to Aurora PostgreSQL
~~~
- Create a backup and put it in Amazon S3
- Import it using the aws_s3 Aurora extension
→ 백업을 생성한 후 해당 백업을 Amazon S3에 두고 aws_s3 Aurora 확장자를 사용해서 새로운 데이터베이스를 생성할 수 있다.
~~~

- Use DMS if both databases are up and running  
→ `Amazon DMS`를 이용해서 두 데이터베이스가 가동되는 채로 데이터베이스 간 지속적인 마이그레이션을 수행하는 방법도 있다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---