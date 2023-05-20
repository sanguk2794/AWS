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
→ 머신이 작동할 때 명령을 시작하는 것을 말한다. 스크립트는 처음 시작할 때 한 번만 실행되고 다시 실행되지 않는다. 사용자 데이터가 실행 시 한 번만 실행되도록 지정하거나 매번 인스턴스를 재부팅하거나 시작할 때마다 실행되도록 지정할 수 있다.

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
- `Instance ID` → 인스턴스의 고유 식별자
- `Public IPv4 address` → `EC2` 인스턴스에 접근하기 위해 사용되는 주소
- `Private IPv4 address` → `AWS` 네트워크에서 내부적으로 인스턴스에 접근할 때 사용되는 주소 
- 이외에도 `Hostname`, `Private IP DNS name`, `Platform`, `AMI`, `Key pair` 등의 정보를 확인할 수 있다.
- `Stop Instance`를 통해 인스턴스의 일시정지가 가능하다. 정지중인 인스턴스에는 요금이 부과되지 않는다.
- `Terminate Instance`를 통해 인스턴스를 영구삭제할 수 있다.
- `Start Instance`를 통해 인스턴스를 재기동할 수 있다. 인스턴스를 재기동하면 `Public IPv4` 주소는 변경되지만 `Private IPv4` 주소는 유지된다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
