## AWS CLI
### 1. How can users access AWS?
- To access AWS, you have three options
~~~
AWS Management Console (protected by password + MFA)
AWS Command Line Interface (CLI): protected by access keys
AWS Software Developer Kit (SDK) - for code: protected by access keys
~~~

- Access Keys are generated through the AWS Console
→ 액세스 키는 관리 콘솔을 사용해서 생성할 수 있으며, 사용자들이 자신들의 액세스 키를 직접 관리한다.

- Users manage their own access keys  
→ Access Keys are secret, just like a password. Don’t share them
~~~
Access Key ID ~= username
Secret Access Key ~= password
~~~
→ 액세스 키는 암호와 같다. 그러니 자신만의 액세스 키를 생성했다면 동료와 공유해서는 안 된다.

#### 1. Access key
터미널, 소스 코드에서의 AWS 액세스 시 자격 증명을 위해 사용되며 이 두 방식 모드 완전히 동일한 `Access key`를 사용한다.
명령줄 인터페이스에 이들이 적용되면 `AWS API`에 접근이 가능해진다.
~~~
Access key ID: Your Access key ID
Secret Access Key: Your Secret Access Key

Remember: don’t share your access keys
~~~

#### 2. Create access key
- `IAM` → `Users` → 유저 선택 → `Security credential` → `Access keys` → `Create access key`
→ `Access key`는 오직 생성시에만 표시 및 다운로드가 가능하며, 나중에 복구할 수도 없다. 단 나중에 원하는 새로운 키를 생성할수는 있다.

![Create access key button](https://user-images.githubusercontent.com/97398071/228143836-0581b180-2ed8-4bbe-bbbb-c3379d649efc.png)

- `Access key ID`와 `Secret access key`의 짝으로 이루어져 있다.

![AccessKey](https://user-images.githubusercontent.com/97398071/228142684-eb22853c-3dce-46ec-8b72-b28e93ba8924.png)
 
- `AWS CLI`에 접속할 때 `Access key ID`와 `Secret access key`, `Region` 이름을 입력한다. 예를 들면 `eu-west-1`이다.

![Issue AccessKey](https://user-images.githubusercontent.com/97398071/228144437-bf492a97-e126-44a0-b033-25b24a3b71aa.png)

### 2. What’s the AWS CLI?
- A tool that enables you to interact with AWS services using commands in your command-line shell
→ 쉘에서 명령어를 사용하여 `AWS` 서비스들과 상호작용 할 때 사용하는 도구이다.

- Direct access to the public APIs of AWS services
→ `AWS` 서비스의 공용 `API`로 직접 접근이 가능하다.

- You can develop scripts to manage your resources
→ `CLI`를 통해 리소스를 관리하는 스크립트를 개발해 일부 작업을 자동화 할 수 있다.

- It’s open-source [https://github.com/aws/aws-cli](https://github.com/aws/aws-cli)
→ `CLI`는 오픈소스로 제공되고 있으며, `AWS` 관리 콘솔 대신 사용되기도 한다.

#### 1. AWS CLI Install in windows
- [Installing or updating the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
→ 버전 1보다 성능과 기능이 조금 더 향상된 버전이다.

- `MSI installer`을 실행한다. 설치 완료 후 확인을 위해 `CLI`에서 `AWS` 버전 확인을 위한 명령어를 실행한다. 
2로 시작하는 버전이 나오면 최신 버전의 설치가 성공한 것이다.
~~~
$ aws --version
~~~
![Install AWS CLI](https://user-images.githubusercontent.com/97398071/228144390-c99f7e68-c2ae-4f43-861e-c4fbe6409d9c.png)

#### 2. AWS CLI Install in linux
- 아래 명령어를 실행하면 `AWS CLI`가 설치된다.
~~~ shell script
$ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
$ unzip awscliv2.zip
$ sudo ./aws/install
~~~

- 설치 완료 후 확인을 위해 `CLI`에서 `AWS` 버전 확인을 위한 명령어를 실행한다. 
~~~
$ /usr/local/bin/aws --version
~~~

#### 3. Connection with AWS API
- 아래 명령어를 실행 후 `Access Key`, `Secret Access Key`, `Default region name`, `Default output format`을 설정한다.
~~~
$ aws configure
~~~

- 명령어를 실행해 접속 가능 여부를 확인한다.
~~~
$ aws iam list-users
~~~
![Connect AWS CLI](https://user-images.githubusercontent.com/97398071/228143434-3f78c3a3-8468-40a6-81d1-f3903e66b5e6.png)

### 3. What’s the AWS SDK?
- AWS Software Development Kit (AWS SDK), Language-specific APIs (set of libraries)
_→ 특정 언어로 이루어진 라이브러리의 집합이며, 프로그래밍 언어에 따라 개별 `SDK`가 존재한다.

- Enables you to access and manage AWS services programmatically
→ `AWS 서비스` 또는 `공용 API`에 프로그래밍을 통한 접근이 가능하다. 

- Embedded within your application
→ 코딩을 통해 애플리케이션 내에서 구현한다. 즉 애플리케이션 내에 자체적으로 `AWS SDK`가 존재하는 것이다.

- Supports
→ 다양한 언어의 `SDK`를 제공한다.
~~~
SDKs (JavaScript, Python, PHP, .NET, Ruby, Java, Go, Node.js, C++, …)
Mobile SDKs (Android, iOS, …)
IoT Device SDKs (Embedded C, Arduino, …)
~~~

- Example: AWS CLI is built on AWS SDK for Python
→ 실제로는 `AWS CLI`조차 `Python`용 `AWS SDK`를 통해 구현되어 있다._

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
