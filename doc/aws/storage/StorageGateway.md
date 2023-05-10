## AWS Storage Gateway
### 1. Hybrid Cloud for Storage
- AWS is pushing for ”hybrid cloud”  
→ `AWS`는 하이브리드 클라우드를 권장한다.
~~~
- Part of your infrastructure is on the cloud
→ 일부 인프라를 AWS 클라우드에 둔다.

- Part of your infrastructure is on-premises
→ 일부 인프라를 온프레미스에 둔다.
~~~

- This can be due to
~~~
- Long cloud migrations
→ 클라우드 마이그레이션이 오래 걸리는 경우

- Security requirements
→ 보안 요구조건이 있는 경우

- Compliance requirements
→ 규정 준수 요구조건이 있는 경우
~~~

- S3 is a proprietary storage technology (unlike EFS / NFS)  
→ `S3`는 독점 스토리지 기술로 `NFS` 규정을 준수하는 파일 시스템인 `EFS`와는 다르다. 

- So how do you expose the S3 data on-premises? AWS Storage Gateway!  
→ 독점 기술인 `S3`의 데이터를 온프레미스로 옮기기 위한 방법이 `AWS Storage Gateway` 서비스이다.

- `AWS`의 스토리지 클라우드 네이티브 옵션에는 다음의 종류가 포함된다.
~~~
- Amazon EBS나 EC2 인스턴스 등 블록 스토리지
→ 블록 스토리지는 데이터를 일정한 크기의 덩어리(블록)로 나누어 저장하는 방식이다. 
이렇게 나누어진 각각의 블록은 고유한 주소를 가지고 있으며, 이 주소를 통하여 블록들을 재구성하여 데이터를 불러올 수 있다.

- Amazon EFS, Amazon FSx 등 파일 스토리지
→ 파일 스토리지는 파일과 폴더의 계층구조로 이루어진 방식이다.

- Amazon S3, Amazon Glacier 등 객체 스토리지
→ 오브젝트 스토리지는 오브젝트라는 개별 데이터 단위로 데이터를 저장한다. 오브젝트는 비디오, 오디오뿐 아니라 텍스트, 기타 다른 파일 유형 등의 모든 데이터를 포괄한다.
파일 스토리지와는 다르게 계층구조 없는 평면(flat) 구조로 데이터를 저장한다. 그만큼 접근이 쉽고 빠르며 확장성이 높다.
~~~

### 2. AWS Storage Gateway
- Bridge between on-premises data and cloud data  
→ 온프레미스 데이터와 클라우드 데이터간의 가교 역할을 수행한다.

- Use cases:
~~~
- backup & restore → disaster recovery
→ 재해 복구를 목적으로 온프레미스 데이터를 클라우드에 백업할 수 있다.

- tiered storage
→ 클라우드에는 콜드 데이터를 두고 온프레미스에는 웜 데이터를 두는 등의 활용이 가능하다.

- on-premises cache & low-latency files access
→ AWS Storage Gateway를 온프레미스 캐시로 사용할 수 있다.
~~~

- Types of Storage Gateway:
~~~
- S3 File Gateway
- FSx File Gateway
- Volume Gateway
- Tape Gateway
~~~

![image](https://user-images.githubusercontent.com/97398071/235353350-9ea90436-a4ce-4d75-b1ba-c1741c8231a5.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 1. Amazon S3 File Gateway
- Configured S3 buckets are accessible using the NFS and SMB protocol  
→ `S3` 파일 게이트웨이로 구성한 모든 버킷을 `NFS`와 `SMB, Server Message Block` 프로토콜을 이용해 접근할 수 있다.

- Most recently used data is cached in the file gateway  
→ 최근에 사용된 데이터는 파일 게이트웨이에 캐시로 저장된다.

- Supports S3 Standard, S3 Standard IA, S3 One Zone A, S3 Intelligent Tiering  
→ 여러 스토리지 클래스를 지원한다.

- Transition to S3 Glacier using a Lifecycle Policy  
→ 수명 주기 정책을 사용하면 `Glacier` 아카이브로 데이터를 옮길 수 있다.

- Bucket access using IAM roles for each File Gateway  
→ 버킷 액세스를 위해서는 각각의 `File Gateway`마다 `IAM` 역할을 생성해야 한다.

- SMB Protocol has integration with Active Directory (AD) for user authentication  
→ `Windows` 파일 시스템 네이티브인 `SMB` 프로토콜을 사용하는 경우에는 사용자 인증을 위해 `Active Directory`와 통합해야 한다.

![image](https://user-images.githubusercontent.com/97398071/235352635-8b6b8cfa-64d8-48b8-8a71-73d7459ac6ee.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- `S3`에 대규모의 데이터셋이 저장되어 있습니다. 여러분은 `NFS`, 혹은 `SMB` 프로토콜을 사용해 온프레미스 서버를 통해 이 데이터셋에 액세스하려 합니다. 또한, 온프레미스 `Microsoft AD`를 통해 이러한 파일에 대한 액세스를 인증하고자 합니다. 무엇을 사용해야 할까요?  
→ `AWS Storage Gateway - File Gateway`

#### 2. Amazon FSx File Gateway
- Native access to Amazon FSx for Windows File Server  
→ `Amazon FSx` 파일 서버에 네이티브 액세스를 제공한다.

- Local cache for frequently accessed data  
→ 게이트웨이를 생성해 두면 자주 액세스하는 데이터의 로컬 캐시를 확보할 수 있다. 온프레미스에서 `FSx`로 접근 가능함에도 불구하고 게이트웨이를 생성해야 하는 이유이다.

- Windows native compatibility (SMB, NTFS, Active Directory...)  
→ `Windows native`인 `SMB`, `NTFS`, `Active Directory`등과 호환 가능하다.

- Useful for group file shares and home directories  
→ 그룹 파일 공유가 가능하고, 홈 디렉토리를 공유할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/235352816-70565cdb-ecd5-4ea2-9dd6-92de727ad4d5.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 회사는 `AWS`의 `FSx for Windows` 파일 서버 스토리지에 저장된 수많은 파일과 데이터를 사용하고 있습니다. 이 파일들은 현재 `AWS`에 호스팅된 리소스에서 사용됩니다. 이 파일들을 온프레미스에서 짧은 지연 시간으로 액세스할 수 있어야 합니다. 이를 달성하는 데 도움이 될 만한 `AWS` 서비스는 무엇입니까?  
→ Amazon FSx File Gateway

- 회사에서는 `Amazon S3` 파일 게이트웨이를 실행해 `S3` 버킷에 데이터를 호스팅하고 있으며, `SMB`를 사용해 해당 데이터를 온프레미스에 마운트할 수 있습니다. 현재 데이터는 S3 Standard 스토리지 클래스에 호스팅되어 있는데 S3에 드는 비용을 절감하기 위해 데이터 중 일부를 `S3 Glacier`로 옮기기로 결정했습니다. 데이터를 `S3 Glacier`로 자동으로 옮길 수 있는 가장 효율적인 방법은 무엇입니까?  
→ `S3` 수명 주기 정책을 사용한다.

#### 3. Volume Gateway
- Block storage using iSCSI protocol backed by S3  
→ 블록 스토리지로 `Amazon S3`가 백업하는 `iSCSI` 프로토콜을 사용한다.

- Backed by EBS snapshots which can help restore on-premises volumes!  
→ 볼륨이 `EBS` 스냅샷으로 저장되며 필요에 따라 온프레미스 볼륨을 복구할 수 있다. 

- 볼륨 게이트웨이에는 두 가지 유형이 존재한다. 두 유형의 차이점은 중요하다.
~~~
- Cached volumes: low latency access to most recent data
→ 캐시 모드에서는 기본 데이터 Amazon S3에 저장되는 반면, 자주 액세스하는 데이터는 지연 시간을 줄이기 위해 온프레미스 캐시로 보존한다.

- Stored volumes: entire dataset is on premise, scheduled backups to S3
→ 저장 모드에서는 기본 데이터가 온프레미스에 저장되고 액세스 지연 시간을 줄이기 위해 전체 데이터 세트가 온프레미스에서 제공되는 반면, Amazon S3에 비동기식으로 백업된다.
~~~

![image](https://user-images.githubusercontent.com/97398071/235352841-e8289055-b673-4961-8a40-d0434dbd4e0c.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 온프레미스 서버에 볼륨을 백업하는 데 그 의의가 있다.

#### 4. Tape Gateway
- Some companies have backup processes using physical tapes (!)
- With Tape Gateway, companies use the same processes but, in the cloud  
→ `Tape Gateway`를 사용하면 `physical tapes`를 사용하는 대신 클라우드를 활용해 데이터를 백업할 수 있다.

- Virtual Tape Library (VTL) backed by Amazon S3 and Glacier  
→ `VTL, Virtual Tape Library`는 `Amazon S3`와 `Glacier`를 이용한다.

- Back up data using existing tape-based processes (and iSCSI interface)  
→ 테이프 기반 프로세스의 기존 백업 데이터를 `iSCSI` 인터페이스를 사용하여 백업한다.

![image](https://user-images.githubusercontent.com/97398071/235353000-3bbd5508-baf7-4807-bb9f-263a807b3054.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 테이프 백업에 가상 인피니트 스토리지를 노출하려고 합니다. 여러분은 사용 중인 것과 동일한 소프트웨어를 유지하고, `iSCSI`와 호환 가능한 인터페이스를 사용하려 합니다. 어떤 방법을 사용해야 할까요?  
→ `AWS Storage Gateway - Tape Gateway`

### 3. Storage Gateway – Hardware appliance
- 게이트웨이는 회사 데이터 센터에 설치되어 있어야 한다.

- 하지만 종종 게이트웨이를 실행할 가상 서버가 없는 경우가 있는데, 이 경우 `AWS` 하드웨어를 사용할 수 있다. 이 서비스를 `Storage Gateway – Hardware appliance`라고 한다.

- Using Storage Gateway means you need on-premises virtualization
- Otherwise, you can use a Storage Gateway Hardware Appliance
- You can buy it on amazon.com  
→ 온프레미스 서버가 없는 경우 `Storage Gateway – Hardware appliance`를 사용할 수 있다. 

- Works with File Gateway, Volume Gateway, Tape Gateway  
→ 미니 서버가 될 하드웨어 어플라이언스를 인프라에 설치한 후 파일 게이트웨이, 볼륨 게이트웨이 또는 테이프 게이트웨이로 설정하면 된다.

- Has the required CPU, memory, network, SSD cache resources  
→ 제대로 작동시키기 위해서는 충분한 `CPU`, `memory`, `network`, `SSD cache resources`가 필요하다.

- Helpful for daily NFS backups in small data centers  
→ 소규모 데이터 센터의 일일 `NFS` 백업처럼 가상화가 없을 때 유용하다.

---
#### ▶ Reference
- [블록, 파일, 오브젝트 스토리지 쉽게 이해하기](https://www.dknyou.com/blog/?q=YToxOntzOjEyOiJrZXl3b3JkX3R5cGUiO3M6MzoiYWxsIjt9&bmode=view&idx=10474168&t=board)
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
