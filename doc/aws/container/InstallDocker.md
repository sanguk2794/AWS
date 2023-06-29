## 도커
### 1. 도커란
#### 1. architecture
- 도커는 클라이언트-서버 아키텍처를 사용한다. 
- 클라이언트는 도커 컨테이너를 빌드, 실행 및 배포하는 무거운 작업을 수행하는 도커 데몬과 통신한다. 
- 도커 클라이언트와 데몬은 동일한 시스템에서 실행 될 수 있으며, 원격 도커 데몬에 연결할 수도 있다. 
- 도커 클라이언트와 데몬은 UNIX 소켓 또는 네트워크 인터페이스를 통해 `REST API`를 사용하여 통신한다. 

![image](https://github.com/sanguk2794/AWS/assets/97398071/38fa3478-2994-49ad-8551-9b70f2125fe3)

참조 → [Docker overview, Docker Architecture](https://docs.docker.com/get-started/overview/)

#### 2. Docker
- 도커는 도커 컨테이너를 위한 운영체제이며, 각 서버에 설치되어 컨테이너를 구축, 시작, 중단하는데 사용할 수 있는 명령어를 제공해 준다.

#### 3. Docker Image
- 도커 이미지는 도커 컨테이너 생성 지침이 포함된 읽기 전용 템플릿이다. 이 이미지에는 코드에 필요한 라이브러리, 종속성에 대한 정의, 실행되는 코드가 포함되어 있다.

#### 4. Docker Container
- 도커 이미지를 인스턴스화한 것이 도커 컨테이너이다.

#### 5. 도커 개요
- 도커는 `Go Lang`으로 작성되었으며 `Linux` 커널의 여러 기능을 활용하여 기능을 제공한다. 
- 도커는 격리된 작업 공간을 제공하기 위해 `namespaces`, `cgroup (control groups)` 기능을 사용한다.
- 컨테이너를 실행하면 도커는 해당 컨테이너에 대한 네임스페이스 세트를 생성하고, 이 네임 스페이스는 격리 공간을 제공한다.

###### 1. namespace
- 네임스페이스의 주요 기능은 프로세스를 서로 격리하는 데 사용되는 리눅스 커널 기능이다.
- `namespace`가 프로세스를 서로 격리하는 기술이기에 실제 도커는 격리되어 있는 각각의 프로세스이다.

###### 2. control group
- 제어 그룹은 프로세스 모음의 리소스 사용량(CPU, 메모리, 디스크 I/O, 네트워크 등)을 설정하는데 사용되는 리눅스 커널의 기능이다.

#### 6. 다른 운영체제에서 도커 데몬을 실행할 수 있는 이유
- 운영체제가 리눅스가 아닌 경우에도 도커 컨테이너 간 격리가 가능한 이유는 간단한 명령어를 통해 확인할 수 있다.

~~~ shell
$ docker version
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/919abd66-9a01-4dcd-81fa-3cd38278ae99)

- 서버 정보 중 `OS` 정보가 `linux`로 기재되어 있는 것을 확인할 수 있다.
- 모든 컨테이너는 리눅스 가상 가상 머신 위에 생성되기 때문에, 다른 운영체제라고 하더라도 컨테이너를 격리시키고 리소스를 할당, 제한할 수 있다.

### 2. 도커 생성하기
#### 1. EC2 인스턴스 생성, SSH 접속

![image](https://github.com/sanguk2794/AWS/assets/97398071/0bba32fc-146a-4fde-b817-98e31b93545b)

#### 2. 도커 다운로드, 버전 확인
~~~ shell
$ yum install -y docker
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/dd186c39-a329-495c-824a-5433585a4b57)

~~~ shell
$ docker -v
~~~

#### 3. 도커 시작
~~~ shell
$ sudo service docker start
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/bb1436b3-64d0-478b-9b3d-bb4ad86cd453)

#### 4. 도커 그룹에 sudo 권한 추가, hello-world 컨테이너 실행
- 도커 그룹에 `sudo` 권한을 추가한다.
~~~ shell
$ sudo usermod -aG docker ec2-user
~~~

- `hello-world` 도커 이미지 실행에 성공하면 `Docker Hub`를 통해 `EC2`에 컨테이너를 실행할 수 있다.
~~~ shell
$ docker run hello-world
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/85f5c23b-3c71-4bc5-931b-98f8063fd760)

### 3. 도커로 아파치 서버 실행
#### 1. 아파치 도커 이미지 다운로드
~~~ shell
$ docker pull httpd:latest
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/636ad95c-2df8-4696-95e8-e94e5498c731)

#### 2. 아파치 컨테이너 실행
~~~ shell
$ docker run -d --name apache -p 443:80 httpd
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/df8ca7d0-9f03-4346-bb98-d37160e4f207)

###### 1. 컨테이너 상태 확인
- 현재 실행중인 컨테이너의 상태를 확인할 수 있다.
~~~ shell
$ docker ps
~~~

- 현재 실행중인 컨테이너뿐만 아니라 실행 종료된 컨테이너의 상태를 확인할 수 있다.
~~~ shell
$ docker ps -a
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/bae4a11c-f9a3-441e-ab8e-20260c3eaaf5)

#### 3. 접속 확인
- 아무 생각 없이 8080으로 접속하면 보안 그룹 설정으로 인해 접속이 안 될 수 있다.
- `[Your public IP Address]:443`로 접속해 아파치 서버 접속 여부를 확인할 수 있다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/311834c4-47f2-4e58-920e-50534007e700)

#### 4. 컨테이너 종료
- 컨테이너 중지 명령어이다. 중지할 컨테이너의 아이디 또는 이름을 지정해야 한다.
~~~ shell
$ docker stop apache
~~~

- 컨테이너 삭제 명령어이다. 중지할 컨테이너의 아이디 또는 이름을 지정해야 한다. 실행 중인 컨테이너를 삭제하려고 하면 실패한다.
~~~ shell
$ docker rm apache
~~~
![image](https://github.com/sanguk2794/AWS/assets/97398071/1078cde8-887c-426e-beeb-b5dab4fbbfe9)


---
#### ▶ Reference
- [Docker docs](https://docs.docker.com/)
- [[Docker] 도커 입문하기 1 - 맛보기 및 설치](https://code-masterjung.tistory.com/130)
- [[Docker] 도커 입문하기 2 - 배경 지식 쌓기](https://code-masterjung.tistory.com/131)
- [[Docker] 도커 입문하기 3 - 기본 명령어 및 실습](https://code-masterjung.tistory.com/132)
- [도커로 아파치 웹 서버 구축하기](https://conservative-vector.tistory.com/entry/도커에서-아파치-웹-서버-실행하기)
---
