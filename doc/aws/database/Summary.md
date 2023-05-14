## Databases
### 1. Choosing the Right Database
- We have a lot of managed databases on AWS to choose from  
→ 워크로드에 맞는 올바른 데이터베이스를 사용해야 하고, `AWS`에는 여러 관리형 데이터베이스가 있다.

- Questions to choose the right database based on your architecture:
~~~
- 무엇을 선택할지는 문제가 어떤 아키텍처를 요구하는지에 따라 달라진다.

- Read-heavy, write-heavy, or balanced workload? Throughput needs? Will it change, does it need to scale or fluctuate during the day?
→ 쓰기가 많은지 읽기가 많은지 균형이 맞는 워크로드인지에 따라 달라진다. 처리량은 얼마나 되는지, 처리량이 하루에 얼마나 달라지는지에 따라 달라진다.

- How much data to store and for how long? Will it grow? Average object size? How are they accessed?
→ 데이터가 얼마나 저장되는지, 확장은 가능한지, 평균 객체 크기는 얼마인지, 액세스 빈도와 액세스 방법은 무엇인가에 따라 달라진다.

- Data durability? Source of truth for the data ?
→ 데이터 내구성은 어떻고, 데이터의 진실 공급원은 무엇인가에 따라 달라진다.

- Latency requirements? Concurrent users?
→ 지연 시간 요구 사항이나 동시 사용자 요구 사항이 있는지에 따라 달라진다.

- Data model? How will you query the data? Joins? Structured? Semi-Structured?
→ 데이터 모델은 무엇이고 데이터는 어떻게 쿼리하는지, 조인인지 정형인지 반정형인지에 따라 달라진다.

- Strong schema? More flexibility? Reporting? Search? RDBMS / NoSQL?
→ 스키마가 강력한지, 유연성이 높은지, 데이터베이스 관련 보고가 필요한지, 검색 기능이 필요한지, 관계형 데이터베이스와 NoSQL 중 무엇이 필요한지에 따라 달라진다.

- License costs? Switch to Cloud Native DB such as Aurora?
→ 라이선스 비용이 드는지, Aurora같은 클라우드 네이티브 데이터베이스로 전환할 것인지에 따라 달라진다.
~~~

### 2. Database Types
- RDBMS (= SQL / OLTP): RDS, Aurora – great for joins  
→ `RDBMS`는 `SQL`을 사용하거나 온라인 트랜잭션을 처리할 때 사용한다. `RDS`, `Aurora`가 해당되며, 조인에 유용하다.

- NoSQL database – no joins, no SQL : DynamoDB (~JSON), ElastiCache (key / value pairs), Neptune (graphs), DocumentDB (for MongoDB), Keyspaces (for Apache Cassandra)  
→ `NoSQL`는 유연한 데이터베이스이지만 조인 기능이 없고 일반적으로 `SQL`을 사용하지 않는다. 물론 예외는 있다. `DynamoDB`, `ElastiCache`, `Neptune`, `DocumentDB`, `Keyspaces` 등이 해당된다.

- Object Store: S3 (for big objects) / Glacier (for backups / archives)  
→ 객체 스토어도 데이터베이스이다. `S3`와 `Glacier`이 여기에 포함된다.

- Data Warehouse (= SQL Analytics / BI): Redshift (OLAP), Athena, EMR  
→ `Data Warehouse`는 `SQL` 분석이나 `BI`에 사용된다. `OLAP` 유형의 데이터베이스인 `Redshift`, `Athena`, `EMR`등이 이에 해당된다.

- `OLAP`와 `OLTP`를 구분해야 한다.  
→ `OLAP`는 온라인 분석 프로세싱이며, `OLTP`는 온라인 트랜잭션 프로세싱(OLTP)이다.

- Search: OpenSearch (JSON) – free text, unstructured searches  
→ 검색 유형의 데이터베이스도 존재한다. `OpenSearch` 데이터베이스에서는 자유롭게 텍스트를 입력하거나 비정형 데이터를 검색할 수 있다.

- Graphs: Amazon Neptune – displays relationships between data  
→ 그래프 데이터베이스는 데이터 세트간의 관계를 표시한다. `Amazon Neptune`이 포함된다.

- Ledger: Amazon Quantum Ledger Database  
→ 원장 데이터베이스에는 트랜잭션 목록과 원장을 기록할 수 있다. `Amazon QLDB, Amazon Quantum Ledger Database`가 이에 포함된다.

- Time series: Amazon Timestream  
→ `Time series` 데이터베이스에는 `Amazon Timestream`가 포함된다.

### 3. Amazon RDS – Summary
- Managed PostgreSQL / MySQL / Oracle / SQL Server / MariaDB / Custom  
→ 관리형 `PostgreSQL`, `MySQL`, `Oracle`, `SQL Server`, `MariaDB`, 사용자 지정 `DB`가 포함된다.

- Provisioned RDS Instance Size and EBS Volume Type & Size  
→ `RDS`를 사용할 때는 `RDS`의 인스턴스 크기와 `EBS` 볼륨 유형 및 크기를 프로비저닝해야 한다.

- Auto-scaling capability for Storage  
→ 오토스케일링 기능이 있어도 프로비저닝은 필요하다.

- Support for Read Replicas and Multi AZ  
→ 읽기 용량 확장을 위한 읽기 전용 복제본을 지원한다. 또, 고가용성 목적으로 대기 데이터베이스를 다중 `AZ`에 제공한다. 대기 데이터베이스는 재해 발생 대기용이며 쿼리를 수행할 수는 없다.

- Security through IAM, Security Groups, KMS , SSL in transit  
→ `RDS` 데이터베이스의 보안은 `IAM`을 통해 설정할 수 있다. 사용자 이름과 비밀번호로 `DB`에 연결할 수 있게 하거나 일부 사용자에게 `IAM` 인증을 부여할 수 있다. 또, 네트워크 보안을 위해 `RDS`에 보안 그룹을 설정할 수 있다.
저장 데이터 암호화에는 `KMS`를, 전송 데이터 암호화에는 `SSL`과 `TLS`를 사용할 수 있다.

- Automated Backup with Point in time restore feature (up to 35 days)  
→ 자동 백업 옵션이 있다. 최대 35일까지 지원된다. 복구시에는 새로운 데이터베이스가 생성된다.

- Manual DB Snapshot for longer-term recovery  
→ 장기 보존 백업이 필요한 경우에는 수동 `DB` 스냅샷을 사용하면 된다.

- Managed and Scheduled maintenance (with downtime)  
→ 유저 관리 기능을 예약할 수 있기 때문에 데이터베이스에 다운타임이 발생할 수 있다. 기본 `EC2` 인스턴스에 보안 패치를 실행할 때 등등 상황에서 발생할 수 있다.

- Support for IAM Authentication, integration with Secrets Manager  
→ `RDS` 프록시를 강제하여 `RDS`에 `IAM` 인증을 추가할 수 있다. 또, `Secrets Manager`와 통합하여 `DB` 자격 증명을 관리할 수도 있다.

- RDS Custom for access to and customize the underlying instance (Oracle & SQL Server)  
→ 기본 인스턴스에 액세스하고 사용자 지정하기 위해서는 `RDS` 사용자 지정 옵션을 사용해야 한다. `Oracle`과 `SQL Server` 유형의 데이터베이스에서만 지원된다.

- Use case: Store relational datasets (RDBMS / OLTP), perform SQL queries, transactions  
→ `Amazon RDS`는 관계형 데이터베이스를 저장하는데 활용된다. 이에는 `RDBMS`와 `OLTP`가 포함된다. 또, `SQL`을 실행할 수도 있고 트랜잭션에도 활용된다.

- 다음 중 `SQL` 언어 호환성 및 삽입, 업데이트, 삭제와 같은 변환 프로세싱 기능을 통해 관계형 데이터셋의 저장을 보조해주는 데이터베이스는 무엇인가요?  
→ `Amazon RDS`

### 4. Amazon Aurora – Summary
- Compatible API for PostgreSQL / MySQL, separation of storage and compute  
→ `Aurora`는 데이터베이스 엔진인 `PostgreSQL`, `MySQL`과 호환되는 `API`이다. 컴퓨팅과 스토리지가 분리된 특별한 서비스이다.

- Storage: data is stored in 6 replicas, across 3 AZ – highly available, self-healing, auto-scaling  
→ 스토리지의 경우 기본적으로 데이터를 세 가용 영역에 걸쳐 여섯 개의 복제본에 저장한다. 이 설정은 바꿀 수 없다. 가용성이 아주 높은 것이다.
뿐만 아니라 스토리지에 문제가 발생하면 백그라운드에서 자가 복구 처리가 이루어진다. 또 오토 스케일링 기능이 내장되어 있어 스토리지 확장도 문제없다.

- Compute: Cluster of DB Instance across multiple AZ, auto-scaling of Read Replicas  
→ 컴퓨팅의 경우 클러스터화된 실제 데이터베이스 인스턴스를 여러 가용 영역에 걸쳐 저장할 수도 있다.

- Cluster: Custom endpoints for writer and reader DB instances  
→ 데이터베이스 인스턴스 클러스터가 있다. 읽기와 쓰기를 할 때 사용자 지정 엔드 포인트인 리더 엔드 포인트와 라이터 엔드 포인트가 필요하다.

- Same security / monitoring / maintenance features as RDS  
→ `RDS`와 동일한 보안 모니터링, 유지 관리 기능을 가진다.

- Know the backup & restore options for Aurora  
→ `Aurora`의 백업 및 복구 옵션

- Aurora Serverless – for unpredictable / intermittent workloads, no capacity planning  
→ `Aurora Serverless`는 워크로드가 간헐적이거나 예측할 수 없는 경우 용량 계획을 하지 않아도 되므로 굉장히 유용하다.

- Aurora Multi-Master – for continuous writes failover (high write availability)  
→ 쓰기 고가용성을 위해 쓰기 장애 조치가 지속적으로 필요할 때 `Aurora Multi-Master`를 사용하면 `DB` 인스턴스 여러 개가 스토리지에 쓰기를 할 수 있도록 설정할 수 있다.

- Aurora Global: up to 16 DB Read Instances in each region, < 1 second storage replication  
→ 글로벌 데이터베이스를 원한다면 `Aurora Global`을 사용할 수 있다. 최대 16개의 데이터베이스 읽기 전용 인스턴스를 제공하고 리전 간 스토리지 복제에 걸리는 시간은 일반적으로 1초 미만이다. 중요하다.
또, 기존 리전에 문제가 발생하면 보조 리전을 새 기본 리전으로 승격시킬 수 있다.

- Aurora Machine Learning: perform ML using SageMaker & Comprehend on Aurora  
→ `Aurora Machine Learning`을 사용하면 `SageMaker`와 `Comprehend on Aurora`를 사용해 머신 러닝을 수행할 수 있다.

- Aurora Database Cloning: new cluster from existing one, faster than restoring a snapshot  
→ 프로덕션 데이터베이스로부터 테스트 데이터베이스나 스테이징 데이터베이스를 만들려면 `Aurora Database Cloning` 기능을 사용해 기존 클러스터에서 새 `Aurora` 클러스터를 만들면 된다. 이는 스냅샷을 사용해 복구하는 것보다 훨씬 빠르다.

- Use case: same as RDS, but with less maintenance / more flexibility / more performance / more features  
→ 사용 사례는 `RDS`와 기본적으로 같다. 하지만 `Aurora`는 유지, 관리할 부분이 적고 유연성이 높으며 성능도 더 뛰어나고 내장된 기능도 더 많다.

- 여러분은 온라인 트랜잭션 프로세싱(OLTP)을 수행하려 합니다. 이 작업에는 오토 스케일링 기능이 내장되어 있고, 기반 스토리지에 대해 최대 복제본 수를 제공하는 데이터베이스를 사용하고자 합니다. 이 경우, 다음 중 어떤 AWS 서비스를 추천할 수 있을까요?  
→ `Aurora`

### 5. Amazon ElastiCache – Summary
- Managed Redis / Memcached (similar offering as RDS, but for caches)  
→ 관리형 `Redis`, `Memcached`로 `RDS`와 비슷한 기능을 제공하지만 캐싱 작업에 사용된다.

- In-memory data store, sub-millisecond latency  
→ 인메모리 데이터 스토어로 데이터를 읽을 때 1밀리초 미만의 지연시간을 제공한다.

- Must provision an EC2 instance type  
→ 캐시를 위한 `EC2` 인스턴스 유형을 프로비저닝해야 계속 진행할 수 있다.

- Support for Clustering (Redis) and Multi AZ, Read Replicas (sharding)  
→ `Redis`용에서는 클러스터 생성, 다중 `AZ`, 샤딩을 위한 읽기 전용 복제본을 추가할 수 있다.

- Security through IAM, Security Groups, KMS, Redis Auth  
→ `IAM`을 통한 프로비저닝한 보안과 보안 그룹, `KMS`, `Redis` `Auth`를 사용할 수 있다.

- Backup / Snapshot / Point in time restore feature  
→ 백업 및 스냅샷, 지정 시간 복구 기능을 사용할 수 있다.

- Managed and Scheduled maintenance  
→ 관리형 스케줄 유지관리가 가능하다.

- Requires some application code changes to be leveraged  
→ `ElastiCache`를 사용해 `RDS`가 결합된 데이터베이스에서 캐싱 작업을 수행하려면 애플리케이션 코드가 `ElastiCache`를 활용하도록 코드를 수정해 주어야 한다. 중요하다.
시험에서 코드 변경이 필요없는 캐싱 솔루션을 물어본다면 `ElastiCache`는 선택하면 안 된다.

- Use Case: Key/Value store, Frequent reads, less writes, cache results for DB queries, store session data for websites, cannot use SQL.  
→ `ElastiCache`의 사용 사례에는 키-값 스토어가 있으며, 데이터베이스를 자주 읽는다면 데이터베이스 쿼리를 캐싱하는 것이 좋다. 웹사이트 사용자를 위한 세션 데이터를 저장할 수도 있다. `ElastiCache`에서는 `SQL`을 저장할 수 없다.

- 다음 중 `Redis API`와 호환이 가능한 캐싱 기능을 제공하는 `AWS` 서비스는 무엇인가요?  
→ `ElastiCache`

### 6. Amazon DynamoDB – Summary
- AWS proprietary technology, managed serverless NoSQL database, millisecond latency  
→ `AWS`의 독점 기술로 관리형 서버리스 `NoSQL` 데이터베이스이며, 밀리초 수준의 지연 시간을 제공한다.

- Capacity modes: provisioned capacity with optional auto-scaling or on-demand capacity
~~~
- 선택할 수 있는 용량 모드는 두 가지이다. 
- 첫 번째는 선택형 오토 스케일링이 탑재된 프로비저닝된 용량 모드이다. 이 모드는 점진적으로 늘어나거나 점진적으로 줄어드는 이중 워크로드가 있을 때 유용하다.
- 온디맨드 용량 모드에서는 용량을 프로비저닝 하지 않아도 괜찮다. 오토 스케일링이 자동적으로 실행되므로 워크로드를 예측하기 어려울 때나 데이터베이스의 수요가 급증할 때 유용한 모드이다.
~~~

- Can replace ElastiCache as a key/value store (storing session data for example, using TTL feature)  
→ `ElastiCache` 대신 `DynamoDB`에 키-값을 저장할 수 있다. 웹사이트의 세션 데이터를 저장하는 좋은 방법이다. `TTL` 기능과 결합하면 일정 시간 후에 레코드가 만료되게 설정할 수 있다.

- Highly Available, Multi AZ by default, Read and Writes are decoupled, transaction capability  
→ 가용성이 아주 높다. 기본적으로 다중 가용 영역에 걸쳐있기 때문이다. 읽기와 쓰기가 완전히 분리되어 있고 `DynamoDB` 테이블에서 트랜잭션을 처리할 수 있다.

- DAX cluster for read cache, microsecond read latency  
→ `DynamoDB`와 호환되는 읽기 캐시, `DAX` 클러스터를 생성할 수 있다. `DAX` 클러스터의 특징은 읽기 지연 시간이 100만분의 1초라는 것이다. 시험에 나올 수 있다.

- Security, authentication and authorization is done through IAM  
→ 보안, 인증, 권한 부여는 `IAM`을 통해 처리된다. 

- Event Processing: DynamoDB Streams to integrate with AWS Lambda, or Kinesis Data Streams  
→ `DynamoDB`에는 이벤트 처리 용량이 있어 `DynamoDB Streams`로 데이터베이스의 모든 변경 사항을 스트리밍할 수 있다. 또, 람다와 통합하는 것도 가능하다. 그러면 `DynamoDB` 테이블에서 변경이 발생할 때마다 람다가 실행될 것이다.
`DynamoDB Data Streams` 대신 `Kinesis Data Stream`을 통해 데이터를 전송할수도 있다. `Kinesis Firehose`와 같은 다른 통합 기능도 사용할 수 있어지는 이점이 있다.

- Global Table feature: active-active setup  
→ 글로벌 테이블 기능이 있다. 여러 리전간에 다중 활성 복제가 가능하다.

- Automated backups up to 35 days with PITR (restore to new table), or on-demand backups  
→ 백업 옵션은 두 가지이다. 첫 번째는 자동 백업이다. 지정 시간 복구를 활성화하면 새 `DynamoDB` 테이블에 테이블을 복구할 수 있다. 35일까지 가능하다. 백업 보유 기간을 더 길게 설정하려면 온디맨드 백업을 활성화해서 새 테이블을 복구하면 된다.

- Export to S3 without using RCU within the PITR window, import from S3 without using WCU  
→ 지정 시간 복구 기간 내에 읽기 용량 단위를 사용하지 않고 `DynamoDB` 테이블을 `Amazon S3`로 내보낼 수 있다. 

- Great to rapidly evolve schemas  
→ 시험에서 스키마를 빠르게 전개해야 하는 경우가 나온다면 유연한 데이터베이스 스키마를 사용해야 하므로 `DynamoDB`가 탁월한 선택이 된다.

- Use Case: Serverless applications development (small documents 100s KB), distributed serverless cache, doesn’t have SQL query language available  
→ `400KB` 미만의 문서를 다루는 작은 서버리스 애플리케이션 개발, 서버리스 캐시 분산 등이 있다. `DynamoDB` 또한 `SQL`을 사용할 수 없다.

- 온프레미스 `MongoDB NoSQL` 데이터베이스를 `AWS`로 이전하려 합니다. 데이터베이스 서버를 관리하고 싶지 않기 때문에 고가용성 및 높은 내구성과 신뢰성을 제공하며, 가급적이면 서버리스인 관리형 `NoSQL` 데이터베이스를 사용하려 합니다. 이런 경우, 어떤 데이터베이스를 선택해야 할까요?  
→ `DynamoDB`

- 한 기상 예보 기관은 미국 여러 도시에서 주요 기상 지표를 수집하여 키-값 쌍 형태의 데이터로 만들어 1분에 한 번씩 `AWS` 클라우드로 전송하고 있습니다. 솔루션 아키텍트로서 이 데이터를 안정적으로 처리 및 저장할 수 있는 고가용성 솔루션을 구축하는 데 어떤 `AWS` 서비스를 사용하시겠습니까? (2개를 고르시오.)  
→ `Lambda`, `DynamoDB`, `RDS`는 키-값 쌍 데이터를 저장하기에 적합하지 않으므로 이 선택지는 오답이다.

### 7. Amazon S3 – Summary
- S3 is a… key / value store for objects  
→ `S3`는 객체를 키-값으로 저장한다.

- Great for bigger objects, not so great for many small objects  
→ `S3`는 객체를 키-값으로 저장하므로 큰 객체를 저장할 때 유용하지만 여러 개의 작은 객체를 저장할 때는 유용하지 않다.

- Serverless, scales infinitely, max object size is 5 TB, versioning capability  
→ `S3`는 서버리스이며 확장성이 무한하다. 객체의 최대 크기는 `5TB`이며, 버저닝을 수행한다.

- Tiers: S3 Standard, S3 Infrequent Access, S3 Intelligent, S3 Glacier + lifecycle policy  
→ 스토리지 계층도 다양하다. `S3 Standard`, `S3 Infrequent Access`,` S3 Intelligent`, `S3 Glacier`이 있으며, 계층을 변환할 때에는 `lifecycle policy`를 사용하면 된다.

- Features: Versioning, Encryption, Replication, MFA-Delete, Access Logs…  
→ 버저닝, 암호화, 복제, 멀티 팩터 인증 삭제와 액세스 로그를 지원한다.

- Security: IAM, Bucket Policies, ACL, Access Points, Object Lambda, CORS, Object/Vault Lock  
→ 보안 기능에는 `IAM` 보안, `S3` 버킷 정책이 있다. 또, `ACL`이 있다. 액세스 포인트를 생성할 수 있다. 또, `S3 Object Lambda`를 사용해 객체를 애플리케이션에 전송하기 전에 수정할 수 있다. `CORS` 정책도 있고, `Glacier`의 객체 잠금 또는 볼트 잠금 개념도 알아야 한다.

- Encryption: SSE-S3, SSE-KMS, SSE-C, client-side, TLS in transit, default encryption  
→ 암호화 매커니즘은 총 4개가 존재한다. `S3` 내의 키를 활용하는 `SSE-S3`, `KMS` 키를 사용하는 `SSE-KMS`, `SSE-C`, 클라이언트 암호화가 존재한다.

- Batch operations on objects using S3 Batch, listing files using S3 Inventory  
→ `S3` 버킷에 있는 모든 파일을 한 번에 작업하려면 `S3 batch`를 사용해 배치 작업을 수행해야 한다. 기존 파일을 다른 버킷으로 복사하거나 메타데이터를 변경하는 등의 작업에 유용하다. 파일 목록은 `S3` 인벤토리를 사용해 생성할 수 있다.

- Performance: Multi-part upload, S3 Transfer Acceleration, S3 Select  
→ `Amazon S3`의 향상된 성능에는 파일을 병렬식으로 업로드할 수 있는 멀티파트 업로드가 있다. 또, `Amazon S3` 전송 가속화를 통해 `S3` 파일을 한 리전에서 다른 리전으로 더 빠르게 전송할 수 있다. 또, `S3 Select`로 `Amazon S3`에서 필요한 데이터만 검색할 수도 있다.

- Automation: S3 Event Notifications (SNS, SQS, Lambda, EventBridge)  
→ 자동화 관련으로는 `S3 Event Notifications`이 있다. `SQS`, `SNS`, `Lambda`, `Eventbridge` 인터페이스가 있어 새로운 객체가 생성되는 이벤트에 반응할 수 있다.

- Use Cases: static files, key value store for big files, website hosting  
→ 정적 파일, 큰 파일의 키-값 스토어 또는 웹 사이트의 호스팅이 있다.

- 각각의 크기가 `100MB`인 한 세트의 파일들이 있는데, 이 파일을 안전하고 내구성이 높은 키-값 스토어에 저장하려 합니다. 이 경우, 다음 중 어떤 `AWS` 서비스를 사용하는 게 권장될까요?  
→ `Amazon S3`

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---