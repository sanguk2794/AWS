## Docker
### 1. What is Docker?
- Docker is a software development platform to deploy apps  
→ 도커는 앱 배포를 위한 컨테이너 기반의 소프트웨어 개발 플랫폼이며, 오픈소스 가상화 플랫폼 중 하나이다.

- Apps are packaged in containers that can be run on any OS  
→ 컨테이너에 앱이 패키징되며, 이 때 컨테이너는 표준화되어 있어서 어떤 운영체제에서든 실행시킬 수 있다.

- Apps run the same, regardless of where they’re run  
→ 다시 말해, 앱이 컨테이너에 패키징되면 어느 운영체제에서든 같은 방식으로 실행이 가능하며, 코드가 문제없이 실행될 것임을 확신할 수 있다.
~~~
- Any machine No compatibility issues
→ 어떤 기기든 호환성 문제가 없다.

- Predictable behavior
→ 행동을 예측 가능하다.

- Less work
→ 작업이 적어진다.

- Easier to maintain and deploy
→ 유지 및 배포가 쉽다.

- Works with any language, any OS, any technology
→ 언어, 운영체제, 기술에 상관 없이 실행이 가능하다.
~~~

- Use cases: microservices architecture, lift-and-shift apps from on-premises to the AWS cloud,...  
→ 마이크로서비스 아키텍처가 도커 사용의 대표적 예시이다. 

- 도커 에이전트를 실행하면 도커 컨테이너를 시작할 수 있다. 내부 실행 프로그램은 다르지만 서버 관점에서 보면 모두 도커 컨테이너이다.

- 도커 내에서 애플리케이션 뿐만 아니라 `MySQL` 등 데이터베이스도 실행 가능하다. 또, 하나의 운영 체제에서 여러 개의 도커 컨테이너를 실행할 수도 있다.

![image](https://user-images.githubusercontent.com/97398071/235595756-24883bf5-779c-4063-9313-f591d036032c.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 클러스터 위에서 서비스나 태스크가 실행된다. 즉 클러스터를 먼저 생성해야 한다.

### 2. Where are Docker images stored?
- Docker images are stored in Docker Repositories  
→ 도커 이미지는 도커 레포지토리에 저장한다.

- Docker Hub (https://hub.docker.com)
~~~
- Public repository
→ 아주 유명한 퍼블릭 레포지토리이다.

- Find base images for many technologies or OS (e.g., Ubuntu, MySQL, …)
→ 많은 기술에 맞는 기본 이미지를 찾을 수 있다.
~~~

- Amazon ECR (Amazon Elastic Container Registry)
~~~
- Private repository
→ 기본적으로 비공개 레포지토리이다.

- Public repository (Amazon ECR Public Gallery https://gallery.ecr.aws)
→ 단, Amazon ECR Public Gallery 공개 레포지토리 옵션도 존재한다.
~~~

### 3. Docker vs. Virtual Machines
- Docker is ”sort of” a virtualization technology, but not exactly  
→ 도커 역시 가상화 기술의 일종이다. 하지만, 순전히 가상화 기술은 아니다.

- Resources are shared with the host => many containers on one server  
→ 리소스가 호스트와 공유되어 한 서버에서 다수의 컨테이너를 공유할 수 있다. 

- 가상 머신에서는 `Hipervisor` 위에서 동작하는 각각의 게스트 `OS`가 리소스를 공유하지 않는다.

- 반면 도커 컨테이너의 경우, `Docker Daemon` 위의 많은 컨테이너들이 네트워킹이나 데이터 등 리소스를 공유한다.  
→ 가상 머신보다 덜 안전하지만 하나의 서버에 많은 컨테이너를 실행할 수 있기 때문에 도커 컨테이너를 많이 사용한다.

![image](https://user-images.githubusercontent.com/97398071/235597347-42214511-e8f8-4d96-a4ff-212083b2485b.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 4. Getting Started with Docker
- 도커 파일을 생성하기 위해서는 일단 `Dockerfile`을 작성해야 한다. 이 파일은 도커 컨테이너의 `configuration` 파일이다.

- 베이스 도커 이미지에 몇 가지 파일을 추가해서 구축하면 도커 이미지가 된다.

- 이 도커 이미지는 푸시해서 도커 레포지토리에 저장할 수 있다. 퍼블릭 레포지토리인 `Docker Hub`에 푸시하거나 `Amazon ECR`에 푸시한다.

- 푸시한 이미지를 `pull`해서 도커를 실행하게 된다. 도커 이미지를 실행하면 도커 컨테이너가 되고 도커를 구축할 때 사용했던 코드를 자동적으로 실행한다.

![image](https://user-images.githubusercontent.com/97398071/235598312-a5167480-2886-4c69-9d3a-405928fb26fe.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 5. Docker Containers Management on AWS
- Amazon Elastic Container Service (Amazon ECS)
~~~
- Amazon’s own container platform
→ Amazon ECS는 도커 관리를 위한 아마존의 전용 플랫폼이다.
~~~

- Amazon Elastic Kubernetes Service (Amazon EKS)
~~~
- Amazon’s managed Kubernetes (open source)
→ Kubernetes의 관리형 버전으로 오픈 소스 프로젝트이다.
~~~

- AWS Fargate
~~~
- Amazon’s own Serverless container platform
→ 아마존의 서버리스 컨테이너 플랫폼이다.

- Works with ECS and with EKS
→ ECS와 EKS를 함께 작동시킬 수 있다.
~~~

- Amazon ECR:
~~~
- Store container images
→ 컨테이너의 이미지를 저장하는데 사용한다.
~~~

### 6. 컨테이너 운영 환경
- 컨테이너들의 스케줄링, 상태 체크, 로드 밸런싱, 모니터링 등 컨테이너를 종합 관리해주는 툴을 말한다.

#### 1. 쿠버네티스
- 컨테이너 운영 환경 중 가장 널리 사용되는 것은 쿠버네티스이다.
- 대규모 컨테이너를 관리했던 구글의 노하우와 강력한 확장성, 마이크로소프트, `Red Hat`, `IBM` 등 수많은 기업의 참여, 잘 짜인 설계가 쿠버네티스가 왕좌에 오른 원동력이다.
- 컨테이너와 관련된 많은 예제가 웹 애플리케이션을 다루지만, 실제 세상엔 더 다양한 형태의 애플리케이션이 있다. 쿠버네티스는 `Deployment`, `StatefulSets`, `DaemonSet`, `Job`, `CronJob`등 다양한 배포 방식을 지원한다.
- 다양한 웹 애플리케이션을 하나의 로드 밸런서로 서비스하기 위해 `Ingress` 기능을 제공한다.
- `AWS`, 구글 클라우드, 마이크로소프트 애저는 물론 수십 개의 클라우드 업체에서 모듈을 제공하여 관리자는 동일한 설정 파일을 서로 다른 클라우드에서 동일하게 사용할 수 있다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
