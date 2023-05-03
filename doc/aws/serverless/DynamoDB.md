## Amazon DynamoDB
### 1. Amazon DynamoDB
- Fully managed, highly available with replication across multiple AZs  
→ 완전 관리형 데이터베이스이다. 데이터가 다중 `AZ`간에 복제되므로 가용성이 높다.

- NoSQL database - not a relational database - with transaction support  
→ 클라우드 데이터베이스이며, `AWS`의 독점 서비스이다. 트랜잭션이 지원되는 `NoSQL` 데이터베이스이다. 

- Scales to massive workloads, distributed database  
→ 방대한 워크로드로 확장이 가능하다. 데이터베이스가 내부에서 분산되기 때문이다.

- Millions of requests per seconds, trillions of row, 100s of TB of storage  
→ 초당 수백만 개의 요청을 처리하며, 수조 개의 행, 수백 `TB`의 스토리지를 가질 수 있다.

- Fast and consistent in performance (single-digit millisecond)  
→ 성능이 한 자릿수 밀리초이며 일관성 또한 높다. 성능이 뛰어나다. 인식해두자.

- Integrated with IAM for security, authorization and administration  
→ 보안과 관련된 모든 기능은 `IAM`과 통합되어 있다. 보안, 권한 부여, 관리 기능이 포함된다.

- Low cost and auto-scaling capabilities  
→ 비용이 적게 들고 오토 스케일링 기능이 탑재되어 있다.

- No maintenance or patching, always available  
→ 유지 관리나 패치 없이도 항상 사용할 수 있다. 데이터베이스의 프로비저닝이 필요없다

- Standard & Infrequent Access (IA) Table Class  
→ 테이블 클래스는 두 종류이다. 액세스가 빈번한 데이터에는 `Standard` 클래스에, 액세스가 빈번하지 않은 데이터는 `IA` 테이블 클래스에 저장한다.

- 모든 레코드에 대해 같은 속성을 지정할 필요가 없다. 레코드마다 속성이 달라도 괜찮다.

### 2. DynamoDB - Basics
- DynamoDB is made of Tables  
→ `DynamoDB`는 테이블로 구성되며 데이터베이스를 생성할 필요가 없다.

- Each table has a Primary Key (must be decided at creation time)  
→ `DynamoDB`에 테이블을 생성하면 각 테이블에 기본 키가 부여된다. 그리고 기본 키는 생성시 결정된다.

- 기본키는 `Partition key`와 `Sort key`를 결합한 복합키이다. 이 중 `Sort key`는 `optional`이다.

- Each table can have an infinite number of items (= rows)  
→ 각 테이블에 데이터를 추가한다. 레코드를 무한히 추가할 수 있다.

- Each item has attributes (can be added over time – can be null)  
→ 각 항목은 속성을 가지며 속성은 열에 표시된다. 이 속성 값은 나중에 추가할 수 있고, `null`이 될 수도 있다.

- Maximum size of an item is 400KB  
→ 항목의 최대 크기는 `400KB`이다. 따라서 큰 객체를 저장할 때에는 적합하지 않다.

- Data types supported are:
~~~
- Scalar Types – String, Number, Binary, Boolean, Null
- Document Types – List, Map
- Set Types – String Set, Number Set, Binary Set
~~~

- Therefore, in DynamoDB you can rapidly evolve schemas
→ 데이터의 유형과 구성 면에서 스키마를 빠르게 전개해야 할 때 `DynamoDB`를 선택하면 좋다. 중요하다.

![image](https://user-images.githubusercontent.com/97398071/235818985-16633634-fef2-4ffa-b3c3-c398304837f9.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- DynamoDB 표를 생성하기 전에는 DynamoDB 테이블이 실행될 EC2 인스턴스를 프로비저닝해야 합니다.  
→ 아니다. `DynamoDB`는 프로비저닝, 패치, 혹은 관리할 서버가 없고 설치, 유지 및 운용해야 할 소프트웨어가 없는 서버리스 서비스이다. 
`DynamoDB`는 용량 조정 및 성능 유지를 위한 테이블의 확장 및 축소를 자동으로 스케일링한다. 이는 프로비저닝(`RCU` & `WCU` 지정) 및 온디맨드 (사용한 만큼의 비용 책정) 용량 모드 모두를 제공한다.

- DynamoDB 테이블 내 항목의 최대 크기는 ...................입니다.  
→ `400KB`

### 3. DynamoDB – Read/Write Capacity Modes
- Control how you manage your table’s capacity (read/write throughput)  
→ `DynamoDB`를 사용하려면 읽기/쓰기 용량 모드를 설정해야 한다. 테이블 관리 모드를 제어하는 데 두 가지 방식이 있다.

- Provisioned Mode (default)  
→ 기본 설정이다. 미리 용량을 프로비저닝한다. 로드를 예측할 수 있고 서서히 전개되며 비용 절감을 원할 때 적합하다.
~~~
- You specify the number of reads/writes per second
→ 초당 읽기/쓰기 요청 수를 예측해서 미리 지정하면 그것이 테이블의 용량이 된다.

- You need to plan capacity beforehand, pay for provisioned Read Capacity Units (RCU) & Write Capacity Units (WCU)
→ 미리 용량을 계획하고 프로비저닝된 RCU와 WCU만큼의 비용을 지불하는 방식이다.

- Possibility to add auto-scaling mode for RCU & WCU
→ 미리 용량을 계획한 경우에도 오토 스케일링이 가능하다. RCU와 WCU에 따라 RCU 또는 WCU가 자동적으로 늘어나거나 줄어든다. 
~~~

- On-Demand Mode
~~~
- Read/writes automatically scale up/down with your workloads
→ 읽기와 쓰기 용량이 워크로드에 따라 자동적으로 확장된다.

- No capacity planning needed
→ 미리 용량 계획을 할 필요가 없으므로, RCU, WCU의 개념 자체가 없다.

- Pay for what you use, more expensive ($$$)
→ 정확히 사용한 만큼 비용을 지불한다. 모든 읽기와 쓰기에 대해 비용을 지불해야 한다.

- Great for unpredictable workloads, steep sudden spikes
→ 비싼 솔루션이지만, 워크로드를 예측할 수 없거나 급격히 증가하는 경우에 유용하다.
~~~

- 수천 개의 트랜잭션에서 수백만 개의 트랜잭션으로 1분 이내에 확장해야 하는 애플리케이션이 있다면 빠르게 확장되지 않는 프로비저닝 모드는 적합하지 않다.

- 또, 트랜잭션이 없거나 하루에 많아야 4~5회밖에 되지 않는 워크로드에서도 온디맨드 모드가 더욱 적합하다. 어떤 모드가 더 적합한지 선택할 수 있도록 준비해야 한다.

- DynamoDB 테이블을 10 `RCU`와 10 `WCU`로 프로비저닝했습니다. 한 달 후, 더 많은 읽기 트래픽을 처리하기 위해 `RCU`를 증가시키려 합니다. 어떻게 해야 할까요?
→ `RCU`는 증가, `WCU`는 동일하게 유지

- 여러분은 `DynamoDB`를 데이터 저장소로 활용하여 원활하고 지속적인 사용 경험을 제공하는 프로덕션 환경에서 애플리케이션을 실행하고 있습니다. 하지만 예측할 수 없는 대량의 요청을 테스트하려면 개발 모드에서도 애플리케이션을 실행해야 있습니다. 이런 경우에 제안할 수 있는 가장 경제적인 솔루션은 무엇입니까?  
→ 프로덕션 환경에서는 프로비저닝된 용량 모드 및 오토 스케일링을 사용하고 개발 환경에서는 온디맨드 용량 모드를 사용한다.

### 4. DynamoDB Accelerator (DAX)
- Fully-managed, highly available, seamless in-memory cache for DynamoDB  
→ 고가용성의 완전 관리형 인메모리 캐시이다.

- Help solve read congestion by caching  
→ `DynamoDB`에 읽기 작업이 많을 때 `DAX` 클러스터를 생성하고 데이터를 캐싱하여 읽기 혼잡을 해결한다.

- Microseconds latency for cached data  
→ `DAX`는 캐시 데이터에 대해 마이크로초 수준의 지연 시간을 제공한다. 중요한 키워드이다.

- Doesn’t require application logic modification (compatible with existing DynamoDB APIs)  
→ `DAX` 클러스터는 기존 `DynamoDB API`와 호환되므로 애플리케이션 로직을 수정할 필요가 없다.

- 5 minutes TTL for cache (default)  
→ 캐시의 기본 `TTL`은 5분이나 변경할 수 있다.

- `DynamoDB`를 데이터베이스로 사용하는 전자 상거래 웹사이트가 있습니다. 곧 크리스마스 세일 기간에 들어갈 예정이며, 굉장히 인기가 높은 몇몇 항목들이 높은 조회수를 기록할 것으로 예상하고 있습니다. 안타깝게도 작년에는 높은 트래픽의 양으로 인해 `ProvisionedThroughputExceededException` 예외 처리 오류가 발생했었습니다. 이 오류의 재발을 방지하려면 어떻게 해야 할까요?  
→ `DAX` 클러스터 생성하기

- 한 회사가 `Lambda`, `DynamoDB`, `Step Functions`으로 구성된 `AWS`에 서버리스 애플리케이션을 보유하고 있습니다. 지난달에 애플리케이션으로 들어오는 요청 개수가 증가하여 DynamoDB 비용이 증가하였고 요청 처리 속도도 느려지기 시작했습니다. 추가 조사를 진행한 결과, 요청의 대부분은 `DynamoDB` 테이블의 쿼리에 대한 읽기 요청이었습니다. 스로틀링을 방지하고 비용을 효율적으로 절감하려면 어떤 방법을 사용해야 합니까?  
→ `DAX`를 사용해 자주 요청되는 읽기 데이터를 캐싱한다.

- `AWS`에 호스팅할 예정인 웹 사이트가 현재 개발 진행 중에 있습니다. 이 웹 사이트에는 로그인한 사용자의 사용자 세션과 자동 만료 시간을 저장하고, 만료된 사용자 세션을 삭제해야 한다는 요구 사항이 있습니다. 다음 중 이런 경우에 가장 적합한 `AWS` 서비스는 무엇입니까?  
→ 사용자 세션을 `DynamoDB` 테이블에 저장하고 `TTL`을 활성화한다.

#### 1. DynamoDB Accelerator (DAX) vs. ElastiCache
- `DAX`는 `DynamoDB` 앞에 있고 개별 객체의 캐시와 쿼리와 스캔 캐시를 처리하는데 유용하다.

- 집계 결과와 같은 캐시는 `Amazon ElastiCache`가 좋고, `DynamoDB`는 대용량의 연산을 저장할 때 좋다. 두 서비스는 상호 보완적인 성격을 가진다.

- `DynamoDB`에 캐싱 솔루션을 추가할 때는 보통 `DAX`를 사용한다.

![image](https://user-images.githubusercontent.com/97398071/235819117-b6b86fe5-a183-45ff-9aa9-ae1dc6083fbc.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 5. DynamoDB – Stream Processing
- Ordered stream of item-level modifications (create/update/delete) in a table  
→ 생성, 수정, 삭제를 포함한 스트림을 생성할 수 있다.

- Use cases:
~~~
- React to changes in real-time (welcome email to users)
→ 사용자 테이블에 새로운 사용자가 등록됐을 때 환영 이메일을 보낼 수 있다. 테이블의 변경 사항에 실시간으로 반응하는 데 활용할 수 있는 것이다.

- Real-time usage analytics
→ 실시간으로 자동 분석을 수행할 수 있다.

- Insert into derivative tables
→ 파생 테이블을 삽입할 수 있다.

- Implement cross-region replication
→ 리전 간 복제를 실행할 수 있다.

- Invoke AWS Lambda on changes to your DynamoDB table
→ DynamoDB 테이블의 변경 사항에 대해 Lambda 함수를 실행할 수도 있다.
~~~

- `DynamoDB`에는 `DynamoDB Streams`, `Kinesis Data Streams`, 두 종류의 스트림 처리 방식이 있다.
- DynamoDB Streams
~~~
- 24 hours retention
→ 보존 기간이 24시간이다.

- Limited # of consumers
→ 소비자 수가 제한된다.

- Process using AWS Lambda Triggers, or DynamoDB Stream Kinesis adapter
→ 람다 트리거와 함께 사용하면 좋고, 자체적으로 읽기를 실행하려면 DynamoDB Stream Kinesis adapter를 사용한다.
~~~

- Kinesis Data Streams (newer)
~~~
- `Kinesis Data Streams`에 변경 사항을 바로 보내는 방법이다.
- 1 year retention
→ 이 스트림은 보존 기간이 1년이다.

- High # of consumers
→ 더 많은 수의 소비자 수를 갖는다.

- Process using AWS Lambda, Kinesis Data Analytics, Kinesis Data Firehose, AWS Glue Streaming ETL…
→ 람다, Kinesis Data Analytics, Kinesis Data Firehose, AWS Glue Streaming ETL 등 데이터를 처리하는 방법이 훨씬 많다.
~~~

- 애플리케이션에 쿼리를 수행하면 이 결과가 `DynamoDB Streams` 또는 `Kinesis Data Streams`에 전송된다.
- `Kinesis Data Streams`를 사용하면 `Kinesis Data Firehose`를 사용할 수 있다.
- 분석 목적으로 데이터를 `Amazon Redshift`로 전송하고, 데이터를 아카이빙하려면 `S3`로 전송한다. `Amazon OpenSearch`로 보내면 인덱싱이나 검색이 가능하다. 
- `DynamoDB` 스트림을 사용하면 처리 계층을 둘 수 있다.
- `EC2` 인스턴스에서 애플리케이션을 실행하려면 `KCL Adapter`나 람다 함수를 사용한다.
- 처리 계층에서 `SNS`로 알림을 보내거나 `DynamoDB` 테이블을 필터링하거나 변환할 수 있다. `Amazon OpenSearch`로 보내어 처리 계층의 데이터를 전송할 수도 있다.
- 이외에도 여러 변형이 가능하다.

![image](https://user-images.githubusercontent.com/97398071/235819238-ab12bbf2-8a7b-49cf-b11b-3ed08dbdacca.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- DynamoDB를 데이터 저장소로 사용하는 모바일 애플리케이션을 개발했습니다. 신규 사용자가 가입한 후 환영 이메일을 자동으로 보내려고 합니다. 이를 달성하는 가장 효율적인 방법은 무엇일까요?  
→ `DynamoDB Streams`을 활성화하고 이메일을 보내기 위해 람다 함수를 호출하도록 구성, `DynamoDB Streams`을 사용하면, `DynamoDB` 테이블의 항목 레벨 수정의 시간 순서를 파악할 수 있습니다. 이는 `AWS Lambda`에 통합되어 있기 때문에, 이벤트에 실시간으로 자동 응답하는 트리거를 생성할 수 있다.

### 6. DynamoDB Global Tables
- 여러 리전 간에 복제가 가능한 테이블이이며, 두 테이블간의 양방향 복제가 가능하다.

- Make a DynamoDB table accessible with low latency in multiple-regions  
→ 복수의 리전에서 짧은 지연 시간으로 액새스할 수 있게 도와준다.

- Active-Active replication  
→ 다중 활성 복제가 가능하다. 애플리케이션이 모든 리전에서 테이블을 읽고 쓸 수 있다는 의미이다.

- Applications can READ and WRITE to the table in any region  
→ 글로벌 테이블을 활성화하려면 `DynamoDB` 스트림을 활성화해야 한다.

![image](https://user-images.githubusercontent.com/97398071/235819292-ed7a05f0-6ce0-47c0-9d56-fc34cbbe01e6.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 7. DynamoDB – Time To Live (TTL)
- Automatically delete items after an expiry timestamp  
→ 만료 타임스탬프가 지나면 자동으로 항목을 삭제하는 기능이다.

- Use cases: reduce stored data by keeping only current items, adhere to regulatory obligations, web session handling…  
→ 최근 항목만 저장하도록 하거나, 2년 후 데이터를 삭제해야한다는 규정을 따라야 할 때 사용한다. 시험에서 자주 등장하는 사례는 웹 세션 핸들링이다.

![image](https://user-images.githubusercontent.com/97398071/235819346-202eb705-bfae-4316-9c29-f60c3a3008d4.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 8. DynamoDB – Backups for disaster recovery
- Continuous backups using point-in-time recovery (PITR)  
→ 재해 복구에도 `DynamoDB`를 활용할 수 있다. `지정 시간 복구, PITR`를 이용하여 지속적인 백업을 할 수 있다.
~~~
- Optionally enabled for the last 35 days
→ 활성화를 선택하면 35일 동안 지속된다.

- Point-in-time recovery to any time within the backup window
→ 활성화하면 백업 기간 내에는 언제든 지정 시간 복구를 실행할 수 있다.

- The recovery process creates a new table
→ 복구를 진행할 경우 새로운 테이블을 생성한다.
~~~

- On-demand backups
~~~
- Full backups for long-term retention, until explicitely deleted
→ 지정 시간 복구보다 더 긴 백업 옵션이다. 이 백업은 직접 삭제할 때까지 보존된다.

- Doesn’t affect performance or latency
→ 온디맨드 백업은 DynamoDB의 성능이나 지연 시간에 영향을 주지 않는다.

- Can be configured and managed in AWS Backup (enables cross-region copy)
→ 백업을 좀 더 제대로 관리할 수 있는 방법 중 하나로 AWS Backup 서비스가 있다. 백업에 수명 주기 정책을 활성화할 수 있고, 재해 복구 목적으로 리전 간 백업을 복사할 수 있따.

- The recovery process creates a new table
→ 복구를 진행할 경우 새로운 테이블을 생성한다.
~~~

### 9. DynamoDB – Integration with Amazon S3
- Export to S3 (must enable PITR)  
→ `S3`에 테이블을 내보낼 수 있다.
~~~
- Works for any point of time in the last 35 days
→ 지정 시간 복구를 활성화해야 한다. 지정 시간 복구를 활성화한 상태이므로 최근 35일 이내 어떤 시점의 테이블도 내보낼 수 있다.

- Doesn’t affect the read capacity of your table
→ 테이블을 내보내도 테이블의 읽기 용량이나 성능에 영향을 주지 않는다.

- Perform data analysis on top of DynamoDB
→ 따라서 DynamoDB를 S3로 먼저 내보낸 후 데이터 분석을 수행할 수 있다.

- Retain snapshots for auditing
→ 감사 목적으로 스냅샷을 확보할 수 있다.

- ETL on top of S3 data before importing back into DynamoDB
→ 데이터를 DynamoDB로 다시 가져오기 전에 데이터 ETL 등 대규모 변경을 실행할 수 있다.

- Export in DynamoDB JSON or ION format
→ 내보낼 때는 DynamoDB JSON이나 ION 포멧을 이용한다.
~~~

- Import to S3
~~~
- Import CSV, DynamoDB JSON or ION format
→ S3에서 CSV, JSON, ION 형식으로 내보낸 후 새로운 DynamoDB를 생성하는 식이다.

- Doesn’t consume any write capacity
→ 쓰기 용량을 소비하지 않는다.

- Creates a new table
→ 새로운 테이블을 생성한다.

- Import errors are logged in CloudWatch Logs
→ 가져올 때 발생한 오류는 모두 CloudWatch Logs에 기록된다.
~~~

- 여러분이 `DevOps` 엔지니어로 일하고 있는 축구 회사에는 서버 쪽에 `DynamoDB` 테이블을 둔 웹 사이트가 있습니다. 현재 시청자 의견에 대한 보고서를 생성하기 위해 분석 팀과 함께 업무를 진행하고 있습니다. 분석 팀은 DynamoDB의 데이터를 JSON 포맷으로 S3 버킷에 호스팅한 다음, 거기서 작업을 시작하고 보고서를 생성하기를 원합니다. `DynamoDB` 데이터를 `JSON` 파일로 변환하는 데 가장 적합한 경제적인 방식은 무엇입니까?  
→ `DynamoDB` 테이블을 선택하고 `S3`로 내보내기를 선택한다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---