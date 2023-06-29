## 도커 이미지 생성
### 1. git 설치
~~~ shell
$ yum -y install git
~~~

![img](https://github.com/sanguk2794/AWS/assets/97398071/071fa57d-05c7-4306-8ea0-73fd51a9665d)

### 2. 레포지토리 다운로드
- 해당 예제 코드는 공식 홈페이지에 기재되어 있다. [getting-started](https://github.com/docker/getting-started/tree/master/app)

~~~ shell
$ git clone https://github.com/docker/getting-started.git
~~~

![img](https://github.com/sanguk2794/AWS/assets/97398071/e30b5ce5-669d-4d77-95bb-cf14039dc3e4)

- 다운로드 후의 파일 트리이다.

![img](https://github.com/sanguk2794/AWS/assets/97398071/b32aaf6c-a4a9-43fd-a315-b777396ba712)

### 3. Dockerfile
- `Dockerfile`은 컨테이너 이미지를 만드는데 사용되는 단순한 텍스트 기반 지침 스크립트이다.

![img](https://github.com/sanguk2794/AWS/assets/97398071/4da5a5aa-8495-4f98-aa4a-2c862a36de5e)

~~~
- FROM → 베이스 이미지를 선택할 수 있다. OS라고 생각하면 간단하다. Alpine은 매우 작은 Linux 배포판인 Alpine Linux를 기반으로 하는 기본 이미지이다. 
- RUN → 도커 이미지가 생성되기 전에 수행할 쉘 명령어를 작성할 때 사용한다.
- WORKDIR → WORKDIR 지시자는 뒤에 오는 모든 지시자(RUN, CMD, COPY, ADD 등)에 대한 작업 디렉토리를 설정할 때 사용한다.
- COPY → 도커 클라이언트의 현재 디렉토리에서 파일을 추가한다. WORKDIR를 별도로 지정했다면 로컬에 있는 파일들이 도커 컨테이너로 복사될때 WORKDIR에 정의한 디렉토리로 복사된다.
- CMD → 컨테이너 내에서 실행할 명령을 지정하며, Dockerfile 내에서 한 번만 사용할 수 있다.
~~~

### 4. 도커 빌드
#### 1. 빌드 실행 
- `.`를 설정하면 `Dockerfile`을 현재 디렉터리에서 찾는다.
~~~ shell
$ docker build -t getteing-started .
~~~

#### 2. 안 되서 Docker Compose 다운로드
→ 깃허브의 `Compose repository` 릴리즈 페이지에서 `Docker Compose`의 현재 안정 버전 릴리즈를 다운로드받는다.
~~~ shell
$ sudo curl \
    -L "https://github.com/docker/compose/releases/download/1.26.2/docker-compose-$(uname -s)-$(uname -m)" \
    -o /usr/local/bin/docker-compose

~~~

- 바이너리 파일에 실행 권한을 부여한다.
~~~ shell
$ sudo chmod +x /usr/local/bin/docker-compose
~~~

- 버전 확인
~~~ shell
$ docker-compose --version
~~~

#### 3. 버전 확인이 안 될때
- 다음과 같은 오류 발생

> Error loading Python lib '/tmp/_MEIMioI5Q/libpython3.7m.so.1.0': dlopen: libcrypt.so.1: cannot open shared object file: No such file or directory
 
- 파이썬 3 버전 확인, 버전이 달랐다.
~~~
$ python3 --version
~~~

- 도커 실행 파일의 [파이썬 설정 변경](https://realpython.com/python-versions-docker/#setting-up-your-python-environment)




---
#### ▶ Reference
- [Docker docs](https://docs.docker.com/)
- [도커 이미지 생성하기](https://ajdkfl6445.gitbook.io/study/devops/docker/make-image)
- [getting-started](https://github.com/docker/getting-started/tree/master)
- [AWS EC2 인스턴스(Ubuntu 18.04 LTS)에 Docker Compose 설치](https://insight.infograb.net/docs/aws/installing-docker-compose-on-aws-ec2/)
- [](https://realpython.com/python-versions-docker/#setting-up-your-python-environment)
---
