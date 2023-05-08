## AWS Certificate Manager (ACM)
### 1. AWS Certificate Manager (ACM)
- Easily provision, manage, and deploy TLS Certificates  
→ `TLS` 인증서를 쉽게 프로비저닝, 관리 및 배포할 수 있도록 도와주는 서비스이다.

- Provide in-flight encryption for websites (HTTPS)  
→ `SSL`, `TLS`는 웹사이트에서 전송 중 암호화를 제공하는 데 사용된다.

- Supports both public and private TLS certificates  
→ `ACM`은 퍼블릭과 프라이빗 `TLS` 인증서를 지원한다.

- Free of charge for public TLS certificates  
→ 퍼블릭 `TLS` 인증서를 사용하는 것은 무료이다.

- Automatic TLS certificate renewal  
→ 인증서를 자동으로 갱신하는 기능이 있다.

- Integrations with (load TLS certificates on)
~~~
- ACM은 여러 AWS 서비스와 통합되어 있다.

- Elastic Load Balancers (CLB, ALB, NLB)
→ ELB와 ACM을 통합할 수 있다.

- CloudFront Distributions
→ CloudFront 배포와 ACM을 통합할 수 있다.

- APIs on API Gateway
→ API Gateway와 ACM을 통합할 수 있다.
~~~

- Cannot use ACM with EC2 (can’t be extracted)  
→ 단, `EC2` 인스턴스는 `ACM`를 사용할 수 없다. 퍼블릭 인증서일 경우 추출이 불가능하기 때문이다.

![image](https://user-images.githubusercontent.com/97398071/236859215-cb34b8c0-17a9-428d-9a6b-b9a88976dfdf.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. ACM – Requesting Public Certificates
- 퍼블릭 인증서가 요청되는 과정은 다음과 같다.

- List domain names to be included in the certificate
~~~
- 우선 인증서에 포함할 도메인 이름을 나열한다. 도메인의 수에는 제한이 없다.

- Fully Qualified Domain Name (FQDN): corp.example.com 
→ 전체 주소 도메인 이름을 사용 가능하다.

- Wildcard Domain: *.example.com
→ 와일드카드 도메인도 사용 가능하다.
~~~

- Select Validation Method: DNS Validation or Email validation 
~~~
- 유효성 검증 방법을 선택한다. DNS 검증과 이메일 검증 중 하나를 선택해야 한다.

- DNS Validation is preferred for automation purposes 
→ 자동화를 목적으로 SSL 인증서를 자동 갱신하려면 DNS 검증을 쓰는 편이 낫다.

- Email validation will send emails to contact addresses in the WHOIS database 
→ 이메일 검증을 사용한다면 ACM이 도메인에 등록된 연락처로 이메일을 보내 해당 인증서를 요청했는지 여부를 확인한다.

- DNS Validation will leverage a CNAME record to DNS config (ex: Route 53)
→ DNS 검증을 사용하기로 했다면 DNS 구성에서 CNAME 레코드를 생성해 도메인 소유권을 증명해야 한다.

- Route 53이 있다면 ACM과 통합해 소유권 증명 작업을 자동으로 수행해 준다.
~~~
 
- It will take a few hours to get verified  
→ 몇 시간 후 유효성 검증이 완료되면 인증서가 발행된다.
 
- The Public Certificate will be enrolled for automatic renewal 
~~~
- 새로 발행된 퍼블릭 인증서가 자동 갱신 목록에 추가된다.

- ACM automatically renews ACM-generated certificates 60 days before expiry
→ ACM은 스스로 생성된 모든 인증서를 만료 60일 전에 자동으로 갱신해 준다.
~~~

### 3. ACM – Importing Public Certificates
- Option to generate the certificate outside of ACM and then import it  
→ `ACM` 외부에서 생성된 인증서를 `ACM`으로 임포트할 수 있다.

- No automatic renewal, must import a new certificate before expiry  
→ `ACM` 외부에서 생성되었기 때문에 자동 갱신은 불가능하다. 그렇기에 인증서가 만료되기 전에 직접 새 인증서를 임포트해야 한다.

- ACM sends daily expiration events starting 45 days prior to expiration  
→ `ACM` 서비스는 인증서 만료 45일 전부터 매일 만료 이벤트를 `EventBridge` 서비스에 전송한다.

- The # of days can be configured  
→ 며칠 전부터 알려줄지에 대해서는 설정으로 변경이 가능하다.

- Events are appearing in EventBridge  
→ `EventBridge`를 통해 매일 인증서 만료 이벤트가 발생한다고도 할 수 있다.

- AWS Config has a managed rule named acm-certificate-expiration-check to check for expiring certificates (configurable number of days)  
→ `AWS Config`를 사용할 수 있다. `acm-certificate-expiration-check`라는 만료된 인증서를 확인하는 관리형 규칙을 통해 일수를 조정할 수도 있다.

![image](https://user-images.githubusercontent.com/97398071/236859957-d0a0454a-f0cb-436a-acb6-ff18ae1b964e.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 4. ACM – Integration with ALB
- `ACM` 서비스가 `ALB`와 통합되는 과정은 다음과 같다.
~~~
- 백엔드에 오토 스케일링 그룹이 있는 ALB와 ACM으로 프로비저닝 및 유지관리되는 TLS 인증서가 있다고 가정한다.
- ALB에서는 HTTP에서 HTTPS로의 리디렉션 규칙을 설정할 수 있다. 사용자가 HTTP 프로토콜으로 ALB에 액세스할 때 ALB에서 HTTPS로 리디렉션하는 요청을 반환하도록 설정할 수 있다.
- 그 후 사용자는 HTTPS 프로토콜로 ALB에 액세스한다. 이 때 ACM에서 나오는 TLS 인증서를 사용한다.
- 요청이 HTTPS 프로토콜을 통과하면 오토 스케일링 그룹으로 곧바로 전달된다.
~~~

![image](https://user-images.githubusercontent.com/97398071/236860060-18fb05a2-cc83-422f-946d-ae8dd5931a3e.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 5. API Gateway - Endpoint Types
- 엔드 포인트 유형에는 3가지가 존재한다.

- Edge-Optimized (default): For global clients
~~~
- 엣지 최적화는 글로벌 클라이언트를 위한 유형이다.

- Requests are routed through the CloudFront Edge locations (improves latency)
→ 먼저 CloudFront 엣지 로케이션으로 요청을 라우팅하여 지연을 줄이는 방법이다.

- The API Gateway still lives in only one region
→ API 게이트웨이는 여전히 한 지역에만 존재한다.
~~~

- Regional:
~~~
- For clients within the same region
→ 리전 엔드 포인트 유형은 클라이언트가 API Gateway와 같은 리전에 있을 때에 쓰인다.

- Could manually combine with CloudFront (more control over the caching strategies and the distribution)
→ 이 경우 CloudFront는 사용할 수 없지만 자체 CloudFront 배포를 생성하여 캐싱 및 배포 전략을 제어할 수는 있다.
~~~

- Private:
~~~
- Can only be accessed from your VPC using an interface VPC endpoint (ENI)
→ 프라이빗 API Gateway 엔드포인트는 인터페이스 VPC 엔드 포인트(ENI)를 통해 VPC 내부에만 액세스할 수 있다.

- Use a resource policy to define access
→ API Gateway에 대한 액세스를 정의하는 리소스 정책이 필요하다.
~~~

### 6. ACM – Integration with API Gateway
- `ACM`은 엣지 최적화 및 리전 엔드포인트에 적합하다.

- Create a Custom Domain Name in API Gateway  
→ `ACM`을 `API Gateway`와 통합하려면 우선 API `Gateway`에 사용자 지정 도메인 이름이라는 리소스를 생성, 설정해야 한다.

- Edge-Optimized (default): For global clients
~~~~
- Requests are routed through the CloudFront Edge locations (improves latency)
→ 요청이 CloudFront의 엣지 로케이션에서 라우팅된다.

- The API Gateway still lives in only one region
→ API Gateway는 여전히 하나의 리전에 속해 있는 상황이다.

- The TLS Certificate must be in the same region as CloudFront, in us-east-1
→ 이 때 TLS 인증서가 CloudFront 배포에 연결되기 때문에 TLS 인증서가 반드시 CloudFront와 같은 리전에 배포되어야 한다.

- Then setup CNAME or (better) A-Alias record in Route 53
→ Route 53에 CNAME이나 별칭 레코드를 설정해 DNS를 가리키도록 한다.
~~~~

- Regional:
~~~
- For clients within the same region
→ 동일한 리전 내 클라이언트를 위한 엔드포인트이다.

- The TLS Certificate must be imported on API Gateway, in the same region as the API Stage
→ TLS 인증서를 API Gateway에 가져올 때는 같은 리전에 속해 있어야 한다.

- Then setup CNAME or (better) A-Alias record in Route 53
→ Route 53에 CNAME이나 별칭 레코드를 설정해 DNS를 가리키도록 한다.
~~~

![image](https://user-images.githubusercontent.com/97398071/236866321-5e3e7751-db6b-408e-9120-df528d7f2598.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---