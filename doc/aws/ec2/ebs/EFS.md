## EFS
### 1. Amazon EFS – Elastic File System
- Managed NFS (network file system) that can be mounted on many EC2
→ 일래스틱 파일 시스템은 관리형 `NFS`이다. 즉, 네트워크 파일 시스템이다. 네트워크 파일 시스템이므로 여러 `EC2` 인스턴스에 마운트될 수 있다.

![image](https://user-images.githubusercontent.com/97398071/232503495-111c69e3-65a0-4258-8625-e2f8508b8636.png)

- EFS works with EC2 instances in multi-AZ
→ 이 `EC2` 인스턴스들은 여러 가용 영역에 있을 수 있다. `EFS`의 장점 중 하나이다.

![image](https://user-images.githubusercontent.com/97398071/232497341-a331be3b-7dd3-4b79-9546-53e784d7f229.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Highly available, scalable, expensive (3x gp2), pay per use
→ 가용성과 확장성이 높다. 그렇기에 기본적으로 `gp2` 볼륨의 3배 정도 비싸다. 또, 이는 사용량만큼 지불하므로 미리 용량을 프로비저닝하지 않아도 괜찮다.

#### 1. Mount
물리적인 장치를 특정한 위치(대개는 디렉토리)에 연결시켜 주는 과정을 마운트라고 한다.
좁은 의미로는 유닉스 계열의 운영 체제에서의 `mount` 명령어 또는 그 명령어를 사용하는 것을 말한다.

### 2. Amazon EFS – Elastic File System
- Use cases: content management, web serving, data sharing, Wordpress
→ 사용 사례에는 컨텐츠 관리, 웹 서버, 데이터 공유, `Wordpress` 등이 포함된다.

- Uses NFSv4.1 protocol
→ 내부적으로는 `NFS` 프로토콜을 사용한다.

- Uses security group to control access to EFS
→ `EFS`에 대한 액세스를 제어하기 위해서는 보안 그룹을 설정해야 한다. 

- Compatible with Linux based AMI (not Windows)
→ `Windows`가 아닌 `Linux` 기반 `AMI`와 호환된다.

- Encryption at rest using KMS
→ `KMS`를 사용해 `EFS` 드라이브의 저장 데이터를 암호화할 수 있다.

- POSIX file system (~Linux) that has a standard file API
→ 리눅스의 표준 파일 시스템인 `POSIX` 시스템을 사용하며, 표준 파일 `API`를 가진다.

- File system scales automatically, pay-per-use, no capacity planning!
→ 미리 용량을 계획하지 않아도 된다. 파일 시스템에 따라 자동으로 확장되고 사용량에 따라 요금을 지불하면 된다.

### 3. EFS – Performance & Storage Classes
- `EFS Standard` 및 `Standard-IA` 스토리지 클래스는 한 개 이상의 가용 영역을 사용할 수 없는 경우에도 
데이터에 지속적인 가용성을 제공하도록 설계된 지역 스토리지 클래스이다.

- EFS Scale
~~~
- 1000s of concurrent NFS clients, 10 GB+ /s throughput
→ 수천 개의 NFS 클라이언트에서 EFS에 동시 액세스할 수 있으며, 이 때 처리량은 10 GB+ /s이다.

- Grow to Petabyte-scale network file system, automatically
→ 용량을 미리 프로비저닝하지 않아도 네트워크 파일 시스템에 따라 사이즈가 자동 확장된다.
~~~

![image](https://user-images.githubusercontent.com/97398071/232504761-b33c6495-02e4-4fd8-975b-70a89a117882.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Performance mode (set at EFS creation time)
→ `EFS`를 사용할 때에는 다양한 성능 모드를 설정할 수 있다. 
~~~
- General purpose (default): latency-sensitive use cases (web server, CMS, etc…)
→ 먼저 범용 모드가 있다. 기본 설정으로 네트워크 지연 시간에 민감한 웹 서버, CMS와 같은 사용 사례에서 사용한다.

- Max I/O – higher latency, throughput, highly parallel (big data, media processing)
→ I/O를 최대화하고 싶다면 Max I/O를 사용해야 한다. 지연 시간과 처리량, 병렬 처리 성능이 향상된다.
파일 시스템에 빅데이터나 미디어 처리 작업이 있을 때 유용하다.
~~~

- Throughput mode
~~~
- Bursting (1 TB = 50MiB/s + burst of up to 100MiB/s)
- Provisioned: set your throughput regardless of storage size, ex: 1 GiB/s for 1 TB storage
→ 처리량 모드는 버스팅 모드가 기본값이다. 버스팅 모드에서는 사용 공간이 많을수록 버스팅 용량과 처리량이 늘어나는데,
프로비저닝 모드에서는 스토리지 크기에 상관 없이 처리량을 설정할 수 있다.
~~~

### 4. EFS – Storage Classes
- 스토리지 클래스에도 여러 옵션들이 있다.
 
- Storage Tiers (lifecycle management feature – move file after N days)
→ 스토리지 계층을 설정할 수 있다. 일정 기간 후에 파일을 다른 계층으로 옮기는 기능이다.

- Standard: for frequently accessed files
→ 액세스가 빈번한 파일은 보통 `Standard` 계층에 저장한다.

- Infrequent access (EFS-IA): cost to retrieve files, lower price to store. Enable EFS -IA with a Lifecycle Policy
→ `EFS-IA, Infrequent Access` 계층을 사용하면 파일을 검색할 경우 검색에 대한 비용이 발생한다. 하지만, 이 `EFS-IA`에 파일을 저장하는 비용은 낮다.  
이를 활성화하기 위해서는 수명 주기 정책을 사용해야 한다.
 
- Availability and durability
→ 가용성과 내구성 측면에서는 두 가지 옵션이 있다.

- Standard: Multi-AZ, great for prod - One Zone: One AZ, great for dev, backup enabled by default, compatible with IA (EFS One Zone-IA)
→ `Standard` 옵션은 `EFS`를 다중 `AZ`에 설정한다. 이는 한 `AZ`가 중단되더라도 `EFS` 파일 시스템에 영향을 주지 않기 때문에 프로덕션 사용 사례에 적합하다. 

### 5. Create EFS
- `Amazon EFS` → `File System` → `Create` → `Availability and durability`
→ 가용성과 내구성 옵션 중 `Regional`은 여러 가용 영역간에 데이터를 복제하고 싶을 때 사용된다.
이와 반대인 `One Zone`은 하나의 가용 영역에만 데이터가 저장되므로 가용 영역이 다운되면 `EFS`도 같이 떨어진다. 

- `Lifecycle management`
→ 수명 주기 관리를 통해 비용 절감을 할 수 있다. 자주 사용되지 않는 객체를 다른 액세스 계층인 `IA` 스토리지 클래스로 이동시키면 비용이 절감된다.
이동 후에 첫 액세스를 통해 `IA` 밖으로 이동시킬 것인지 이동시키지 않을 것인지 설정할 수 있다.

- `performance`

![image](https://user-images.githubusercontent.com/97398071/232508619-0bd1e13d-f6af-457a-9681-a3a48519daef.png)

- `Network`
→ `AZ`와 서브넷 아이디, 보안 그룹을 설정할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/232508084-3d6a7274-581a-47df-b45b-8fad3a99dd99.png)

### 6. mount EFS
- `EC2` 인스턴스에 `EFS`를 마운트하기 위해서는 인스턴스를 생성할 때 서브넷과 `File System` 설정이 필수적이다.
![image](https://user-images.githubusercontent.com/97398071/232510677-c50a08bf-f8a5-425e-bb77-8a6d2e983b0f.png)

- 파일 시스텀 설정은 `EFS`를 선택한다. 
![image](https://user-images.githubusercontent.com/97398071/232510996-a4ebec8f-6cf6-4d48-a2a6-7ddfc9aa721b.png)

- 이 때 `Automatically create and attach security groups`를 체크하면 보안 그룹이 자동으로 연결된다.
또, 보안 그룹의 인바운드 룰에 `NFS` 프로토콜 유형과 2049 포트를 자동으로 추가해 준다.

- 또, `Automatically mount shared file system by attaching required user data script`를 체크하면 공유 파일 시스템을 자동으로 마운트 할 수 있다.
이전에는 `EC2` 인스턴스에 직접 실행하고 데이터 스크립트를 직접 생성해야 했다고 한다.

- 아래 명령어로 연결된 `EFS` 시스템에 파일을 추가할 수 있다. 추가된 파일은 같은 `EFS`에 연결된 다른 인스턴스에서도 확인할 수 있다. 
물론 가용 영역이 달라도 상관없다.
~~~ shell script
$ ls /[NFC path]
$ sudo su
$ echo "hello world" > /[NFC path]/[fileName].[extension]
$ ls /[NFC path]
~~~

### 7. EBS vs EFS
#### 1. EBS volumes
- `EBS` 볼륨은 한 번에 하나의 인스턴스에만 연결이 가능하고, 특정 가용 영역에 한정된다. `EBS`의 유형 중 `gp2`는 디스크 크기가 늘어나면 `I/O`도 함께 증가한다.
또, `io1`은 `I/O`를 볼륨 크기와 관계 없이 독립적으로 증가시킬 수 있다. 중요한 데이터베이스를 실행할 때 좋은 방법이다.
~~~
- one instance (except multi-attach io1/io2)
- are locked at the Availability Zone (AZ) level
- gp2: IO increases if the disk size increases
- io1: can increase IO independently
~~~

- To migrate an EBS volume across AZ
→ `EBS`를 다른 가용 영역으로 옮기고자 할 때에는 가장 먼저 스냅샷을 찍어야 한다. 스냅샷을 찍은 후 다른 `AZ`에서 그 스냅샷을 복원시키면 해당 `AZ`에 
`EBS` 볼륨이 생성된다. `EBS`의 스냅샷이나 백업을 만들 때에는 `EBS` 볼륨 내의 `I/O`를 전부 사용하게 되니 인스턴스가 `EBS`를 사용 중이 아닐 때에만 실행해야 한다.
그렇지 않으면 성능에 문제가 생길 수 있다.
~~~
- Take a snapshot
- Restore the snapshot to another AZ
- EBS backups use IO and you shouldn’t run them while your application is handling a lot of traffic
~~~

- Root EBS Volumes of instances get terminated by default if the EC2 instance gets terminated. (you can disable that)
→ `EC2` 인스턴스가 종료되면 인스턴스 내의 루트 볼륨도 기본적으로 종료된다. 원할 경우 이 동작은 비활성화가 가능하다.

#### 2. EFS
- Mounting 100s of instances across AZ
→ 여러 개의 `AZ`에 걸쳐 무수히 많은 인스턴스들에 연결될 수 있다. 
`EFS Mount Target`을 사용하면 특정 `AZ`에서 `EC2` 인스턴스들과 `EFS` 드라이브를 연결해 줄 수도 있다.

- EFS share website files (WordPress)
→ `WordPress` 같은 웹 사이트 파일을 공유할 때에도 `EFS`를 사용한다.

- Only for Linux Instances (POSIX)
→ 이는 `Linux` 인스턴스에서만 가능한데 `POSIX` 파일 시스템이라 `Windows`에서 구동되지 않기 때문이다.

- EFS has a higher price point than EBS
→ `EFS`는 `EBS`에 비해 훨씬 비싸다. 거의 세 배 정도 비싸다.

- Can leverage EFS-IA for cost savings
→ 하지만 가격을 절약하고 싶을 경우에는 스토리지 티어로 `EFS-IA`로 설정하고 제품 수명 정책을 사용함으로써 비용을 절감할 수 있다.

- Remember: EFS vs EBS vs Instance Store
→ `EFS`는 사용한 만큼만 비용이 청구된다. 하지만, `EBS`는 실제 사용한 양이 아니라 `EBS` 드라이브의 크기에 따라 정해진 양의 비용이 청구된다. 
`EFS`는 다수의 인스턴스에 걸쳐 연결해야 하는 네트워크 파일 시스템에 적합하다.

---
#### ▶ Reference
- [EFS 스토리지 클래스](https://docs.aws.amazon.com/ko_kr/efs/latest/ug/storage-classes.html)
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
