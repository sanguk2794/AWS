## Redshift
### 1. Redshift Overview
- Redshift is based on PostgreSQL, but it’s not used for OLTP  
→ 데이터베이스이자 분석 엔진인 `Redshift`는 `PostgreSQL` 기반이지만 `온라인 트랜잭션 처리, OLTP`에는 사용되지 않는다.

- It’s OLAP – online analytical processing (analytics and data warehousing)  
→ 온라인 분석 처리를 의미하는 `OLAP` 유형의 데이터베이스이며 분석과 데이터 웨어하우징에 사용한다.

- 10x better performance than other data warehouses, scale to PBs of data  
→ 다른 데이터 웨어하우징들과 비교했을 때 성능이 10배 이상 뛰어나다. 데이터가 `PB` 규모로 확장되므로 모든 데이터를 `Redshift`에 로드하면 빠르게 분석할 수 있다.

- Columnar storage of data (instead of row based) & parallel query engine  
→ `Redshift`의 성능 향상이 가능한데, 이는 열 기반 데이터 스토리지를 사용하기 때문이다. 행 기반이 아니라 병렬 쿼리 엔진이 있는 것이다.

- Pay as you go based on the instances provisioned  
→ `Redshift` 클러스터에서 프로비저닝한 인스턴스에 대한 비용만 지불하면 된다.

- Has a SQL interface for performing the queries  
→ 쿼리를 수행할 때 `SQL`문을 사용할 수 있다.

- BI tools such as Amazon Quicksight or Tableau integrate with it  
→ `Amazon Quicksight`같은 `BI` 도구나 `Tableau`를 `Redshift`에 통합할 수 있다.

- vs Athena: faster queries / joins / aggregations thanks to indexes  
→ `Redshift`는 먼저 모든 데이터를 `Redshift`에 로드해야 한다. `Redshift`의 쿼리가 더 빠르고 조인과 통합을 훨씬 더 빠르게 수행할 수 있는 이유는 `Athena`에는 없는 인덱스가 있기 때문이다.

- `S3`의 임시 쿼리라면 `Athena`가 좋은 사용 사례가 되지만 쿼리가 많고 복잡하며 조인하거나 집계하는 등 집중적인 데이터 웨어하우스라면 `Redshift`가 더 뛰어난 성능을 보여준다.

- 대량의 컬럼형 데이터에 대해 분석 쿼리를 효율적으로 수행할 수 있는 데이터베이스를 구성하고자 합니다. 이 데이터 웨어하우스에 `Amazon QuickSight`와 같은 보고 및 대시보드 도구를 연결해 사용하려고 합니다. 가장 적합한 `AWS` 기술을 무엇입니까?  
→ `Amazon Redshift`

- `Redshift`에서 클러스터와 데이터 저장소 사이의 모든 `COPY`와 `UNLOAD` 트래픽이 `VPC`를 통하도록 강제하는 기능은 무엇입니까?  
→ 향상된 `VPC` 라우팅

### 2. Redshift Cluster
- Leader node: for query planning, results aggregation  
→ 리더 노드는 쿼리를 계획하고 결과를 집계한다.

- Compute node: for performing the queries, send results to leader  
→ 컴퓨트 노드는 실제로 쿼리를 실행하고 결과를 리더 노드에 보낸다.

- You provision the node size in advance  
→ `Redshift` 클러스터는 노드 크기를 미리 프로비저닝해야 한다.

- You can used Reserved Instances for cost savings  
→ 비용을 절감하기 위해 예약 인스턴스를 사용할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/235944522-c1429bd1-6720-4fe7-b7eb-728aeab28481.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. Redshift – Snapshots & DR(Disasters Recovery)
- Redshift has “Multi-AZ” mode for some clusters  
→ `Redshift`에는 다중 `AZ` 모드가 없고 클러스터가 한 개의 `AZ`에 있으므로 재해 복구 전략을 적용하려면 스냅샷을 사용해야 한다.

- Snapshots are point-in-time backups of a cluster, stored internally in S3  
→ 스냅샷은 클러스터의 지정 시간 백업으로 `S3` 내부에 저장된다.

- Snapshots are incremental (only what has changed is saved)  
→ 변경된 사항만 저장되므로 많은 공간을 절약할 수 있다.

- You can restore a snapshot into a new cluster  
→ 새로운 `Redshift` 클러스터에 스냅샷을 복원할 수 있다.

- 스냅샷에는 수동화와 자동화, 두 가지 모드가 있다.
~~~
- Automated: every 8 hours, every 5 GB, or on a schedule. Set retention between 1 to 35 days
→ 스냅샷이 8시간마다 또는 5GB마다 생성되도록 예약할 수 있다. 이 때 자동화된 스냅샷의 보존 기간을 1일부터 35일까지로 설정할 수 있다.

- Manual: snapshot is retained until you delete it
→ 수동 스냅샷을 사용할 경우 직접 스냅샷을 삭제하기 전까지 스냅샷이 유지된다.
~~~

- You can configure Amazon Redshift to automatically copy snapshots (automated or manual) of a cluster to another AWS Region  
→ 자동이든 수동이든 클러스터의 스냅샷을 다른 `AWS` 리전에 자동으로 복사하도록 `Redshift`를 구성하여 재해 복구 전략을 적용할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/235944747-9835dd1a-7341-4897-98b0-3ab5ac1550ed.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 솔루션 아키텍트로서 `Redshift` 클러스터에 대한 재해 복구 계획을 수립하는 업무를 맡았습니다. 어떤 작업을 해야 합니까?  
→ 자동 스냅샷을 활성화한 다음, `Redshift` 클러스터가 스냅샷을 다른 `AWS` 리전으로 자동 복사하도록 설정한다.

### 4. Loading data into Redshift: Large inserts are MUCH better
- `Redshift`에 데이터를 삽입하는 방법에는 3가지가 있다.
~~~
- Amazon Kinesis Data Firehose
→ Firehose가 다양한 소스로부터 데이터를 받아서 Redshift에 보내는 것이다.

- S3 using COPY command
→ S3에 데이터를 로드하고 Redshift에서 복사 명령을 실행하면 IAM 역할을 사용해 S3 버킷에서 Redshift 클러스터로 데이터를 복사한다.

- JDBC Driver
~~~

![image](https://user-images.githubusercontent.com/97398071/235944996-8c516051-2a35-4b2d-acf8-b2b6e889ad4b.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 5. Redshift Spectrum
- Query data that is already in S3 without loading it  
→ `S3`에 있는 데이터를 `Redshift`를 사용해 분석은 하지만 `Redshift`에 로드는 하지 않을 수 있다.

- Must have a Redshift cluster available to start the query  
→ `Redshift Spectrum`을 사용하려면 쿼리를 실행할 수 있는 `Redshift` 클러스터가 필요하다.

- The query is then submitted to thousands of Redshift Spectrum nodes  
→ 일단 쿼리를 실행하면 `S3`에 있는 데이터에 쿼리를 실행할 수천 개의 `Redshift Spectrum` 노드에 쿼리가 제출된다.

- 이 기능을 사용하면 `Redshift`로 데이터를 로드하지 않아도 되므로 클러스터에서 프로비저닝한 것보다 더 많은 처리 능력을 활용할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/235945253-570d2563-2d5a-4b5f-8c67-8dea9d85aab2.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 한 `IT` 회사에서 `Amazon Redshift`를 사용해 유통 조직을 위한 맞춤형 데이터 웨어하우스 솔루션을 구축했습니다. 회사의 일일 분석 보고서에는 지난 1년간의 데이터만 사용되기 때문에 비용 최적화 작업의 일환으로 1년이 지난 과거 데이터를 모두 S3로 이동하려고 합니다. 하지만 분석가들은 일일 보고서와 지난 과거 데이터를 상호 참조하는 기능을 계속해서 사용하고 싶어 합니다. 회사에서는 최소한의 비용과 최소한의 노력을 들여 솔루션을 개발하기를 원합니다. 솔루션 아키텍트로서 이 사례를 해결하기 위해 어떤 방법을 제안하시겠습니까?  
→ `Redshift Spectrum`을 사용해 `S3`에 있는 과거 데이터를 가리키는 `Redshift` 클러스터 테이블을 생성한다. 분석 팀은 과거 데이터를 쿼리하여 `Redshift`의 일일 보고서와 상호 참조할 수 있다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
