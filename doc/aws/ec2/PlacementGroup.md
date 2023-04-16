## Placement Groups
### 1. Placement Groups
- Sometimes you want control over the EC2 Instance placement strategy
→ `EC2` 인스턴스가 `AWS` 인프라에 배치되는 방식을 제어하고자 할 때 사용한다.

- That strategy can be defined using placement groups
→ `AWS`의 하드웨어와 직접적인 상호 작용을 하지는 않지만 `EC2` 인스턴스가 각각 어떻게 배치되기를 원하는지 `AWS`에게 알려주는 것이다.

- When you create a placement group, you specify one of the following strategies for the group
→ 배치 그룹을 만들 때에는 세 가지 전략을 사용할 수 있다.
~~~
- Cluster—clusters instances into a low-latency group in a single Availability Zone
→ 단일 가용 영역 내에서 지연 시간이 짧은 하드웨어 설정으로 인스턴스를 그룹화할 수 있는 클러스터 배치 그룹이 있다.
이 전략은 높은 성능을 제공하지만 위험도 또한 높다

- Spread—spreads instances across underlying hardware (max 7 instances per group per AZ)
→ 분산 배치 그룹은 인스턴스가 다른 하드웨어에 분산된다. 크리티컬 애플리케이션이 있는 경우, 이 분산 배치 그룹을 활용한다.
가용 영역별로 분산된 배치 그룹당 7개의 EC2 인스턴스만 가질 수 있다는 제한 사항이 있다.

- Partition—spreads instances across many different partitions (which rely on different sets of racks) within an AZ. 
Scales to 100s of EC2 instances per group (Hadoop, Cassandra, Kafka)
→ 분할 배치 그룹은 분산 배치 그룹과 비슷하게 인스턴스를 분하한다.
하지만, 분할 배치 그룹은 여러 파티션에 인스턴스가 분할되어 있고 이 파티션은 가용 영역 내의 하드웨어에 의존하기 때문에 다른 그룹의 실패로부터 격리되지 않는다.
~~~

### 2. Placement Groups Cluster
- 클러스터 배치 그룹의 경우, 모든 `EC2` 인스턴스가 동일한 `Rack`에 존재한다. 이는 동일한 하드웨어와 동일한 가용 영역 내에 있다는 의미이다.

![image](https://user-images.githubusercontent.com/97398071/232227585-266b9b88-c161-4cc4-9c63-a203196ad2e6.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 클러스터 배치 그룹의 특징은 다음과 같다.
~~~
- Pros: Great network (10 Gbps bandwidth between instances with Enhanced Networking enabled - recommended)
→ 물리적 거리가 가깝기에 네트워크 연결 속도가 엄청나게 빠르다는 장점이 있다.

- Cons: If the rack fails, all instances fails at the same time
→ 랙에 실패가 발생하면, 즉 하드웨어에 실패가 발생하면 모든 EC2 인스턴스가 동시에 실패한다는 문제점이 존재한다.

- Use case: 
- Big Data job that needs to complete fast
- Application that needs extremely low latency and high network throughput
→ 매우 빨리 완료되어야 할 빅데이터 작업을 수행하거나, 극히 짧은 지연 시간과 높은 네트워크 처리량을 필요로 하는 애플리케이션에 대한 요청이 있을 경우 
이런 실패의 위험을 감수하고 사용한다.
~~~

- 애플리케이션에 매우 높은 대역폭과 짧은 지연 시간이 필요할 경우 클러스터 배치 그룹은 이를 수행할 수 있는 좋은 방법이다.

#### 1. Rack
`Rack Server` 혹은 `Server Rack`이라고 불리는 랙은 하나의 물리적인 서버를 말한다.

### 3. Placement Groups Spread
- 분산 배치 그룹에서는 애플리케이션의 실패 위험을 최소화하려고 한다. 그래서 분산 배치 그룹을 선택하면 모든 `EC2` 인스턴스가 다른 하드웨어에 위치하게 된다.

![image](https://user-images.githubusercontent.com/97398071/232227611-409eb609-5093-4f13-b709-74e6ada748d8.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 분산 배치 그룹에 특징은 다음과 같다.
~~~
- Pros: Can span across Availability Zones (AZ), Reduced risk is simultaneous failure
→ 여러 가용 영역에 걸쳐 있을 수 있다. 그러므로 동시 실패의 위험이 감소한다.

- Cons: Limited to 7 instances per AZ per placement group
→ 배치 그룹의 가용 영역당 인스턴스가 7개로 제한된다. 즉 배치 그룹의 규모에 제한이 생긴다.
그래서 크기가 적당하지만, 너무 크지 않은 그런 애플리케이션에서만 사용 가능하다.

- Use case: 
- Application that needs to maximize high availability
- Critical Applications where each instance must be isolated from failure from each other
→ 가용성을 극대화하고 위험을 줄여야 하는 애플리케이션 또는 인스턴스 오류를 서로 격리해야 하는 크리티컬 애플리케이션의 경우에 선택해 사용한다.
~~~

### 4. Placement Groups Partition
- 분할 배치 그룹의 경우에는 여러 가용 영역의 파티션에 인스턴스를 분산할 수 있다. 가용 영역당 최대 7개의 파티션이 존재할 수 있다.
여러 가용 영역에 여러개의 파티션을 만들면 최대 수백 개의 `EC2` 인스턴스를 생성할 수 있다. 이것이 분산 배치 그룹과의 차이이다.

- 각 파티션은 `AWS`의 `Rack`을 나타낸다. 파티션이 많으면 인스턴스가 여러 하드웨어 랙에 분산되어 각각의 파티션들이 실패로부터 격리된다.

- Use cases: HDFS, HBase, Cassandra, Kafka
→ 파티션들 전반에 걸쳐 데이터와 서버를 퍼뜨려 두도록 파티션 인식 가능한 애플리케이션의 경우에 사용한다.

![image](https://user-images.githubusercontent.com/97398071/232227631-b194d2df-7e2f-44c8-aa08-d70f2dddb327.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 1. HDFS
`HDFS, Hadoop Distributed File System`는 수십 테라 또는 페타바이트 이상의 대용량 파일을 분산된 서버에 저장하고, 
많은 클라이언트가 저장된 데이터를 빠르게 처리할 수 있도록 설계된 파일 시스템이다.

#### 2. HBase
아파치 `HBase`는 하둡 플랫폼을 위한 공개 비관계형 분산 데이터베이스이다.

#### 3. Kafka
아파치 카프카는 아파치 소프트웨어 재단이 스칼라로 개발한 분산 데이터 스트리밍 플랫폼이다.

카프카는 빠른 속도로 대용량의 데이터를 처리하는 데 용이하며 낮은 지연 시간으로 메시지를 빠르게 처리하는 것이 가능하다. 
또한 복제, 파티셔닝 등과 같은 기능을 사용하여 카프카 시스템을 쉽게 확장시킬 수 있으며, 전달할 메시지를 파일로 저장함으로써 데이터의 안정성과 신뢰성을 높일 수 있다.

### 5. Create Placement Groups
- `EC2` → `Placement Group` → `Create Placement Group`
→ 클러스터, 분산, 파티션 전략 중 하나를 선택해 배치 그룹을 생성할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/232229116-c20f7369-56ca-4c76-810f-ccfb520e2408.png)

### 6. Set Placement group of Instance
- `EC2` → `Instances` → `Launch an instance` → `Details` → `Placement group name`
→ 미리 생성해 둔 배치 그룹 전략 중 하나를 선택할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/232228899-71a604b5-a0dc-4aac-b99f-246bda9d8fad.png)


---
#### ▶ Reference
- [아파치 카프카](https://ko.wikipedia.org/wiki/아파치_카프카)
- [하둡 프로그래밍(4) – 빅데이터 – HDFS 하둡 분산 파일 시스템(1)](https://hoing.io/archives/23070)
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
