## Types of load balancer on AWS
### 1. Types of load balancer on AWS
- AWS has 4 kinds of managed Load Balancers  
→ `AWS`에는 네 종류의 관리형 로드 밸런서가 있다.

- Overall, it is recommended to use the newer generation load balancers as they provide more features  
→ 결론부터 보면 더 많은 기능을 가지고 있는 신형 로드 밸런서를 사용하는 것이 권장된다.

### 2. Classic Load Balancer (v1 - old generation)
- 2009년에 출시된 로드 밸런서로 `CLB`라고 불린다.
- 이제 `AWS`에서 지원되지 않으며 곧 `AWS` 콘솔에서 사용할 수 없게 될 예정이다.

### 3. Application Load Balancer (v2 - new generation)
- 2016년도에 출시된 로드 밸런서로 `ALB`라고 불린다. 마이크로 서비스나 컨테이너 기반 애플리케이션에 가장 좋은 로드 밸런서로 도커와 `Amazon ECS`에 가장 적합하다.

- Has a port mapping feature to redirect to a dynamic port in ECS  
→ 포트 매핑 기능이 있어서 `EC2` 인스턴스의 동적 포트로의 리다이렉션이 가능하다.

- Application load balancers is Layer 7 (HTTP)  
→ 7계층 전용 로드 밸런서로 `HTTP`, `HTTPS`, `WebSocket`, `gRPC, Google Remote Procedure Call` 프로토콜을 지원한다.

- Load balancing to multiple HTTP applications across machines (target groups)  
→ 머신 간 다수 `HTTP` 애플리케이션의 라우팅에 사용된다. 머신들은 `target group`이라는 그룹으로 묶이게 된다. `target group`는 여러 리전의 인스턴스를 묶을 수 있다.

- Load balancing to multiple applications on the same machine (ex: containers)  
→ 동일 `EC2` 인스턴스 상의 여러 애플리케이션에 부하를 분산한다. 이를 위해 컨테이너와 `ECS`를 사용한다.

- Support redirects (from HTTP to HTTPS for example)  
→ 리다이렉트를 지원한다. `HTTP`에서 `HTTPS`로 트래픽을 자동 리다이렉트할 때 로드 밸런서 레벨에서 가능하다.

- Routing tables to different target groups:  
→ 경로 라우팅을 지원한다. 이를 통해 트래픽을 다른 대상 그룹으로 라우팅할 수 있다.
~~~
- Routing based on path in URL (example.com/users & example.com/posts)
→ URL 대상 경로에 기반한 라우팅이 가능하다.

- Routing based on hostname in URL (one.example.com & other.example.com)
→ 호스트 이름에 기반한 라우팅이 가능하다.

- Routing based on Query String, Headers (example.com/users?id=123&order=false)
→ 쿼리 문자열과 헤더에 기반한 라우팅이 가능하다.
~~~

- 다음은 쿼리 문자열과 헤더에 기반한 라우팅의 예제이다.

![image](https://user-images.githubusercontent.com/97398071/233109048-712949bf-8e97-42b1-8bb7-6ef39d5a2839.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 하나의 로드 밸런서로 다수의 애플리케이션을 처리할 수 있다.

- Fixed hostname (XXX.region.elb.amazonaws.com)  
→ `ALB`를 사용할 경우 고정 호스트 이름이 부여된다.

- The application servers don’t see the IP of the client directly  
→ 애플리케이션 서버는 클라이언트의 `IP`를 직접 보는 것이 불가능하다.

- The true IP of the client is inserted in the header X-Forwarded-For  
→ 클라이언트의 실제 `IP`는 `X-Forwarded-For`라고 불리는 헤더에 삽입된다. `ALB`를 사용할 때, `EC2` 인스턴스에서 클라이언트의 `IP`를 알기 위해서는 `X-Forwarded-For` 헤더를 확인해야 한다.

- We can also get Port (X-Forwarded-Port) and proto (X-Forwarded-Proto)  
→ 클라이언트의 실제 `Port`는 `X-Forwarded-Port`라고 불리는 헤더에 삽입되며, 프로토콜은 `X-Forwarded-Proto`라고 불리는 헤더에 삽입된다.

#### 1. 라우팅
- 컴퓨터 네트워크는 노드라고 하는 여러 시스템과 이러한 노드를 연결하는 경로 또는 링크로 구성되며, 상호 연결된 네트워크에서 두 노드 간의 통신은 여러 경로를 통해 이루어질 수 있다.
- 라우팅은 네트워크에서 미리 정해진 규칙을 사용하여 여러 경로 중 최상의 경로를 선택하는 프로세스이다.

#### 2. Application Load Balancer (v2) Target Groups
- 대상 그룹은 다음과 같다.
~~~
- EC2 instances (can be managed by an Auto Scaling Group) – HTTP
- ECS tasks (managed by ECS itself) – HTTP
- Lambda functions – HTTP request is translated into a JSON event
- IP Addresses – must be private IPs
- ALB can route to multiple target groups
- Health checks are at the target group level  
~~~

- 상태 확인은 대상 그룹 레벨에서 이루어진다.

#### 3. Create Application Load Balancer
- `EC2` → `Load Balancing` → `Load Balancers` → `Create load balancer`로 이동하면 로드 밸런서를 생성할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/233112486-8363ecc5-b010-45ce-a33a-5e8309c3950e.png)

- 로드밸런서 종류 선택 화면은 다음과 같다. 각각의 로드 밸런서가 어떤 트래픽을 위한 것인지는 이미지를 통해 확인할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/233112844-b7e8afc0-40a6-4d57-8992-f4ca8e7a0f1e.png)

- `Scheme`은 인터넷 연결, `Internet-facing`으로 설정하고 주소 종류는 `IPv4`로 설정한다.

![image](https://user-images.githubusercontent.com/97398071/233115715-1cf6864a-a35a-443c-a99b-a0be4960003a.png)

- `Network mapping`에서는 로드 밸런서를 배포할 곳과 가용 영역을 선택해야 한다.

![image](https://user-images.githubusercontent.com/97398071/233116124-fcd1272c-79cd-4264-badb-a00fa5f418e0.png)

- `Security group`의 할당도 필요하다.

![image](https://user-images.githubusercontent.com/97398071/233116451-04ae2ff2-357f-48a3-b541-77bb8904c7b3.png)

- `Target group`을 생성해야 한다.

![image](https://user-images.githubusercontent.com/97398071/233117007-603074d4-85a1-4bef-883e-ef870778ea8d.png)

- `Target group`을 생성할 때, 인스턴스 뿐만 아니라 `IP addresses`, `Lambda function` 등을 선택하는 것도 가능하다.

![image](https://user-images.githubusercontent.com/97398071/233117177-cf7db438-2e7d-40a3-9915-cfd70c4f4f15.png)

#### 4. Port mapping
- `port mapping`은 컴퓨터 네트워크에서 패킷이 라우터나 방화벽과 같은 네트워크 게이트웨이를 가로지르는 동안 하나의 `IP` 주소와 포트 번호 결합의 통신 요청을 다른 곳으로 넘겨주는 `NAT, 네트워크 주소 변환`의 응용이다.
- 이 기법은 게이트웨이의 반대쪽에 위치한 보호/내부망에 상주하는 호스트에 대한 서비스를 생성하기 위해 흔히 사용되며, 통신하는 목적지 `IP` 주소와 포트 번호를 내부 호스트에 다시 매핑함으로써 이루어진다.
- `port forwarding`이라고도 불린다.

### 4. Network Load Balancer (v2 - new generation)
- 2017년도에 출시된 로드 밸런서로 `NLB`라고 불린다. `L4` 로드 밸런서이므로 `TCP`와 `UDP` 트래픽을 다룬다. `TCP`와 `UDP`, `TLS, secure TCP` 프로토콜을 지원한다.
- 초고성능 환경을 구성할 때 사용한다. 지연 시간을 최소로 유지하면서 초당 수백만 건의 요청을 처리하는 것이다.

- NLB has one static IP per AZ, and supports assigning Elastic IP (helpful for whitelisting specific IP)  
→ `AZ`별로 하나의 고정 `IP`를 가진다. 탄력적 `IP` 주소를 각 `AZ`에 할당할 수 있으며, 여러 개의 고정 `IP`를 가진 애플리케이션을 노출할 때 유리하다. 1~3개의 `IP`로만 액세스할 수 있는 애플리케이션을 만드는 경우 네트워크 로드 밸런서를 옵션으로 고려하는게 좋다.

![image](https://user-images.githubusercontent.com/97398071/233411894-ca5da75b-f2e5-4451-ad34-ba66d51996ef.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- NLB are used for extreme performance, TCP or UDP traffic  
→ 고성능, `TCP`, `UDP`, `정적 IP`가 나오면 네트워크 로드 밸런스를 고려하는 것이 좋다.

- Not included in the AWS free tier  
→ 네트워크 로드 밸런서는 `AWS`의 프리티어에 포함되지 않는다.

#### 1. Network Load Balancer – Target Groups
- EC2 instances

- IP Addresses – must be private IPs  
→ `IP Adress`는 하드코딩되어야 하며, `private IP`만 허용된다.

- Application Load Balancer  
→ `Application Load Balancer` 앞에서 사용할 수 있다.

- `NLB`, `ALB`를 함께 사용한다면 `NLB`를 통해 고정 `IP` 주소를 얻을 수 있고 `ALB`를 통해 `HTTP` 유형의 트래픽을 처리하는 규칙을 얻을 수 있다.

- Health Checks support the TCP, HTTP and HTTPS Protocols  
→ 대상 그룹에 대한 상태 확인시 `TCP`뿐만 아니라 `HTTP`, `HTTPS`를 포함한 세 가지 프로토콜을 지원한다.

- 네트워크 로드 밸런서에서는 밸런서의 보안 그룹을 정의하지 않는다. 네트워크 로드 밸런서로 들어온 모든 트래픽이 곧장 `EC2` 인스턴스로 들어간다. 들어온 트래픽을 허용할 것인지 아닌지는 `EC2` 인스턴스의 보안 그룹을 통해 결정된다.

### 5. Gateway Load Balancer
- 2020년도에 출시된 로드 밸런서로 `GWLB`라고 불린다. 보안, 침입 방지, 방화벽 등에 특화되어 있다.

- Deploy, scale, and manage a fleet of 3rd party network virtual appliances in AWS  
→ 배포 및 확장과 `AWS`의 타사 네트워크 가상 어플라이언스의 플릿 관리에 사용된다.

![image](https://user-images.githubusercontent.com/97398071/233411690-07fc2535-cadd-4a65-a81a-03d8bd173e8c.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Operates at layer 3 (Network layer) – IP Protocol  
→ 3계층인 `IP` 프로토콜에서 작동한다.

- Combines the following functions:
~~~
- Transparent Network Gateway – single entry/exit for all traffic
→ 네트워크 게이트웨이 역할을 수행한다. VCP의 모든 트래픽이 GWLB가 되는 단일 엔트리와 출구를 통과하기 때문이다.

- Load Balancer – distributes traffic to your virtual
→ 대상 그룹의 가상 어플라이언스 집합에 그 트래픽을 분산하는 로드 밸런서 역할을 수행한다.
~~~

- Uses the GENEVE protocol on port 6081  
→ `GWLB`는 6081번 포트의 `GENEVE` 프로토콜을 사용한다.

#### 1. Gateway Load Balancer – Target Groups
- EC2 instances
- IP Addresses – must be private IPs

#### 2. 어플라이언스
- `Appliance`는 기기, 장치등의 의미이다. 하지만, 이 단어를 `IT` 용어로 사용할 때에는 기능이나 용도에 특화된 기기나 장치를 가리킨다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---