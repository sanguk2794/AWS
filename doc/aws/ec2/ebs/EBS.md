## EC2 Instance Storage
### 1. What’s an EBS Volume?
- An EBS (Elastic Block Store) Volume is a network drive you can attach to your instances while they run  
→ EBS 볼륨은 인스턴스가 실행 중인 동안 연결 가능한 네트워크 드라이브이다.

- It allows your instances to persist data, even after their termination  
→ `EBS`를 사용한다면 인스턴스가 종료된 후에도 데이터를 지속할 수 있다. 이것이 바로 `EBS Volume`을 쓰는 주목적이다. 

- Analogy: Think of them as a “network USB stick”, It’s a network drive (i.e. not a physical drive)  
→ `EBS`는 `네트워크 USB`라고 생각하면 편리하다. 실제 연결이 네트워크를 통해 이루어지는 네트워크 드라이버이다.
통신시 네트워크를 이용하므로 컴퓨터가 다른 서버에 도달할 때 까지의 지연이 생길 수 있다.

- They are bound to a specific availability zone, An EBS Volume in us-east-1a cannot be attached to us-east-1b  
→ `EBS Volume`은 하나의 `AZ`에서만 사용 가능하다. `us-east-1a`의 `EBS Volume`을 `us-east-1b`에서 사용할 수 없다.
`Attach`하기 위해서는 반드시 `EC2`의 `AZ`와 `EBS Volume`의 가용 영역을 일치시켜야 한다.

- To move a volume across, you first need to snapshot it  
→ 단, 스냅샷을 이용할 경우 다른 `AZ`로도 볼륨을 옮길 수 있다.

- Have a provisioned capacity (size in GBs, and IOPS (I/O per seconds))  
→ 볼륨이기 때문에 용량을 미리 결정해야 한다. 그러면 해당 프로비전 용량에 따라 요금이 청구되고 더 좋은 성능이나 큰 사이즈가 필요하면 이후에 용량을 늘릴 수도 있다.

- Free tier: 30 GB of free EBS storage of type General Purpose (SSD) or Magnetic per month  
→ 무료 등급으로는 매달 `30GB`의 `EBS` 스토리지를 `범용 SSD` 또는 마그네틱 유형으로 제공한다.

- It can be detached from an EC2 instance and attached to another one quickly  
→ `EC2` 인스턴스로부터 분리될 수 있고, 매우 빠르게 다른 인스턴스로 연결될 수 있다.

- They can only be mounted to one instance at a time (at the CCP level)  
→ `CCP` 레벨의 `EBS Volume`은 한 번에 하나의 인스턴스에만 마운트할 수 있다. 
하나의 `EBS`에 2개의 인스턴스가 연결되는 것은 불가능하나, 여러 `EBS`가 하나의 인스턴스에 연결되는 것은 문제없이 가능하다. 

![image](https://user-images.githubusercontent.com/97398071/232322378-b44dc275-4f5e-4bd0-90d0-d790ebff9995.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 생성한 `EBS`를 연결하지 않고 그대로 둘 수 있다. 
- 인스턴스와 연결된 `EBS Volume` 정보는 `EC2` → `Instances` → `Storage`에서 확인 가능하다.

![image](https://user-images.githubusercontent.com/97398071/232322884-c6ffbe30-947f-4915-85fb-83d7f0c20098.png)

### 2. EBS – Delete on Termination attribute
- `EC2` 인스턴스를 통해 `EBS Volume`을 생성하는 경우 `Delete on Terminaton` 속성을 지정할 수 있다.  
→ 루트 볼륨은 인스턴스 종료와 함께 삭제가 기본값이며, 다른 `EBS` 볼륨은 삭제하지 않는 것이 기본값이다.

![image](https://user-images.githubusercontent.com/97398071/232322678-3d5f6df8-0c54-42b5-99ed-964059f365e8.png)

- This can be controlled by the AWS console / AWS CLI  
→ 이 설정은 `AWS` 콘솔뿐만 아니라 `CLI`에서도 수정 가능하다.

- 회사에서 `EC2` 인스턴스와 별도의 영구 스토리지를 제공하는 `EBS` 볼륨에 비즈니스 크리티컬 데이터를 저장하려고 합니다. 개발 팀에서 테스트를 진행하던 중에 `EC2` 인스턴스를 종료하면 연결된 `EBS` 볼륨 또한 소실된다는 것을 발견했고, 이는 예상과 상반된 결과였습니다. 솔루션 아키텍트의 관점에서 이 문제를 어떻게 설명할 수 있습니까?  
→ `EBS` 볼륨이 `Amazon EC2` 인스턴스의 루트 볼륨으로 설정되었다. 인스턴스를 종료하면 기본 동작에 의해 연결된 루트 볼륨도 함께 종료된다.

### 3. EBS Snapshots
- Make a backup (snapshot) of your EBS volume at a point in time  
→ `EBS Snapshot`은 `EBS Volume`의 특정 시점에 대한 백업이다.

- Not necessary to detach volume to do snapshot, but recommended  
→ `EC2`의 인스턴스로부터 `EBS` 볼륨을 분리할 필요는 없지만 `AWS`는 분리할 것을 권장한다.

- Can copy snapshots across AZ or Region
→ `EBS` 스냅샷은 다른 `AZ`나 다른 리전에서도 복사해서 사용할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/232323611-c6fb4422-6519-4496-b236-155e16277fd8.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 1. EBS Snapshots Features
- EBS Snapshot Archive  
→ 아카이브 티어로 스냅샷을 옮길 수 있다. 최대 75%까지 저렴하지만 스냅샷을 아카이브 티어로 옮기면 스냅샷을 복원하는 데 24시간에서 72시간이 걸린다.

- Recycle Bin for EBS Snapshots  
→ `EBS` 스냅샷을 삭제할 때 영구 삭제하는 대신에 휴지통에 넣을 수 있다. 실수로 삭제하더라도 휴지통에서 복원 가능한 것이다. 복원 가능 기간은 1일에서 1년 사이로 설정할 수 있다.

- Fast Snapshot Restore (FSR)  
→ 빠른 스냅샷 복원은 스냅샷의 완전 초기화를 통해 첫 사용에서의 지연 시간을 없애는 기능이다. 스냅샷이 아주 크고 `EBS` 볼륨 또는 `EC2` 인스턴스를 빠르게 초기화해야 할 때 특히 유용하다. 하지만 비용이 많이 들기 때문에 주의가 필요하다.

- `EC2` → `Elsatic Block Store` → `Volumes`에서 `EBS`의 스냅샷을 생성할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/232324012-d0b2a49b-cbfe-43bc-9690-2e92c1e63c0a.png)

- `EC2` → `Elsatic Block Store` → `Snapshot` → `Copy Snapshot`을 통해 다른 `AZ`, 다른 리전으로 스냅샷을 복사할 수 있다.
이 기능은 재해 복구 전략을 수립해 데이터를 다른 지역에 백업하거나 할 때 유용하게 사용할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/232324272-4b0639df-9f55-4af3-b4fe-0a8383026314.png)

- `EC2` → `Elsatic Block Store` → `Snapshot` → `Create volume from snapshot`을 통해 스냅샷으로부터 볼륨을 생성할 수 있다.

- `EC2` → `Elsatic Block Store` → `Snapshot` → `Recycle bin`은 스냅샷과 `AMI`가 실수로 지워지지 않도록 보호해 준다.

![image](https://user-images.githubusercontent.com/97398071/232324437-ebd35737-180d-4144-bb26-8a24216fa28e.png)

### 4. EBS Volume
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

#### 1. EBS Volume Types Use cases General Purpose SSD
- Cost effective storage, low-latency  
→ 비용 효율적이며, 짧은 지연시간을 자랑하는 스토리지이다.

- System boot volumes, Virtual desktops, Development and test environments  
→ 가상 데스크톱, 개발, 테스트 환경 등에서 사용할 수 있다.

- 크기는 `1GB`부터 `16 TB`까지 다양하다.

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

#### 2. EBS Volume Types Use cases Provisioned IOPS (PIOPS) SSD
- Critical business applications with sustained IOPS performance or applications that need more than 16,000 IOPS  
→ `IOPS` 성능을 유지할 필요가 있는 주요 비즈니스 애플리케이션이나 `16,000 IOPS` 이상을 요하는 애플리케이션에 적합한 `EBS volume`이다.

- Great for databases workloads (sensitive to storage perf and consistency)  
→ 일반적으로 데이터베이스 워크로드에 알맞다.

- `io1`과 `io2`중에서는 최신 세대를 고르는 것이 좋다. 동일한 비용일 경우 `io2`가 더 높은 내구성과 기가 당 `IOPS`를 제공한다.
~~~
- Max PIOPS: 64,000 for Nitro EC2 instances & 32,000 for other
- Can increase PIOPS independently from storage size
→ gp3와 동일하게 IOPS, 처리량을 독자적으로 설정할 수 있다.

- io2 have more durability and more IOPS per GiB (at the same price as io1)
→ 보다 최신 버전인 io2는 io1과 동일한 비용으로 더 높은 내구성과 기가 당 `IOPS`를 제공한다.

- io2 Block Express (4 GiB – 64 TiB):
→ 이 외에도 io2 블록 익스프레스가 존재한다. 좀 더 고성능 유형의 볼륨이다. 지연 시간이 밀리초 미만이며, 엄청난 IOPS를 자랑한다.

- Supports EBS Multi-attach
→ io1과 io2 모두 EBS Volume의 Multi-attach를 제공한다. 하나의 EBS 볼륨을 같은 가용 영역에 있는 여러 EC2 인스턴스에 연결할 수 있다.
~~~

#### 3. EBS Volume Types Use cases Hard Disk Drives (HDD)
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

#### 4. EBS Multi-Attach – io1/io2 family
- Attach the same EBS volume to multiple EC2 instances in the same AZ  
→ `AWS`는 `EBS Volume`의 다중 연결 기능을 제공한다. 하나의 `EBS` 볼륨을 같은 가용 영역에 있는 여러 `EC2` 인스턴스에 연결할 수 있음을 의미한다.
하지만, 이는 `io1`과 `io2` 제품군에서만 사용할 수 있다.

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

### 5. EBS Encryption
- When you create an encrypted EBS volume, you get the following:  
→ `EBS` 볼륨을 생성하면 암호화가 동시다발적으로 일어난다.
~~~
- Data at rest is encrypted inside the volume
→ 저장 데이터가 볼륨 내부에 암호화된다.

- All the data in flight moving between the instance and the volume is encrypted
→ 인스턴스와 볼륨 간의 전송 데이터 역시 암호화된다.

- All snapshots are encrypted
→ 스냅샷 또한 암호화된다.
 
- All volumes created from the snapshot
→ 스냅샷으로 생성한 볼륨 역시 모두 암호화된다.
~~~

- Encryption and decryption are handled transparently (you have nothing to do)  
→ 이 때의 암호화 및 복호화 매커니즘은 보이지 않게 처리되기에 아무 것도 하지 않아도 괜찮다.

- Encryption has a minimal impact on latency  
→ 암호화는 지연 시간에는 영향이 거의 없다.

- EBS Encryption leverages keys from KMS (AES-256)  
→ `KMS`에서 암호화 키를 생성하며, `AES-256` 암호화 표준을 사용한다.

#### 1. Encryption: encrypt an unencrypted EBS volume
- `EBS Volume`을 암호화하거나 암호화를 푸는 방법에 대한 내용이다.

- Create an EBS snapshot of the volume  
→ 우선 볼륨의 `EBS` 스냅샷을 생성해야 한다. 암호화되지 않은 `EBS` 볼륨에서 생성한 스냅샷은 암호화되지 않기 때문에 이 스냅샷은 암호화되어 있지 않다.

![image](https://user-images.githubusercontent.com/97398071/232492753-a1137eef-b8b1-428e-8d5b-f544a8fd058e.png)

- Encrypt the EBS snapshot ( using copy )  
→ 복사 기능을 통해 `EBS` 스냅샷을 암호화한다.
 
- Create new ebs volume from the snapshot ( the volume will also be encrypted )  
→ `Snapshots` → `Actions` → `Create volume from snapshot`을 선택하면 암호화된 스냅샷을 이용해 새 `EBS` 볼륨을 생성할 수 있다.
암호화된 스냅샷을 이용하면 해당 볼륨 또한 암호화된다.

- 그렇다고, 비암호화 스냅샷으로부터 암호화 `EBS` 볼륨을 생성할 수 없는 것은 아니다. 
- `Snapshots` → `Actions` → `Create volume from snapshot`에서 `Encryption` → `Encrypt this volume`을 체크한 상태로 `EBS` 볼륨을 생성하면
비암호화 스냅샷으로부터 암호화 `EBS` 볼륨을 생성할 수 있다.
  
- Now you can attach the encrypted volume to the original instance  
→ 이 암호화된 볼륨을 인스턴스의 원본에 연결할 수 있다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
