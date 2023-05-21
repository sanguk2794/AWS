## EBS Volume
### 1. EBS Volume
- EBS Volumes come in 6 types  
→ `EBS` 볼륨에는 총 6개 유형이 있다.
~~~
- gp2 / gp3 (SSD): General purpose SSD volume that balances price and performance for a wide variety of workloads
→ 범용 SSD 볼륨으로 다양한 워크로드에 대해 가격과 성능의 절충안이 되어준다.

- io1 / io2 (SSD): Highest-performance SSD volume for mission-critical low-latency or high-throughput workloads
→ 최고 성능을 자랑하는 SSD 볼륨으로 미션 크리티컬하며 낮은 지연 시간이 필요한 대용량의 워크로드에 사용된다.

- st1 (HDD): Low cost HDD volume designed for frequently accessed, throughput-intensive workloads
→ 저비용의 HDD 볼륨으로 접근이 잦고 처리량이 많은 워크로드에 사용된다.

- sc1 (HDD): Lowest cost HDD volume designed for less frequently accessed workloads
→ 가장 비용이 적게 드는 HDD 볼륨으로 접근 빈도가 낮은 워크로드에 사용된다.
~~~

- EBS Volumes are characterized in Size | Throughput | IOPS (I/O Ops Per Sec)  
→ `EBS` 볼륨은 크기, 처리량, `IOPS` 등 요소에 의해 결정된다.

- Only gp2/gp3 and io1/io2 can be used as boot volumes  
→ `EC2` 인스턴스에는 `SSD` 볼륨만이 부팅 볼륨으로 사용될 수 있다. 이는 루트 `OS`가 실행될 위치에 해당한다.

### 2. EBS Volume Types Use cases General Purpose SSD
- Cost effective storage, low-latency  
→ 비용 효율적이며, 짧은 지연시간을 자랑하는 스토리지이다.

- System boot volumes, Virtual desktops, Development and test environments  
→ 가상 데스크톱, 개발, 테스트 환경 등에서 사용할 수 있다.

- 크기는 `1GB`부터 `16TB`까지 다양하다.

- `gp3`:
~~~
- Baseline of 3,000 IOPS and throughput of 125 MiB/s
- Can increase IOPS up to 16,000 and throughput up to 1000 MiB/s independently
→ 최신 세대의 볼륨이다. IOPS와 처리량을 독자적으로 설정할 수 있다.
~~~

- `gp2`:
~~~
- Small gp2 volumes can burst IOPS to 3,000
- Size of the volume and IOPS are linked, max IOPS is 16,000
- 3 IOPS per GB, means at 5,334 GB we are at the max IOPS
→ 좀 더 오래된 버전으로 볼륨이 더 작다. 볼륨과 IOPS가 연결되어 있어서 IOPS와 처리량을 증가시키기 위해서는 볼륨의 사이즈를 같이 증가시켜야 한다는 단점이 있다.
~~~

### 3. EBS Volume Types Use cases Provisioned IOPS (PIOPS) SSD
- Critical business applications with sustained IOPS performance or applications that need more than 16,000 IOPS  
→ `IOPS` 성능을 유지할 필요가 있는 주요 비즈니스 애플리케이션이나 `16,000 IOPS` 이상을 요하는 애플리케이션에 적합한 `EBS volume`이다.

- Great for databases workloads (sensitive to storage perf and consistency)  
→ 일반적으로 데이터베이스 워크로드에 알맞다.

- `io1`과 `io2`중에서는 최신 세대를 고르는 것이 좋다. 동일한 비용이라면 `io2`가 더 높은 내구성과 기가 당 `IOPS`를 제공한다.
~~~
- Max PIOPS: 64,000 for Nitro EC2 instances & 32,000 for other
- Can increase PIOPS independently from storage size
→ gp3와 동일하게 IOPS, 처리량을 독자적으로 설정할 수 있다.

- io2 have more durability and more IOPS per GiB (at the same price as io1)
→ 보다 최신 버전인 io2는 io1과 동일한 비용이라면 더 높은 내구성과 기가 당 IOPS를 제공한다.

- io2 Block Express (4 GiB – 64 TiB):
→ 이 외에도 io2 블록 익스프레스가 존재한다. 좀 더 고성능 유형의 볼륨이다. 지연 시간이 밀리초 미만이며, 엄청난 IOPS를 자랑한다.

- Supports EBS Multi-attach
→ io1과 io2 모두 EBS Volume의 Multi-attach를 제공한다. 하나의 EBS 볼륨을 같은 가용 영역에 있는 여러 EC2 인스턴스에 연결할 수 있다.
~~~

### 4. EBS Volume Types Use cases Hard Disk Drives (HDD)
- Cannot be a boot volume  
→ 이 타입들은 루트 볼륨이 될 수 없는 이전 유형의 볼륨에 해당한다.

- 125 GiB to 16 TiB

- Throughput Optimized HDD
~~~
- st1
→ 처리량 최적화 HDD로 빅데이터, 데이터 웨어하우스, 로그 처리에 적합하다.

- sc1
→ 콜드 HDD이다. 아카이브 데이터용으로 접근 빈도가 낮은 데이터에 적합하다. 최저 비용으로 데이터를 저장할 때 사용한다.
~~~

![image](https://user-images.githubusercontent.com/97398071/232329028-f57caa96-21d1-4bc9-b8e9-1d70480e8f49.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 5. EBS Multi-Attach – io1/io2 family
- Attach the same EBS volume to multiple EC2 instances in the same AZ  
→ `AWS`는 `EBS Volume`의 다중 연결 기능을 제공한다. 하나의 `EBS` 볼륨을 같은 가용 영역에 있는 여러 `EC2` 인스턴스에 연결할 수 있음을 의미한다. 하지만, 이는 `io1`과 `io2` 제품군에서만 사용할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/232489574-c34fa3c1-ab2a-462b-96d7-aec98015a202.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Each instance has full read & write permissions to the high-performance volume  
→ 각 인스턴스는 고성능 볼륨에 대한 읽기 및 쓰기 권한을 전부 가진다.

- Use case:
~~~
- Achieve higher application availability in clustered Linux applications (ex: Teradata)  
→ 애플리케이션의 가용성을 높이기 위해 클러스터링된 Linux 애플리케이션에 사용

- Applications must manage concurrent write operations  
→ 애플리케이션이 동시 쓰기 작업을 관리해야 할 때 사용한다. 다중 연결 또한 해당 AZ 내에서만 사용할 수 있다.

- Up to 16 EC2 Instances at a time  
→ 한 번에 16개의 EC2 인스턴스만 같은 볼륨에 연결할 수 있다. 중요하다.

- Must use a file system that’s cluster-aware (not XFS, EX4, etc…)  
→ 다중 연결을 실행하려면 반드시 클러스터 인식 파일 시스템을 사용해야 한다.
~~~

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---