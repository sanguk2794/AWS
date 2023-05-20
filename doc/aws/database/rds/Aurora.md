## Amazon Aurora
### 1. Amazon Aurora
- Aurora is a proprietary technology from AWS (not open sourced)  
→ `AWS` 고유의 기술이다. 오픈 소스는 아니지만 `postgres` 및 `MySQL`과 호환된다. `postgres Edition`, `MySQL Edition`의 두 가지 에디션이 존재한다.

- Aurora is “AWS cloud optimized” and claims 5x performance improvement over MySQL on RDS, over 3x the performance of Postgres on RDS  
→ 클라우드에 최적화되어 있다. 이외에도 여러 최적화를 통해 `RDS`의 `MySQL`보다 5배, `Postgres`보다 3배 높은 성능을 보여준다. 단순 성능 뿐 아니라 다양한 개선점들이 존재한다.

- Aurora storage automatically grows in increments of 10GB, up to 128 TB.  
→ `Aurora`의 스토리지는 자동으로 확장한다. `10GB`에서 시작하지만 데이터베이스에 더 많은 데이터를 넣을수록 자동으로 `128TB`까지 커진다.

- Aurora can have 15 replicas while MySQL has 5, and the replication process is faster (sub 10 ms replica lag)  
→ 읽기 전용 복제본의 경우 15개까지 생성 가능하다. `RDS`에서는 5개만 생성이 가능했었다. 읽기 전용 복제본의 복제 속도도 훨씬 빠르다.

- Failover in Aurora is instantaneous. It’s HA (High Availability) native.  
→ `Aurora`의 장애 조치는 즉각적이기에 다중 `AZ`보다 훨씬 빠르다. 그리고 기본적으로 클라우드 네이티브이므로 가용성이 높다.
 
- Aurora costs more than RDS (20% more) – but is more efficient  
→ 비용은 `RDS`에 비해 약 20% 정도 높지만 스케일링 측면에서는 훨씬 더 효율적이다. 특정 상황에서는 오히려 비용을 절감할 수도 있다.

- `Aurora` 인스턴스를 삭제할 때는 `Reader` 인스턴스 → `Writer` 인스턴스 → 클러스터 인스턴스의 순으로 삭제를 진행해야 한다.

### 2. Aurora High Availability and Read Scaling
- `Aurora`의 가장 중요한 특징은 높은 가용성과 읽기 스케일링이다.

- 6 copies of your data across 3 AZ  
→ `Aurora`는 3개의 `AZ`에 걸쳐 있으며 무언가를 기록할 때마다 6개의 사본을 저장한다. 따라서 `Aurora`는 가용성이 높다.
~~~
- 4 copies out of 6 needed for writes
→ 쓰기에는 6개의 사본 중 4개만 있으면 된다. 이는 AZ 하나가 작동하지 않아도 괜찮음을 뜻한다.

- 3 copies out of 6 need for reads
→ 읽기에는 6개 사본 중 3개만 있으면 된다. 읽기 가용성 또한 높다.

- Self healing with peer-to-peer replication
→ 자가 복구 과정이 있다. 일부 데이터가 손상되거나 문제가 있으면 백엔드에서 P2P 복제를 통한 자가 복구가 진행된다.

- Storage is striped across 100s of volumes
→ 단일 볼륨에 의존하지 않고 수백 개의 볼륨을 사용한다. 백엔드에서 진행되는 과정이며 리스크를 크게 감소시켜 준다.
~~~ 

![image](https://user-images.githubusercontent.com/97398071/233848289-b779f8ec-9027-4a11-b66e-649526216b96.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- One Aurora Instance takes writes (master)  
→ `RDS`의 다중 `AZ`와 유사하게 쓰기 인스턴스는 하나뿐이다. 따라서, `Aurora`에도 마스터가 존재한다.

- Automated failover for master in less than 30 seconds  
→ 마스터가 작동하지 않으면 평균 30초 이내로 장애 조치가 시작된다. 마스터에 문제가 생기면 읽기 전용 복제본 중 하나가 마스터가 되어 작동하지 않는 마스터를 대체한다.

- Master + up to 15 Aurora Read Replicas serve reads  
→ 마스터 외에 읽기를 제공하는 읽기 전용 복제본을 15개까지 생성할 수 있다. 따라서 복제본을 많이 두고 읽기 워크로드를 스케일링할 수 있다.

- Support for Cross Region Replication  
→ 이 복제본들은 리전 간 복제를 지원한다. 데이터베이스 클러스터에 다른 리전을 추가할 수 있다. 글로벌 기능을 갖추고 있는 것이다.

- `Aurora`는 언제나 마스터가 바뀔 수 있으므로 `Writer endpoint`를 제공한다. 이 `Writer endpoint`는 `DNS` 이름으로 항상 마스터를 가리킨다.
따라서, 장애 조치 후에도 클라이언트는 `Writer endpoint`와 상호작용하게 되며 올바른 인스턴스로 자동으로 리다이렉트된다. 

- 또, 복제본을 자동 스케일링해 항상 적절한 수의 읽기 전용 복제본이 존재하도록 할 수 있다. 그렇기에 애플리케이션이 복제본의 위치를 찾기 어려울 수 있는데 이를 위해 제공되는 기능이 `Reader endpoint`이다. `Writer endpoint`와 같이 연결 로드 밸런싱에 도움을 준다.

![image](https://user-images.githubusercontent.com/97398071/233848330-2e345b42-3dc4-490f-a476-96fb84e5714b.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. Features of Aurora
- `Aurora`의 특징은 다음과 같다.
~~~
- Automatic fail-over → 자동 장애 조치
- Backup and Recovery → 백업 및 복구
- Isolation and security → 격리 및 보안
- Industry compliance → 산업 규정 준수
- Push-button scaling → 자동 스케일링을 통한 버튼식 스케일링
- Automated Patching with Zero Downtime → 제로 다운타임 자동 패치 설치
- Advanced Monitoring → 고급 모니터링
- Routine Maintenance → 통상 유지 관리
- Backtrack: restore data at any point of time without using backups → 과거 어떤 시점의 데이터로도 복원 가능
~~~

#### 1. Aurora Replicas - Auto Scaling
- 복제본의 오토스케일링이 일어나면 자동으로 리더 엔드포인트가 연장 혹은 단축된다. 그 중 스케일 아웃이 일어날 경우 읽기가 좀 더 분산된 형태로 이루어진다. 이를 통해 전반적인 `CPU` 사용량 감소를 기대할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/233849527-d5f817e7-5796-4ef4-af68-832252442972.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 2. Aurora – Custom Endpoints
- Define a subset of Aurora Instances as a Custom Endpoint  
→ 일부 복제본은 다른 것보다 크기가 더 크다. 이렇게 하는 이유는 `Aurora`가 인스턴스의 서브셋을 사용자 지정 엔드포인트로 정의하기 위함이다.

- Example: Run analytical queries on specific replicas  
→ 다른 것보다 크기가 더 큰 일부 복제본에 사용자 지정 엔드포인트를 정의하고 분석 쿼리를 실행하면 보다 뛰어난 성능을 보여줄 수 있다.

- The Reader Endpoint is generally not used after defining Custom Endpoints  
→ 사용자 지정 엔드포인트를 지정한 후에는 일반적으로 리더 엔드포인트는 사용하지 않는다. 그렇다고 리더 엔드포인트가 사라지는 것은 아니다.

![image](https://user-images.githubusercontent.com/97398071/233849604-fc13913a-1deb-4abc-b0cc-c96c3b56236e.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 실제 프로젝트에서는 다양한 종류의 워크로드에 대해 사용자 지정 엔드포인트를 여러 개 만들 수 있다.

#### 3. Aurora Serverless
- Automated database instantiation and auto-scaling based on actual usage  
→ 실제 사용량에 기반한 자동 데이터베이스의 인스턴스화와 자동 스케일을 가능하게 해 준다.

- Good for infrequent, intermittent or unpredictable workloads  
→ 이는 비정기적, 간헐적, 또는 예측 불허한 워크로드에 유용하다.

- No capacity planning needed  
→ 용량 계획을 세울 필요가 전혀 없으며 각 `Aurora` 인스턴스에 대해 매 초당 비용을 지불하게 된다.

- Pay per second, can be more cost-effective  
→ 사용한 만큼만 지불하면 되므로 비용 면에서 훨씬 효율적이다. 

#### 4. Aurora Multi-Master
- In case you want immediate failover for write node (HA)  
→ 멀티 마스터는 `write node`에 대한 즉각적 장애 조치를 통해 높은 가용성을 갖추고자 할 때 유용하다.

- Every node does R/W - vs promoting a RR as the new master  
→ 멀티 마스터를 설정할 경우 `Aurora` 클러스터의 모든 노드에서 읽기 및 쓰기가 가능해진다. 기존에는 하나의 쓰기 노드만이 있고 그 노드에 장애가 발생하면 읽기 전용 복제본 하나를 새로운 마스터로 승격시켜야 했다.

![image](https://user-images.githubusercontent.com/97398071/233850178-e09375e4-45ab-4848-b789-959bd858b14d.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 5. Global Aurora
- Aurora Cross Region Read Replicas:  
→ `Aurora` 리전 간 복제본은 재해 복구에 많은 도움이 되며 간단하게 생성 가능하다.
~~~
- Useful for disaster recovery
- Simple to put in place
~~~

- Aurora Global Database (recommended):  
~~~
- 최근에는 리전 간 복제본을 만드는 것보다 Aurora Global 데이터베이스를 만드는 것이 권장된다.
- 1 Primary Region (read / write)

- Up to 5 secondary (read-only) regions, replication lag is less than 1 second
- Up to 16 Read Replicas per secondary region
→ 복제 지연이 1초 미만인 보조 읽기 전용 리전을 다섯 개까지 설정할 수 있고 각 보조 지역마다 읽기 전용 복제본을 16개까지 생성 가능하다.

- Helps for decreasing latency
→ 세계 각지에 있는 읽기 전용 복제본의 지연 시간을 단축할 수 있다.

- Promoting another region (for disaster recovery) has an RTO of < 1 minute
- Typical cross-region replication takes less than 1 second
→ 한 리전의 데이터베이스의 작동이 중단될 경우 재해 복구 목격으로 다른 지역을 승격하는데 필요한 복구 시간 목표는 1분 미만이다. 
~~~

#### 6. Aurora Machine Learning
- `Aurora`는 `AWS` 내의 머신 러닝 서비스와의 통합을 지원한다.

- Enables you to add ML-based predictions to your applications via SQL  
→ `Aurora` 머신 러닝의 개념은 `ML` 기반 예측을 `SQL` 인터페이스로 애플리케이션에 적용하는 것이다.

- Simple, optimized, and secure integration between Aurora and AWS ML services  
→ `Aurora`와 다양한 `AWS` 머신 러닝 서비스를 쉽고 최적화된 방식으로 안전하게 통합할 수 있다.

- Supported services  
~~~
- SagaMaker와 Amazon Comprehend 서비스를 지원한다. 
- SagaMaker는 백엔드에서 어떤 머신 러닝 모델이라도 사용할 수 있도록 지원해주고 Amazon Comprehend는 감정 분석을 지원해 준다.
~~~

- You don’t need to have ML experience
→ `Aurora` 머신 러닝을 사용하기 위해 머신 러닝 경험은 필요하지 않다.
 
- Use cases: fraud detection, ads targeting, sentiment analysis, product recommendations
→ 활용 예시로 감지, 맞춤 광고, 감정 분석, 제품 추천등이 있으며 모두 `Aurora` 내에서 가능하다. 

- `Application`이 `Aurora`에 아주 간단한 쿼리 (예를 들어 추천 상품은?) 를 조회하면 `Aurora`가 머신 러닝 서비스로 데이터를 전송하고, 머신 러닝 서비스는 `Aurora`로 직접 예측을 반환해 준다. 그리고 `Aurora`는 쿼리의 결과를 애플리케이션에 반환한다.

![image](https://user-images.githubusercontent.com/97398071/233850883-5884b322-6962-4dcd-b2c2-097fd8d20d30.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 4. event trigger
- `RDS 이벤트`는 `DB 인스턴스 이벤트`, `DB 파라미터 그룹 이벤트`, `DB 보안 그룹 이벤트`, `DB 스냅샷 이벤트`와 같은 운영 이벤트만 제공한다. 데이터 수정 이벤트(INSERT, DELETE 등)를 캡처하는 것은 불가능하다.
- 기본 함수 또는 저장 프로시저를 사용하면 `Amazon Aurora MySQL-Compatible Edition DB` 클러스터에서 `AWS Lambda` 함수를 호출할 수 있다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---