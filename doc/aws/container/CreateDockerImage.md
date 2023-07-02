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
- `.`를 설정하면 `Dockerfile`을 현재 디렉토리에서 찾는다.
~~~ shell
$ docker build -t getteing-started .
~~~

- 빌드 실행 중 다음과 같은 에러가 발생했다.
> Sending build context to Docker daemon  12.93MB
> Step 1/25 : FROM --platform=$BUILDPLATFORM python:alpine AS base
> failed to parse platform : "" is an invalid component of "": platform specifier component must match "^[A-Za-z0-9_-]+$": invalid argument

- [[Question] understand of "--platform=$BUILDPLATFORM" in dockerfile](https://github.com/moby/buildkit/issues/1241)

- 결과적으로 빌드 플랫폼 설정의 문제였다. platform 매개변수 추가를 통해 [다중 플랫폼 설정](https://www.docker.com/blog/faster-multi-platform-builds-dockerfile-cross-compilation-guide/)이 가능하다.
~~~ shell
$ docker buildx build -t getteing-started --platform=linux/amd64 .
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/5a4f5837-3212-413f-8649-dab5b07a48b8)

#### 1. 해결하기 위해서 실행했던 것
##### 1. docker-compose 다운로드
- `docker-compose.xml` 파일의 `args` 설정이 빌드시 읽히지 않는 것이 문제라고 판단해, `docker-compose`를 다운로드했다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/639b8780-04a9-4d2e-9a2e-19e186f3c663)

- 깃허브의 `Compose repository` 릴리즈 페이지에서 `Docker Compose`의 현재 안정 버전 릴리즈를 다운로드했다.

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

###### 1. `docker-compose`의 버전 확인 시 에러 발생
- 다음과 같은 에러 발생

> Error loading Python lib '/tmp/_MEIMioI5Q/libpython3.7m.so.1.0': dlopen: libcrypt.so.1: cannot open shared object file: No such file or directory

- `libxcrypt-compat` 다운로드로 해결
~~~ shell
$ sudo dnf install libxcrypt-compat
~~~

###### 1. docker-compose
- `docker-compose`는 다중 컨테이너 애플리케이션을 정의, 공유할 수 있도록 개발된 도구로 단일 명령을 사용하여 한 번에 실행 또는 종료를 수행할 수 있다.
- `docker-compose up` 명령어 하나로 문서에 정의한 서비스들이 한꺼번에 컨테이너로 실행하거나, `docker-compose down`이라는 명령어로 정의한 모든 서비스를 종료할 수 있다.
- 실행, 종료에는 아래와 같은 `yaml` 설정이 필요하다.
~~~ yaml
services:
  app:
    ## application image
    image: app_v2

  mysq:
    ## database image
    image: mysql
~~~





---
#### ▶ Reference
- [Docker docs](https://docs.docker.com/)
- [도커 이미지 생성하기](https://ajdkfl6445.gitbook.io/study/devops/docker/make-image)
- [getting-started](https://github.com/docker/getting-started/tree/master)
- [AWS EC2 인스턴스(Ubuntu 18.04 LTS)에 Docker Compose 설치](https://insight.infograb.net/docs/aws/installing-docker-compose-on-aws-ec2/)
- [Setting Up Your Python Environment](https://realpython.com/python-versions-docker/#setting-up-your-python-environment)
- [Faster Multi-Platform Builds: Dockerfile Cross-Compilation Guide](https://www.docker.com/blog/faster-multi-platform-builds-dockerfile-cross-compilation-guide/)
- [[Question] understand of "--platform=$BUILDPLATFORM" in dockerfile](https://github.com/moby/buildkit/issues/1241)
---
