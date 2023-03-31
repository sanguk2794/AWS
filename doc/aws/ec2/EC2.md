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
→ 엘라스틱 로드 밸런서로 로드를 분산시킬 수 있다.

Scaling the services using an auto-scaling group (ASG)
→ 오토 스케일링 그룹을 통해 서비스를 확장할 수 있다.
~~~  

- Knowing EC2 is fundamental to understand how the Cloud works
→ `AWS EC2`의 사용법을 아는 것은 클라우드 작동 방식을 이해할 때 필수적이다. 
클라우드는 언제든지 컴퓨팅을 대여할 수 있으며, `EC2`는 그 대표적인 예시이기 때문이다.

### 2. EC2 sizing & configuration options
- Operating System (OS): Linux, Windows or Mac OS
→ `Linux`가 가장 인기있으며, `Window`와 `Mac OS` 또한 선택할 수 있다.

- How much compute power & cores (CPU)
→ 가상 머신에 사용할 컴퓨팅 성능과 코어의 양을 선택할 수 있다. 즉 `CPU`의 개수이다.

- How much random-access memory (RAM)
→ 랜덤 액세스 메모리의 사이즈도 선택할 수 있다.

- How much storage space
~~~
Network-attached (EBS & EFS)
→ 용량 또한 선택할 수 있다. 예를 들면 네트워크를 통해 연결할 스토리지가 필요한지 여부가 있다.

hardware (EC2 Instance Store)
→ EC2 인스턴스 스토어가 된다.
~~~

- Network card: speed of the card, Public IP address
→ `EC2` 인스턴스에 연결할 네트워크의 종류를 선택할 수 있다. 속도가 빠른 네트워크 카드를 원하는지 어떤 종류의 공용 `IP`를 원하는지 설정할 수 있다.

- Firewall rules: security group
→ 방화벽 규칙, 보안 그룹의 설정이 가능하다. 

- Bootstrap script (configure at first launch): EC2 User Data

### 3. EC2 User Data
- It is possible to bootstrap our instances using an EC2 User data script.
→ `EC2` 사용자 데이터 스크립트를 사용하여 인스턴스를 부트스트래핑 할 수 있다. 

- bootstrapping means launching commands when a machine starts, That script is only run once at the instance first start
→ 머신이 작동할 때 명령을 시작하는 것을 말한다. 스크립트는 처음 시작할 때 한 번만 실행되고 다시 실행되지 않는다.

- EC2 user data is used to automate boot tasks such as:
~~~
Installing updates
Installing software
Downloading common files from the internet
Anything you can think of
~~~

→ 인스턴스를 부팅할 때 자동화하고 싶은 작업(업데이트, 소프트웨어 설치, 일반적인 파일을 다운로드, 생각하는 모든 것)들을 설정할 수 있다.
사용자 데이터 스크립트에 작업을 더 추가할수록 부팅시 인스턴스가 할 일이 늘어날 뿐이다.

- The EC2 User Data Script runs with the root user
→ `EC2` 사용자 데이터 스크립트는 루트 계정에서 실행된다. 또, 시작 패스는 `root` 패스이다. 
즉, 시작 스크립트는 `root 폴더`, `sudo`를 기준으로 작성해야한다.

TODO 이미지
### 4. Create EC2 Instance 
- `EC2` → `Instance` → `Launch an instance`
→ 인스턴스 생성을 위한 버튼이다.

- Set name and tag
→ 인스턴스명과 태그를 작성한다.

- `Application and Os Images (Amazon Machine Image)`
→ `EC2` 인스턴스에 사용할 기본 이미지를 선택한다.

- `Instance type`
→ `CPU` 개수, 메모리 용량, 비용에 따라 달라진다. `Compare instance types` 버튼을 클릭하면 모든 타입의 인스턴스를 확인할 수 있다.

- `Key pair (login)`
→ `SSH` 유틸리티를 이용해 인스턴스에 접근할 때 사용된다. 키 페어 없이도 인스턴스의 생성은 가능하다.

- `Create Key Pair` → `Set Key pair type` → `Set Private key file format`

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
~~~
`無料利用枠の対象のお客様は、最大 30 GB の EBS 汎用 (SSD) ストレージまたはマグネティックストレージを取得できます。`
~~~

- `Advanced details`
→ `User data`는 인스턴스가 처음으로 실행될 때, 즉 인스턴스의 전체 수명주기 중에 단 한 번만 실행된다. 
TODO 코드 확인

- `Launch Instance`

#### 1. SSH
`SSH (Secure SHell, SSH)`는 네트워크 상의 다른 컴퓨터에 로그인하거나 원격 시스템에서 명령을 실행하고 다른 시스템으로 파일을 복사할 수 있도록 해주는 응용 프로그램 또는 그 프로토콜을 가리킨다. 
즉, 네트워크 프로토콜 중 하나로 컴퓨터와 컴퓨터가 인터넷과 같은 `Public Network`를 통해서 서로 통신을 할 때 보안적으로 안전하게 통신을 하기 위해 사용하는 프로토콜이다.

예를 들어 원격 저장소, 소스 코드를 원격 저장소인 깃헙에 푸쉬할 때 `SSH`를 활용해 파일을 전송하게 된다.

#### 2. RSA
`RSA` 암호는 공개키 암호시스템의 하나로, 암호화뿐만 아니라 전자서명이 가능한 최초의 알고리즘으로 알려져 있다.
로널드 라이베스트(Ron Rivest), 아디 샤미르(Adi Shamir), 레너드 애들먼(Leonard Adleman)의 연구에 의해 체계화되었으며, `RSA`라는 이름은 이들 3명의 이름 앞글자를 딴 것이다.

안전한 통신을 위해 `Public Key`와 `Private Key`를 활용한다.
공개키는 모두에게 알려져 있으며 메시지를 암호화하는데 쓰이며, 암호화된 메시지는 개인키를 가진 자만이 복호화하여 열어볼 수 있다.

`RSA`는 개인 키로부터 공개 키를 만들어 낼 수 있다. 어떻게 그게 가능한지 보기 위해서 아래 명령어를 사용해보자.
~~~ shell script
$ openssl rsa -text -in private.pem
~~~

위 명령어로 출력된 결과에 공개 키를 정의하는데 필요한 `modulus`와 `publicExponent` 정보가 포함되어 있는 것을 확인할 수 있다.
실제 공개키를 추출하기 위해서는 아래의 명령어를 실행한다.
~~~
$ openssl rsa -in private.pem -outform PEM -pubout -out public.pem
~~~

#### 3. pem, ppk
`PEM(Privacy Enhanced Mail)`은 `Base64`로 인코딩된 `ASCII` 텍스트 파일이며,
`PPK` 파일은 `PuTTY` 개인 키의 저장소 역할을 한다. `

`Mac`, `Linux`, `Windows 10`에서는 `.pem` 형식을 사용하지만, `Windows 10` 미만의 윈도우 버전에서는 `.ppk`를 `PuTTY`와 함께 사용한다. 
즉, `Windows 7`, `Windows 8`을 제외하고는 `.pem`을 선택해야 한다.

###### 1. PuTTY
`PuTTY`는 `SSH(Secure Shell), Telnet, TCP` 접속을 위한 클라이언트입니다.
서버에 단말 장비를 통해서 원격으로 접속하여 작업할 필요가 있을 때
윈도우같은 개인 `PC`에서도 서버로 접속할 수 있도록 물리적인 단말장비가 아닌 논리적인 가상 단말기를 제공합니다.

#### 4. Drive, Storage, Volume
드라이브(Drive)는 `SSD`, 하드디스크 등의 물리적인 저장장치를 말한다.

스토리지(Storage)는 한 개 이상의 드라이브로 구성된 대용량 데이터 저장장치를 말한다. 
영어 뜻으로는 원래 저장 장소를 의미하여 데이터 저장장치 그 자체를 스토리지 또는 스토리지 디바이스라고도 한다.
여러 개의 드라이브를 하나의 드라이브로 사용하기 위한 기술을 레이드(Raid, Redundant Array of Independent Disk)라고 한다.

볼륨(Volume)은 논리적인 드라이브를 말한다. 윈도우에서의 파티션과 같다.
하나의 하드디스크를 하나로 쓸 수 있지만, `C`, `D` 등 여러 개로 나누어 사용할 수도 있다.

### 5. Instance Detail
- `Instance ID` → 인스턴스의 고유 식별자
- `Public IPv4 address` → `EC2` 인스턴스에 접근하기 위해 사용할 주소
- `Private IPv4 address` → `AWS` 네트워크에서 내부적으로 인스턴스에 접근할 때 사용한다. 
- 이외에도 `Hostname`, `Private IP DNS name`, `Platform`, `AMI`, `Key pair` 등의 정보를 확인할 수 있다.
- `Stop Instance`를 선택하면 인스턴스를 일시정지 할 수 있다. 정지중인 인스턴스에는 요금이 부과되지 않는다.
- `Terminate Instance`를 선택하면 인스턴스 정보를 영구삭제 할 수 있다.
- `Start Instance`를 선택하면 인스턴스를 재기동할 수 있다. 인스턴스를 재기동하면 `Public IPv4` 주소는 변경되지만, 
`Private IPv4` 주소는 유지된다.

#### 1. IPv4
`IPv4`는 인터넷 프로토콜의 4번째 판이며 전 세계적으로 사용된 첫 번째 인터넷 프로토콜이다. 
`IPv4`는 인터넷에 연결된 각 장치 간 정보를 주고받을 수 있는 주소를 나타낸다.
이 인터넷 프로토콜은 약 43억 개의 고유한 IP 주소를 가질 수 있는 32비트 숫자의 형태이다.

#### 2. IPv6
인터넷은 `IPv4` 프로토콜로 구축되어 왔으나 `IPv4` 프로토콜의 주소가 32비트라는 제한된 주소 공간 및 국가별로 할당된 주소가 
거의 소진되고 있다는 한계점으로 인해 지속적인 인터넷 발전에 문제가 예상되어, 이에 대한 대안으로서 `IPv6` 프로토콜이 제안되었으며,
국제 표준이 `RFC`를 통해서 확정되었고, 실제로 `IPv6` 주소는 휴대폰 및 컴퓨터에 할당되어 적용되고 있다.

### 6. EC2 Instance Types - Overview
- You can use different types of EC2 instances that are optimised for different use cases [in this web site](https://aws.amazon.com/ec2/instance-types/)

- AWS has the following naming convention : example → `m5.2xlarge`
~~~
m → instance class 
→ 인스턴스 클래스, m은 범용 인스턴스임을 나타낸다.

5 → generation (AWS improves them over time)
→ 인스턴스의 세대

2xlarge → size within the instance class
→ 인스턴스 클래스 내의 크기, 크기가 클 수록 더 많은 메모리와 `CPU`를 가진다.
~~~
TODO 스크린샷

#### 1. EC2 Instance Types – General Purpose
- Great for a diversity of workloads such as web servers or code repositories 
→ 범용의 인스턴스는 웹 서버나 코드 저장소와 같은 다양한 작업에 적합하다. 인스턴스 이름은 `M`으로 시작된다.

- Balance between: Compute, Memory, Networking
→ 또, 컴퓨팅, 메모리, 네트워킹간의 균형도 잘 맞다.

#### 2. EC2 Instance Types – Compute Optimized
- Great for compute-intensive tasks that require high performance 
→ 컴퓨터 집약적인 고사양 작업에 최적화된 인스턴스이다. 이러한 고성능 인스턴스는 `C`로 시작하는 이름을 가지고 있다.

- processors:
~~~
Batch processing workloads → 데이터 일괄 처리
Media transcoding → 미디어 트랜스코딩
High performance web servers → 고성능 웹 서버
High performance computing (HPC) → 고성능 컴퓨팅
Scientific modeling & machine learning → 머신 러닝
Dedicated gaming servers → 전용 게임 서버 
~~~

###### 1. Media transcoding
###### 2. HPC

#### 3. EC2 Instance Types – Memory Optimized
- Fast performance for workloads that process large data sets in memory
→ 이 유형의 인스턴스는 메모리에서 대규모 데이터셋을 처리하는 유형의 작업에 빠른 성능을 제공한다.
메모리는 `RAM`을 뜻한다. 기본적으로 인스턴스 이름은 램을 나타내는 `R`로 시작되지만, `X`이나 대용량 메모리 `Z`도 존재한다.

- Use cases: 
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

###### 1. in-memory databases
디스크가 아닌 주 메모리에 모든 데이터를 보유하고 있는 데이터베이스. 디스크 검색보다 자료 접근이 훨씬 빠른 것이 가장 큰 장점이다. 
데이터 양의 빠른 증가로 데이터베이스 응답 속도가 떨어지는 문제를 해결할 수 있는 대안이 인 메모리 데이터베이스이다. 
전형적인 디스크 방식은 디스크에 저장된 데이터를 대상으로 쿼리를 수행하지만, 인 메모리 방식은 메모리상에 색인을 넣어 필요한 모든 정보를 메모리상의 색인을 통해 빠르게 검색할 수 있다.

단점이라면 매체가 휘발성이라는 것. DB 서버 전원이 갑자기 꺼져버리면 안에 있는 자료들이 초고속즉시삭제 되어버린다. 
그래서 보통은 로그인 세션 같은, 서버가 꺼져서 날아가도 상관 없는 임시 데이터에 주로 쓰인다. 
거기다 속도 때문에 쓰는 것이기에 압축 따윈 쓰지 않으며, 데이터에 비해 `RAM` 용량이 넉넉하지 않을 경우 가상메모리를 쓰게 되어 역효과가 일어나기도 한다.

###### 2. Business intelligence
비즈니스 인텔리전스(BI)는 조직이 더 나은 의사결정을 내리고, 
정보를 기반으로 행동을 취하고, 보다 효율적인 비즈니스 프로세스를 구현할 수 있게 해주는 역량을 의미한다. BI 역량을 갖추면 다음과 같은 일들이 가능하다.
~~~
조직 전반의 최신 데이터 수집
이해하기 쉬운 양식으로 데이터 표시(표, 그래프 등)
조직 내 직원들에게 시의적절하게 데이터 전달
~~~

#### 4. EC2 Instance Types – Storage Optimized
- Great for storage-intensive tasks that require high, sequential read and write access to large data sets on local storage
→ 로컬 스토리지에서 대규모의 데이터셋에 액세스할 때 적합한 인스턴스이다. 이름은 `I`, `G` 또는 `H1`으로 시작한다.
애플리케이션에 대해 대기 시간이 짧은, 수만 단위의 무작위 `IOPS`(초당 I/O 작업 수)를 지원하도록 최적화되었습니다.

- Use cases: 
~~~
High frequency online transaction processing (OLTP) systems
→ 고주파 온라인 트랜잭션 처리인 OLTP

Relational & NoSQL databases
→ 관계형과 비관계형인 NoSQL 데이터베이스

Cache for in-memory databases (for example, Redis)
Data warehousing applications
Distributed file systems
~~~
 
###### 1. OLTP
OLTP(온라인 트랜잭션 처리)는 온라인 뱅킹, 쇼핑, 주문 입력 또는 텍스트 메시지 전송 등 동시에 발생하는 다수의 
트랜잭션을 실행하는 데이터 처리 유형입니다. 이러한 트랜잭션은 전통적으로 경제 또는 재무 트랜잭션이라고 칭하며, 
기업이 회계 또는 보고 목적으로 언제든 정보에 액세스할 수 있도록 기록 및 보호됩니다.

###### 2. Data warehouse
데이터 웨어하우스는 보고 및 분석을 위해 구조화된 데이터(데이터베이스 테이블, Excel 시트) 및 반구조화된 데이터(XML 파일, 웹 페이지)를 저장하는 중앙 집중식 레포지토리이며,
많은 양의 정보를 저장할 수 있기 때문에 사용자가 데이터 마이닝, 데이터 시각화 및 기타 형태의 비즈니스 인텔리전스 보고에 사용할 수 있는 풍부한 과거 데이터에 쉽게 액세스할 수 있도록 한다.

---
#### ▶ Reference
- [[네트워크] SSH란?](https://hanamon.kr/네트워크-ssh란/)
- [인증서 파일 형식 및 확장자의 차이점 비교 설명 (Certificate file format & extensions)](https://www.letmecompile.com/certificate-file-format-extensions-comparison/)
- [[스토리지] RAID 정리 1. RAID 기본 설명 및 RAID Level (레이드 레벨)](https://harryp.tistory.com/806)
- [IPv4](https://ko.wikipedia.org/wiki/IPv4)
- [IPv6](https://ko.wikipedia.org/wiki/IPv6)
- [인 메모리 데이터베이스](https://namu.wiki/w/인%20메모리%20데이터베이스)
- [비즈니스 인텔리전스(BI)란?](https://www.oracle.com/kr/what-is-business-intelligence/)
- [OLTP란 무엇인가?](https://www.oracle.com/kr/database/what-is-oltp/)
- [데이터 웨어하우스란 무엇입니까?](https://azure.microsoft.com/ko-kr/resources/cloud-computing-dictionary/what-is-a-data-warehouse/#data-warehouse-definition)
---