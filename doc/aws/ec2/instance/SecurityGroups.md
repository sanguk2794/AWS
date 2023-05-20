## Security Groups
### 1. Introduction to Security Groups
- Security Groups are the fundamental of network security in AWS  
→ 보안 그룹은 `AWS 클라우드`에서 네트워크 보안을 실행하는데 핵심이 된다.

- They control how traffic is allowed into or out of our EC2 Instances.  
→ 보안 그룹을 통해 `EC2` 인스턴스에 들어오고 나가는 트래픽을 제어할 수 있다.

- Security groups only contain rules  
→ 보안 그룹은 허용 규칙만을 포함한다.

- Security groups rules can reference by IP or by security group  
→ `IP Address`를 참조해 규칙을 만들 수도 있다. 컴퓨터의 위치나 다른 보안 그룹을 참조하는 것이다. 외부에서 `EC2` 인스턴스로 들어오는 것이 허용되면 아웃바운드 트래픽도 수행할 수 있다.

- Security groups are acting as a “firewall” on EC2 instances  
~~~
- Access to Ports
→ Port 액세스를 제어할 수 있다.

- Authorised IP ranges – IPv4 and IPv6
→ 인증된 IP 주소의 범위를 확인해 IPv4인지 IPv6인지 확인할 수 있다.

- Control of inbound network (from other to the instance)
→ 외부에서 인스턴스로 들어오는 인바인드 네트워크를 통제할 수 있다.

- Control of outbound network (from the instance to other)
→ 인스턴스에서 외부로 나가는 아웃바운드 네트워크를 통제할 수 있다.
~~~

![Security Group Diagram](https://user-images.githubusercontent.com/97398071/231209003-3206de7e-2207-4c3e-9fad-45e08c7b1150.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 보안 그룹은 `Type`과 프로토콜, 트래픽이 인스턴스에서 통과하는 위치인 포트 범위, `IP Address`의 범위인 소스 등을 포함한다.  
→ 허용되지 않은 소스로부터의 접근은 방화벽이 차단하기 때문에 외부에서 액세스가 불가능하고 액세스 시 타임아웃이 발생한다.

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

### 2. Referencing other security groups Diagram
- 보안 그룹 설정을 통한 인바운드 트래픽 제어 시나리오이다.
~~~
- EC2 인스턴스에 보안 그룹 1이라는 보안 그룹을 설정했다. 이 보안 그룹의 기본적인 인바인드 규칙은 보안 그룹 1의 인바운드를 보안 그룹 2에 허용하는 것이다.
- 인바운드 규칙으로 보안 그룹 2의 연결을 허용했다. 따라서 보안 그룹 2를 통한 연결은 허용된다.
- 인바운드 규칙으로 보안 그룹 3의 연결을 명시적으로 허용하지 않았다. 따라서 보안 그룹 3을 통한 연결은 거부된다.
~~~

![Referencing other security groups Diagram](https://user-images.githubusercontent.com/97398071/231212524-46b0725c-2ac9-4c96-9176-fa975e6afa4a.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. Classic Ports to know
- 22 = SSH (Secure Shell) - log into a Linux instance
- 21 = FTP (File Transfer Protocol) – upload files into a file share

- 22 = SFTP (Secure File Transfer Protocol) – upload files using SSH  
→ `SSH`는 다른 시스템으로 파일을 전송할 수 있는 파일 전송 프로토콜 역할도 수행한다.

- 80 = HTTP – access unsecured websites
- 443 = HTTPS – access secured websites

- 3389 = RDP (Remote Desktop Protocol) – log into a Windows instance  
→ 원격 데스크톱 프로토콜이다. 윈도우 인스턴스에 로그인할 때 사용된다. 22번 포트는 리눅스용이며 3389번 포트는 윈도우 인스턴스용인 것이다.

### 4. Practice Network and Security Groups
![Security Groups](https://user-images.githubusercontent.com/97398071/231216317-8e9b3281-0687-4468-8966-0ea27b3a8d54.png)

- `Network & Security` → `Security Groups`  
→ 보안 그룹을 확인할 수 있으며, 각 보안 그룹의 인바인드 규칙, 아웃바인드 규칙을 설정할 수 있다.

![Inbound Rule](https://user-images.githubusercontent.com/97398071/231217169-40d482f6-bf18-47da-b0a4-65942e215c14.png)

- `SSH`를 시도하거나 `HTTP` 쿼리를 실행하는데 타임아웃이 발생한다면 그건 100% `EC2` 보안 그룹의 설정 문제이다.

### 5. SSH Summary
- `SSH (Secure SHell)`는 네트워크 상의 다른 컴퓨터에 로그인하거나 원격 시스템에서 명령을 실행하고 다른 시스템으로 파일을 복사할 수 있도록 해주는 응용 프로그램 또는 그 프로토콜을 가리킨다. 
- 예를 들어 원격 저장소, 소스 코드를 원격 저장소인 깃헙에 푸쉬할 때 `SSH`를 활용해 파일을 전송하게 된다.

- 컴퓨터 운영체제에 따라 `SSH` 접속 방법이 나눠진다.  
→ `Mac`, `Linux`, `Windows 10`에서는 `.pem`을 사용하지만, `Windows 10` 미만의 윈도우 버전에서는 `.ppk`를 `PuTTY`와 함께 사용한다. `PuTTY`는 `SSH(Secure Shell)` 접속을 위한 클라이언트이다.
`EC2` 서버에 접속하기 위한 다른 방법으로 `EC2 Instance Connect`도 있는데, 이를 사용하면 터미널이나 클라이언트 프로그램이 아닌 웹 브라우저로 `EC2` 인스턴스에 연결할 수 있다. `EC2 Instance Connect`는 모든 버전의 `Mac`, `Linux`, 그리고 `Windows`에서 사용이 가능하다는 장점이 있다.

![image](https://user-images.githubusercontent.com/97398071/231314770-6745238d-b5ab-41a7-bf51-5d607a730923.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 6. RSA
- `RSA`는 공개키 암호시스템의 하나로, 암호화뿐만 아니라 전자서명이 가능한 최초의 알고리즘으로 알려져 있다.
- 로널드 라이베스트(Ron Rivest), 아디 샤미르(Adi Shamir), 레너드 애들먼(Leonard Adleman)의 연구에 의해 체계화되었으며, `RSA`라는 이름은 이들 3명의 이름 앞글자를 딴 것이다.

- `RSA` 암호는 안전한 통신을 위해 `Public Key`와 `Private Key`를 활용한다.
→ 공개키는 모두에게 알려져 있으며 메시지를 암호화하는데 쓰이며, 암호화된 메시지는 개인키를 가진 자만이 복호화하여 열어볼 수 있다.

#### 1. 개인키로부터 공개키 생성
- `RSA`는 개인키로부터 공개키를 생성할 수 있다. 해당 커맨드의 출력 결과에 공개 키를 정의하는데 필요한 `modulus`와 `publicExponent`의 정보가 포함되어 있는 것을 확인할 수 있다.
~~~ shell script
$ openssl rsa -text -in private.pem
~~~

- 개인키를 공개키를 추출할 때에는 아래의 커맨드를 실행한다.
~~~ shell script
$ openssl rsa -in private.pem -outform PEM -pubout -out public.pem
~~~

### 7. How to SSH into your EC2 Instance in Windows
- `openssl`을 설치한다. `PuTTY`, `Xshell`, `PowerShell` 여러 응용 프로그램이 있으므로 편한 것을 설치하자.

- 접속
~~~ shell script
$ ssh -i ./[your private keypair file name] ec2-user@[your public ip address] 
~~~

![image](https://user-images.githubusercontent.com/97398071/231524143-3f6cc140-5fc6-4afc-867b-97c4765560ef.png)

- 혹시 안 된다면 키 파일의 소유자를 확인해보자. 윈도우는 `chown` 키워드가 없지만 속성, 보안 항목에서 소유자를 확인하거나 변경할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/231525116-7ab55f5b-9b71-4a66-a941-2669e8855889.png)

- 접속을 종료하기 위해서는 `exit`를 입력하거나 `ctrl + z`를 눌러야 한다.

### 8. EC2 Instance Connect
- 인스턴스 선택 후, `Connect` 버튼을 누르면 `Connect to instance` 화면으로 이동한다. 여기에서 주황색 연결 버튼을 누르면 `EC2 Instance Connect`를 통해 인스턴스에 접근 가능하다.

![image](https://user-images.githubusercontent.com/97398071/231527229-b025fc05-e9aa-413c-a640-3b608c55bd61.png)

- `EC2 instance Connect` 또한 인바운드 설정에 의존한다. 보안 그룹을 통해 `SSH` 접속을 허용하지 않는다면 `EC2 instance Connect`를 통한 서버 접속 또한 불가능하다.

![image](https://user-images.githubusercontent.com/97398071/231528323-a592ca66-0ac0-44b2-a6da-a5e1a38e193b.png)

- 자격 증명이 필요한 서비스는 `Access Key`를 요구한다. 하지만 `Access Key`는 서버상에 절대로 입력하면 안 된다. 다른 유저들이 인스턴스에 입력된 자격 증명 정보를 회수할 수 있게 되기 때문이다.

~~~ shell script
$ aws iam list-users
~~~

![image](https://user-images.githubusercontent.com/97398071/231529746-04a8aba5-cd7c-450c-bcc6-302b07112133.png)

- 이런 때를 위해 필요한 것이 바로 `IAM Role`이다. `Action` → `Security` → `Modify IAM Role` 버튼을 누르면 `IAM Role`의 설정이 가능하다.

![image](https://user-images.githubusercontent.com/97398071/231530277-4354140e-cb7e-48a1-b0a6-420e9328e90c.png)

---
#### ▶ Reference
- [[네트워크] SSH란?](https://hanamon.kr/네트워크-ssh란/)
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---