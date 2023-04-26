## load balancing
### 1. What is load balancing?
- Load Balances are servers that forward traffic to multiple servers (e.g., EC2 instances) downstream  
→ 서버 또는 서버셋으로 트래픽을 백엔드나 다운스트림 `EC2` 인스턴스 또는 서버들로 전달하는 역할을 한다. 로드 밸런서를 통해 `EC2` 인스턴스로 가는 부하를 분산시킬 수 있다.

![image](https://user-images.githubusercontent.com/97398071/233083545-0eafa2b5-5773-446e-b42d-d37a8211b47d.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2.Why use a load balancer? 
- Spread load across multiple downstream instances  
→ 부하를 다수의 다운스트림 인스턴스로 분산할 수 있다.

- Expose a single point of access (DNS) to your application  
→ 외부에 애플리케이션의 단일 액세스 지점, `DNS`만을 노출하게 된다. 
- `AWS`에서 관리하는 기반 인프라가 변경되더라도 별도의 설정 변경 없이 로드밸런서를 통해 접속 가능하다.

- Do regular health checks to your instances  
→ 인스턴스의 상태를 확인한다. EC2 인스턴스가 `ALB`의 상태 확인에 실패할 경우, 이는 비정상인 것으로 표시되어 종료되며 `ASG`는 새로운 `EC2` 인스턴스를 생성한다.

- Seamlessly handle failures of downstream instances  
→ 로드 밸런서를 통해 인스턴스의 장애를 원활하게 처리할 수 있다. 상태 확인 매커니즘을 통해 트래픽을 보낼 수 없는 인스턴스를 확인하고 그쪽으로 트래픽을 보내지 않는 것이다.

- Provide SSL termination (HTTPS) for your websites  
→ `SSL`의 종료가 가능하므로 웹 사이트에 암호화된 `HTTPS` 트래픽을 가질 수 있다.

- Enforce stickiness with cookies  
→ 쿠키 고정도를 강화할 수 있다. 쿠키 고정도를 강화하면 고정 세션값을 부여함으로써 사용자 경험을 개선하고 네트워크 리소스 사용을 최적화할 수 있다.
`HTTP/S`는 세션 지속성을 염두에 두고 고안되지 않은 상태 비저장 프로토콜이므로 이는 매우 유용하게 사용된다.

- High availability across zones  
→ 고가용성을 지닌다.

- Separate public traffic from private traffic  
→ 클라우드 내의 개인 트래픽과 공공 트래픽을 분리할 수 있다.

### 3. Why use an Elastic Load Balancer?
- An Elastic Load Balancer is a managed load balancer  
→ 일래스틱 로드 밸런서는 관리형 로드 밸런서이다. 
~~~
- AWS guarantees that it will be working  
→ AWS가 관리하며 어떤 경우에도 작동할 것을 보장하며

- AWS takes care of upgrades, maintenance, high availability  
→ AWS가 업그레이드 유지 관리 및 고가용성을 책임지고

- AWS provides only a few configuration knobs  
→ AWS가 로드 밸런서의 작동 방식을 수정할 수 있게끔 구성 놉을 제공해 준다.
~~~

- It costs less to setup your own load balancer but it will be a lot more effort on your end  
→ 일래스틱 로드 밸런서는 무조건 사용하는 것이 추전된다. 
자체 로드 밸런서를 마련하는 것보다 저렴하며, 굉장히 번거로운 확장성 측면에서의 로드 밸런서 관리를 `AWS`가 보장해주기 때문이다.

- It is integrated with many AWS offerings / services  
→ 로드 밸런서는 다수의 `AWS` 서비스들과 통합되어 있다.
~~~
- EC2, EC2 Auto Scaling Groups, Amazon ECS
- AWS Certificate Manager (ACM), CloudWatch
- Route 53, AWS WAF, AWS Global Accelerator
~~~

### 4. Health Checks
- Health Checks are crucial for Load Balancers  
→ `Health check`는 일래스틱 로드 밸런서가 `EC2` 인스턴스의 작동이 올바르게 되고 있는지 확인할 때 사용되며, 이 `Health check`는 로드 밸런스의 작동에 매우 중요한 역할을 한다.
인스턴스가 제대로 작동하는 중이 아니라면 해당 인스턴스로는 트래픽을 보낼 수 없기 때문이다.

- They enable the load balancer to know if instances it forwards traffic to are available to reply to requests

- If the response is not 200 (OK), then the instance is unhealthy  
→ 로드 밸런서는 `EC2` 인스턴스가 괜찮다는 신호, 즉 `HTTP` 상태 코드가 200이 아니라면 인스턴스의 상태를 `unhealthy`로 기록하고 그쪽으로 트래픽을 보내지 않는다.

### 5. Types of load balancer on AWS
- AWS has 4 kinds of managed Load Balancers  
→ `AWS`에는 네 종류의 관리형 로드 밸런서가 있다.

- Overall, it is recommended to use the newer generation load balancers as they provide more features  
→ 결론부터 보면 더 많은 기능을 가지고 있는 신형 로드 밸런서를 사용하는 것이 권장된다.

- Some load balancers can be setup as internal (private) or external (public) ELBs

#### 1. Classic Load Balancer (v1 - old generation)
- 2009년에 출시된 로드 밸런서로 `CLB`라고 불린다.
- `HTTP`, `HTTPS`, `TCP`, `SSL, secure TCP`를 지원한다.
- 이제 `AWS`에서 지원되지 않으며, 곧 `AWS` 콘솔에서도 사용할 수 없다. 그렇기에, 시험에도 나오지 않는다.

#### 2. Application Load Balancer (v2 - new generation)
- 2016년도에 출시된 로드 밸런서로 `ALB`라고 불린다. 마이크로 서비스나 컨테이너 기반 애플리케이션에 가장 좋은 로드 밸런서로 도커와 `Amazon ECS`에 가장 적합하다.

- Has a port mapping feature to redirect to a dynamic port in ECS  
→ 왜냐하면 포트 매핑 기능이 있어서 `EC2` 인스턴스의 동적 포트로의 리다이렉션이 가능하기 때문이다.

- Application load balancers is Layer 7 (HTTP)  
→ 7계층, `HTTP` 전용 로드 밸런서로 `HTTP`, `HTTPS`, `WebSocket` 프로토콜을 지원한다.

- Load balancing to multiple HTTP applications across machines (target groups)  
→ 머신 간 다수 `HTTP` 애플리케이션의 라우팅에 사용된다. 머신들은 `target group`이라는 그룹으로 묶이게 된다. `target group`는 여러 리전의 인스턴스를 묶을 수 있다.

- Load balancing to multiple applications on the same machine (ex: containers)  
→ 동일 `EC2` 인스턴스 상의 여러 애플리케이션에 부하를 분산한다. 이를 위해 컨테이너와 `ECS`를 사용한다.

- Support redirects (from HTTP to HTTPS for example)  
→ 리다이렉트를 지원한다. `HTTP`에서 `HTTPS`로 트래픽을 자동 리다이렉트할 때 로드 밸런서 레벨에서 가능하다.

- 한 웹 사이트가 애플리케이션 로드 밸런서 뒤에 있는 오토 스케일링 그룹의 EC2 인스턴스에서 호스팅되고 있습니다.
현재 `HTTP`로 서비스 중인 웹 사이트를 `HTTPS`로 바꾸는 작업을 진행하고 있습니다. `ACM` 인증서를 발급받아 애플리케이션 로드 밸런서에 적용한 상태입니다.
사용자들이 `HTTP`가 아닌 `HTTPS`를 사용해 웹 사이트에 접속하게 하려면 어떻게 해야 합니까?  
→ 애플리케이션 로드 밸런서가 `HTTP`를 `HTTPS`로 리디렉션하도록 설정한다.

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

###### 1. 라우팅
라우팅은 네트워크에서 경로를 선택하는 프로세스이다. 컴퓨터 네트워크는 노드라고 하는 여러 시스템과 이러한 노드를 연결하는 경로 또는 링크로 구성되며, 
상호 연결된 네트워크에서 두 노드 간의 통신은 여러 경로를 통해 이루어질 수 있다. 라우팅은 미리 정해진 규칙을 사용하여 여러 경로 중 최상의 경로를 선택하는 프로세스이다.

###### 2. Application Load Balancer (v2) Target Groups
- EC2 instances (can be managed by an Auto Scaling Group) – HTTP
- ECS tasks (managed by ECS itself) – HTTP
- Lambda functions – HTTP request is translated into a JSON event
- IP Addresses – must be private IPs
- ALB can route to multiple target groups
- Health checks are at the target group level  
→ 상태 확인은 대상 그룹 레벨에서 이루어진다.

- `ELB`는 포함되지 않는다.

###### 3. Application Load Balancer (v2) Good to Know
- Fixed hostname (XXX.region.elb.amazonaws.com)  
→ `ALB`를 사용할 경우 고정 호스트 이름이 부여된다.

- The application servers don’t see the IP of the client directly  
→ 애플리케이션 서버는 클라이언트의 `IP`를 직접 보는 것이 불가능하다.

- The true IP of the client is inserted in the header X-Forwarded-For  
→ 클라이언트의 실제 `IP`는 `X-Forwarded-For`라고 불리는 헤더에 삽입된다.

- We can also get Port (X-Forwarded-Port) and proto (X-Forwarded-Proto)  
→ 클라이언트의 실제 `Port`는 `X-Forwarded-Port`라고 불리는 헤더에 삽입되며, 프로토콜은 `X-Forwarded-Proto`라고 불리는 헤더에 삽입된다.

- 즉 `ALB`를 사용할 때, `EC2` 인스턴스가 클라이언트의 `IP`를 알기 위해서는 `X-Forwarded-Port`와 `X-Forwarded-Proto`를 확인해야 한다.

###### 4. Create Application Load Balancer
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

###### 5. Port mapping
`port forwarding`이라고도 불리는 `port mapping`은 컴퓨터 네트워크에서 패킷이 라우터나 방화벽과 같은 
네트워크 게이트웨이를 가로지르는 동안 하나의 IP 주소와 포트 번호 결합의 통신 요청을 다른 곳으로 넘겨주는 네트워크 주소 변환(NAT)의 응용이다.

이 기법은 게이트웨이의 반대쪽에 위치한 보호/내부망에 상주하는 호스트에 대한 서비스를 생성하기 위해 흔히 사용되며, 
통신하는 목적지 IP 주소와 포트 번호를 내부 호스트에 다시 매핑함으로써 이루어진다.

#### 3. Network Load Balancer (v2 - new generation)
- 2017년도에 출시된 로드 밸런서로 `NLB`라고 불린다. `L4` 로드 밸런서이므로 `TCP`와 `UDP` 트래픽을 다룬다.
`TCP`와 `UDP`, `TLS, secure TCP` 프로토콜을 지원한다.

- 초고성능 환경을 구성할 때 사용한다. 지연 시간을 최소로 유지하면서 초당 수백만 건의 요청을 처리하는 것이다.

- NLB has one static IP per AZ, and supports assigning Elastic IP (helpful for whitelisting specific IP)  
→ `AZ`별로 하나의 고정 `IP`를 가진다. 탄력적 `IP` 주소를 각 `AZ`에 할당할 수 있으며, 여러 개의 고정 `IP`를 가진 애플리케이션을 노출할 때 유리하다.
1 ~ 3개의 `IP`로만 액세스할 수 있는 애플리케이션을 만드는 경우 네트워크 로드 밸런서를 옵션으로 고려하는게 좋다.

![image](https://user-images.githubusercontent.com/97398071/233411894-ca5da75b-f2e5-4451-ad34-ba66d51996ef.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- NLB are used for extreme performance, TCP or UDP traffic  
→ 고성능, `TCP`, `UDP`, `정적 IP`가 나오면 네트워크 로드 밸런스를 고려하는 것이 좋다. 

- 규정 준수를 위해, `고정된 정적 IP 주소`를 최종 사용자에게 노출하여 사용자들이 안정적이고, 규제 기관의 승인을 받은 방화벽 규칙을 작성할 수 있도록 하려 합니다.
이런 경우, 다음 중 어떤 종류의 `Elastic Load Balancer`를 사용해야 할까요?  
→ `NLB`

- Not included in the AWS free tier  
→ 네트워크 로드 밸런서는 `AWS`의 프리티어에 포함되지 않는다.

###### 1. Network Load Balancer – Target Groups
- EC2 instances
- IP Addresses – must be private IPs  
→ `IP Adress`는 하드코딩되어야 하며, `private IP`만 허용된다.

- Application Load Balancer  
→ `Application Load Balancer` 앞에서 사용할 수 있다. 

- `NLB`, `ALB`를 함께 사용한다면 `NLB`를 통해 고정 `IP` 주소를 얻을 수 있고 `ALB`를 통해 `HTTP` 유형의 트래픽을 처리하는 규칙을 얻을 수 있다.

- Health Checks support the TCP, HTTP and HTTPS Protocols  
→ 대상 그룹이 수행하는 상태 확인시 `TCP`뿐만 아니라 `HTTP`, `HTTPS`를 포함한 세 가지 프로토콜을 지원한다.
  
- 네트워크 로드 밸런서에서는 밸런서의 보안 그룹을 정의하지 않는다. 네트워크 로드 밸런서로 들어온 모든 트래픽이 곧장 `EC2` 인스턴스로 들어간다.
들어온 트래픽을 허용할 것인지 아닌지는 `EC2` 인스턴스의 보안 그룹을 통해 결정된다.

#### 4. Gateway Load Balancer
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

###### 1. Gateway Load Balancer – Target Groups
- EC2 instances
- IP Addresses – must be private IPs

###### 2. 어플라이언스
`Appliance`는 기기, 장치등의 의미이다. 하지만, 이 단어를 `IT` 용어로 사용할 때에는 기능이나 용도에 특화된 기기나 장치를 가리킵니다.

### 7. Load Balancer Security Groups
- 유저는 `HTTP` 또는 `HTTPS`를 사용해 어디서든 로드 밸런서에 접근이 가능하다. 따라서 로드밸런서는 다음과 같은 보안 그룹 규칙을 가진다.

![image](https://user-images.githubusercontent.com/97398071/233096655-4c4d6a0b-32ab-4a4c-9eec-87db313c7be1.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 여기에서 `EC2` 인스턴스는 로드 밸런서를 통해 들어오는 트래픽만을 허용해야 하기 때문에 인스턴스는 다음과 같은 보안 규칙을 가진다.  
→ 소스가 `IP` 범위가 아닌 로드 밸런서의 보안 그룹이 된다는 것이 중요하다. 이렇게 함으로써 `EC2` 인스턴스는 로드 밸런서에서 온 트래픽만을 허용하는 강화된 보안 매커니즘을 실현할 수 있게 된다.

![image](https://user-images.githubusercontent.com/97398071/233097202-fe1b0364-b493-45bc-b0f3-bf325cd10ebd.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Sticky Session](https://www.imperva.com/learn/availability/sticky-session-persistence-and-cookies/)
- [라우팅이란 무엇입니까?](https://aws.amazon.com/ko/what-is/routing/)
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
