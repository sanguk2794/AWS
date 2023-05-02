## AWS Lambda
### 1. Why AWS Lambda
- Amazon EC2
~~~
- Virtual Servers in the Cloud
→ Amazon EC2는 클라우드의 가상 서버이기 때문에 프로비저닝이 필요하다.

- Limited by RAM and CPU
→ 그렇기에 메모리와 CPU 크기가 제한된다.

- Continuously running
→ 지속적으로 실행되어야 하고, 최적화를 하려면 인스턴스를 효율적으로 시작하고 중단해야할 필요가 있다.

- Scaling means intervention to add / remove servers
→ ASG로 스케일링이 가능하다. 이는 서버를 늘리고 줄이는 작업이 필요하다는 의미도 된다.
~~~

- Amazon Lambda
~~~
- Virtual functions – no servers to manage!
→ 람다는 가상의 함수이다. 관리할 서버 없이 코드를 프로비저닝하면 함수가 실행된다.

- Limited by time - short executions
→ 제한 시간이 있어서 실행 시간이 짧다. 

- Run on-demand
→ 온디맨드로 실행된다. 람다 함수가 실행중이 아니면 비용 또한 청구되지 않는다.

- Scaling is automated!
→ 스케일링이 자동화된다. 더 많은 람다 함수가 동시에 필요한 경우 AWS가 자동으로 프로비저닝해서 람다 함수를 늘려준다.
~~~

![image](https://user-images.githubusercontent.com/97398071/235742473-3199432b-fc22-4bfa-a5fb-dc068abf47c2.png)

### 2. Benefits of AWS Lambda
- Easy Pricing:  
→ 가격 책정이 아주 쉬워진다.
~~~
- Pay per request and compute time
→ 람다가 수신하는 요청의 횟수에 따라 과금이 이루어진다.

- Free tier of 1,000,000 AWS Lambda requests and 400,000 GBs of compute time
→ 프리 티어에서도 람다를 넉넉히 제공해준다. 
~~~

- Integrated with the whole AWS suite of services  
→ 다양한 `AWS` 서비스와 통합되기도 한다.

- Integrated with many programming languages  
→ `Lambda`에 여러가지의 프로그래밍 언어를 사용할 수 있어서 상당히 자유로운 편이다.

- Easy monitoring through AWS CloudWatch  
→ `CloudWatch`와의 모니터링 통합이 쉽다.

- Easy to get more resources per functions (up to 10GB of RAM!)  
→ 함수당 최대 `10GB`의 램을 프로비저닝 할 수 있다.

- Increasing RAM will also improve CPU and network!  
→ 함수의 램을 증가시키면 `CPU` 및 네트워크 품질과 성능도 함께 향상된다.

- 프로그래머의 관점에서 보면 코드 몇 가지를 람다에 업로드하는 것만으로도 아주 빠른 실행이 가능하기에 굉장히 유용한 기능이다.  
→ 람다는 코드를 배포하고 작동시키는데 아주 매끄럽게 실행될 뿐만 아니라 자동으로 스케일링되며 완전한 서버리스이다.

### 3. AWS Lambda language support
- Node.js (JavaScript) 
- Python 
- Java (Java 8 compatible) 
- C# (.NET Core) 
- Golang 
- C# / Powershell 
- Ruby 
- Custom Runtime API (community supported, example Rust)  
→ 왠만한 언어는 모두 지원하는데, 이는 `사용자 지정 런타임 API, Custom Runtime API` 덕분이다. 예를 들면 위에 없는 언어 중 하나인 `Rust` 또한 사용 가능한데 이에 대한 오픈 소스 프로젝트가 있기에 가능한 것이다.

- Lambda Container Image  
→ 람다 컨테이너 이미지는 아주 특별하다. 
~~~
- The container image must implement the Lambda Runtime API 
→ 컨테이너 자체가 람다의 런타임 API를 구현해야 한다. 그렇지 않은 컨테이너 이미지는 실행이 불가능하다.

- ECS / Fargate is preferred for running arbitrary Docker image
→ 람다의 런타임 API를 구현하지 않은 컨테이너 이미지는 ECS 또는 Fargate에서 실행시켜야 한다는 것은 중요하다.
~~~

### 4. AWS Lambda Integrations Main ones
- API Gateway  
→ `REST API`를 생성한다. 그리고 람다 함수를 호출한다.

- Kinesis  
→ 람다를 이용해 데이터를 변환한다.

- DynamoDB  
→ 트리거를 생성할 때 사용한다. 데이터베이스에서 무슨 일이 생기면 람다 함수가 작동되도록 한다.

- S3  
→ 객체 파일이 생성되거나 할 때 람다 함수가 작동되도록 한다.

- CloudFront
- CloudWatch Events EventBridge  
→ `AWS`의 인프라에 어떤 일이 생기고 그 상황에 대응하고자 할 때 람다 함수가 작동되도록 한다. 예를 들면 파이프라인이 끊어졌을 때 상황에 따라 자동화가 가능하다.

- CloudWatch Logs  
→ 어디든 해당 로그를 스트림할 수 있다.

- SNS  
→ 알림과 토픽에 대처할 수 있다.

- SQS  
→ 대기열 메세지를 처리할 수 있다.

- Cognito  
→ 유저가 데이터베이스에 로그인 할 때마다 응답한다.

### 5. Use case
#### 1. Serverless Thumbnail creation
- `S3` 버킷이 있고 섬네일을 생성하고 싶은 상황이다.

- `Amazon S3`에 새 이미지가 업로드되는 이벤트가 생기고 `S3` 이벤트 알림을 통해 람다 함수가 작동된다.

- 이 람다 펑션에는 섬네일을 생성하는 코드가 있다. 해당 섬네일은 다른 `S3` 버킷이나 같은 `S3` 버킷으로 푸시 및 업로드된다.

- 또 람다 함수는 몇몇 데이터를 `DynamoDB`에 삽입할 수 있다. 이미지 이름, 크기, 생성 날짜 등 이미지의 메타데이터가 삽입된다.

- 람다를 사용해 기능을 자동화하고, `S3`에 새 이미지와 앱이 생성되는 이벤트에 대한 반응형 아키텍처를 얻을 수 있다.

![image](https://user-images.githubusercontent.com/97398071/235739087-34ad33f5-7787-40e8-b520-1ca1f6072633.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 2. Serverless CRON Job
- `CRON`이란 `EC2` 인스턴스에서 작업을 생성하는 방법이며, 5분마다 또는 월요일 10시마다 등등 설정을 지정할 수 있다.
- 이 `CRON`은 가상 서버에 실행해야 한다. `EC2` 인스턴스 등이다.
- `CloudWatch` 이벤트 규칙 또는 `EventBridge` 규칙을 만들고 1시간마다 작동되게 설정해서 1시간마다 람다 함수와 통합하면 태스크를 수행할 수 있게 된다.
- `Rambda` 뿐만 아니라 `CloudWatch` 서비스도 역시 서버리스이다.

![image](https://user-images.githubusercontent.com/97398071/235739853-f4f56e8b-2d7d-46ca-baf6-48cbba8b8d85.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 6. AWS Lambda Pricing: example
- You can find overall pricing information here: https://aws.amazon.com/lambda/pricing/  
→ 최신 가격 책정 예시는 [AWS Lambda 요금](https://aws.amazon.com/lambda/pricing/)에서 확인할 수 있다.

- Pay per calls
~~~
- First 1,000,000 requests are free 
→ 호출당 청구는 처음 백만 건은 무료이다.

- $0.20 per 1 million requests thereafter ($0.0000002 per request)
→ 이후 백만 건 요청마다 20센트가 과금된다.
~~~

- Pay per duration: (in increment of 1 ms)
~~~
- 400,000 GB-seconds of compute time per month for FREE
→ 한달에 400,000 GB-초의 컴퓨팅 시간은 무료로 사용할 수 있다. 여기서의 GB-초는 함수가 1GB RAM을 가질 때 실행시간이 40만 초이다.

- == 400,000 seconds if function is 1GB RAM 
→ 1GB RAM을 사용했을 때 무료 시간이 40만 초이다.

- == 3,200,000 seconds if function is 128 MB RAM 
→ 만약 128MB RAM을 사용한다면 무료로 사용 가능한 실행시간이 약 8배 늘어난다.

- After that $1.00 for 600,000 GB-seconds
→ 이후에는 600,000 GB-초당 1달러가 과금된다.
~~~

- It is usually very cheap to run AWS Lambda so it’s very popular  
→ 람다로 코드를 실행하면 매우 저렴하다. 그래서 애플리케이션을 만들 때 널리 사용된다.

### 7. AWS Lambda Limits to Know - per region
- 시험에 자주 나오는 내용이며, 이 한도는 리전 스코프이다. 한도에는 실행 한도와 배포 한도가 있다. 람다가 적합 여부를 판단할 수 있는 중요한 정보이므로 외우는 것이 좋다.
- Execution:
~~~
- Memory allocation: 128 MB – 10GB (1 MB increments)
→ 실행 시 메모리 할당량은 128MB - 10GB이다. 메모리는 1MB씩 증가시킬 수 있다.

- Maximum execution time: 900 seconds (15 minutes)
→ 최대 실행 시간은 900초, 즉 15분이다. 이를 초과하면 람다 실행에 바람직하지 않다.

- Environment variables (4 KB)
→ 환경변수는 4KB까지 가질 수 있다. 

- Disk capacity in the “function container” (in /tmp): 512 MB to 10GB
→ 람다 함수를 생성하는 동안 큰 파일을 가져올 때 사용할 수 있는 임시공간이 있으며, 최대 크기는 10GB이다.

- Concurrency executions: 1000 (can be increased)
→ 람다 함수는 최대 1000개까지 동시 실행이 가능하다. 
~~~

- Deployment:
~~~
- Lambda function deployment size (compressed .zip): 50 MB
→ 압축 시 최대 크기는 50MB이다.

- Size of uncompressed deployment (code + dependencies): 250 MB
→ 압축하지 않았을 때의 최대 크기는 250MB이다.

- Can use the /tmp directory to load other files at startup
→ 이 용량이 넘는 파일의 경우에는 /tmp 공간을 사용해야 한다.

- Size of environment variables: 4 KB
→ 환경변수는 4KB까지 가질 수 있다. 
~~~

### 8. Customization At The Edge
- 보통 함수와 애플리케이션을 특정 리전에서 배포하지만, `CloudFront`를 사용할 때에는 엣지 로케이션을 통해 콘텐츠를 배포한다.

- Many modern applications execute some form of the logic at the edge  
→ 많은 모던 애플리케이션은 애플리케이션에 도달하기 전에 엣지에게 로직을 실행할 수 있도록 요구한다. 이를 엣지 함수라고 한다.

- Edge Function:
~~~
- A code that you write and attach to CloudFront distributions
→ CloudFront 배포에 연결하는 코드이다.

- Runs close to your users to minimize latency
→ 이 함수는 사용자 근처에서 실행되 지연시간을 최소화하는 것이 목적이다.
~~~

- CloudFront provides two types: CloudFront Functions & Lambda@Edge  
→ `CloudFront`에는 `CloudFront` 함수와 `Lambda@Edge` 함수, 두 종류의 함수가 있다.

- You don’t have to manage any servers, deployed globally  
→ 엣지 함수를 사용하면 서버를 관리할 필요가 없다. 전역적으로 배포되기 때문이다.

- Use case: customize the CDN content  
→ 사용 사례에는 `CloudFront`의 `CDN` 컨텐츠를 커스터마이즈하는 경우를 들 수 있다.

- Pay only for what you use, Fully serverless  
→ 사용한 만큼의 비용을 지불하며 완전한 서버리스이다.

#### 1. CloudFront Functions & Lambda@Edge Use Cases
- Website Security and Privacy  
→ 웹사이트 보안과 프라이버시에 활용 가능하다.

- Dynamic Web Application at the Edge  
→ 엣지에서의 동적 웹 애플리케이션 로드에 활용 가능하다.

- Search Engine Optimization (SEO)  
→ 검색 엔진 최적화에 활용 가능하다.

- Intelligently Route Across Origins and Data Centers  
→ 오리진 및 데이터 센터 간의 지능형 경로에 활용 가능하다.

- Bot Mitigation at the Edge  
→ 엣지에서의 봇 완화에 활용 가능하다.

- Real-time Image Transformation  
→ 엣지에서의 실시간 이미지 변환에 활용 가능하다.

- A/B Testing  
→ `A/B` 테스트에 활용 가능하다. 

- User Authentication and Authorization  
→ 사용자 인증 및 권한 부여에 활용 가능하다.

- User Prioritization  
→ 사용자 우선순위 지정에 활용 가능하다.

- User Tracking and Analytics  
→ 사용자 추적 및 분석에 활용 가능하다.

###### 1. A/B 테스트
분할 테스트 또는 버킷 테스트라고도 하는 `A/B` 테스트는 두 가지 콘텐츠를 비교하여 방문자/뷰어가 더 높은 관심을 보이는 버전을 확인한다.
주요 측정지표를 기반으로 가장 성공적인 버전을 측정하기 위해 변형(`B`) 버전과 비교하여 컨트롤(`A`) 버전을 검증한다.

#### 2. CloudFront Functions
- `CloudFront` 함수의 원리와 활용 방법은 다음과 같다. 요청을 하는 유저를 여기에서는 뷰어로 지칭한다.

![image](https://user-images.githubusercontent.com/97398071/235751032-39ff6415-d67b-4d98-920f-8d03acaef218.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Lightweight functions written in JavaScript  
→ `CloudFront` 함수는 `Javascript`로 작성된 경량 함수이다. 이 함수는 뷰어 요청과 응답을 수정한다.

- For high-scale, latency-sensitive CDN customizations  
→ 확장성이 높고 지연 시간에 민감한 `CDN` 사용자 지정에 사용된다.

- Sub-ms startup times, millions of requests/second  
→ 시작 시간은 1 밀리초 미만이며 초당 백만 개의 요청을 처리한다.

- Used to change Viewer requests and responses:  
→ 앞서 말한대로 뷰어 요청과 응답을 수정할 때만 사용된다.
~~~
- Viewer Request: after CloudFront receives a request from a viewer
→ CloudFront가 뷰어로부터 요청을 받으면 뷰어 요청을 수정할 수 있다.

- Viewer Response: before CloudFront forwards the response to the viewer
→ CloudFront가 뷰어에게 응답을 보내기 전에 응답을 수정할 수 있다.
~~~

- Native feature of CloudFront (manage code entirely within CloudFront)  
→ `CloudFront`의 네이티브 기능으로 모든 코드가 `CloudFront`에서 직접 관리된다.

#### 3. Lambda@Edge
- `CloudFront Functions`보다 좀 더 많은 기능을 한다.
- Lambda functions written in NodeJS or Python  
→ 이 함수는 `Node.js`나 `Python`으로 작성된다.

- Scales to 1000s of requests/second  
→ 초당 수천 개의 요청을 처리할 수 있다.

- Used to change CloudFront requests and responses:  
→ 모든 `CloudFront` 요청 및 응답을 변경할 수 있다.
~~~
- Viewer Request – after CloudFront receives a request from a viewer
→ CloudFront가 뷰어로부터 요청을 받으면 뷰어 요청을 수정할 수 있다.

- Viewer Response – before CloudFront forwards the response to the viewer
→ CloudFront가 뷰어에게 응답을 보내기 전에 응답을 수정할 수 있다.

- Origin Request – before CloudFront forwards the request to the origin
→ CloudFront가 오리진에 요청을 전송하기 전에 수정할 수 있다.

- Origin Response – after CloudFront receives the response from the origin
→ CloudFront가 오리진에게서 받은 응답을 수정할 수 있다.
~~~

- Author your functions in one AWS Region (us-east-1), then CloudFront replicates to its locations  
→ 함수는 `us-east-1` 리전에만 작성할 수 있다. `CloudFront`의 배포를 관리하는 리전과 같은 리전이다. 함수를 작성하면 `CloudFront`가 모든 로케이션에 해당 함수를 복제한다.

#### 4. CloudFront Functions vs. Lambda@Edge
- 가장 눈에 띄는 차이는 런타임 지원 언어이다. `CloudFront Functions`는 `Javascript`지만 `Lambda@Edge`는 `Node.js`나 `Python`를 지원한다.
- `CloudFront Functions`의 확장성은 매우 높다. 수백만 개의 요청을 처리한다. 반면에 `Lambda@Edge`는 수천 개에 불과하다.
- 트리거 발생 위치도 다르다. `CloudFront Functions`는 뷰어에게만 영향을 미치는 반면 `Lambda@Edge`는 뷰어와 오리진 모두에게 영향을 미칠 수 있다.
- `CloudFront Functions`의 최대 실행 시간은 1밀리초 미만이다. 아주 빠르고 간단하다. 하지만, `Lambda@Edge`의 실행에는 5~10초가 소요된다.

#### 5. CloudFront Functions vs. Lambda@Edge - Use Cases
- CloudFront Functions  
→ 모든 작업이 1밀리초 이내에 이루어진다.
~~~
- Cache key normalization : Transform request attributes (headers, cookies, query strings, URL) to create an optimal Cache Key
→ CloudFront Functions는 캐시 키를 정규화한다. 요청 속성을 변환하여 최적의 캐시 키를 생성한다.

- Header manipulation : Insert/modify/delete HTTP headers in the request or response
→ 요청이나 응답에 `HTTP` 헤더를 삽입, 수정, 삭제하도록 헤더를 조작할 수 있다.

- URL rewrites or redirects
→ URL을 다시 쓰거나 리디렉션할 수 있다.

- Request authentication & authorization : Create and validate user-generated tokens (e.g., JWT) to allow/deny requests
→ 요청을 허용 또는 거부하기 위해 JWT을 생성하거나 검증하는 요청 인증 및 권한 부여에도 사용된다.
~~~

- Lambda@Edge
~~~
- Longer execution time (several ms)
→ 실행 시간이 10초가 걸릴 수도 있다.

- Adjustable CPU or memory
→ CPU와 메모리가 증가하므로 여러 라이브러리를 로드할 수 있다.

- Your code depends on a 3rd libraries (e.g., AWS SDK to access other AWS services)
→ 타사 라이브러리에 코드를 의존시킬 수도 있다. 가령 SDK에서 다른 AWS 서비스에 액세스할 수 있도록 할 수 있다.

- Network access to use external services for processing
→ 네트워크 액세스를 통해 외부 서비스에서 데이터를 처리할 수 있어 대규모 데이터 통합을 수행할 수 있다.

- File system access or access to the body of HTTP requests
→ 파일 시스템이나 HTTP 요청 본문에도 바로 액세스할 수 있다.
~~~

### 9. Lambda by default
- By default, your Lambda function is launched outside your own VPC (in an AWS-owned VPC)  
→ 기본적으로 람다 함수를 시작하면 `VPC`의 외부에서 시작된다.

- Therefore, it cannot access resources in your VPC (RDS, ElastiCache, internal ELB…)  
→ `VPC`에서 리소스를 액세스할 권한이 없다. `RDS`, `ElastiCache`, `internal ELB`를 실행하면 람다 함수가 해당 서비스에 액세스할 수 없는 것이다.

- 단, 인터넷의 퍼블릭 `API`에 액세스하는 것은 가능하다. 아래 사진에서 `DynamoDB`에 액세스 가능한 이유는 `DynamoDB`가 `AWS` 클라우드의 퍼블릭 리소스이기 때문이다.

![image](https://user-images.githubusercontent.com/97398071/235754470-bed6000e-a52b-4502-9207-a3876b928bb8.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 10. Lambda in VPC
- 람다 함수가 `VPC` 내부의 리소스를 액세스하지 못하는 문제를 해결할 수 있다. `VPC` 내에서 람다 함수를 실행하는 것이 그것이다.

- You must define the VPC ID, the Subnets and the Security Groups  
→ `VPC ID`, 람다를 실행하려는 서브넷 지정, 람다 함수에 보안 그룹 추가가 필요하다.

- Lambda will create an ENI (Elastic Network Interface) in your subnets  
→ 그러면 람다가 서브넷에 `ENI`를 생성해 액세스할 수 있게 된다.

![image](https://user-images.githubusercontent.com/97398071/235755230-f6419e90-f305-4838-af2c-e1979f044988.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 11. Lambda with RDS Proxy
- `VPC` 내에서 람다를 사용하는 대표적인 사용 사례는 `RDS` 프록시이다.

- If Lambda functions directly access your database, they may open too many connections under high load  
→ `RDS` 데이터베이스에 직접 액세스하면 큰 문제가 발생할 수 있다. 
람다 함수의 수가 너무 많이 생성되었다 사라지길 반복하면 개방된 연결이 너무 많아서 `RDS` 데이터베이스의 로드가 상승해 시간 초과등의 문제가 발생한다는 것이다.

- RDS Proxy  
→ 이를 해결하는 방법은 `RDS` 프록시를 시작하는 것이다. 프록시를 통해 연결을 한 곳으로 모아 `RDS` 데이터베이스 인스턴스 연결의 수를 줄일 수 있다. 
`RDS` 프록시에는 세 가지 장점이 있다.
~~~
- Improve scalability by pooling and sharing DB connections
→ 데이터베이스 연결의 풀링과 공유를 통해 확장성을 향상시킨다.

- Improve availability by reducing by 66% the failover time and preserving connections
→ 장애가 발생할 경우 장애 조치 시간을 66%까지 줄여 가용성을 향상시키고 연결을 보존한다. 이는 RDS와 Aurora에 모두 적용된다.

- Improve security by enforcing IAM authentication and storing credentials in Secrets Manager
→ RDS 프록시 수준에서 IAM을 강제하여 보안을 높일 수 있고, 자격 증명은 Secrets Manager에 저장된다.
~~~

- The Lambda function must be deployed in your VPC, because RDS Proxy is never publicly accessible  
→ 람다 함수를 `RDS` 프록시에 연결할 수 있으려면 `VPC` 내에서 람다 함수를 시작해야 한다. `RDS` 프록시 또한 퍼블릭 액세스가 불가능하다.

![image](https://user-images.githubusercontent.com/97398071/235755390-60318b91-6c27-4fc9-9c0b-94b8e9ccaeac.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [A/B 테스트는 무엇인가요?](https://www.oracle.com/kr/cx/marketing/what-is-ab-testing/)
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---