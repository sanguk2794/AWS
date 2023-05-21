## load balancing
### 1. What is load balancing?
- Load Balances are servers that forward traffic to multiple servers (e.g., EC2 instances) downstream  
→ 서버 또는 서버셋으로의 트래픽을 백엔드나 다운스트림 `EC2` 인스턴스 또는 서버들로 전달하는 역할을 한다. 로드 밸런서를 통해 `EC2` 인스턴스로 가는 부하를 분산시킬 수 있다.

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
→ 일래스틱 로드 밸런서는 무조건 사용하는 것이 추전된다. 자체 로드 밸런서를 마련하는 것보다 저렴하며, 굉장히 번거로운 확장성 측면에서의 로드 밸런서 관리를 `AWS`가 보장해주기 때문이다.

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

### 5. Load Balancer Security Groups
- 유저는 `HTTP` 또는 `HTTPS`를 사용해 어디서든 로드 밸런서에 접근이 가능하다. 따라서 로드밸런서는 다음과 같은 보안 그룹 규칙을 가진다.

![image](https://user-images.githubusercontent.com/97398071/233096655-4c4d6a0b-32ab-4a4c-9eec-87db313c7be1.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 여기에서 `EC2` 인스턴스는 로드 밸런서를 통해 들어오는 트래픽만을 허용해야 하기 때문에 인스턴스는 다음과 같은 보안 규칙을 가진다.  
→ 소스가 `IP` 범위가 아닌 로드 밸런서의 보안 그룹이 된다는 것이 중요하다. 이렇게 함으로써 `EC2` 인스턴스는 로드 밸런서에서 온 트래픽만을 허용하는 강화된 보안 매커니즘을 실현할 수 있게 된다.

![image](https://user-images.githubusercontent.com/97398071/233097202-fe1b0364-b493-45bc-b0f3-bf325cd10ebd.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [라우팅이란 무엇입니까?](https://aws.amazon.com/ko/what-is/routing/)
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
