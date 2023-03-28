## EC2
### 1. Amazon EC2
- EC2 is one of the most popular of AWS’ offering
→ `EC2`는 아마존에서 가장 인기 있는 서비스이다. 

- EC2 = Elastic Compute Cloud = Infrastructure as a Service
→ `EC2`는 `AWS`에서 제공하는 서비스형 `Infrastructure`이다.

- It mainly consists in the capability of :
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
→ `EC2`는 높은 수준에서 보면 많은 것을 포함하고 있으며, 그렇기에 하나의 서비스가 아니다. 

- Knowing EC2 is fundamental to understand how the Cloud works
→ `AWS EC2`의 사용법을 아는 것은  클라우드 작동 방식을 이해할 때 필수적이다. 
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
즉, 시작 스크립트는 `root 폴더`를 기준으로, `sudo` 기준으로 작성해야한다.
