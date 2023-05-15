## Amazon Keyspaces
### 1. Amazon Keyspaces (for Apache Cassandra)
- Apache Cassandra is an open-source NoSQL distributed database  
→ `Keyspaces`는 `AWS`의 관리형 `Apache Cassandra`를 보조한다. `Cassandra`는 오픈 소스 `NoSQL` 분산 데이터베이스이다. `Apache Cassandra`가 나오면 이 데이터베이스만 떠올리면 된다.

- A managed Apache Cassandra-compatible database service  
→ `Keyspaces`를 사용하면 클라우드에서 `AWS`가 `Cassandra`를 직접 관리해준다.

- Serverless, Scalable, highly available, fully managed by AWS  
→ 서버리스 서비스이며 확장성과 가용성이 높은 `AWS`의 완전 관리형 서비스이다.

- Automatically scale tables up/down based on the application’s traffic  
→ 애플리케이션 트래픽에 따라 테이블을 자동으로 확장하거나 축소한다.

- Tables are replicated 3 times across multiple AZ  
→ 테이블 데이터는 여러 `AZ`에 걸쳐 세 번 복제된다.

- Using the Cassandra Query Language (CQL)  
→ `Keyspaces`에서 쿼리를 수행하려면 `Cassandra Query Language, CQL`를 사용해야 한다.

- Single-digit millisecond latency at any scale, 1000s of requests per second  
→ 어떤 규모에서도 지연 시간이 10밀리초 미만으로 짧고, 초당 수천 건의 요청을 처리한다.

- Capacity: On-demand mode or provisioned mode with auto-scaling  
→ `DynamoDB`처럼 `On-demand mode`, `provisioned mode` 두 개의 용량 모드가 존재한다. `DynamoDB`와 동일하다.

- Encryption, backup, Point-In-Time Recovery (PITR) up to 35 days  
→ 암호화와 백업 기능을 제공하고 최대 35일까지의 지정 시간 복구 기능을 제공한다.

- Use cases: store IoT devices info, time-series data, …  
→ 사용 사례로는 `IoT` 장치 정보와 `time-series` 데이터 저장 등이 있다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---