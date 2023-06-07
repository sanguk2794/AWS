## EC2 Instance Types
### 1. EC2 Instance Types - Overview
- You can use different types of EC2 instances that are optimised for different use cases [in this website](https://aws.amazon.com/ec2/instance-types/)
- AWS has the following naming convention : example → `m5.2xlarge`
~~~
- m
→ instance class
→ 인스턴스 클래스를 나타낸다. m은 범용 인스턴스임을 나타낸다.

- 5
→ generation (AWS improves them over time)
→ 인스턴스의 세대를 나타낸다.

- 2xlarge
→ size within the instance class
→ 인스턴스 클래스 내의 크기를 나타낸다. 크기가 클수록 더 많은 메모리와 CPU를 가진다.
~~~

![Instance Types](https://user-images.githubusercontent.com/97398071/229366224-3b89e0e1-acc2-4e7a-80de-0efc74013529.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. EC2 Instance Types – General Purpose
- Great for a diversity of workloads such as web servers or code repositories  
→ 범용 인스턴스는 웹 서버나 코드 저장소와 같은 다양한 작업에 적합하다. 범용 인스턴스는 `M`으로 시작된다.

- Balance between: Compute, Memory, Networking  
→ 범용 인스턴스는 컴퓨팅, 메모리, 네트워킹간의 균형이 잘 맞는다.

### 3. EC2 Instance Types – Compute Optimized
- Great for compute-intensive tasks that require high performance  
→ 고사양 작업에 최적화된 인스턴스이다. 이러한 고성능 인스턴스는 `C`로 시작하는 이름을 가지고 있다.

- Use cases
~~~
- Batch processing workloads 
→ 데이터 일괄 처리

- Media transcoding 
→ 미디어 트랜스코딩

- High performance web servers 
→ 고성능 웹 서버

- High performance computing (HPC) 
→ 고성능 컴퓨팅

- Scientific modeling & machine learning 
→ 머신 러닝

- Dedicated gaming servers 
→ 전용 게임 서버 
~~~

#### 1. Media transcoding
- 트랜스코딩은 이미 압축된 파일(일반적으로 오디오 또는 비디오)을 다른 파일 형식으로 변환하는 프로세스를 말한다. 
- 일반적으로 대상 장치가 포맷을 지원하지 않거나 기억 공간이 충분치 않은 경우 또는 비호환 또는 구식 데이터를 현대의 더 양호한 지원을 제공하는 포맷으로 변환할 때 사용한다.

#### 2. HPC
- `HPC, High Performance Computing`는 동시에 작동하는 강력한 프로세서 클러스터를 사용하여 방대한 다차원 데이터 세트(빅데이터)를 처리하고 복잡한 문제를 초고속으로 해결하는 기술이다.
- `HPC` 시스템은 일반적으로 가장 빠른 상용 데스크탑, 노트북 또는 서버 시스템보다 백만 배 이상 빠른 속도로 작동한다.

### 4. EC2 Instance Types – Memory Optimized
- Fast performance for workloads that process large data sets in memory  
→ 이 유형의 인스턴스는 메모리에서 대규모 데이터셋을 처리하는 유형의 작업에 빠른 성능을 제공한다. 메모리는 `RAM`을 뜻한다. 기본적으로 램을 나타내는 `R`로 시작되는 이름을 가지고 있으나, `X`나 대용량 메모리 `Z`도 존재한다.

- Use cases
~~~
- High performance, relational/non-relational databases
→ 인메모리 데이터베이스가 되는 고성능의 관계형 또는 비관계형의 데이터베이스

- Distributed web scale cache stores
→ ElastiCache(AWS 서비스 중 하나)를 예로 들 수 있는 분산 웹스케일 캐시 저장소

- In-memory databases optimized for BI (business intelligence)
→ 비즈니스 인텔리전스에 최적화된 인메모리 데이터베이스

- Applications performing real-time processing of big unstructured data
→ 대규모 비정형 데이터의 실시간 처리를 실행하는 애플리케이션
~~~

#### 1. in-memory database
- 디스크가 아닌 주 메모리에 모든 데이터를 보유하는 데이터베이스이다. 디스크 검색보다 자료 접근이 훨씬 빠른 것이 가장 큰 장점이다.
- 데이터 양의 빠른 증가로 데이터베이스 응답 속도가 떨어지는 문제를 해결할 수 있는 대안 중 하나이다. 전형적인 디스크 방식은 디스크에 저장된 데이터를 대상으로 쿼리를 수행하는 것에 비해 인메모리 방식은 메모리상에 색인을 넣어 필요한 모든 정보를 메모리상의 색인을 통해 빠르게 검색할 수 있기 때문이다.
- 단점이라면 매체가 휘발성이라는 것이다. `DB` 서버 전원이 갑자기 꺼져버리면 안에 있는 자료들이 초고속으로 즉시 삭제된다. 
- 그래서 보통은 로그인 세션 같은 서버가 꺼져서 날아가도 상관 없는 임시 데이터에 주로 쓰인다.
- 속도 때문에 쓰는 것이기에 압축 따윈 쓰지 않으며, 데이터에 비해 `RAM` 용량이 넉넉하지 않을 경우 가상메모리를 쓰게 되어 역효과가 일어나기도 한다.

#### 2. Business intelligence
- 비즈니스 인텔리전스(BI)는 조직이 더 나은 의사결정을 내리고 정보를 기반으로 행동을 취하고 보다 효율적인 비즈니스 프로세스를 구현할 수 있게 해주는 역량을 의미한다.

- BI 역량을 갖추면 다음과 같은 일들이 가능하다.
~~~
- 조직 전반의 최신 데이터 수집
- 이해하기 쉬운 양식으로 데이터 표시(표, 그래프 등)
- 조직 내 직원들에게 시의적절하게 데이터 전달
~~~

### 5. EC2 Instance Types – Storage Optimized
- Great for storage-intensive tasks that require high, sequential read and write access to large data sets on local storage  
→ 로컬 스토리지에서 대규모의 데이터셋에 액세스할 때 적합한 인스턴스이다. 이름은 `I`, `G` 또는 `H1`으로 시작한다. 애플리케이션에 대해 대기 시간이 짧은, 수만 단위의 무작위 `IOPS`(초당 I/O 작업 수)를 지원하도록 최적화되어 있다.

- Use cases
~~~
- High frequency online transaction processing (OLTP) systems
→ 고주파 온라인 트랜잭션 처리인 OLTP

- Relational & NoSQL databases
→ 관계형과 비관계형인 NoSQL 데이터베이스

- Cache for in-memory databases (for example, Redis)
- Data warehousing applications
- Distributed file systems
~~~

#### 1. OLTP
- `OLTP, 온라인 트랜잭션 처리`는 온라인 뱅킹, 쇼핑, 주문 입력 또는 텍스트 메시지 전송 등 동시에 발생하는 다수의 트랜잭션을 실행하는 데이터 처리 유형이다.  
→ 이러한 트랜잭션은 전통적으로 경제 또는 재무 트랜잭션이라고 칭하며, 기업이 회계 또는 보고 목적으로 언제든 정보에 액세스할 수 있도록 기록 및 보호된다.

#### 2. Data Warehouse
- 데이터 웨어하우스는 보고 및 분석을 위해 데이터베이스 테이블, `Excel` 시트 등의 구조화된 데이터와 `XML` 파일, 웹 페이지 등의 반구조화된 데이터를 저장하는 중앙 집중식 레포지토리이다.
- 많은 양의 정보를 저장할 수 있기 때문에 사용자가 데이터마이닝, 데이터 시각화 및 기타 형태의 `BI` 보고에 사용할 수 있는 풍부한 과거 데이터에 쉽게 액세스할 수 있도록 한다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---