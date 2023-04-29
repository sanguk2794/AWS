## Global Accelerator
### 1. Global users for our application
• You have deployed an application and have global users who want to access it directly.  
→ 글로벌 애플리케이션을 배포했다. 하지만 이 애플리케이션은 한 리전에만 배포되어 있다.

• They go over the public internet, which can add a lot of latency due to many hops  
→ 공용 애플리케이션에 접속할 때에는 공용 인터넷을 통하게 된다. 그렇기에 수많은 홉으로 인해 상당한 지연이 발생할 수 있다.
이 홉들은 위험 요소이다. 연결이 끊길 수 있기 때문이다.

![image](https://user-images.githubusercontent.com/97398071/235306683-46420af1-7ef6-43fb-b146-4f3170167967.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

• We wish to go as fast as possible through AWS network to minimize latency  
→ 지연 시간을 최소화하기 위해 최대한 빨리 `AWS` 네트워크로 이동하는 것이 좋다. 이를 위해서 `Global Accelerator`를 사용한다.

### 2. Unicast IP vs Anycast IP
• Unicast IP: one server holds one IP address  
→ 유니캐스트는 1:1로 데이터를 통신하는 방식이다. 현재 네트워크상에서 가장 많이 사용된다. 유니캐스트에서 하나의 서버는 하나의 `IP` 주소를 가진다.

![image](https://user-images.githubusercontent.com/97398071/235306868-abebb316-426a-4450-a307-37e226ede767.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

• Anycast IP: all servers hold the same IP address and the client is routed to the nearest one  
→ 네트워크에 연결된 서버 중 가장 가까운 서버로 라우팅된다. 애니캐스트에서 모든 서버는 동일한 `IP`를 가진다. `IPv6` 기반으로 작동한다.

![image](https://user-images.githubusercontent.com/97398071/235306882-cf47dfec-9f26-471d-9ab8-5dc44128ac7e.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. AWS Global Accelerator
• Leverage the AWS internal network to route to your application  
→ 애플리케이션의 라우팅에 `AWS` 내부의 글로벌 네트워크를 활용한다. 

• 2 Anycast IP are created for your application  
→ `Global Accelerator`는 애니캐스트의 개념을 사용한다. 어떤 애플리케이션에 대해서도 전 세계의 유저들에게 두 개의 고정 `IP` 주소를 제공한다는 점에서 매우 독특하다.

• The Anycast IP send traffic directly to Edge Locations  
→ 애니캐스트 `IP`는 사용자와 가장 가까운 엣지 로케이션으로 트래픽을 직접 전송한다.

• The Edge locations send the traffic to your application  
→ 엣지 로케이션은 훨씬 안정적이고 지연시간도 적은 사설 `AWS` 네트워크를 거쳐 `ALB`로 트래픽을 전송한다.

![image](https://user-images.githubusercontent.com/97398071/235307001-c7f698a0-5ae9-48c5-8391-f6730d754ef0.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

• Works with Elastic IP, EC2 instances, ALB, NLB, public or private  
→ `Global Accelerator`는 `Elastic IP`, `EC2 instances`, `ALB`, `NLB`와 함께 작동하며 공용, 사설이 모두 허용된다.

• Consistent Performance  
→ `AWS` 사설 네트워크를 거치기에 안정적인 성능을 보여준다.
~~~
• Intelligent routing to lowest latency and fast regional failover
→ 지능형 라우팅으로 지연 시간이 가장 짧은 앳지 로케이션으로 연결된다. 이 과정에서 무언가 잘못된다면 신속한 리전 장애 조치가 이루어진다.

• No issue with client cache (because the IP doesn’t change)
→ 아무것도 캐시하지 않기에 클라이언트 캐시와도 문제가 없다. 사용하는 2개의 애니캐스트 IP는 변하지 않기 때문이다.

• Internal AWS network
→ 앳지 로케이션을 거쳐 AWS 네트워크로 이동한다.
~~~

• Health Checks
~~~
• Global Accelerator performs a health check of your applications
→ `Global Accelerator`는 애플리케이션에 대해 상태 확인을 실행한다.

• Helps make your application global (failover less than 1 minute for unhealthy)
→ 한 리전에 있는 한 `ALB`에 대해 상태확인에 실패하면 자동화된 장애 조치가 1분 안에 정상 엔드 포인트로 실행된다.

• Great for disaster recovery (thanks to the health checks)
→ 상태 확인을 통한 재해 복구에 특히 뛰어나다.
~~~

• Security
~~~
• only 2 external IP need to be whitelisted
→ 클라이언트가 화이트리스트여야 하는 단 두 개의 외부 IP만 존재하기 때문에 보안 측면에서도 매우 안전하다.

• DDoS protection thanks to AWS Shield
→ 자동적으로 DDos의 방어도 가능하다. AWS Shield를 활용하기 때문이다.
~~~

• 한 기업에서 웹 애플리케이션을 `AWS` 클라우드로 이전해 `EC2` 오토 스케일링 그룹에 있는 `EC2` 인스턴스 세트를 사용하려고 합니다. 웹 애플리케이션은 여러 개의 컴포넌트로 구성되어 있기 때문에 특정 웹 애플리케이션 컴포넌트로 라우팅하려면 호스트 기반 라우팅 기능이 필요합니다. 이 웹 애플리케이션은 이용객이 아주 많기 때문에 고객들의 방화벽 화이트리스트에 등록될 수 있도록 정적 IP 주소를 사용해야 합니다. 또한 이 애플리케이션을 이용하는 고객들이 전 세계에 흩어져 있으므로 짧은 지연 시간으로 서비스를 제공해야 합니다. 다음 중 정적 `IP` 주소를 할당하고 전 세계에 짧은 지연 시간으로 서비스를 제공하는 데 도움이 될 만한 `AWS` 서비스는 무엇입니까?  
→ `AWS Global Accelerator` + `ALB`

### 4. AWS Global Accelerator vs CloudFront
• They both use the AWS global network and its edge locations around the world  
→ 둘 서비스 모두 `AWS`의 글로벌 네트워크와 엣지 로케이션 기반으로 동작한다.

• Both services integrate with AWS Shield for DDoS protection.  
→ 두 서비스 모두 `AWS Shield`와 통합 가능하다. 이를 통해 `DDoS`로부터 보호받는다.

• CloudFront
~~~
• Improves performance for both cacheable content (such as images and videos)
→ 이미지, 비디오 등 캐싱이 가능한 정적 컨텐츠의 성능을 향상시킨다.

• Dynamic content (such as API acceleration and dynamic site delivery)
→ API Accelerator, 동적 사이트 전달 등 동적 컨텐츠에 대해서도 성능을 향상시킬 수 있다.

• Content is served at the edge
→ 이 내용은 엣지 로케이션으로부터 제공된다. 그렇기 때문에 가끔 한 번씩 오리진으로부터 파일을 가져온다. 
대부분 상황에서 캐시된 내용을 엣지로부터 가져와 전달한다.
~~~

• Global Accelerator
~~~
• Improves performance for a wide range of applications over TCP or UDP
→ TCP나 UDP상의 다양한 애플리케이션의 성능을 향상시킨다.

• Proxying packets at the edge to applications running in one or more AWS Regions.
→ 애플리케이션이 하나 이상의 AWS 리전에서 실행되는 경우, 엣지 로케이션에서 애플리케이션으로 이동하는 패킷이 프록시된다.
일단 모든 요청이 애플리케이션 쪽으로 전달되는 것이다. 캐싱은 불가능하다.

• Good fit for non-HTTP use cases, such as gaming (UDP), IoT (MQTT), or Voice over IP
→ 게임이나 IoT, Voice over IP와 같은 비 HTTP를 사용할 때 매우 유용하다.

• Good for HTTP use cases that require static IP addresses
→ 글로벌하게 고정 IP를 요구하는 HTTP를 사용할 때 매우 유용하다.

• Good for HTTP use cases that required deterministic, fast regional failover
→ 결정적이고 신속한 리전 장애 조치가 필요할 때 매우 유용하다.
~~~

---
#### ▶ Reference
- [유니캐스트, 브로드캐스트, 멀티캐스트, 애니캐스트](https://majjangjjang.tistory.com/147)
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
