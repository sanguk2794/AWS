## AWS
### What’s AWS?
- AWS (Amazon Web Service) is a Cloud Provider 
→ `AWS`는 클라우드 공급자이다.

- They provide you with servers and services that you can use on demand and scale easily 
→ `AWS`는 쉽게 확장 가능한 서버와 서비스를 제공한다.

- AWS powers some of the biggest websites in the world 
→ amazon.com, Netflix

- `AWS`는 모든 서비스가 상호 연결되어 있다. (Interconnection)

#### 1. 온디맨드, 온프레미스
`on-demand`는 「요구가 있을 때 언제든지」라는 의미이며 수요가 있을 경우 즉시 서버와 서비스를 제공하는 방식을 말한다.

이와 반대되는 개념인 `on-premise`는 자체적으로 보유하고 있는 서버에 직접 설치해 운영하는 방식을 말한다.
클라우드 이전에 가장 일반적으로 사용되던 방식이다.

### 2. User Registration
- Set Root user email address, Account name
→ `Root user email address`에는 기본 이메일 주소를, `account name`에는 계정 이름을 입력한다.

- Set Root user password

- Set Contact Information
→ 계정의 사용 용도를 입력한다. 사용 용도에는 `Personal`과 `Business`가 존재한다. 
이와 함께 이름, 전화번호, 국적 등의 정보를 입력한다.

- Billing Information
→ 결제를 위한 신용카드 정보를 등록한다.

- Support Plan
→ 지원 플랜을 설정한다.
~~~
Basic Support
Developer Support
Business Support
~~~

- 입력이 끝나면 계정의 활성화를 기다린다. 활성화가 처리가 끝나면 이메일이 도착하고, 
이후부터 `AWS` 계정을 본격적으로 사용할 수 있게 된다.

### 3. AWS Cloud History
- Internally launched, 2002
→ 2002년, 아마존 웹 사이트의 내부 서버용으로 사용하기 위해 출시했다.
 
- Amazon infrastructure is one of their core strength. Idea to market → 2003
→ 2003년, 핵심 역량중 하나인 인프라를 다른 사용자를 대상으로 제공하는 서비스를 준비하기 시작한다.

- Launched publicly with SQS → 2004
→ 2004년, 첫 번째 서비스인 `SQS`를 출시했다.

- Re-launched publicly with SQS, S3 & EC2 → 2006
→ 2006년, 서비스를 확장해 `AWS`를 재출시했다.

- Launched in Europe → 2007
→ 유럽에서 서비스를 시작했다.

- 이런 식으로 확장을 거듭해 지금의 `AWS`가 되었다. 
`AWS`는 `IaaS`의 개척자였으며, 10년 동안 선두를 지키고 있다. 

### 4. AWS Cloud Number Facts
- AWS accounts for 47% of the market in 2019 (Microsoft is 2nd with 22%)
→ `AWS`의 마켓 점유율은 47%로 업계 2위인 마이크로소프트의 22%보다 2배 이상 높다.

- Pioneer and Leader of the AWS Cloud Market for the 9th consecutive year
→ `AWS`는 클라우드 시장의 개척자였고 9년동안 선두를 지키고 있다. 

### 5. AWS Cloud Use Cases
`AWS`는 거의 모든 경우에 사용이 가능하다.

- AWS enables you to build sophisticated, scalable application
→ 복잡하고 확장 가능한 `Application`을 만드는 데 사용할 수 있다.

- Applicable to a diverse set of industries
→ 다양한 산업 분야에 적용할 수 있다.

- Use cases include
~~~
Enterprise IT
Backup & Storage
Big Data analytics 
Website hosting
Mobile & Social Apps
Gaming
~~~

#### 1. Enterprise
응용 프로그램 또는 플랫폼이 규모에 관계없이 가장 민감한 요구사항까지 처리할 수 있는 신뢰할 수 있고 강력한 애플리케이션의 특성을 말한다.

이 단어를 클라우드에 적용한 엔터프라이즈 클라우드는 프라이빗 클라우드, 퍼블릭 클라우드, 분산 클라우드를 통합하여 
어느 클라우드에서나 인프라와 애플리케이션을 관리할 수 있도록 단일 제어 지점을 제공하는 통합된 IT 운영 환경을 말한다.

#### 2. Hosting
호스팅(Hosting)이란 서버 컴퓨터의 전체 또는 일정 공간을 이용할 수 있도록 임대해 주는 서비스를 말한다. 
사용자가 직접 서버를 구입하고 운영할 필요 없이 호스팅 업체가 미리 준비해 놓은 서버를 빌려 사용하는 형식이다.

### 6. AWS Global Infrastructure
`AWS`는 국제적인 `Infrastructure`이다. 이를 위해 전세계적으로 다수의 `AWS Regions`을 보유하고 있다.
또, 각각의 리전들은 네트워크로 연결되는데 이 네트워크들은 `AWS`의 사설 네트워크이다.

#### 1. AWS Regions
- AWS has Regions all around the world
→ `AWS`의 리전들은 전 세계에 걸쳐 있으며 각각의 리전은 `us-east-1` 또는 `eu-west-3`와 같은 고유 이름을 가진다.

- A region is a cluster of data centers
→ 하나의 리전은 데이터센터의 집합이다.

3. Most AWS services are region-scoped
→ 대부분의 `AWS` 서비스들은 특정 리전에 국한된다. 
한 리전에서 사용하던 어떤 서비스를 다른 리전에서 사용할 경우 서비스를 처음 사용하는 셈이 된다.

#### 2. How to choose an AWS Region?
> If you need to launch a new application, where should you do it?

답은 당연히 상황에 따라 다르다는 것이다. 하지만, 이 선택에 영향을 미칠 수 있는 요소는 역시 존재한다.

- Compliance with data governance and legal requirements: data never leaves a region without your explicit permission
→ 서비스를 제공하는 해당 국가의 법률을 준수해야 한다는 것이다. 예를 들어 프랑스 정보는 프랑스 자국 내에 있어야 한다는 제약이 존재할 수 있다.

- Proximity to customers: reduced latency
→ 사용자와 지리적으로 근접하는지 여부는 지연시간에 큰 영향을 미친다. 대부분의 사용자가 있는 곳과 가까운 리전을 선택하는 것이 좋다.

- Available services within a Region: new services and new features aren’t available in every Region
→ 모든 리전이 모든 서비스를 제공하지는 않는다. 사용하고자 하는 서비스를 해당 리전에서 제공하는지 확인해야 한다.

- Pricing: pricing varies region to region and is transparent in the [service pricing page](https://aws.amazon.com/free/?nc1=h_ls&all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=*all&awsf.Free%20Tier%20Categories=*all)
→ 리전마다 요금이 달라진다. 해당 요금 정보를 확인하려면 서비스 요금 정보 페이지를 참고하면 된다.

#### 3. AWS Availability Zones
- Each region has many availability zones (usually 3, min is 3, max is 6)
→ 각각의 리전은 최소 3개, 최대 6개의 가용영역을 포함한다. 
가용영역 또한 각각의 가용영역별로 `ap-southeast-2a`, `ap-southeast-2b`, `ap-southeast-2c`와 같은 고유한 아이디를 가진다.

- Each availability zone (AZ) is one or more discrete data centers with redundant power, networking, and connectivity.
→ 각각의 가용영역은 여분의 전원 네트워킹, 그리고 통신 기능을 갖춘 하나 또는 그 이상의 데이터센터로 이루어져 있다.

- They’re separate from each other, so that they’re isolated from disasters
→ 가용영역들은 서로 떨어져 있는데, 재해와 같은 상황에 대비하기 위해서이다.

- They’re connected with high bandwidth, ultra-low latency networking
→ 가용영역들은 높은 대역폭과 낮은 지연 네트워크로 연결되어 있다. 

![Region](https://user-images.githubusercontent.com/97398071/228144815-3ac48a11-f9e1-465a-890e-117b9e494c80.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

###### 1. 대역폭
대역폭은 일반적으로는 정보를 전송할 수 있는 능력, 즉 주어진 시간에 한 지점에서 다른 지점으로 얼마나 많은 양의 정보를 
전송할 수 있는지를 뜻한다.

#### 4. AWS Points of Presence (Edge Locations)
- Amazon has 216 Points of Presence (205 Edge Locations & 11 Regional Caches) in 84 cities across 42 countries
→ 아마존은 42개국, 84개 도시에 216개가 넘는 전송 거점을 가지고 있다. 그렇기에, 최소 지연시간으로 최종 사용자에게 컨텐츠를 제공하는데 유용하다.

### 7. Tour of the AWS Console
`AWS`의 서비스는 크게 글로벌 서비스와, 리전에 국한되는 `Region-scoped` 서비스로 나누어진다.

#### 1. AWS has Global Services
- Identity and Access Management (IAM) → ID 및 액세스 관리
- Route 53 (DNS service) → DNS 서비스
- CloudFront (Content Delivery Network) → 요청 캐싱을 통한 트래픽 감소, 응답속도 향상 
- WAF (Web Application Firewall) → 방화벽

#### 2. Most AWS services are Region-scoped
- Amazon EC2 (Infrastructure as a Service) → 클라우드에서 확장 가능한 컴퓨팅 용량을 제공
- Elastic Beanstalk (Platform as a Service) → 용량 프로비저닝, 로드 밸런싱, 조정, 애플리케이션 상태 모니터링에 대한 세부 정보를 자동으로 처리
- Lambda (Function as a Service) → 서버를 프로비저닝하거나 관리하지 않고도 코드를 실행할 수 있게 해주는 컴퓨팅 서비스
- Rekognition (Software as a Service) → 이미지 및 비디오 분석

###### 1. 프로비저닝 (provisioning)
사용자의 요구에 맞게 시스템 자원을 할당, 배치, 배포해 두었다가 필요 시 시스템을 즉시 사용할 수 있는 상태로 미리 준비해 두는 것을 말한다.
클라우드를 도입하거나 클라우드 환경에서 사용하는 것은 클라우드 `Vendor`로부터 서비스를 프로비저닝받아 사용한다고 볼 수 있다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
- [단순 IT 마케팅 용어가 아니다, ‘엔터프라이즈급’의 진정한 의미 5가지](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=intralinkskorea&logNo=221065958722)
- [호스팅](https://namu.wiki/w/호스팅)
- [대역폭과 처리 능력](http://web.mit.edu/rhel-doc/4/RH-DOCS/rhel-isa-ko-4/ch-bandwidth.html)
- [프로비저닝](https://ko.wikipedia.org/wiki/프로비저닝)
- [프로비저닝(Provisioning) 이란?](https://jins-dev.tistory.com/entry/프로비저닝Provisioning-이란)
---
