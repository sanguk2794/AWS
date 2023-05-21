## EC2 Instance Storage
### 1. What’s an EBS Volume?
- An EBS (Elastic Block Store) Volume is a network drive you can attach to your instances while they run  
→ `EBS` 볼륨은 인스턴스가 실행 중인 동안 연결 가능한 네트워크 드라이브이다.

- It allows your instances to persist data, even after their termination  
→ `EBS`를 사용한다면 인스턴스가 종료된 후에도 데이터를 지속할 수 있다. 이것이 바로 `EBS Volume`을 쓰는 주목적이다. 

- Analogy: Think of them as a “network USB stick”, It’s a network drive (i.e. not a physical drive)  
→ `EBS`는 `네트워크 USB`라고 생각하면 편리하다. 실제 연결이 네트워크를 통해 이루어지는 네트워크 드라이버이다. 통신시 네트워크를 이용하므로 컴퓨터가 다른 서버에 도달할 때 까지의 지연이 생길 수 있다.

- They are bound to a specific availability zone, An EBS Volume in us-east-1a cannot be attached to us-east-1b  
→ `EBS Volume`은 하나의 `AZ` 내에서만 사용 가능하다. `us-east-1a`의 `EBS Volume`을 `us-east-1b`에서 사용할 수 없다. `Attach`하기 위해서는 반드시 `EC2`의 `AZ`와 `EBS Volume`의 가용 영역을 일치시켜야 한다.

- To move a volume across, you first need to snapshot it  
→ 단, 스냅샷을 이용할 경우 다른 `AZ`로 볼륨을 옮길 수 있다.

- Have a provisioned capacity (size in GBs, and IOPS (I/O per seconds))  
→ 볼륨이기 때문에 용량을 미리 결정해야 한다. 그러면 해당 프로비전 용량에 따라 요금이 청구된다.

- `EBS` 볼륨은 프로덕션 중의 라이브 구성 변경을 지원한다. 서비스 중단 없이 볼륨 유형, 볼륨 크기 및 `IOPS` 용량을 수정할 수 있다.

- Free tier: 30 GB of free EBS storage of type General Purpose (SSD) or Magnetic per month  
→ 무료 등급으로는 매달 `30GB`의 `EBS` 스토리지를 `범용 SSD` 또는 마그네틱 유형으로 제공한다.

- It can be detached from an EC2 instance and attached to another one quickly  
→ `EC2` 인스턴스로부터 분리될 수 있고, 매우 빠르게 다른 인스턴스로 연결될 수 있다.

- They can only be mounted to one instance at a time (at the CCP level)  
→ `CCP` 레벨의 `EBS Volume`은 한 번에 하나의 인스턴스에만 마운트할 수 있다. 하나의 `EBS`에 2개의 인스턴스가 연결되는 것은 불가능하나, 여러 `EBS`가 하나의 인스턴스에 연결되는 것은 가능하다. 

![image](https://user-images.githubusercontent.com/97398071/232322378-b44dc275-4f5e-4bd0-90d0-d790ebff9995.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 생성한 `EBS`를 연결하지 않고 그대로 둘 수 있다. 
- 인스턴스와 연결된 `EBS Volume` 정보는 `EC2` → `Instances` → `Storage`에서 확인 가능하다.

![image](https://user-images.githubusercontent.com/97398071/232322884-c6ffbe30-947f-4915-85fb-83d7f0c20098.png)

### 2. EBS – Delete on Termination attribute
- `EC2` 인스턴스를 통해 `EBS Volume`을 생성하는 경우 `Delete on Terminaton` 속성을 지정할 수 있다.  
→ 루트 볼륨은 인스턴스 종료와 함께 삭제가 기본값이며, 그 외의 볼륨은 `EBS` 볼륨은 삭제하지 않는 것이 기본값이다.

![image](https://user-images.githubusercontent.com/97398071/232322678-3d5f6df8-0c54-42b5-99ed-964059f365e8.png)

- This can be controlled by the AWS console / AWS CLI  
→ 이 설정은 `AWS` 콘솔뿐만 아니라 `CLI`에서도 수정 가능하다.

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
→ 아카이브 티어로 스냅샷을 옮길 수 있다. 최대 75%까지 저렴하다. 하지만 스냅샷을 복원하는 데 24시간에서 72시간이 걸린다.

- Recycle Bin for EBS Snapshots  
→ 스냅샷을 삭제할 때 영구 삭제하는 대신에 휴지통에 넣을 수 있다. 실수로 삭제하더라도 휴지통에서 복원 가능해진다. 복원 가능 기간은 1일에서 1년 사이에서 설정할 수 있다.

- Fast Snapshot Restore (FSR)  
→ 빠른 스냅샷 복원은 스냅샷의 완전 초기화를 통해 첫 사용에서의 지연 시간을 없애는 기능이다. 스냅샷이 아주 크고 `EBS` 볼륨 또는 `EC2` 인스턴스를 빠르게 초기화해야 할 때 유용하다. 하지만 비용이 많이 들기 때문에 주의가 필요하다.

- 스냅샷의 자동 백업이 가능하다.
→ `Amazon DLM, Amazon Data Lifecycle Manager`를 사용하면 `Amazon EBS` 볼륨을 백업하기 위해 생성된 스냅샷의 생성, 보존 및 삭제를 자동화할 수 있다.

- EBS backups use IO and you shouldn’t run them while your application is handling a lot of traffic
→ 스냅샷이나 백업을 생성할 때 `EBS` 볼륨 내의 `I/O`를 전부 사용한다. 그러므로 인스턴스가 `EBS`를 사용 중이 아닐 때에만 실행해야 한다.

#### 2. Create Snapshot
- `EC2` → `Elsatic Block Store` → `Volumes`에서 `EBS`의 스냅샷을 생성할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/232324012-d0b2a49b-cbfe-43bc-9690-2e92c1e63c0a.png)

- `EC2` → `Elsatic Block Store` → `Snapshot` → `Create volume from snapshot`을 통해 스냅샷으로부터 볼륨을 생성할 수 있다.
- 
- `EC2` → `Elsatic Block Store` → `Snapshot` → `Recycle bin`은 스냅샷과 `AMI`가 실수로 지워지지 않도록 보호해 준다.

![image](https://user-images.githubusercontent.com/97398071/232324437-ebd35737-180d-4144-bb26-8a24216fa28e.png)

- `EC2` → `Elsatic Block Store` → `Snapshot` → `Copy Snapshot`을 통해 다른 `AZ`, 다른 리전으로 스냅샷을 복사할 수 있다. 이 기능은 재해 복구 전략을 수립하기 위해 데이터를 다른 지역에 백업할 때 유용하다.

![image](https://user-images.githubusercontent.com/97398071/232324272-4b0639df-9f55-4af3-b4fe-0a8383026314.png)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
