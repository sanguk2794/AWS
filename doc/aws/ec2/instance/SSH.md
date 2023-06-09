## SSH
### 1. SSH Summary
- `SSH (Secure SHell)`는 네트워크 상의 다른 컴퓨터에 로그인하거나 원격 시스템에서 명령을 실행하고 다른 시스템으로 파일을 복사할 수 있도록 해주는 응용 프로그램 또는 그 프로토콜을 가리킨다.
- 예를 들어 원격 저장소, 소스 코드를 원격 저장소인 깃헙에 푸쉬할 때 `SSH`를 활용해 파일을 전송하게 된다.

- 컴퓨터 운영체제에 따라 `SSH` 접속 방법이 나눠진다.  
→ `Mac`, `Linux`, `Windows 10`에서는 `.pem`을 사용하지만, `Windows 10` 미만의 윈도우 버전에서는 `.ppk`를 `PuTTY`와 함께 사용한다. `PuTTY`는 `SSH(Secure Shell)` 접속을 위한 클라이언트이다.

- `EC2` 서버에 접속하기 위한 다른 방법으로 `EC2 Instance Connect`도 있는데, 이를 사용하면 터미널이나 클라이언트 프로그램이 아닌 웹 브라우저로 `EC2` 인스턴스에 연결할 수 있다. `EC2 Instance Connect`는 모든 버전의 `Mac`, `Linux`, 그리고 `Windows`에서 사용이 가능하다.

![image](https://user-images.githubusercontent.com/97398071/231314770-6745238d-b5ab-41a7-bf51-5d607a730923.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. RSA
- `RSA`는 공개키 암호시스템의 하나로, 암호화뿐만 아니라 전자서명이 가능한 최초의 알고리즘으로 알려져 있다.
- 로널드 라이베스트, 아디 샤미르, 레너드 애들먼의 연구에 의해 체계화되었으며 `RSA`라는 이름은 이들 3명의 이름 앞글자를 딴 것이다.

- `RSA` 암호는 안전한 통신을 위해 `Public Key`와 `Private Key`를 활용한다.  
→ 공개키는 모두에게 알려져 있으며 메시지를 암호화하는데 쓰인다. 암호화된 메시지는 개인키를 가진 자만이 복호화하여 열어볼 수 있다.

- `RSA`는 개인키로부터 공개키를 생성할 수 있다. 공개키를 정의하는데 필요한 `modulus`와 `publicExponent`의 정보가 포함되어 있기 때문이다.
~~~ shell script
$ openssl rsa -text -in private.pem
~~~

- 개인키를 이용해 공개키를 추출하려면 아래의 커맨드를 실행해야 한다.
~~~ shell script
$ openssl rsa -in private.pem -outform PEM -pubout -out public.pem
~~~

### 3. How to SSH into your EC2 Instance in Windows
- `openssl`을 설치한다. `PuTTY`, `Xshell`, `PowerShell` 등 여러 응용 프로그램이 있으므로 편한 것을 설치하자.

- 접속을 위한 명령어는 다음과 같다.
~~~ shell script
$ ssh -i ./[your private keypair file name] ec2-user@[your public ip address] 
~~~

![image](https://user-images.githubusercontent.com/97398071/231524143-3f6cc140-5fc6-4afc-867b-97c4765560ef.png)

- 혹시 안 된다면 키 파일의 소유자를 확인해보자. 윈도우는 `chown` 키워드가 없지만 속성, 보안 항목에서 소유자를 확인하거나 변경할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/231525116-7ab55f5b-9b71-4a66-a941-2669e8855889.png)

- 접속을 종료하기 위해서는 `exit`를 입력하거나 `ctrl + z`를 눌러야 한다.

### 4. EC2 Instance Connect
- 인스턴스 선택 후, `Connect` 버튼을 누르면 `Connect to instance` 화면으로 이동한다. 여기에서 주황색 연결 버튼을 누르면 `EC2 Instance Connect`를 통해 인스턴스에 접근 가능하다.

![image](https://user-images.githubusercontent.com/97398071/231527229-b025fc05-e9aa-413c-a640-3b608c55bd61.png)

- `EC2 instance Connect` 또한 인바운드 설정에 의존한다. 보안 그룹을 통해 `SSH` 접속을 허용하지 않는다면 `EC2 instance Connect`를 통한 서버 접속 또한 불가능하다.

![image](https://user-images.githubusercontent.com/97398071/231528323-a592ca66-0ac0-44b2-a6da-a5e1a38e193b.png)

- 자격 증명이 필요한 서비스는 `Access Key`를 요구한다. 하지만 `Access Key`는 서버상에 절대로 입력하면 안 된다. 다른 유저들이 인스턴스에 입력된 자격 증명 정보를 회수할 수 있게 되기 때문이다.

![image](https://user-images.githubusercontent.com/97398071/231529746-04a8aba5-cd7c-450c-bcc6-302b07112133.png)

- 액세스 키를 입력하는 대신 `IAM Role`을 설정할 수 있다. `Action` → `Security` → `Modify IAM Role` 버튼을 누르면 인스턴스의 `IAM Role` 설정이 가능하다.

![image](https://user-images.githubusercontent.com/97398071/231530277-4354140e-cb7e-48a1-b0a6-420e9328e90c.png)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---