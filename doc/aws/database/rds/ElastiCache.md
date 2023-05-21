## Amazon ElastiCache
### 1. Amazon ElastiCache Overview
- The same way RDS is to get managed Relational Database  
→ `RDS`와 동일한 방식으로 관계형 데이터베이스를 관리할 수 있다. 

![image](https://user-images.githubusercontent.com/97398071/234034994-5aa91cd0-31fe-490e-b6c1-60029181ffd0.png)

- ElastiCache is to get managed Redis or Memcached  
→ 일래스틱 캐시는 레디스 또는 멤캐시트와 같은 캐시 기술을 관리할 수 있다.

- Caches are in-memory databases with really high performance, low latency  
→ 캐시는 높은 성능과 낮은 지연 시간을 가진 인메모리 데이터베이스이다.

- Helps reduce load off of databases for read intensive workloads  
→ 읽기 집약적인 워크로드의 부하를 줄이는데 도움을 준다. 데이터베이스가 매번 쿼리되지 않고 캐시되기 때문이다.

- Helps make your application stateless  
→ 애플리케이션 상태를 일래스틱 캐시에 저장해 애플리케이션을 무상태로 만들 수 있다. 

- AWS takes care of OS maintenance / patching, optimizations, setup, configuration, monitoring, failure recovery and backups  
→ `AWS`는 일래스틱 캐시에도 `RDS`와 동일한 유지보수를 수행한다.

- Using ElastiCache involves heavy application code changes  
→ 일래스틱 캐시를 사용할 때 애플리케이션에 대한 몇 가지 어려운 코드 변경을 요청할 수도 있다. 단순한 활성화가 아니라 캐시를 사용한다.

- `Auto-failover`를 통해 고가용성을 얻을 수 있다. 읽기 전용 복제에 대한 자동 장애 조치가 이루어지기 때문이다.

#### 1. ElastiCache Solution Architecture - DB Cache
- 일래스틱 캐시 사용을 위한 아키텍쳐의 예시로 `DB Cache`를 들 수 있다.

- Applications queries ElastiCache, if not available, get from RDS and store in ElastiCache.  
→ 일래스틱 캐시가 `RDS`와 애플리케이션 사이에 존재하고 애플리케이션은 일래스틱 캐시에 쿼리를 수행한다. 캐시 미스의 경우에는 데이터베이스에서 데이터를 가져와서 데이터베이스에서 읽는다.

![image](https://user-images.githubusercontent.com/97398071/234031615-bf7cfca3-5aa2-4fc8-b0bc-736c34b8bde4.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Helps relieve load in RDS  
→ 이는 `RDB`의 부하를 줄이는데 도움을 준다. 

- Cache must have an invalidation strategy to make sure only the most current data is used in there.  
→ 하지만 데이터를 캐시에 저장하기 때문에 캐시 무효화 전략이 있어야하며 가장 최근 데이터만 사용하는지 확인해야 한다. 이것이 캐싱 사용할 때의 가장 큰 어려움이다.

#### 2. ElastiCache Solution Architecture – User Session Store
- User logs into any of the application  
→ 사용자 세션을 저장해 애플리케이션을 무상태로 만들 수 있다.

- The application writes the session data into ElastiCache  
→ 사용자가 애플리케이션의 모든 계정에 로그인하면 애플리케이션이 일래스틱 캐시에 세션 데이터를 기록하는 것이다.

- The user hits another instance of our application  
→ 사용자가 애플리케이션의 다른 인스턴스로 리디렉션되면 애플리케이션은 일래스틱 캐시에서 직접 세션 캐시를 검색할 수 있다. 그래서 사용자는 계속 로그인한 상태로 한 번 더 로그인 할 필요가 없다.

- The instance retrieves the data and the user is already logged in  
→ 사용자의 세션 데이터를 일래스틱 캐시에 기록해서 애플리케이션을 무상태로 만든 것이다.

![image](https://user-images.githubusercontent.com/97398071/234032257-dd1c13a0-6cdc-42fd-ac92-32156a2efd82.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. ElastiCache – Redis vs Memcached
- REDIS  
~~~
- 고가용성, 백업, 읽기 전용 복제본이 레디스의 대표적 특징이다.

- Multi AZ with Auto-Failover
→ 레디스는 자동 장애 조치로 다중 AZ에 배포된다.

- Read Replicas to scale reads and have high availability
→ 읽기 전용 복제본은 읽기 스케일링에 사용되며 가용성이 높다.

- Data Durability using AOF persistence
→ 지속성으로 인한 데이터 내구성이 존재한다.

- Backup and restore features
→ 백업과 복원 기능이 있다. RDS와 유사하다.
~~~

- MEMCACHED  
→ 데이터를 손실할 수 없는 단순한 분산 캐시이다. 가용성이 높지 않고 백업과 복원 기능도 없다.
~~~
- Multi-node for partitioning of data (sharding)
→ 데이터 분할에 다중 노드를 사용한다. 이를 샤딩이라고 한다.

- No high availability (replication)
→ 가용성이 높지 않다.

- Non persistent
→ 복제가 발생하지 않는다.

- No backup and restore
→ 백업과 복원 기능도 없다.

- Multi-threaded architecture
→ 다중 쓰레드 아키텍쳐로 몇몇 샤딩과 함께 캐시에서 실행되는 여러 인스턴스가 있다. 
~~~

### 3. ElastiCache – Cache Security
- All caches in ElastiCache:
~~~
- Do not support IAM authentication
→ 일래스틱 캐시의 모든 캐시는 IAM 인증을 지원하지 않는다.

- IAM policies on ElastiCache are only used for AWS API-level security
→ 일래스틱 캐시에서 정의할 IAM 정책은 AWS API 수준 보안에만 사용된다. 이는 캐시 생성, 캐시 삭제와 같은 종류의 작업을 의미한다.
~~~

- Redis AUTH
~~~
- You can set a “password/token” when you create a Redis cluster
→ 레디스를 인증하려면 레디스 AUTH를 사용하여 레디스 클러스터를 생성할 때 비밀번호나 토큰을 설정할 수 있다. 이렇게 하면 캐시에 들어갈 때 비밀번호를 사용할 수 있다.

- This is an extra level of security for your cache (on top of security groups)
→ 이는 캐시에 사용할 수 있는 보안 그룹에 대한 추가적인 수준의 보안이다.

- Support SSL in flight encryption
→ 전송 중 암호화를 위해 SSL 보안을 지원할 수 있다.
~~~

- Memcached
~~~
- Supports SASL-based authentication (advanced)
→ 좀 더 높은 수준인 SASL 기반 인증을 지원한다.
~~~   

### 4. Patterns for ElastiCache
- 일래스틱 캐시에 데이터를 불러오는 패턴에는 세 가지가 존재한다.

- Lazy Loading: all the read data is cached, data can become stale in cache  
→ 모든 읽기 데이터가 캐시된다. 레이지 로딩의 가장 대표적 예시가 `DB Cache`이다.

- Write Through: Adds or update data in the cache when written to a DB (no stale data)  
→ 데이터가 데이터베이스에 기록될 때마다 캐시에 추가하거나 업데이트한다.

- Session Store: store temporary session data in a cache (using TTL features)  
→ 세션 저장소로 사용할 수 있다. 이 때 `Time To Live` 속성을 통해 세션을 만료시킬 수 있다.

### 5. ElastiCache – Redis Use Case
- Gaming Leaderboards are computationally complex  
→ 게임 리더보드 생성에 사용할 수 있다. 게임 어떤 시점에서든 1등, 2등, 3등의 판별이 가능하다.

- Redis Sorted sets guarantee both uniqueness and element ordering  
→ 레디스에는 고유성과 요소 순서를 모두 보장하는 정렬된 집합이라는 기능이 있다.

- Each time a new element added, it’s ranked in real time, then added in correct order  
→ 요소가 추가될 때마다 실시간으로 순위가 매겨진 다음 올바른 순서로 추가된다. 레디스 클러스터가 있는 경우 실시간으로 1, 2, 3위 플레이어가 있는 실시간 리더보드를 생성한다는 개념이다. 모든 레디스 캐시는 동일한 리더보드를 사용할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/234041990-f63d1674-2b3a-4d95-b765-06583f74d05b.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---