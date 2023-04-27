## EC2
### 1. Amazon EC2
- EC2 is one of the most popular of AWS offering
→ `EC2`는 아마존에서 가장 인기 있는 서비스이다. 

- EC2 = Elastic Compute Cloud = Infrastructure as a Service
→ `EC2`는 `AWS`에서 제공하는 서비스형 `Infrastructure`이다.

- It mainly consists in the capability of EC2, EBS, ELB, ASG
→ `EC2`는 높은 수준에서 보면 많은 것을 포함하고 있으며, 그렇기에 하나의 서비스가 아니다. 
~~~
Renting virtual machines (EC2)
→ 가상 머신을 EC2에서 임대할 수 있다. 이를 EC2 인스턴스라고 한다.

Storing data on virtual drives (EBS)
→ 데이터를 가상 드라이브 또는 EBS 볼륨에 저장할 수 있다.

Distributing load across machines (ELB)
→ 일래스틱 로드 밸런서로 로드를 분산시킬 수 있다.

Scaling the services using an auto-scaling group (ASG)
→ 오토 스케일링 그룹을 통해 서비스를 확장할 수 있다.
~~~  

- Knowing EC2 is fundamental to understand how the Cloud works
→ `AWS EC2`의 사용법을 아는 것은 클라우드 작동 방식을 이해할 때 필수적이다. 클라우드는 언제든지 컴퓨팅을 대여할 수 있으며 `EC2`는 그 대표적인 예시이기 때문이다.

### 2. EC2 sizing & configuration options
- Operating System (OS): Linux, Windows or Mac OS
→ `Linux`가 가장 인기있으며 `Window`와 `Mac OS` 또한 선택할 수 있다.

- How much compute power & cores (CPU)
→ 가상 머신에 사용할 컴퓨팅 성능과 코어의 양을 선택할 수 있다. 즉 `CPU`의 개수를 선택할 수 있다.

- How much random-access memory (RAM)
→ 랜덤 액세스 메모리의 사이즈도 선택할 수 있다.

- How much storage space
~~~
- Network-attached (EBS & EFS)
→ 용량 또한 선택할 수 있다. 예를 들면 네트워크를 통해 연결할 스토리지가 필요한지 여부가 있다.

- hardware (EC2 Instance Store)
~~~

- Network card: speed of the card, Public IP address
→ `EC2` 인스턴스에 연결할 네트워크의 종류를 선택할 수 있다. 속도가 빠른 네트워크 카드를 원하는지 어떤 종류의 공용 `IP`를 원하는지 설정할 수 있다.

- Firewall rules: security group
→ 방화벽 규칙, 보안 그룹의 설정이 가능하다.

- Bootstrap script (configure at first launch): EC2 User Data

### 3. Create EC2 Instance
- `EC2` → `Instances` → `Launch an instance`
→ 인스턴스 생성을 위한 버튼이다.

![Create instance button](https://user-images.githubusercontent.com/97398071/229365921-a4eda42b-2682-4006-909d-954faf6c7510.png)

- Set name and tag
→ 인스턴스명과 태그를 작성한다.

- `Application and Os Images (Amazon Machine Image)`
→ `EC2` 인스턴스에 사용할 기본 이미지를 선택한다.

![Set name, tag, AMI](https://user-images.githubusercontent.com/97398071/229365918-745e675d-e506-40b8-82c8-6e9dba2e8021.png)

- `Instance type`
→ `CPU` 개수, 메모리 용량, 비용에 따라 달라진다. `Compare instance types` 버튼을 클릭하면 모든 타입의 인스턴스를 확인할 수 있다.

- `Key pair (login)`
→ `SSH` 유틸리티를 이용해 인스턴스에 접근할 때 사용된다. 키 페어를 설정하지 않더라도 인스턴스의 생성은 가능하다. 하지만, `SSH`를 통한 접속이 불가능하다.

![Set instanceType, Keypair](https://user-images.githubusercontent.com/97398071/229365919-d96cb2da-ae60-45a3-880a-083485b6ceb7.png)

- `Create Key Pair` → `Set Key pair type` → `Set Private key file format`

![Create keypair](https://user-images.githubusercontent.com/97398071/229365920-da11d8ef-84fe-4769-afbd-a1612976c74c.png)

- `Network settings`
→ 인스턴스에 보안 그룹이 첨부되어 있으며, 이를 통해 트래픽 허용 규칙을 설정할 수 있다. 중복 체크가 가능하다.
~~~
- Allow SSH traffic from
→ 모든 곳에서 오는 SSH 트래픽을 허용한다.

- Allow HTTPs traffic from the internet
→ 모든 곳에서 오는 HTTPs 트래픽을 허용한다.

- Allow HTTP traffic from the internet
→ 모든 곳에서 오는 HTTP 트래픽을 허용한다.
~~~

- `Storage (volumes)`
→ `Advanced` → `Delete on termination`의 기본값은 `Yes`이다. `EC2` 인스턴스를 종료하면 그 볼륨도 삭제될 것이라는 것을 의미한다.

![Set network, storage](https://user-images.githubusercontent.com/97398071/229365917-55574eef-17c9-42f3-b610-926988bf2afc.png)

- `Advanced details` → `User data`
→ `User Data`에 작성한 쉘스크립트는 인스턴스가 처음으로 실행될 때, 즉 인스턴스의 전체 수명주기 중에 단 한 번만 실행된다. 

- `Launch Instance`

#### 1. EC2 User Data
- It is possible to bootstrap our instances using an EC2 User data script.
→ `EC2` 사용자 데이터 스크립트를 사용하여 인스턴스의 부트스트랩이 가능하다.

- bootstrapping means launching commands when a machine starts, That script is only run once at the instance first start
→ 머신이 작동할 때 명령을 시작하는 것을 말한다. 스크립트는 처음 시작할 때 한 번만 실행되고 다시 실행되지 않는다.

- EC2 user data is used to automate boot tasks such as: Installing updates, Installing software, Downloading common files from the internet, Anything you can think of
→ 인스턴스를 부팅할 때 자동화하고 싶은 작업(업데이트, 소프트웨어 설치, 파일 다운로드, 그 외의 생각하는 모든 것)들을 설정할 수 있다.
사용자 데이터 스크립트에 작업을 더 추가할수록 부팅시 인스턴스가 할 일이 늘어날 뿐이다.

- The EC2 User Data Script runs with the root user
→ 시작 스크립트를 작성할 때에는 `root 폴더`, `sudo`를 기준으로 작성해야한다. `User Data Script`는 `root` 권한을 가지고 `root` 패스에서 시작되기 때문이다.

#### 2. Drive, Storage, Volume
###### 1. Drive
드라이브는 `SSD`, 하드디스크 등의 물리적인 저장장치를 말한다.

###### 2. Storage
스토리지는 한 개 이상의 드라이브로 구성된 대용량 데이터 저장장치를 말한다. 
여러 개의 드라이브를 하나의 스토리지로 사용하기 위한 기술을 `레이드 (Raid, Redundant Array of Independent Disk)`라고 한다.

###### 3. Volume
볼륨은 논리적인 드라이브를 말한다. 윈도우에서의 파티션과 같다. 하나의 하드디스크를 하나로 쓸 수 있지만, `C`, `D` 등 여러 개로 나누어 사용할 수도 있는 것이다.

#### 3. Instance Detail
- `Instance ID` → 인스턴스의 고유 식별자
- `Public IPv4 address` → `EC2` 인스턴스에 접근하기 위해 사용되는 주소
- `Private IPv4 address` → `AWS` 네트워크에서 내부적으로 인스턴스에 접근할 때 사용되는 주소 
- 이외에도 `Hostname`, `Private IP DNS name`, `Platform`, `AMI`, `Key pair` 등의 정보를 확인할 수 있다.
- `Stop Instance`를 통해 인스턴스의 일시정지가 가능하다. 정지중인 인스턴스에는 요금이 부과되지 않는다.
- `Terminate Instance`를 통해 인스턴스를 영구삭제할 수 있다.
- `Start Instance`를 통해 인스턴스를 재기동할 수 있다. 인스턴스를 재기동하면 `Public IPv4` 주소는 변경되지만 `Private IPv4` 주소는 유지된다.

### 4. EC2 Instance Types - Overview
- You can use different types of EC2 instances that are optimised for different use cases [in this website](https://aws.amazon.com/ec2/instance-types/)
- AWS has the following naming convention : example → `m5.2xlarge`
~~~
m → instance class
→ 인스턴스 클래스, m은 범용 인스턴스임을 나타낸다.

5 → generation (AWS improves them over time)
→ 인스턴스의 세대

2xlarge → size within the instance class
→ 인스턴스 클래스 내의 크기, 크기가 클수록 더 많은 메모리와 `CPU`를 가진다.
~~~
![Instance Types](https://user-images.githubusercontent.com/97398071/229366224-3b89e0e1-acc2-4e7a-80de-0efc74013529.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 1. EC2 Instance Types – General Purpose
- Great for a diversity of workloads such as web servers or code repositories 
→ 범용의 인스턴스는 웹 서버나 코드 저장소와 같은 다양한 작업에 적합하다. 범용 인스턴스는 `M`으로 시작된다.

- Balance between: Compute, Memory, Networking
→ 범용 인스턴스는 컴퓨팅, 메모리, 네트워킹간의 균형이 잘 맞는다.

#### 2. EC2 Instance Types – Compute Optimized
- Great for compute-intensive tasks that require high performance 
→ 고사양 작업에 최적화된 인스턴스이다. 이러한 고성능 인스턴스는 `C`로 시작하는 이름을 가지고 있다.

- Use cases
~~~
- Batch processing workloads → 데이터 일괄 처리
- Media transcoding → 미디어 트랜스코딩
- High performance web servers → 고성능 웹 서버
- High performance computing (HPC) → 고성능 컴퓨팅
- Scientific modeling & machine learning → 머신 러닝
- Dedicated gaming servers → 전용 게임 서버 
~~~

###### 1. Media transcoding
트랜스코딩은 이미 압축된 파일(일반적으로 오디오 또는 비디오)을 다른 파일 형식으로 변환하는 프로세스를 말한다.
일반적으로 대상 장치가 포맷을 지원하지 않거나 기억 공간이 충분치 않은 경우 또는 비호환 또는 구식 데이터를 현대의 더 양호한 지원을 제공하는 포맷으로 변환할 때 사용한다.

###### 2. HPC
`HPC, High Performance Computing`는 동시에 작동하는 강력한 프로세서 클러스터를 사용하여 방대한 다차원 데이터 세트(빅데이터)를 처리하고 복잡한 문제를 초고속으로 해결하는 기술이다.
`HPC` 시스템은 일반적으로 가장 빠른 상용 데스크탑, 노트북 또는 서버 시스템보다 백만 배 이상 빠른 속도로 작동한다.

#### 3. EC2 Instance Types – Memory Optimized
- Fast performance for workloads that process large data sets in memory
→ 이 유형의 인스턴스는 메모리에서 대규모 데이터셋을 처리하는 유형의 작업에 빠른 성능을 제공한다.
메모리는 `RAM`을 뜻한다. 기본적으로 램을 나타내는 `R`로 시작되는 이름을 가지고 있으나, `X`나 대용량 메모리 `Z`도 존재한다.

- Use cases
~~~
High performance, relational/non-relational databases
→ 인 메모리 데이터베이스가 되는 고성능의 관계형 또는 비관계형의 데이터베이스

Distributed web scale cache stores
→ ElastiCache(AWS 서비스 중 하나)를 예로 들 수 있는 분산 웹스케일 캐시 저장소

In-memory databases optimized for BI (business intelligence)
→ 비즈니스 인텔리전스에 최적화된 인 메모리 데이터베이스

Applications performing real-time processing of big unstructured data
→ 대규모 비정형 데이터의 실시간 처리를 실행하는 애플리케이션
~~~

###### 1. in-memory database
디스크가 아닌 주 메모리에 모든 데이터를 보유하는 데이터베이스이다. 디스크 검색보다 자료 접근이 훨씬 빠른 것이 가장 큰 장점이다. 
데이터 양의 빠른 증가로 데이터베이스 응답 속도가 떨어지는 문제를 해결할 수 있는 대안 중 하나이다.
전형적인 디스크 방식은 디스크에 저장된 데이터를 대상으로 쿼리를 수행하는 것에 비해 인메모리 방식은 메모리상에 색인을 넣어 필요한 모든 정보를 메모리상의 색인을 통해 빠르게 검색할 수 있기 때문이다.

단점이라면 매체가 휘발성이라는 것. `DB` 서버 전원이 갑자기 꺼져버리면 안에 있는 자료들이 초고속으로 즉시 삭제된다.
그래서 보통은 로그인 세션 같은 서버가 꺼져서 날아가도 상관 없는 임시 데이터에 주로 쓰인다.
거기다 속도 때문에 쓰는 것이기에 압축 따윈 쓰지 않으며, 데이터에 비해 `RAM` 용량이 넉넉하지 않을 경우 가상메모리를 쓰게 되어 역효과가 일어나기도 한다.

###### 2. Business intelligence
비즈니스 인텔리전스(BI)는 조직이 더 나은 의사결정을 내리고 정보를 기반으로 행동을 취하고 보다 효율적인 비즈니스 프로세스를 구현할 수 있게 해주는 역량을 의미한다. 
BI 역량을 갖추면 다음과 같은 일들이 가능하다.
~~~
- 조직 전반의 최신 데이터 수집
- 이해하기 쉬운 양식으로 데이터 표시(표, 그래프 등)
- 조직 내 직원들에게 시의적절하게 데이터 전달
~~~

#### 4. EC2 Instance Types – Storage Optimized
- Great for storage-intensive tasks that require high, sequential read and write access to large data sets on local storage
→ 로컬 스토리지에서 대규모의 데이터셋에 액세스할 때 적합한 인스턴스이다. 이름은 `I`, `G` 또는 `H1`으로 시작한다.
애플리케이션에 대해 대기 시간이 짧은, 수만 단위의 무작위 `IOPS`(초당 I/O 작업 수)를 지원하도록 최적화되어 있다.

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
 
###### 1. OLTP
`OLTP, 온라인 트랜잭션 처리`는 온라인 뱅킹, 쇼핑, 주문 입력 또는 텍스트 메시지 전송 등 동시에 발생하는 다수의 트랜잭션을 실행하는 데이터 처리 유형이다.
이러한 트랜잭션은 전통적으로 경제 또는 재무 트랜잭션이라고 칭하며, 기업이 회계 또는 보고 목적으로 언제든 정보에 액세스할 수 있도록 기록 및 보호된다.

###### 2. Data warehouse
데이터 웨어하우스는 보고 및 분석을 위해 구조화된 데이터(데이터베이스 테이블, Excel 시트) 및 반구조화된 데이터(XML 파일, 웹 페이지)를 저장하는 중앙 집중식 레포지토리이며,
많은 양의 정보를 저장할 수 있기 때문에 사용자가 데이터마이닝, 데이터 시각화 및 기타 형태의 비즈니스 인텔리전스 보고에 사용할 수 있는 풍부한 과거 데이터에 쉽게 액세스할 수 있도록 한다.

### 5. Introduction to Security Groups
- Security Groups are the fundamental of network security in AWS
→ 보안 그룹은 `AWS 클라우드`에서 네트워크 보안을 실행하는데 핵심이 된다.

- They control how traffic is allowed into or out of our EC2 Instances.
→ 보안 그룹을 통해 `EC2` 인스턴스에 들어오고 나가는 트래픽을 제어할 수 있다.

- Security groups only contain rules
→ 보안 그룹은 허용 규칙만을 포함한다.

- Security groups rules can reference by IP or by security group
→ `IP Address`를 참조해 규칙을 만들 수도 있다. 컴퓨터의 위치나 다른 보안 그룹을 참조하는 것이다.
외부에서 `EC2` 인스턴스로 들어오는 것이 허용되면 아웃바운드 트래픽도 수행할 수 있다.

- Security groups are acting as a “firewall” on EC2 instances
→ 보안 그룹은 `EC2` 인스턴스의 방화벽으로, `Port`로의 액세스를 통제하거나 인증된 `IP` 주소의 범위를 확인해 `IPv4`인지 `IPv6`인지 확인할 수 있다.
또, 외부에서 인스턴스로 들어오는 인바인드 네트워크와 인스턴스에서 외부로 나가는 아웃바운드 네트워크를 통제할 수 있다.
~~~
- Access to Ports
- Authorised IP ranges – IPv4 and IPv6
- Control of inbound network (from other to the instance)
- Control of outbound network (from the instance to other)
~~~
![Security Group Diagram](https://user-images.githubusercontent.com/97398071/231209003-3206de7e-2207-4c3e-9fad-45e08c7b1150.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 보안 그룹은 `Type`과 프로토콜, 트래픽이 인스턴스에서 통과하는 위치인 포트 범위, `IP Address`의 범위인 소스 등을 포함한다.
→ `0.0.0.0/0`는 전체 `IP`, 아래의 `122.149.196.85/32`는 하나의 `IP`를 뜻한다. 허용되지 않은 소스로부터의 접근은 방화벽이 차단하기 때문에 액세스 불가능하며 타임아웃이 발생한다.
설정하지 않으면 기본적으로 모든 트래픽을 허용한다.

![image](https://user-images.githubusercontent.com/97398071/231207724-7d957894-0ffb-4b84-93e2-5ba10172e6fa.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Can be attached to multiple instances
→ 보안 그룹은 여러 인스턴스에 연결할 수 있으며 보안 그룹과 인스턴스간의 1대1 관계는 없다. 실제로 인스턴스에도 여러 그룹에 연결할 수 있다.

- Locked down to a region / VPC combination
→ 보안 그룹은 지역과 `VPC`의 결합으로 통제되어 있다. 그래서 지역을 변경하면 새로운 보안 그룹을 생성하거나 다른 `VPC`를 생성해야 한다.

- Does live “outside” the EC2 – if traffic is blocked the EC2 instance won’t see it
→ 보안 그룹은 `EC2` 외부에 존재한다. 그래서 트래픽이 차단되면 `EC2` 인스턴스는 확인할 수 없다. `EC2`에서 실행하는 애플리케이션이 아니라 `EC2` 외부의 방화벽인 것이다.

- It’s good to maintain one separate security group for SSH access
→ `SSH` 액세스에 대해서 별도의 보안 그룹을 유지하는 것이 좋다. 보통 `SSH` 액세스는 가장 복잡하므로 보안 그룹의 설정에는 주의를 기울여야 한다.

- If your application is not accessible (time out), then it’s a security group issue
→ 타임 아웃으로 `EC2`에 접근할 수 없으면 이것은 보안 그룹의 문제이다.

- If your application gives a “connection refused“ error, then it’s an application error or it’s not launched
→ 하지만, 연결 거부 오류가 발생하면 보안 그룹은 실행됐고 트래픽은 통과했지만 애플리케이션에 문제가 생겼거나 실행되지 않는 등 문제가 발생한 것이다.

- All inbound traffic is blocked by default, All outbound traffic is authorised by default
→ 기본적으로 모든 인바운드 트래픽은 거부되며, 모든 아웃바운드 트래픽은 허용된다. 

#### 1. VPC
`VPC (Virtual Private Cloud)`를 통해 `VPC`별로 네트워크를 구성할 수 있고 각각의 `VPC`에 따른 내트워크 설정이 가능하다. 각각의 `VPC`는 완전히 독립된 네트워크처럼 작동한다.

#### 2. Referencing other security groups Diagram
`EC2` 인스턴스가 있고 보안 그룹 1이라는 보안 그룹이 있다. 이 보안 그룹의 기본적인 인바인드 규칙은 보안 그룹 1의 인바운드를 보안 그룹 2에 허용하는 것이다.
보안 그룹 1의 인바운드 규칙으로 보안 그룹 3은 연결되어 있지 않기 때문에 연결이 거부되고 실행되지 않는다.

![Referencing other security groups Diagram](https://user-images.githubusercontent.com/97398071/231212524-46b0725c-2ac9-4c96-9176-fa975e6afa4a.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 3. Classic Ports to know
- 22 = SSH (Secure Shell) - log into a Linux instance
- 21 = FTP (File Transfer Protocol) – upload files into a file share
- 22 = SFTP (Secure File Transfer Protocol) – upload files using SSH, `SSH`는 다른 시스템으로 파일을 전송할 수 있는 파일 전송 프로토콜 역할도 수행한다.
- 80 = HTTP – access unsecured websites
- 443 = HTTPS – access secured websites
- 3389 = RDP (Remote Desktop Protocol) – log into a Windows instance, 원격 데스크톱 프로토콜이다. 윈도우 인스턴스에 로그인할 때 사용된다. 
22번 포트는 리눅스용이며, 3389번 포트는 윈도우 인스턴스용인 것이다.

#### 4. Practice Network and Security Groups
![Security Groups](https://user-images.githubusercontent.com/97398071/231216317-8e9b3281-0687-4468-8966-0ea27b3a8d54.png)

- `Network & Security` → `Security Groups`
→ 보안 그룹을 확인할 수 있으며, 각 보안 그룹의 인바인드 규칙, 아웃바인드 규칙을 설정할 수 있다.

![Inbound Rule](https://user-images.githubusercontent.com/97398071/231217169-40d482f6-bf18-47da-b0a4-65942e215c14.png)

→ `SSH`를 시도하거나 `HTTP` 쿼리를 실행하는데 타임아웃이 발생한다면 그건 100% `EC2` 보안 그룹의 설정 문제이다.

#### 5. SSH Summary
`SSH (Secure SHell)`는 네트워크 상의 다른 컴퓨터에 로그인하거나 원격 시스템에서 명령을 실행하고 다른 시스템으로 파일을 복사할 수 있도록 해주는 응용 프로그램 또는 그 프로토콜을 가리킨다. 예를 들어 원격 저장소, 소스 코드를 원격 저장소인 깃헙에 푸쉬할 때 `SSH`를 활용해 파일을 전송하게 된다.

- 컴퓨터 운영체제에 따라 `SSH` 접속 방법이 나눠진다.
→ `Mac`, `Linux`, `Windows 10`에서는 `.pem`을 사용하지만, `Windows 10` 미만의 윈도우 버전에서는 `.ppk`를 `PuTTY`와 함께 사용한다. 
`PuTTY`는 `SSH(Secure Shell)` 접속을 위한 클라이언트이다.
`EC2` 서버에 접속하기 위한 다른 방법으로 `EC2 Instance Connect`도 있는데, 이를 사용하면 터미널이나 클라이언트 프로그램이 아닌 웹 브라우저로 `EC2` 인스턴스에 연결할 수 있다.
`EC2 Instance Connect`는 모든 버전의 `Mac`, `Linux`, 그리고 `Windows`에서 사용이 가능하다는 장점이 있다.

![image](https://user-images.githubusercontent.com/97398071/231314770-6745238d-b5ab-41a7-bf51-5d607a730923.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 6. RSA
`RSA`는 공개키 암호시스템의 하나로, 암호화뿐만 아니라 전자서명이 가능한 최초의 알고리즘으로 알려져 있다.
로널드 라이베스트(Ron Rivest), 아디 샤미르(Adi Shamir), 레너드 애들먼(Leonard Adleman)의 연구에 의해 체계화되었으며, `RSA`라는 이름은 이들 3명의 이름 앞글자를 딴 것이다.

`RSA` 암호는 안전한 통신을 위해 `Public Key`와 `Private Key`를 활용한다.
공개키는 모두에게 알려져 있으며 메시지를 암호화하는데 쓰이며, 암호화된 메시지는 개인키를 가진 자만이 복호화하여 열어볼 수 있다.

`RSA`는 개인 키로부터 공개 키를 만들어 낼 수 있다. 어떻게 그게 가능한지 보기 위해서 아래 명령어를 사용해보자.
~~~ shell script
$ openssl rsa -text -in private.pem
~~~

위 명령어로 출력된 결과에 공개 키를 정의하는데 필요한 `modulus`와 `publicExponent` 정보가 포함되어 있는 것을 확인할 수 있다.
실제 공개키를 추출하기 위해서는 아래의 명령어를 실행한다.
~~~ shell script
$ openssl rsa -in private.pem -outform PEM -pubout -out public.pem
~~~

#### 7. How to SSH into your EC2 Instance in Windows
- `openssl` 설치
- 접속
~~~ shell script
$ ssh -i ./[your private keypair file name] ec2-user@[your public ip address] 
~~~

![image](https://user-images.githubusercontent.com/97398071/231524143-3f6cc140-5fc6-4afc-867b-97c4765560ef.png)

- 혹시 안 된다면 파일의 소유자 확인이 필수이다. 리눅스처럼 `chown` 변경 키워드는 없으나, 속성, 보안 항목에서 소유자를 확인, 변경할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/231525116-7ab55f5b-9b71-4a66-a941-2669e8855889.png)

- 접속을 종료하기 위해서는 `exit`를 입력하거나 `ctrl + z`를 눌러야 한다.

### 8. EC2 Instance Connect
- 인스턴스 선택 후, `Connect` 버튼을 누르면 `Connect to instance` 화면으로 이동한다. 여기에서 주황색 연결 버튼을 누르면 `EC2 Instance Connect`를 통해 인스턴스에 접근 가능하다.

![image](https://user-images.githubusercontent.com/97398071/231527229-b025fc05-e9aa-413c-a640-3b608c55bd61.png)

- `EC2 instance Connect` 또한 인바운드 설정에 의존한다. 보안 그룹을 통해 `SSH` 접속을 허용하지 않는다면 `EC2 instance Connect`를 통한 서버 접속 또한 불가능하다.

![image](https://user-images.githubusercontent.com/97398071/231528323-a592ca66-0ac0-44b2-a6da-a5e1a38e193b.png)

- 자격 증명이 필요한 서비스는 `Access Key`를 요구한다. 하지만 `Access Key`는 서버상에 절대로 입력하면 안 된다. 
다른 유저들이 인스턴스에 입력된 자격 증명 정보를 회수할 수 있게 되기 때문이다.
~~~ shell script
$ aws iam list-users
~~~
![image](https://user-images.githubusercontent.com/97398071/231529746-04a8aba5-cd7c-450c-bcc6-302b07112133.png)

- 이런 때를 위해 필요한 것이 바로 `IAM Role`이다. `Action` → `Security` → `Modify IAM Role` 버튼을 누르면 `IAM Role`의 설정이 가능하다.

![image](https://user-images.githubusercontent.com/97398071/231530277-4354140e-cb7e-48a1-b0a6-420e9328e90c.png)

### 6. EC2 Instances Purchasing Options
![タイトルなし](https://user-images.githubusercontent.com/97398071/232197280-294d4f07-9b90-4253-93ca-c4eb1d2dfa67.png)

- On-Demand Instances – short workload, predictable pricing, pay by second
→ 리눅스 또는 윈도우는 초 단위, 다른 모든 운영체제는 1시간 단위로 청구가 이루어진다. 따라서 비용 예측이 쉬우면서 비용이 가장 많이 든다.
하지만, 바로 지불할 금액은 없고 장기적인 약정도 필요없다는 장점 또한 있다. 
단기적이고 중단 없는 워크로드가 필요할 때나 애플리케이션의 거동을 예측할 수 없을 때 가장 추천되는 구매 옵션이다.

- Reserved Instance (1 & 3 years)
→ 예약 인스턴스를 사용하기 위한 사용자는 특정한 인스턴스의 속성의 예약이 필요하며, 이 때 지정해야 하는 속성에는 인스턴스의 타입, 리전, `Tenancy`, 운영체제 등이 포함된다. 
이 외에도 예약 기간의 설정 (1년, 3년), 선결제 여부에 따라 할인폭이 달라진다. 사용량이 일정한 애플리케이션이라면 예약 인스턴스를 사용하는 것이 가장 효율적이다.

- Convertible Reserved Instance
→ 전환형 예약 인스턴스라는 특별한 유형의 예약 인스턴스도 있는데 한 개 이상의 전환형 예약 인스턴스를 운영 체제, 테넌트를 비롯하여 구성이 다른 전환형 예약 인스턴스와 교환할 수 있다. 
교환 횟수에 제한이 없으며 유연성이 더 크기 때문에 예약 인스턴스와 비교했을 때 할인율은 더 적다.

- Savings Plans (1 & 3 years) – commitment to an amount of usage, long workload
→ 장기간 사용으로 할인을 받을 수 있는 플랜이다. 예약 플랜과 비슷하게 70%의 할인을 제공하지만 다음 1년 내지 3년간 시간당 10달러로 약정을 해야한다.
사용량이 한도를 넘으면 절약 플랜은 온디맨드 가격으로 청구가 되므로 주의가 필요하다.

- Spot Instances – short workloads, cheap, can lose instances (less reliable)
→ 온디맨드와 비교해 높은 폭의 할인을 받을 수 있는 플랜이다. 
하지만, 만일 스팟 가격이 사용자가 정의한 지불 용의 가격을 넘게 된다면 언제라도 그 인스턴스들이 손실될 수 있어서 신뢰성이 낮다.
그래서 스팟 인스턴스는 `AWS`에서 가장 비용효율적인 인스턴스이며, 인스턴스가 고장에 대한 회복력이 있다면 아주 유용할 것이다. 
 
- 지불 용의 최고가보다 스팟 인스턴스의 가격이 높아질 경우 선택 가능한 옵션으로는 `stop`, `terminate`의 두 가지가 있다.
`stop`의 경우 스팟의 가격이 지불 용의 최고가보다 낮아지면 다시 재기동을 수행하며, `terminate`의 경우 인스턴스를 완전히 종료한다.

- Dedicated Hosts – book an entire physical server, control instance placement
→ 전용 호스트는 물리적 서버 전체를 예약해서 인스턴스 배치를 제어할 수 있다.

- Dedicated Instances – no other customers will share your hardware
→ 다른 고객이 하드웨어를 공유하지 않는다.

- Capacity Reservations – reserve capacity in a specific AZ for any duration
→ 용량 예약을 이용하면 원하는 기간 동안 특정한 `AZ`에 용량을 예약할 수 있다. 그리고 필요할 때마다 그 용량에 접근할 수 있다.
기간 약정이 없고 언제라도 용량을 예약하고 취소할 수 있다. 청구 할인도 없는 용량을 예약하는 것이 유일한 목적인 플랜이다.
용량 예약을 하며 청구 할인을 받기 위해서는 예약 인스턴스 또는 절약 플랜과 결합시켜야 한다.

#### 1. 전용 인스턴스와 전용 호스트의 차이
성능이나 보안상의 물리적 차이는 없으나 전용 호스팅의 경우 인스턴스가 물리적 서버에 배치되는 방법에 대한 추가적인 제어와 가시성을 제공하고 
시간이 지나도 계속 같은 물리적 서버에 인스턴스를 배포할 수 있다. 이 부분이 전용 호스팅과 전용 인스턴스의 중요한 차이점이다. 

즉, 전용 호스팅을 사용하면 기존 서버 한정 소프트웨어 라이선스를 사용하고 기업 규정 준수 및 규제 요구 사항을 해결할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/232194586-44a641d5-1781-4658-85fa-71fc944813ed.png)

출처 → [Amazon EC2 전용 호스트](https://aws.amazon.com/ko/ec2/dedicated-hosts/)

#### 2. 워크로드
워크로드란 고객 대면 애플리케이션이나 백엔드 프로세스 같이 비즈니스 가치를 창출하는 리소스 및 코드 모음을 말한다.

#### 3. Tenancy
테넌트란 소프트웨어 인스턴스에 대해 공통이 되는 특정 접근 권한을 공유하는 사용자들을 말한다.

#### 4. Which purchasing option is right for me?
- On demand: coming and staying in resort whenever we like, we pay the full price
→ 원하는 때에 원하는 용량을 사용하고자 할 때 추천된다.

- Reserved: like planning ahead and if we plan to stay for a long time, we may get a good discount.
→ 사용량이 일정한 인스턴스를 오래 사용할 것이 확실할 때 추천된다.

- Savings Plans: pay a certain amount per hour for certain period and stay in any room type
→ 매월 일정한 금액을 지출할 것임을 알고 있을 때 추천된다.

- Spot instances: the hotel allows people to bid for the empty rooms and the highest bidder keeps the rooms. You can get kicked out at any time
→ 사용한다면 배치 작업, 데이터 분석, 실패해도 복원력이 있는 워크로드에 주로 사용하나 그보다 더 내고자 하는 용의를 지닌 사용자가 나타난다면 언제든 그 인스턴스들은 손실될 수 있다는 점은 주의해야 한다.
 
- Dedicated Hosts: We book an entire building of the resort
→ 자신만의 하드웨어를 소유할 수 있다는게 장점이다.

- Capacity Reservations: you book a room for a period with full price even you don’t stay in it
→ 예약은 하는데 사용하지 않을 수도 있다. 단, 비용은 온디맨드와 동일하다.

- 시험에서 물어보는 것은 해당하는 워크로드에 적합한 타입의 인스턴스이다.

![image](https://user-images.githubusercontent.com/97398071/231547395-e3cb4385-a532-4547-995a-2694e810138d.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 5. How to terminate Spot Instances?
스팟 인스턴스 생성시에는 원하는 인스턴스의 개수, 지불 의사가 있는 인스턴스 최고 가격, `AMI` 등 요구되는 사양, 그리고 요청의 유효 기간, 요청의 유형 설정이 필요하다.

그 중 요청의 유형에는 일회성 요청과 지속적 요청이 있다.
일회성 요청의 경우에는 스팟 요청이 이행되는 즉시 인스턴스가 시작되며, 스팟 요청은 사라진다.
하지만, 지속적 요청의 경우에는 설정한 스팟 요청의 `Valid from`부터 `Valid until`까지의 유효 기간 동안 어떠한 이유로 방해를 받은 경우에 스팟 요청이 다시 전달되어 요청이 검증되고 나면 스팟 인스턴스가 재시작된다.

스팟 요청이 취소되기 위해서는 `open`, `active` 혹은 `disabled` 상태여야 하며, 스팟 요청을 취소한다고 기존 실행중이던 인스턴스들이 종료되는 것은 아니다.
그러므로, 스팟 인스턴스를 영구히 종료하고 재실행되는 일이 없도록 하기 위해서는 아래의 순서가 필요하다.
~~~
1. 스팟 요청 취소
2. 해당 요청과 연결된 스팟 인스턴스를 종료
~~~

반대로 수행할 경우 스팟 요청이 다시 작동을 해서 인스턴스를 재기동시킨다.

#### 6. Spot Fleets
비용을 극한으로 절감하기 위한 방법이다. 
- Spot Fleets = set of Spot Instances + (optional) On-Demand Instances
→ 한 세트의 스팟 인스턴스에다가 선택적으로 온디맨드 인스턴스를 조합해 사용하는 방식이다.
그래서 집합이라는 뜻의 `Fleet`이라는 이름이 붙었다.

스팟 플릿은 사용자의 요구 사항을 충족하는 스팟 용량 풀을 선택하고 플릿에 대한 목표 용량을 충족하는 스팟 인스턴스를 시작합니다. 
기본적으로 스팟 집합은 플릿에서 스팟 인스턴스가 종료된 후 교체 인스턴스를 시작하여 목표 용량을 유지하도록 설정되어 있습니다. 
스팟 플릿을 인스턴스가 종료된 후에는 유지되지 않는 일회성 요청으로 제출할 수도 있습니다. 스팟 플릿 요청에 온디맨드 인스턴스 요청을 포함할 수 있습니다.

- The Spot Fleet will try to meet the target capacity with price constraints
→ 스팟 플릿은 사용자의 요구 사항을 충족하는 스팟 용량 풀을 선택하고 플릿에 대한 목표 용량을 충족하는 스팟 인스턴스를 선택해 실행시켜 준다.
기본적으로 스팟 집합은 플릿에서 스팟 인스턴스가 종료된 후 교체 인스턴스를 시작하여 목표 용량을 유지하도록 설정되어 있다. 
- 만약 스팟 플릿이 정해진 예산 또는 원하는 용량에 달한 경우에는 인스턴스 실행을 멈춘다.

- Strategies to allocate Spot Instances:
~~~
lowestPrice: from the pool with the lowest price (cost optimization, short workload)
→ 스팟 플릿이 가장 적은 비용을 가진 풀에서부터 인스턴스를 실행해 준다. 비용 최적화가 가능하며 아주 짧은 워크로드가 있을때 적합한 옵션이다.
스팟 플릿이 자동적으로 최저 가격의 스팟 인스턴스를 요청해주므로 추가적 비용 절감이 가능하다.

diversified: distributed across all pools (great for availability, long workloads)
→ 정의한 기존의 모든 풀에 걸쳐 분산된다. 한 풀이 중단되더라도 다른 풀이 활성되어 있기에 긴 워크로드에 적합하고 가용성이 뛰어나다. 

capacityOptimized: pool with the optimal capacity for the number of instances
→ 인스턴스 개수에 따라서 최적 용량으로 실행이 되고 적절한 풀을 찾아 주는 옵션이다. 
~~~

#### 7. 스팟 인스턴스와 스팟 플릿의 차이점
스팟 풀을 이용하면 인스턴스 유형을 다양하게 정의할 수 있다는 것은 반드시 기억해두어야 한다.
스팟 인스턴스가 단순히 사용하고자 하는 인스턴스의 유형과 가용 영역을 사용자가 지정해서 사용하는 것과 달리 스팟 플릿은 조건에 따라 인스턴스 유형과 가용 영역을 자동으로 선택해준다.

#### 8. Create Spot Request
- `EC2` → `Spot Request`

![image](https://user-images.githubusercontent.com/97398071/232196181-fd4bf1c1-55b3-4ff4-bd25-e774e518c606.png)

- `Pricing history`
→ 현재 스팟 인스턴스의 요금 내역을 확인할 수 있다.
 
![image](https://user-images.githubusercontent.com/97398071/232196239-705bf034-0f1a-4022-abd0-5d1dfb8e4d7b.png)

- `EC2` → `Spot Request` → `Request Spot Instance`
→ 스팟 리퀘스트를 생성할 수 있다. 조건에 맞다면 스팟 인스턴스가 생성된다.

- 그냥 스팟 인스턴스를 생성하고자 한다면 `EC2` → `Instances` → `Launch an instance` → `Details`에서 스팟 인스턴스 요청을 체크하고 상세 설정하는 것만으로 충분하다.

![image](https://user-images.githubusercontent.com/97398071/232196821-a243c105-e43d-4292-9acf-27e3f9198158.png)

---
#### ▶ Reference
- [Amazon EC2 전용 호스트](https://aws.amazon.com/ko/ec2/dedicated-hosts/)
- [워크로드](https://docs.aws.amazon.com/ko_kr/wellarchitected/latest/userguide/workloads.html)
- [[네트워크] SSH란?](https://hanamon.kr/네트워크-ssh란/)
---
