## Mobile application: MyTodoList
### 1. Mobile application: MyTodoList
- Expose as REST API with HTTPS  
→ `HTTPS` 엔드 포인트가 있는 `REST API`가 호출되어야 한다.

- Serverless architecture  
→ 서버리스 아키텍처이다.

- Users should be able to directly interact with their own folder in S3  
→ 사용자가 원한다면 스스로 데이터를 관리하도록 `S3`에 있는 폴더와 직접 상호작용이 가능해야 한다.

- Users should authenticate through a managed serverless service  
→ 사용자가 관리형 서버리스 서비스로 인증할 수 있어야 한다.

- The users can write and read to-dos, but they mostly read them  
→ 사용자가 할 일을 읽고 쓸 수 있지만, 대부분 읽기이므로 관련 성능이 필요하다.

- The database should scale, and have some high read throughput  
→ 데이터베이스 계층은 확장할 수 있도록 구축해 읽기 처리량을 높여야 한다.

### 2. Mobile app: REST API layer
- 서버리스 아키텍처를 원하고 있으므로, `API Gateway`는 좋은 선택이다. `API Gateway`가 람다함수를 호출하게 함으로써 확장을 허용한다.
- 람다는 데이터베이스에 값을 저장하거나 읽어내야 한다. 서버리스이면서 확장이 잘 되는 `DB`는 `DynamoDB`이다.
- 인증 계층이 있어야 한다. 이를 위해 `Amazon Cognito`같은 서버리스 기술을 사용할 수 있다.
- 이는 굉장히 전형적인 서버리스 `API`의 구조이다.

![image](https://user-images.githubusercontent.com/97398071/235865214-c96cf2ac-0479-4ffe-86a5-1ce1f691a65b.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. Mobile app: giving users access to S3
- 사용자에게 `S3` 버킷 접근 권한이 있어야 한다. `Amazon Cognito`는 `AWS STS`를 통해 임시 자격 증명을 제공할 수 있다.
- 이 자격 증명을 모바일 클라이언트에게 반환하면 `Amazon S3`에서 파일을 저장하거나 회수하거나 `S3`에서 전용 공간에 액세스하도록 허용할 수 있다.
- `AWS` 사용자 자격 증명을 모바일 클라이언트에 저장한다 <- 이것은 절대적인 오답이다. 굉장한 빈출 문제이다.

![image](https://user-images.githubusercontent.com/97398071/235865839-f1a8373f-1e49-448a-aeb7-9a117e56b33c.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 4. Mobile app: high read throughput, static data
- 애플리케이션의 사용자가 늘어나기 시작해 패턴을 살펴봤는데 읽기 처리량이 아주 높다.
- 읽기 처리량을 늘리면서 전체 비용을 줄이기 위해 캐싱 계층으로 `DAX`를 추가한다. 이를 통해 `DynamoDB` 읽기 용량 단위가 많이 필요하지 않게 되고 확장이 용이하며 비용도 줄어든다.

![image](https://user-images.githubusercontent.com/97398071/235866462-d952a413-b953-4bf6-90d0-0818afb18c87.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 5. Mobile app: caching at the API Gateway
- 답이 거의 바뀌지 않는다면 `Amazon API Gateway` 레벨에서 람다의 응답을 캐시를 추가해 사용할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/235867056-1a1484f2-e8f7-4d51-8d9d-672a27672e7e.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 6. In this lecture
- Serverless REST API: HTTPS, API Gateway, Lambda, DynamoDB  
→ `HTTPS`, `API Gateway`, `Lambda`, `DynamoDB`를 사용하는 전형적인 서버리스 `REST API` 아키텍처이다.

- Using Cognito to generate temporary credentials with STS to access S3 bucket with restricted policy. App users can directly access AWS resources this way. Pattern can be applied to DynamoDB, Lambda…  
→ 인증을 위한 `Amazon Cognito`와 임시 자격 증명을 발급을 위한 `Amazon STS`를 함께 사용한다.

- Caching the reads on DynamoDB using DAX  
→ `DAX`를 이용해 데이터베이스 읽기 액세스를 캐싱하면 읽기 성능을 개선하고 비용을 절감할 수 있다.

- Caching the REST requests at the API Gateway level  
→ `REST` 요청 캐싱은 응답이 고정된 경우 `API Gateway`에서 가능하다.

- Security for authentication and authorization with Cognito, STS  
→ 보안은 전부 `Cognito`로 가능하다. `Cognito`는 `API Gateway`와 직접 통합되어 있다.

### 7. practice
- 한 스타트업 회사가 `AWS` 상에서 자신들의 애플리케이션을 실행하려 합니다. 이 기업에서는 여러분을 솔루션 아키텍트로 고용해, 완전한 서버리스 `REST API`의 설계 및 구현을 맡겼습니다. 어떤 기술 스택을 사용하는 것을 추천할 수 있을까요?  
→ `API Gateway` + `AWS Lambda`

- 다음 `AWS` 서비스 중 즉시 사용 가능한 캐싱 기능이 없는 것을 고르세요  
→ `Lambda`, 다른 선택지는 `API Gateway`와 `DynamoDB`

- 사용자들에게 전역적으로 배포하고자 하는 대량의 정적 파일이 S3 버킷 내에 저장되어 있습니다. 이 경우, 다음 중 어떤 AWS 서비스를 선택해야 할까요?  
→ `Amazon CloudFront`

- `ap-northeast-1`에 `DynamoDB` 테이블을 생성했으며, 이 테이블을 `eu-west-1`에서도 사용할 수 있도록 하기 위해 `DynamoDB` 글로벌 테이블을 생성하기로 했습니다. `DynamoDB` 글로벌 테이블을 생성하기 전에는 어떤 기능을 먼저 활성화해야 할까요?  
→ `DynamoDB Streams`, `DynamoDB` 버전 관리는 없으므로 주의하자.

- `DynamoDB Streams`을 사용해 `DynamoDB` 테이블로 항목이 추가될 때마다 `Lambda` 함수가 각 아이템을 실행하도록 구성한 상황입니다. 이 함수는 추후 장기적인 처리 업무를 위해 메시지를 SQS 대기열로 삽입하게 되어 있습니다. `Lambda` 함수를 매번 호출해보면 `DynamoDB Stream`을 읽을 수는 있으나 `SQS` 대기열로 메시지를 삽입하지는 못하는 것으로 보입니다. 이 경우, 무엇이 문제일까요?  
→ 람다 실행 `IAM Roles`에 권한이 없음

- `S3` 버킷에 저장된 영상을 인코딩하고, 인코딩된 영상을 다시 `S3` 버킷으로 저장할 목적으로만 사용될 마이크로 서비스 애플리케이션의 아키텍처를 생성하려 합니다. 이 마이크로 서비스 애플리케이션의 신뢰도를 높이고, 실패 시 재시도할 수 있는 기능을 부여하고자 합니다. 각 영상 처리에는 최대 25분이 걸릴 수 있습니다. 아키텍처에 사용되는 서비스는 비동기적이어야 하며, 하루 동안 중지되었다가 그 다음 날 인코딩되지 않은 영상부터 다시 작업을 시작할 수 있는 기능을 포함해야 합니다. 이런 경우, 다음 중 어떤 `AWS` 서비스를 사용하는 게 권장될까요?  
→ `Amazon SQS` + `Amazon EC2`

- 여러분은 전 세계에서 이미지를 다운로드하는 사진 공유 웹사이트를 실행하고 있습니다. 여러분은 `15GB`가 넘는 크기의 아름다운 산 이미지의 마스터 팩을 매 달 웹사이트에 게시합니다. 이 콘텐츠는 Elastic File System(EFS)에 호스팅되어 있으며, Application Load Balancer와 한 세트의 EC2 인스턴스을 통해 배포되었습니다. 여러분은 매달 높은 양의 트래픽을 경험하고 있으며, 이로 인해 EC2 인스턴스의 작업량과 네트워크 비용이 증가하고 있는 상황입니다. 웹사이트를 리팩터링하지 않고 EC2 부하와 네트워크 비용을 줄이기 위해서는 어떤 방법이 권장될까요?  
→ `Amazon CloudFront` 배포 생성

- 이 `AWS` 서비스를 사용하면 실시간으로 초당 기가바이트의 데이터를 포착할 수 있고, 리플레이 기능을 통해 이러한 데이터를 여러 개의 소비 애플리케이션으로 전송할 수 있습니다.  
→ `Kinesis Data Streams`, `Amazon Kinesis Data Streams`은 확장성과 내구성이 뛰어난 실시간 데이터 스트리밍 서비스이다. 이는 웹사이트, 클릭스트림, 데이터베이스 이벤트 스트림, 금융 거래, 소셜 미디어 피드, IT 로그 및 위치 추적 이벤트 등의 수백 개 소스로부터 초당 `GB`에 달하는 데이터를 지속적으로 포착할 수 있다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---