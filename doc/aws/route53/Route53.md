## Route 53
### 1. Amazon Route 53
- A highly available, scalable, fully managed and Authoritative DNS  
→ 고가용성과 확장성을 갖춘 완전히 관리되며 권한있는 `DNS`이다.

- Authoritative = the customer (you) can update the DNS records  
→ 권한이 있다는 것은 고객인 유저가 `DNS` 레코드를 업데이트할 수 있다는 것이다. 즉, `DNS`를 완전히 제어할 수 있다는 것이다.

![image](https://user-images.githubusercontent.com/97398071/234501087-0b617a18-348b-43d5-b847-dc5853c5372e.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Route 53 is also a Domain Registrar  
→ 루트 53는 도메인 레지스트라 중 하나이다.

- Ability to check the health of your resources  
→ 리소스 관련 상태 확인이 가능하다.

- The only AWS service which provides 100% availability SLA  
→ 100% `SLA` 가용성을 제공하는 유일한 `AWS` 서비스이다.

- Why Route 53? 53 is a reference to the traditional DNS port  
→ 53은 `DNS` 서비스에 사용되는 전통적인 `DNS` 포트이다. 그렇기에 루트 53이라는 이름이 붙었다.

#### 1. Route 53 – Records
- How you want to route traffic for a domain  
→ 여러 `DNS` 레코드를 정의하고 레코드를 통해 특정 도메인으로 라우팅하는 방법을 정의할 수 있다.

- Each record contains:  
→ 각 레코드는 다음과 같은 정보를 포함한다.
~~~
- Domain/subdomain Name – e.g., example.com
→ 도메인/서브 도메인 이름

- Record Type – e.g., A or AAAA
→ 레코드 타입

- Value – e.g., 12.34.56.78
→ IP Address

- Routing Policy – how Route 53 responds to queries
→ Route 53이 쿼리에 응답하는 방식

- TTL – amount of time the record cached at DNS Resolvers
→ DNS 리졸버에서 레코드가 캐싱되는 시간이다. TTL이라고 불린다.
~~~

- Route 53 supports the following DNS record types:  
→ 루트 53에서 지원하는 `DNS` 레코드 타입은 다음과 같다.
~~~
- (must know) A / AAAA / CNAME / NS
- (advanced) CAA / DS / MX / NAPTR / PTR / SOA / TXT / SPF / SRV
~~~

#### 2. Route 53 – Record Types
- A – maps a hostname to IPv4  
→ 호스트 이름과 `IPv4 IP Address` 를 매핑한다.

- AAAA – maps a hostname to IPv6  
→ 호스트 이름과 `IPv6 IP Address` 를 매핑한다.

- CNAME – maps a hostname to another hostname  
→ 호스트 이름을 다른 호스트 이름과 매핑한다.
~~~
- The target is a domain name which must have an A or AAAA record
→ 대상 호스트 이름은 A나 AAAA 레코드가 될 수 있다.

- Can’t create a CNAME record for the top node of a DNS namespace (Zone Apex)
→ Route 53에서는 DNS 이름 공간 또는 Zone Apex의 상위 노드에 대한 CNAMES를 생성할 수 없다.

- Example: you can’t create for example.com, but you can create for www.example.com
→ 예를 들어 example.com에 CNAME을 만들수는 없지만 www.example.com에 대한 CNAME 레코드는 생성할 수 있다.
~~~

- NS – Name Servers for the Hosted Zone  
→ 호스팅 존의 이름 서버이다. 서버의 `DNS` 이름 또는 `IP Address`로 호스팅 존에 대한 `DNS` 쿼리에 응답할 수 있다.
~~~
- Control how traffic is routed for a domain
→ 트래픽이 도메인으로 라우팅되는 방식을 제어한다. 
~~~

#### 3. Route 53 – Hosted Zones
- A container for records that define how to route traffic to a domain and its subdomains  
→ 레코드의 컨테이너이다. 도메인과 서브도메인으로 가는 트래픽의 라우팅 방식을 정의한다. 

- 호스팅 영역에는 두 가지 유형이 있다.
~~~
- Public Hosted Zones – contains records that specify how to route traffic on the Internet (public domain names) 
→ application1.mypublicdomain.com
→ 인터넷에서 트래픽을 라우팅하고자 하는 방식에 대한 정보를 담고 있는 컨테이너이다. 

- Private Hosted Zones – contain records that specify how you route traffic within one or more VPCs (private domain names) 
→ application1.company.internal
→ Amazon VPC 서비스로 생성한 하나 이상의 VPC 내에 있는 도메인과 그 하위 도메인에 대하여 Amazon Route 53의 DNS 쿼리 응답 정보가 담긴 컨테이너이다.
~~~

- 두 호스팅 영역의 동작 방식은 기본적으로 같다. 하지만 `Public Hosted Zones`은 공개된 클라이언트로부터 온 쿼리에 대해 응답할 수 있으나, `Private Hosted Zones`은 `VPC` 내에서만 동작한다는 차이점이 존재한다.

![image](https://user-images.githubusercontent.com/97398071/234510459-2638b530-5d81-40d9-8584-465af3c6d187.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- You pay $0.50 per month per hosted zone  
→ `AWS`에서 만드는 어떤 호스팅 존이든 월 50센트를 지불해야 한다.

#### 4. Register Domain
- 아래는 `Route 53`의 대시보드이다.
![image](https://user-images.githubusercontent.com/97398071/234512169-3f8a4848-426c-412d-937d-abaa9351bca9.png)

- `Route 53` → `Domains` → `Registered domains` → `Register Domain`

![image](https://user-images.githubusercontent.com/97398071/234513060-b81f8ad5-c9ca-4eb2-b3c6-5869fef12b2f.png)

- 도메인 이름, 도메인 연락처, 도메인 연락처 공개 여부 등을 설정한다. 도메인 등록에는 최대 몇 시간이 걸릴 수 있다.

- 도메인 생성이 끝나면 `NS`, `SOA` 레코드가 자동적으로 생성된다. 

#### 5. Register Record
- 도메인을 등록하면 해당 도메인에 대한 레코드를 생성할 수 있어진다.
- 레코드 이름, 타입, 값, `TTL`, 라우팅 정책 설정 이후 `Create Record` 버튼을 누르면 레코드가 생성된다.

- 아래 명령어를 통해 `nslookup`, `dig` 명령어를 설치하면 `ip address`를 확인할 수 있다.
~~~ shell
$ sudo yum install -y bind-utils
~~~

- `nslookup` 명령을 통해 도메인의 `ip address`를 확인할 수 있다.
~~~ shell
$ nslookup [url]
~~~

- `dig` 명령을 통해 도메인의 `ip address`, `type`, `TTL` 등 추가 정보까지 확인할 수 있다.
~~~ shell
$ dig [url]
~~~

#### 6. Route 53 – Records TTL (Time To Live)
- 클라이언트가 재요청을 보내거나 같은 호스트 이름으로 접속할 때, `DNS` 시스템에 쿼리를 보내지 않아도 괜찮도록 답변을 캐시에 저장하도록 할 수 있다. 캐시에 답변을 보존하는 시간을 `TTL`이라고 한다. 하지만 캐시에도 시간이 소요되기 때문에 `Cache TTL`이 발생하긴 한다.

- High TTL – e.g., 24 hr  
→ `Route 53`의 트래픽이 현저히 적어진다. 클라이언트의 `DNS` 조회 요청 또한 그 횟수가 획기적으로 줄 것이다. 하지만 클라이언트가 오래된 레코드를 받을 가능성이 존재한다.

- Low TTL – e.g., 60 sec.  
→ `DNS`의 트래픽 양이 많아져서 비용이 많이 들게 된다. `Route 53`의 요청의 양에 따라 요금이 책정되기 때문이다. 하지만, 레코드 변경이 빨라져 레코드 변경 전반 작업이 편리해진다.

- Except for Alias records, TTL is mandatory for each DNS record  
→ `TTL`은 모든 레코드에 있어 필수적이다. 단, 별칭 레코드는 예외이다.

![image](https://user-images.githubusercontent.com/97398071/234520995-34dc2e6f-26ff-429e-8699-feb6c239367d.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. CNAME vs Alias
- AWS Resources (Load Balancer, CloudFront...) expose an AWS hostname: lb1-1234.us-east-2.elb.amazonaws.com and you want myapp.mydomain.com  
→ 로드 밸런서나 클라우드프론트 등 `AWS`의 리소스를 사용하는 경우 호스트 이름이 노출된다. 그리고 보유한 도메인에 호스트 이름을 매핑하고자 할 수 있다.

- CNAME:
~~~
- Points a hostname to any other hostname. (app.mydomain.com => blabla.anything.com)
→ 호스트 이름이 다른 호스트 이름을 향하도록 설정할 때 사용한다.

- ONLY FOR NON ROOT DOMAIN (aka. something.mydomain.com)
→ 루트 도메인 이름이 아닌 경우에만 가능하다.
~~~

- Alias:
~~~
- Points a hostname to an AWS Resource (app.mydomain.com => blabla.amazonaws.com)
→ Route 53에 한정되지만 호스트 이름이 특정 AWS 리소스로 향하도록 할 수 있다.

- Works for ROOT DOMAIN and NON ROOT DOMAIN (aka mydomain.com)
→ 별칭 레코드는 루트 및 비루트 도메인 모두에 작동한다. 이는 아주 유용하다. 

- Free of charge
→ 무료이다.

- Native health check
→ 자체적인 상태 확인이 가능하다.
~~~

#### 1. Route 53 – Alias Records
- Maps a hostname to an AWS resource  
→ 별칭 레코드는 `AWS`의 리소스에만 매핑이 되어 있다.

- An extension to DNS functionality  
- Automatically recognizes changes in the resource’s IP addresses

- Unlike CNAME, it can be used for the top node of a DNS namespace (Zone Apex), e.g.: example.com  
→ `CNAME`과 달리 별칭 레코드는 `Zone Apex`라는 `DNS` 네임스페이스의 상위 노드로 사용될 수 있다.

- Alias Record is always of type A/AAAA for AWS resources (IPv4 / IPv6)  
→ `AWS` 리소스를 위한 별칭 레코드의 타입은 항상 `A` 또는 `AAAA`이다. 즉 리소스는 `IPv4`, `IPv6` 중 하나이다.

- You can’t set the TTL  
→ 별칭 레코드를 사용하면 `TTL`을 설정할 수 없다. `Route 53`에 의해 자동으로 설정된다.

#### 2. Route 53 – Alias Records Targets
- Elastic Load Balancers
- CloudFront Distributions
- API Gateway
- Elastic Beanstalk environments
- S3 Websites  
→ `S3 bucket`은 불가능하다.

- VPC Interface Endpoints
- Global Accelerator accelerator
- Route 53 record in the same hosted zone

![image](https://user-images.githubusercontent.com/97398071/234525129-97f20002-0396-46b8-a0b5-642b136b3043.png)

- But You cannot set an ALIAS record for an EC2 DNS name  
→ `EC2`의 `DNS` 이름에 대해서는 별칭 레코드를 설정할 수 없다. 중요하다.

- `CNAME`을 이용해 `ALB`로 이동시키는 것보다 `Alias`를 이용하는게 보다 좋은 방법이다.  
→ 레코드를 생성할 때 `VALUE` 옆에 있는 `Alias`를 활성화시키고 로드밸런서를 선택하면 된다. 별칭 레코드의 사용은 무료이다.

- `Domain Apex` - 루트 도메인을 도메인으로 사용하는 것을 말한다.  
→ 레코드 생성시 레코드 이름을 비워놓고 타입으로 `CNAME`을 선언한 뒤 `Value` 값으로 `ALB`의 도메인 이름을 설정하는 것은 허용되지 않는다.
`CNAME` 대신 `Alias`를 활용하자.

### 3. Route 53 – Routing Policies
- Define how Route 53 responds to DNS queries  
→ 라우팅 정책은 `Route 53`이 `DNS` 쿼리에 응답하는 것을 돕는다.

- Don’t get confused by the word “Routing”  
→ 라우팅이라는 단어를 혼동해서는 안 된다.
~~~
- It’s not the same as Load balancer routing which routes the traffic
→ 로드 밸런서가 트래픽을 백엔드 EC2 인스턴스와 라우팅하는 것과는 다른 상황이다.

- DNS does not route any traffic, it only responds to the DNS queries
→ 여기에서의 라우팅은 DNS 관점이다. DNS는 트래픽을 라우팅하지 않는다. 
DNS는 DNS 쿼리에만 응답하게 되고 클라이언트들은 이를 통해 HTTP 쿼리 등을 어떻게 처리해야 되는지를 알 수 있게 되는 것이다.  
~~~

- Route 53 Supports the following Routing Policies  
→ `Route 53`이 지원하는 라우팅 정책은 다음과 같다. 
~~~
- Simple → 단순
- Weighted → 가중치 기반
- Failover → 장애 조치
- Latency based → 지연 시간 기반
- Geolocation → 지리적
- Multi-Value Answer → 다중 값 응답
- Geoproximity (using Route 53 Traffic Flow feature) → 지리 근접 라우팅
~~~

#### 1. Routing Policies – Simple
- Typically, route traffic to a single resource  
→ 트래픽을 단일 리소스로 보내는 방식이다.

- Can specify multiple values in the same record  
→ 동일한 레코드에 여러 값을 지정하는 것도 가능하다.

![image](https://user-images.githubusercontent.com/97398071/234530828-855cd3b8-f447-4a57-9c1e-a0d23f5e25bf.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- If multiple values are returned, a random one is chosen by the client  
→ 이렇게 `DNS`에 의해 다중 값을 받은 경우에는 클라이언트에서 그 중 하나를 무작위로 고르게 된다.

![image](https://user-images.githubusercontent.com/97398071/234530937-a8a52242-eadc-4f52-b10e-87299d2448c7.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- When Alias enabled, specify only one AWS resource  
→ 단순 라우팅 정책에 별칭 레코드를 함께 사용하면 하나의 `AWS` 리소스만을 대상으로 지정할 수 있다.

- Can’t be associated with Health Checks  
→ 상태 확인이 불가능하다.

#### 2. Routing Policies – Weighted
- Control the % of the requests that go to each specific resource  
→ 가중치를 활용해 요청의 일부 비율을 특정 리소스로 보내는 식의 제어가 가능하다. 

![image](https://user-images.githubusercontent.com/97398071/234532050-92bf1e18-48fd-4e4b-8585-6a772a1bad83.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Assign each record a relative weight:  
→ 해당 레코드의 가중치를 전체 가중치로 나눈 값이 된다. 그러므로 가중치의 합이 100이 될 필요는 없다.

![image](https://user-images.githubusercontent.com/97398071/234532348-0395780f-435c-460f-8821-08da3cb23578.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- DNS records must have the same name and type  
→ 이를 위해 `DNS` 레코드들은 동일한 이름 유형을 가져야 한다.

- Can be associated with Health Checks  
→ 이는 상태 확인과 관련될 수 있다.

- Use cases: load balancing between regions, testing new application versions…  
→ 서로 다른 지역들에 걸쳐 로드 밸런싱을 할 때, 적은 양의 트래픽을 보내 새 애플리케이션을 테스트할 때 주로 사용한다.

- Assign a weight of 0 to a record to stop sending traffic to a resource  
→ 가중치 0의 값을 보내게 되면 특정 리소스에 트래픽 보내기를 중단해 가중치를 바꿀 수 있다.

- If all records have weight of 0, then all records will be returned equally  
→ 모든 리소스 레코드의 가중치가 0이라면 모든 레코드가 다시 동일한 가중치를 갖게 된다.

#### 3. Routing Policies – Latency-based
- Redirect to the resource that has the least latency close to us  
→ 지연 시간이 가장 짧은 리소스로 리다이렉트하는 정책이다.

- Super helpful when latency for users is a priority  
→ 지연 시간에 민감한 웹사이트나 애플리케이션이 있는 경우에 아주 유용하다.

- Latency is based on traffic between users and AWS Regions  
→ 지연 시간은 유저가 레코드로 가장 가까운 식별된 `AWS` 리전에 연결하기까지 걸리는 시간을 기반으로 측정된다.

- Germany users may be directed to the US (if that’s the lowest latency)  
→ 만약 유저가 독일에 있고 미국에 있는 리소스의 지연 시간이 가장 짧다면 미국 리전으로 인다이렉트된다.

- Can be associated with Health Checks (has a failover capability)  
→ 상태 체크와 연결이 가능하다.

![image](https://user-images.githubusercontent.com/97398071/234537943-dcb981d8-bc59-4772-81a2-c56daf5c4d35.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 레코드 생성시 리전 설정이 필수적이다.

- 지연 시간 레코드의 작동 여부 확인시에는 `VPN`을 사용하면 된다.

#### 4. Routing Policies – Failover (Active-Passive)
- `Primary`와 `Secondary`, 두 타입 중 하나만 선택 가능하다.

- 기본 인스턴스와 재해 대비 보조 인스턴스가 있을 때 기본 인스턴스에 장애가 발생한다면 `Route 53`은 자동으로 보조 인스턴스로 장애 조치를 수행하며 결과를 보내기 시작한다.

- 클라이언트는 정상으로 생각되는 리소스를 자동으로 얻는다. 기본 인스턴스가 정상이면 `Route 53` 또한 기본 레코드로 응답한다.
하지만 상태 확인이 비정상이면 장애 조치에 도움이 되는 두 번째 레코드의 응답을 자동으로 얻게 된다.

![image](https://user-images.githubusercontent.com/97398071/234546964-e4bad270-0c8b-432b-819b-a010480507df.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 5. Routing Policies – Geolocation
- Different from Latency-based! This routing is based on user location  
→ 지리 위치 라우팅은 지연 시간 기반의 정책과는 매우 다르게 사용자의 실제 위치를 기반으로 한다.

- Specify location by Continent, Country or by US State (if there’s overlapping, most precise location selected)  
→ 특정 대륙이나 국가, 혹은 더 정확하게 미국의 경우에는 어떤 주에 있는지 지정하는 것이다.

- Should create a “Default” record (in case there’s no match on location)  
→ 일치하는 위치가 없는 경우를 위한 `Default` 레코드를 생성해야 한다.

- Use cases: website localization, restrict content distribution, load balancing, …  
→ 사용 사례로는 콘텐츠 분산을 제한하고 로드 밸런싱 등을 실행하는 웹사이트 현지화가 있다.

- Can be associated with Health Checks  
→ 이 레코드 또한 상태 확인과 연결할 수 있다. 

![image](https://user-images.githubusercontent.com/97398071/234548443-b73103b5-2ef9-41fb-877b-4f61325a3c95.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 6. Routing Policies – Geoproximity
- Ability to shift more traffic to resources based on the defined bias  
→ 이 정책은 편향값을 사용해 지정된 특정 위치를 기반으로 트래픽을 보낸다.

- To change the size of the geographic region, specify bias values:
~~~
- To expand (1 to 99) – more traffic to the resource
→ 특정 리소스에 더 많은 트래픽을 보내고 싶다면 편향값을 증가시킨다.

- To shrink (-1 to -99) – less traffic to the resource
→ 특정 리소스에 더 적은 트래픽을 보내고 싶다면 편향값을 감소시킨다.
~~~

- Resources can be:
~~~
- AWS resources (specify AWS region)
→ AWS 리소스로 속한 특정 리전을 지정하면 목록에서 자동으로 올바른 라우팅을 계산한다.

- Non-AWS resources (specify Latitude and Longitude)
→ 온프레미스 데이터 센터인 경우 위도와 경도를 지정해서 AWS가 위치를 파악하도록 해야한다.
~~~

- You must use Route 53 Traffic Flow to use this feature  
→ 편향 활용을 위해 `Route 53 Traffic Flow`를 사용한다. 

![image](https://user-images.githubusercontent.com/97398071/234550630-2d4ccb33-fdf5-40f2-8d66-dea645a51fc5.png)
![image](https://user-images.githubusercontent.com/97398071/234550744-c880d673-fde9-4dae-b1cc-8b6c56a49c6d.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 7. Routing Policies – Multi-Value
- Use when routing traffic to multiple resources  
→ 트래픽을 다중 리소스로 라우팅할 때 사용한다.

- Route 53 return multiple values/resources  
→ 그래서 `Route 53`은 다중 값과 리소스를 반환한다.

- Can be associated with Health Checks (return only values for healthy resources)  
→ 상태 확인과 연결하면 다중 값 정책에서 반환되는 유일한 리소스는 정상 상태 확인과 관련이 있다.

- Up to 8 healthy records are returned for each Multi-Value query  
→ 각각의 다중 값 쿼리에 최대 8개의 정상 레코드가 반환된다.

- Multi-Value is not a substitute for having an ELB  
→ `ELB`와 유사해 보이지만 `ELB`를 대체할 수는 없다. 클라이언트 측면에서의 로드 밸런싱인 것이다.

![image](https://user-images.githubusercontent.com/97398071/234551728-0d3c0c1c-595d-42ac-8261-2ea806224087.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 단순 다중값과 다른 점은 상태 확인이 가능하다는 것이다. 이 다중 값 유형이 좀 더 강력한 이유이다.

### 4. Route 53 –  Health Checks
- HTTP Health Checks are only for public resources  
→ 주로 공용 리소스에 대한 상태를 확인하는 방법이다. 물론 개인 리소스의 상태를 확인할 수 있는 방법도 존재한다.

- Health Check => Automated DNS Failover:  
→ 장애가 발생한 리전으로 트래픽을 전송하지 않기 위해서는 `Route 53`에 상태 확인을 생성해야 한다. 이는 `DNS`의 장애 조치를 자동화하기 위한 작업이다. 세 가지 방법이 존재한다.
~~~
- Health checks that monitor an endpoint (application, server, other AWS resource)
→ 공용 엔드 포인트를 모니터링한다. application, server, other AWS resource등이 리소스가 될 수 있다.

- Health checks that monitor other health checks (Calculated Health Checks)
→ 다른 상태 확인을 모니터링한다. 이를 계산된 상태 확인이라고 부른다.

- Health checks that monitor CloudWatch Alarms (full control !!) – e.g., throttles of DynamoDB, alarms on RDS, custom metrics, … (helpful for private resources) 
→ CloudWatch 경보의 상태를 모니터링한다. 제어가 쉽고 개인 리소스들에 아주 유용하다.
~~~

- Health Checks are integrated with CW metrics  
→ 이 상태 확인들은 각자의 지표를 사용하는데 `CloudWatch`에서도 확인이 가능하다.

![image](https://user-images.githubusercontent.com/97398071/234540957-7c23a72e-86d7-432e-af59-f7f96a9dde2d.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- `Health check`를 위한 `URL`은 `Domain/health`가 일반적이다.
- `Route 53` 상태 확인을 사용하여 활성-활성 및 활성-수동 장애 조치 구성을 구성할 수 있다.
- 하나의 기본 레코드와 하나의 보조 레코드를 두고 기본 레코드가 실패했을 때 보조 레코드를 참조할도록 활성-수동 장애 조치 구성을 만들 때, 레코드를 만들고 라우팅 정책에 대한 장애 조치를 지정하기만 하면 된다.

#### 1. Health Checks – Monitor an Endpoint
- About 15 global health checkers will check the endpoint health  
→ 각각의 지역에서 15개 정도의 엔드 포인트 상태 확인 요청이 발생한다. 200 또는 정의한 코드를 받으면 리소스가 정상으로 간주된다.
~~~
- Healthy/Unhealthy Threshold – 3 (default)
→ 상태를 확인하고 임계값을 정상 혹은 비정상으로 설정한다.

- Interval – 30 sec (can set to 10 sec – higher cost)
→ 간격을 설정한다. 10초 또는 30초로 설정이 가능하다. 10초로 설정하는 것을 빠른 상태 확인이라고 부른다.

- Supported protocol: 
→ HTTP, HTTPS, TCP 등 많은 프로토콜들을 지원한다

- If > 18% of health checkers report the endpoint is healthy, Route 53 considers it Healthy. Otherwise, it’s Unhealthy
→ 18% 이상의 상태 확인이 엔드 포인트를 정상이라고 판단하면 Route 53도 이를 정상이라고 간주한다.

- Ability to choose which locations you want Route 53 to use
→ 상태 확인에 사용될 위치 선택이 가능하다.
~~~

- Health Checks pass only when the endpoint responds with the 2xx and 3xx status codes  
→ 상태 확인은 로드 밸런서로부터 `2xx` 또는 `3xx` 코드를 받아야만 통과가 된다.

- Health Checks can be setup to pass / fail based on the text in the first 5120 bytes of the response  
→ 또, 텍스트 기반 응답일 경우 상태 확인은 응답의 처음 5120 바이트를 확인한다. 응답 자체에 해당 텍스트가 있는지 확인하기 위해서이다.

- Configure you router/firewall to allow incoming requests from Route 53 Health Checkers  
→ 상태 확인의 작동이 가능하려면 상태 확인이 애플리케이션 밸런서나 엔드 포인트에 접근이 가능해야 한다. 

#### 2. Route 53 – Calculated Health Checks
- Combine the results of multiple Health Checks into a single Health Check  
→ 여러 개의 상태 결과를 하나로 합쳐주는 기능이다. 하위 상태 확인을 바탕으로 상위 상태 확인을 정의할 수 있다.

- You can use OR, AND, or NOT  
→ `OR`, `AND`, `NOT` 중 하나를 선택 가능하다.

- Can monitor up to 256 Child Health Checks  
→ 하위 상태 확인을 256개까지 모니터링할 수 있다.

- Specify how many of the health checks need to pass to make the parent pass  
→ 상위 상태 확인이 통과하기 위해 몇 개의 상태 확인을 통과해야 하는지도 지정할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/234543979-cda0f662-4b71-4d86-9196-7b973c942523.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 3. Health Checks – Private Hosted Zones
- Route 53 health checkers are outside the VPC  
→ 개인 리소스를 모니터링하는 것은 어려울 수 있다. 모든 `Route 53`의 상태 확인이 공용 웹에 있어 이들은 `VPC` 외부에 존재하기 때문이다.

- They can’t access private endpoints (private VPC or on-premises resource)  
→ 개인 `VPC`, 온프레미스 리소스 등 개인 엔드 포인트에의 접속은 불가능하다.

- You can create a CloudWatch Metric and associate a CloudWatch Alarm, then create a Health Check that checks the alarm itself   
→ `CloudWatch` 지표를 만들어 `CloudWatch` 알람을 할당하는 식으로 이 문제를 해결할 수 있다. `CloudWatch` 경보를 상태 확인에 사용하는 것이다.

![image](https://user-images.githubusercontent.com/97398071/234544805-885631bf-7294-4684-ba0b-05d555af2589.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 5. Domain Registar vs. DNS Service
- You buy or register your domain name with a Domain Registrar typically by paying annual charges (e.g., GoDaddy, Amazon Registrar Inc., …)  
→ 도메인 이름 레지스트라를 통해 원하는 도메인 이름을 구매할 수 있다. 이에 대해서 매년 비용을 지불해야 한다.

- The Domain Registrar usually provides you with a DNS service to manage your DNS records  
→ 대게 레지스트라를 통해 도메인을 등록하면 `DNS` 레코드 관리를 위한 `DNS` 서비스를 제공한다.

- But you can use another DNS service to manage your DNS records  
→ 하지만 어떤 레지스트라를 사용해도 괜찮다. 

#### 1. 3rd Party Registrar with Amazon Route 53
- If you buy your domain on a 3rd party registrar, you can still use Route 53 as the DNS Service provider  
→ 도메인을 타사 등록 대행사에서 구매해도 `DNS` 서비스 제공자로 `Route 53`을 사용 가능하다. 사용하기 위해서는 아래 절차를 거친다.
~~~
1. Create a Hosted Zone in Route 53
→ 공용 호스팅 영역을 생성한다.

2. Update NS Records on 3rd party website to use Route 53 Name Servers
→ 도메인을 구매한 타사 웹사이트에서 NS 또는 이름 서버를 업데이트하면 Route 53 이름 서버를 가리키게 된다.
~~~

![image](https://user-images.githubusercontent.com/97398071/234553367-3ff4335d-58cb-4b2b-bad1-89d57d0dd303.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Domain Registrar != DNS Service, But every Domain Registrar usually comes with some DNS features  
→ 도메인 이름 레지스트라가 일부 `DNS` 기능을 제공하는 것은 맞으나 도메인 레지스트라는 `DNS` 서비스와 다르다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
