## EC2
### 1. Amazon EC2
- EC2 is one of the most popular of AWS offering  
→ `EC2`는 아마존에서 가장 인기 있는 서비스이다. 

- EC2 = Elastic Compute Cloud = Infrastructure as a Service  
→ `EC2`는 `AWS`에서 제공하는 서비스형 `Infrastructure`이다.

- It mainly consists in the capability of EC2, EBS, ELB, ASG  
→ `EC2`는 여러 서비스를 포함하는 개념이다.
~~~
- Renting virtual machines (EC2)
→ 가상 머신을 EC2에서 임대할 수 있다. 이를 EC2 인스턴스라고 한다.

- Storing data on virtual drives (EBS)
→ 데이터를 가상 드라이브 또는 EBS 볼륨에 저장할 수 있다.

- Distributing load across machines (ELB)
→ 일래스틱 로드 밸런서로 로드를 분산시킬 수 있다.

- Scaling the services using an auto-scaling group (ASG)
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
→ 랜덤 액세스 메모리의 사이즈를 선택할 수 있다.

- How much storage space
~~~
- Network-attached (EBS & EFS)
→ 스토리지와 스토리지 용량을 선택할 수 있다. 

- hardware (EC2 Instance Store)
→ 물리적 서버에 연결된 하드웨어 드라이브의 종류를 지정할 수 있다.
~~~

- Network card: speed of the card, Public IP address  
→ `EC2` 인스턴스에 연결할 네트워크의 종류를 선택할 수 있다. 속도가 빠른 네트워크 카드를 원하는지 어떤 종류의 공용 `IP`를 원하는지 설정할 수 있다.

- Firewall rules: security group  
→ 보안 그룹의 설정이 가능하다.

- Bootstrap script (configure at first launch): EC2 User Data  
→ 사용자 스크립트의 설정이 가능하다. 인스턴스 기동시 자동 실행되는 스크립트이다.

### 3. Create EC2 Instance
- `EC2` → `Instances` → `Launch an instance` 버튼을 눌러 인스턴스 생성 화면으로 이동할 수 있다.

![Create instance button](https://user-images.githubusercontent.com/97398071/229365921-a4eda42b-2682-4006-909d-954faf6c7510.png)

- Set name and tag  
→ 인스턴스명과 태그를 작성하는 부분이다.

- `Application and Os Images (Amazon Machine Image)`  
→ `EC2` 인스턴스에 사용할 기본 이미지를 선택하는 부분이다.

![Set name, tag, AMI](https://user-images.githubusercontent.com/97398071/229365918-745e675d-e506-40b8-82c8-6e9dba2e8021.png)

- `Instance type`  
→ 인스턴스의 타입을 설정하는 부분이다. 인스턴스의 타입은 `CPU` 개수, 메모리의 용량, 비용에 따라 달라진다. `Compare instance types` 버튼을 클릭하면 모든 타입의 인스턴스 정보를 확인할 수 있다.

- `Key pair (login)`  
→ `SSH` 유틸리티를 이용해 인스턴스에 접근할 때 사용된다. 키 페어를 설정하지 않으면 `SSH`를 통한 인스턴스 접속이 불가능하다.

![Set instanceType, Keypair](https://user-images.githubusercontent.com/97398071/229365919-d96cb2da-ae60-45a3-880a-083485b6ceb7.png)

- `Create Key Pair` → `Set Key pair type` → `Set Private key file format` 버튼을 눌러 키 페어 생성 화면으로 이동할 수 있다.

![Create keypair](https://user-images.githubusercontent.com/97398071/229365920-da11d8ef-84fe-4769-afbd-a1612976c74c.png)

- `Network settings`  
→ 보안 그룹을 통해 인스턴스에 대한 트래픽 허용 규칙을 설정할 수 있다. 중복 체크가 가능하다.
~~~
- Allow SSH traffic from the internet
→ 인터넷으로부터의 SSH 트래픽을 허용한다.

- Allow HTTPs traffic from the internet
→ 인터넷으로부터의 HTTPs 트래픽을 허용한다.

- Allow HTTP traffic from the internet
→ 인터넷으로부터의 HTTP 트래픽을 허용한다.
~~~

- `Storage (volumes)`
→ 스토리지 설정이 가능하다. 

- `Storage (volumes)` → `Advanced` → `Delete on termination`에서 루트 스토리지의 자동 삭제 여부를 설정 가능하며, 기본값은 `Yes`이다. `EC2` 인스턴스를 종료하면 그 볼륨도 삭제될 것이라는 것을 의미한다.

![Set network, storage](https://user-images.githubusercontent.com/97398071/229365917-55574eef-17c9-42f3-b610-926988bf2afc.png)

- `Advanced details` → `User data`  
→ `User Data`에 작성한 쉘스크립트는 인스턴스가 처음으로 실행될 때, 즉 인스턴스의 전체 수명주기 중에 단 한 번만 실행된다. 

- `Launch Instance`

#### 1. EC2 User Data
- It is possible to bootstrap our instances using an EC2 User data script.  
→ `EC2` 사용자 데이터 스크립트를 사용하여 인스턴스의 부트스트랩이 가능하다.

- bootstrapping means launching commands when a machine starts, That script is only run once at the instance first start  
→ 부트스트랩은 머신의 기동을 위해 실행되는 명령어이다. 스크립트는 처음 시작할 때 한 번만 실행되고 다시 실행되지 않는다.

- EC2 user data is used to automate boot tasks such as: Installing updates, Installing software, Downloading common files from the internet, Anything you can think of  
→ 인스턴스를 부팅할 때 자동화하고 싶은 작업(업데이트, 소프트웨어 설치, 파일 다운로드, 그 외의 생각하는 모든 것)들을 설정할 수 있다. 사용자 데이터 스크립트에 작업을 더 추가할수록 부팅시 인스턴스가 할 일이 늘어날 뿐이다.

- The EC2 User Data Script runs with the root user  
→ 시작 스크립트를 작성할 때에는 `root 폴더`, `sudo`를 기준으로 작성해야한다. `User Data Script`는 `root` 권한을 가지고 `root` 패스에서 시작되기 때문이다.

#### 2. Drive, Storage, Volume
- Drive  
→ 드라이브는 `SSD`, 하드디스크 등의 물리적인 저장장치를 말한다.

- Storage  
→ 스토리지는 한 개 이상의 드라이브로 구성된 대용량 데이터 저장장치를 말한다. 여러 개의 드라이브를 하나의 스토리지로 사용하기 위한 기술을 `레이드 (Raid, Redundant Array of Independent Disk)`라고 한다.

- Volume  
→ 볼륨은 논리적인 드라이브를 말한다. 윈도우에서의 파티션과 같다. 하나의 하드디스크를 하나로 쓸 수 있지만, `C`, `D` 등 여러 개로 나누어 사용할 수도 있는 것이다.

#### 3. Instance Detail
- 인스턴스 세부 정보에서 여러 정보들을 확인할 수 있다.
~~~
- Instance ID  
- Public IPv4 address 
- Private IPv4 address
- Hostname, Platform, AMI, Key pair 등..
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/6705f537-f494-4546-986a-e822506886dc)

#### 4. 인스턴스 정지, 재기동
- `Stop Instance`를 실행해 인스턴스가 일시적으로 정지된다. 정지중인 인스턴스에는 요금이 부과되지 않는다.
- `Terminate Instance`를 실행해 인스턴스를 영구적으로 삭제할 수 있다.
- `Start Instance`를 실행해 인스턴스를 재기동할 수 있다. 인스턴스를 재기동하면 `Public IPv4` 주소는 변경되지만 `Private IPv4` 주소는 유지된다.

#### 5. 인스턴스 개수 제한
- 지역당 `vCPU` 기반 온디맨드 인스턴스의 개수 제한이 있다. 한도 증가 양식을 `AWS`에 제출할 수 있다.
- 새 `AWS` 계정은 여기에 설명된 제한보다 낮은 제한으로 시작할 수 있다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
