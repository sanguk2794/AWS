## API Gateway
### 1. Example: Building a Serverless API
- 람다 함수에서 `API`의 데이터베이스로 `DynamoDB`를 사용할 수 있다. 테이블 생성, 수정, 삭제가 가능하다.
- 클라이언트도 이 람다 함수를 지연 호출하게 하고 싶다. 클라이언트가 직접 람다 함수를 지연 호출할 수 있게 하려면 클라이언트에게 `IAM` 권한이 있어야 한다.
- `API Gateway`를 사용하면 `AWS`의 서버리스 서비스로 `REST API`를 생성할 수 있으므로 람다에의 퍼블릭 엑세스가 가능해진다.
- `API Gateway`를 사용하는 이유는 `HTTP` 엔드포인트 뿐만 아니라 인증, 사용량 계획, 개발 단계 등의 기능을 제공하기 때문이다.

![image](https://user-images.githubusercontent.com/97398071/235819471-ac69d4c9-559f-4ed8-94f6-d543bee49fc8.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. AWS API Gateway
- AWS Lambda + API Gateway: No infrastructure to manage  
→ `API Gateway`를 람다와 결합하면 완전한 서버리스 애플리케이션이 구축되므로 인프라 관리가 필요없다.

- Support for the WebSocket Protocol  
→ `WebSocket` 프로토콜을 지원한다.

- Handle API versioning (v1, v2…)  
→ `API`의 버저닝을 핸들링한다.

- Handle different environments (dev, test, prod…)  
→ `dev`, `test`, `prod` 등 여러 환경을 핸들링한다.

- Handle security (Authentication and Authorization)  
→ 보안에도 활용할 수 있다. 인증, 권한 부여 등 수많은 보안 기능을 `API Gateway`에 활성화할 수 있다.

- Create API keys, handle request throttling  
→ `API` 키를 생성할 수 있고, 클라이언트 요청이 과도할 때 요청을 스로틀링할 수 있다.

- Swagger / Open API import to quickly define APIs  
→ 스웨거나 `Open API`와 같은 공통 표준을 사용하여 신속히 `API`를 정의하여 가져올 수 있다.

- Transform and validate requests and responses  
→ `API Gateway` 수준에서 요청과 응답을 변형하거나 유효성을 검사해 올바른 호출이 실행되게 할 수 있다.

- Generate SDK and API specifications  
→ `SDK`나 `API` 스펙을 생성하거나 `API` 응답을 캐시할 수 있다.

- Cache API responses  
→ 응답을 캐시할 수 있다.

- `API`를 생성하고 각각의 `Method`를 생성한다.

- 메서드 정의 완료 후 `Create` → `Deploy`를 수행하면 호출 가능한 `URL`이 생성된다.

- 서버리스 `API`를 생성하기 위해서는 `Amazon API Gateway`를 ......................와(과) 통합해야 합니다.  
→ `AWS` 람다


### 3. API Gateway – Integrations High Level
- Lambda Function
~~~
- Invoke Lambda function
→ 람다 함수를 실행시킬 수 있다.

- Easy way to expose REST API backed by AWS Lambda
→ 람다 함수를 사용하는 `REST API`를 완전 서버리스 애플리케이션에 노출시키는게 가장 일반적이고 간단한 방법이다.
~~~

- HTTP
~~~
- Expose HTTP endpoints in the backend
→ 백엔드의 HTTP 엔드포인트를 노출시킬 수 있다.

- Example: internal HTTP API on premise, Application Load Balancer… → Add rate limiting, caching, user authentications, API keys, etc…
→ 온프레미스에 HTTP API가 있거나 클라우드 환경에 ALB가 있을 때 API Gateway를 사용하면 속도 제한 기능, 캐싱, 사용자 인증, API 키 등의 기능을 추가할 수 있다.
~~~

- AWS Service
~~~
- Expose any AWS API through the API Gateway
→ 어떤 AWS API라도 호출할 수 있다. 

- Example: start an AWS Step Function workflow, post a message to SQS
→ 단계 함수 워크플로우를 시작할 수 있고, API Gateway에서 직접 SQS에 메세지를 게시할 수도 있다.

- Why? Add authentication, deploy publicly, rate control…
→ 인증을 추가하거나 API를 퍼블릭으로 배포하거나 특정 AWS 서비스에 속도 제한을 추가하기 위해 통합하는 것이다.
~~~

- Kinesis Data Streams example
~~~
- Kinesis Data Streams에 사용자 데이터를 전송할 수는 있지만 AWS 자격 증명은 가질 수 없도록 보안 설정을 할 수 있다.
- Kinesis Data Streams와 클라이언트 사이에 API Gateway를 두는 것이다.
- 클라이언트가 API Gateway로 요청을 보내면 API Gateway는 Kinesis Data Streams에 전송하는 메세지로 구성해 전송한다.
- Kinesis Data Streams에서 Kinesis Data Firehose로 데이터를 전송하고 최종적으로 JSON 형식으로 S3 버킷에 저장한다.
- API Gateway의 장점은 API Gateway를 통해 AWS 서비스를 외부에 노출시킬 수 있다는 것이다.
~~~

![image](https://user-images.githubusercontent.com/97398071/235827961-ea27c370-a941-4130-a8ae-e6631ed289f5.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 4. API Gateway - Endpoint Types
- `API Gateway`의 디플로이 방법은 세 가지이며, 이를 엔드포인트 타입이라고 한다.

- Edge-Optimized (default): For global clients
~~~
- 기본 유형인 엣지 최적화 유형은 글로벌 클라이언트용이다. 전 세계 누구나 API Gateway에 액세스할 수 있다.
- Requests are routed through the CloudFront Edge locations (improves latency)
→ 모든 요청이 CloudFront 엣지 로케이션을 통해 라우팅되므로 지연 시간이 개선된다.

- The API Gateway still lives in only one region
→ API Gateway는 여전히 생성된 리전에 위치하지만 모든 CloudFront 엣지 로케이션에서 액세스될 수 있다.
~~~

- 엣지 최적화 `API` 게이트웨이를 사용할 경우, `API Gateway`가 모든 `AWS` 리전의 `CloudFront` 엣지 로케이션에 존재하게 됩니다.  
→ 아니다. `API` 요청은 가장 가까운 `CloudFront` 엣지 로케이션으로 라우팅되며, 이는 지연 시간을 향상시킵니다. `API Gateway`는 여전히 하나의 `AWS` 리전에 존재한다.

- Regional:
~~~
- CloudFront 엣지 로케이션을 원하지 않을 때는 Regional Deploy를 사용한다.
- For clients within the same region
→ 모든 사용자는 API Gateway를 생성한 리전과 같은 리전에 있어야 한다.

- Could manually combine with CloudFront (more control over the caching strategies and the distribution)
→ 자체 CloudFront 배포를 생성할 수도 있다.
~~~

- Private:
~~~
- Can only be accessed from your VPC using an interface VPC endpoint (ENI)
→ 비공개이다. 그러므로 VPC 내에서만 액세스할 수 있다. ENI 같은 인터페이스 VPC 엔드포인트를 사용한다.

- Use a resource policy to define access
→ API Gateway의 액세스를 정의할 때는 리소스 정책을 사용한다.
~~~

### 5. API Gateway – Security
- User Authentication through
~~~
- 사용자를 식별하는 방법에 여러 가지가 존재한다.

- IAM Roles (useful for internal applications)
→ IAM Roles을 사용하는 것은 EC2 인스턴스에서 실행되는 내부 애플리케이션에서 유용하다. API Gateway의 API에 액세스할 때 IAM Roles를 사용하게 하는 것이다.

- Cognito (identity for external users – example mobile users)
→ 모바일 애플리케이션이나 웹 애플리케이션의 외부 사용자에 대한 보안 조치로 Amazon Cognito를 사용할 수 있다.

- Custom Authorizer (your own logic)
→ 자체 로직을 실행하려면 사용자 지정 권한 부여자를 사용하면 된다. 이 것은 람다 함수가 될 것이다.
~~~

- Custom Domain Name HTTPS security through integration with AWS Certificate Manager (ACM)
~~~
- HTTPS 보안도 있다. 사용자 지정 도메인 이름을 AWS Certificate Manager, ACM과 통합할 수 있다.

- If using Edge-Optimized endpoint, then the certificate must be in us-east-1
→ 엣지 최적화 엔드포인트를 사용할 경우 인증서는 반드시 us-east-1에 있어야 한다.

- If using Regional endpoint, the certificate must be in the API Gateway region
→ 리전 엔드포인트를 사용한다면 인증서는 API Gateway 단계와 동일한 리전에 있어야 한다.

- Must setup CNAME or A-alias record in Route 53
→ Route 53에 CNAME이나 A-별칭 레코드를 설정해 도메인 및 API Gateway를 가리키도록 해야한다.
~~~

- 여러분이 가진 모바일 애플리케이션에서 애플리케이션 사용자에게 S3 버킷에 있는 개인 공간에 액세스할 수 있는 권한을 부여하려고 합니다. 어떻게 해야 합니까?  
→ `Amazon Cognito` 아이덴티티 페더레이션을 사용한다.

- 여러분은 `AWS`에 호스팅할 예정인 새로운 웹 및 모바일 애플리케이션을 개발하고 있으며, 현재 로그인 및 회원 가입 페이지를 구현하는 작업을 진행하고 있습니다. 애플리케이션 백엔드는 서버리스로 되어 있으며 구현에 `Lambda`, `DynamoDB`, `API Gateway`를 사용하고 있습니다. 다음 중 백엔드에 대한 인증을 구성하는 데 가장 쉽고 적합한 방식은 무엇입니까?  
→ `Cognito` 사용자 풀을 사용한다.

- 여러분이 운영 중인 모바일 애플리케이션에서 등록된 사용자들 모두 S3 버킷에 있는 자신의 폴더에서 이미지를 업로드 또는 다운로드할 수 있게 하려고 합니다. 또한 사용자들이 페이스북과 같은 소셜 미디어 계정을 통해 가입 및 로그인할 수 있게 하고자 합니다. 어떤 `AWS` 서비스는 사용해야 합니까?  
→ `Amazon Cognito`

### 6. Amazon Cognito
`Amazon Cognito`는 웹 및 모바일 앱에 대한 인증, 권한 부여 및 사용자 관리를 제공한다. 사용자는 사용자 이름과 암호를 사용하여 직접 로그인하거나 `Facebook`, `Amazon`, `Google` 또는 `Apple` 같은 타사를 통해 로그인할 수 있다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---