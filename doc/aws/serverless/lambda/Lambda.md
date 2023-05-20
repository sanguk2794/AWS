## AWS Lambda
### 1. Why AWS Lambda
- Amazon EC2
~~~
- Virtual Servers in the Cloud
→ Amazon EC2는 클라우드의 가상 서버이기 때문에 프로비저닝이 필요하다.

- Limited by RAM and CPU
→ 그렇기에 메모리와 CPU 크기가 제한된다.

- Continuously running
→ 지속적으로 실행되어야 한다. 비용 최적화를 위해서는 인스턴스를 효율적으로 관리해야 한다.

- Scaling means intervention to add / remove servers
→ ASG로 스케일링이 가능하다. 이는 서버를 늘리고 줄이는 작업이 필요하다는 의미도 된다.
~~~

- Amazon Lambda
~~~
- Virtual functions – no servers to manage!
→ 람다는 서버 관리가 필요없는 가상의 함수이다. 소스 코드를 프로비저닝하면 함수가 실행된다.

- Limited by time - short executions
→ 실행 시간에 제한이 있다.

- Run on-demand
→ 온디맨드로 실행된다. 람다 함수가 실행중이 아니면 비용 또한 청구되지 않는다.

- Scaling is automated!
→ 스케일링이 자동화된다. 더 많은 람다 함수가 동시에 필요한 경우 AWS가 자동으로 프로비저닝해서 람다 함수를 실행시킨다.
~~~

![image](https://user-images.githubusercontent.com/97398071/235742473-3199432b-fc22-4bfa-a5fb-dc068abf47c2.png)

- 람다 함수는 매우 빠르게 스케일링할 수 있기 때문에 동시성이 급증하면 알림을 받을 수 있는 통제장치가 필요하다.

- 람다 함수를 구성하여 레이어 형태로 된 추가적인 코드와 콘텐츠를 가져올 수 있다. 레이어는 라이브러리, 커스텀 런타임 또는 기타 의존성이 포함되어 있는 `ZIP` 아카이브이다. 레이어를 사용하면 배포 패키지에 넣지 않고도 함수에서 라이브러리를 사용할 수 있다.

### 2. Benefits of AWS Lambda
- Easy Pricing:  
~~~
- 가격 책정이 아주 쉬워진다.

- Pay per request and compute time
→ 람다가 수신하는 요청의 횟수에 따라 과금이 이루어진다.

- Free tier of 1,000,000 AWS Lambda requests and 400,000 GBs of compute time
→ 프리 티어에서도 람다를 넉넉히 제공해준다. 
~~~

- Integrated with the whole AWS suite of services  
→ 다양한 `AWS` 서비스와 통합할 수 있다.

- Integrated with many programming languages  
→ `Lambda`에 여러가지의 프로그래밍 언어를 사용할 수 있어서 상당히 자유로운 편이다.

- Easy monitoring through AWS CloudWatch  
→ `CloudWatch`와의 통합을 위한 쉬운 모니터링이 가능하다.

- Easy to get more resources per functions (up to 10GB of RAM!)  
→ 함수당 최대 `10GB`의 램을 프로비저닝 가능하다.

- Increasing RAM will also improve CPU and network!  
→ 함수의 램을 증가시키면 `CPU` 및 네트워크 품질과 성능도 함께 향상된다.

- 프로그래머의 관점에서 보면 코드 몇 가지를 람다에 업로드하는 것만으로도 아주 빠른 실행이 가능하기에 굉장히 유용한 기능이다.  
→ 람다는 코드를 배포하고 작동시키는데 아주 매끄럽게 실행될 뿐만 아니라 자동으로 스케일링되는 완전한 서버리스이다.

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
- 람다와 결합 가능한 리소스의 목록은 다음과 같다.
~~~
- API Gateway
→ REST API를 생성해 람다를 호출한다.

- Kinesis
→ 람다를 이용해 데이터를 변환한다.

- DynamoDB
→ 트리거를 생성할 때 사용한다. 데이터베이스에서 무슨 일이 생기면 람다 함수가 작동되도록 한다.

- S3
→ 객체 파일이 생성되거나 할 때 람다 함수가 작동되도록 한다.

- CloudWatch Logs
→ 어디든 해당 로그를 스트림할 수 있다.

- SNS
→ 알림과 토픽에 대처할 수 있다.

- SQS
→ 대기열 메세지를 처리할 수 있다.

- Cognito
→ 유저가 데이터베이스에 로그인 할 때마다 응답한다.

- CloudFront, CloudWatch Events EventBridge
→ AWS의 인프라에 어떤 일이 생기고 그 상황에 대응하고자 할 때 람다 함수가 작동되도록 한다. 예를 들면 파이프라인이 끊어졌을 때 상황에 따라 자동화가 가능하다.
~~~

### 5. Use case
#### 1. Serverless Thumbnail creation
- `S3` 버킷이 있고 섬네일을 생성하고 싶은 상황이다.
~~~
- Amazon S3에 새 이미지가 업로드되는 이벤트가 생기고 S3 이벤트 알림을 통해 람다 함수가 작동된다.
- 이 람다 펑션에는 섬네일을 생성하는 코드가 있다. 해당 섬네일은 다른 S3 버킷이나 같은 S3 버킷으로 푸시 및 업로드된다.
- 또 람다 함수는 몇몇 데이터를 DynamoDB에 삽입할 수 있다. 이미지 이름, 크기, 생성 날짜 등 이미지의 메타데이터가 삽입된다.
- 람다를 사용해 기능을 자동화하고, S3에 새 이미지와 앱이 생성되는 이벤트에 대한 반응형 아키텍처를 얻을 수 있다.
~~~

![image](https://user-images.githubusercontent.com/97398071/235739087-34ad33f5-7787-40e8-b520-1ca1f6072633.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 2. Serverless CRON Job
- `CRON`이란 `EC2` 인스턴스에서 작업을 생성하는 방법이며, 5분마다 또는 월요일 10시마다 등등 실행 시간을 지정할 수 있다.
- 이 `CRON`은 가상 서버에 실행해야 한다. `EC2` 인스턴스 등이다.
- `CloudWatch` 이벤트 규칙 또는 `EventBridge` 규칙을 만들고 1시간마다 작동되게 설정해서 1시간마다 람다 함수와 통합하면 태스크를 수행할 수 있게 된다.
- `Rambda` 뿐만 아니라 `CloudWatch` 서비스도 역시 서버리스이다.

![image](https://user-images.githubusercontent.com/97398071/235739853-f4f56e8b-2d7d-46ca-baf6-48cbba8b8d85.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 6. AWS Lambda Pricing: example
- You can find overall pricing information here: https://aws.amazon.com/lambda/pricing/  
→ 가격 정보는 [AWS Lambda 요금](https://aws.amazon.com/lambda/pricing/)에서 확인할 수 있다.

- Pay per calls
~~~
- First 1,000,000 requests are free 
→ 처음 백만 건의 호출은 무료이다.

- $0.20 per 1 million requests thereafter ($0.0000002 per request)
→ 이후에는 백만 건 요청마다 20센트가 청구된다.
~~~

- Pay per duration: (in increment of 1 ms)
~~~
- 400,000 GB-seconds of compute time per month for FREE
→ 한달에 400,000 GB/초의 컴퓨팅 시간은 무료로 사용할 수 있다. 여기서의 1 GB/초는 함수가 1GB RAM을 가질 때의 실행시간 1초를 의미한다.

- == 400,000 seconds if function is 1GB RAM 
→ 1GB RAM을 사용했을 때 무료 시간이 40만 초이다.

- == 3,200,000 seconds if function is 128 MB RAM 
→ 만약 128MB RAM을 사용한다면 무료로 사용 가능한 실행시간이 약 8배 늘어난다.

- After that $1.00 for 600,000 GB-seconds
→ 이후에는 600,000 GB/초당 1달러가 과금된다.
~~~

- It is usually very cheap to run AWS Lambda so it’s very popular  
→ 람다로 코드를 실행하면 매우 저렴하다. 그래서 애플리케이션을 만들 때 널리 사용된다.

### 7. AWS Lambda Limits to Know - per region
- 시험에 자주 나오는 내용이다. 람다 스코프 한도, 실행 한도, 배포 한도가 있다. 람다가 적합 여부를 판단할 수 있는 중요한 정보이다.

- Scope
→ 람다의 스코프 한도는 리전이다.

- Execution:
~~~
- Memory allocation: 128 MB – 10GB (1 MB increments)
→ 실행 시 메모리 할당량은 128MB - 10GB이다. 메모리는 1MB씩 증가시킬 수 있다.

- Maximum execution time: 900 seconds (15 minutes)
→ 최대 실행 시간은 900초, 즉 15분이다. 이를 초과하면 람다 실행에 바람직하지 않다.

- Environment variables (4 KB)
→ 환경변수는 4KB까지 가질 수 있다.

- Disk capacity in the “function container” (in /tmp): 512 MB to 10GB
→ 람다 함수 실행을 위해 큰 파일을 가져와야 할 때 사용할 수 있는 임시공간인 Function container가 있으며, 최대 크기는 10GB이다.

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
→ 이 용량이 넘는 파일일 경우 /tmp 공간을 사용해야 한다.

- Size of environment variables: 4 KB
→ 환경변수는 4KB까지 가질 수 있다. 
~~~

### 8. 환경변수 암호화
- 환경 변수를 사용하는 `Lambda` 함수를 생성하거나 업데이트해야 한다면 `AWS KMS`를 사용해 함수를 암호화해야 한다. 생성한 `Lambda` 함수가 호출되면 암호화한 환경변수의 값을 해독해 `Lambda` 코드에서 사용할 수 있다.
- 이미 생성된 `Lambda` 함수에 환경변수 설정을 추가하기 위해서는 암호화 헬퍼와 `KMS`를 사용해야 한다. 이 때에는 자체 `KMS` 키를 생성하고 기본 키 대신 이를 사용해야 한다. 기본 키를 선택하면 오류가 발생한다.

---
#### ▶ Reference
- [A/B 테스트는 무엇인가요?](https://www.oracle.com/kr/cx/marketing/what-is-ab-testing/)
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
