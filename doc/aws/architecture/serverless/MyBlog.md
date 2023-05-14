## MyBlog.com
### 1. Serverless hosted website: MyBlog.com
- This website should scale globally  
→ 이 웹사이트는 글로벌 스케일 아웃이 가능해야 한다.

- Blogs are rarely written, but often read  
→ 보통 블로그를 작성하는 빈도보다 읽는 빈도가 높다.

- Some of the website is purely static files, the rest is a dynamic REST API  
→ 이 웹사이트는 대부분 정적 파일로 구성되고 일부는 동적인 `REST API`로 구성된다.

- Caching must be implement where possible  
→ 캐시를 적용해서 비용을 절감하고 응답 속도를 높으고 좋은 `UX`를 제공하고 싶다.

- Any new users that subscribes should receive a welcome email  
→ 블로그에 처음 방문하는 사람에게 환영 이메일을 발송하고 싶다.

- Any photo uploaded to the blog should have a thumbnail generated  
→ 블로그에 업로드되는 사진은 섬네일이 생성되었으면 좋겠다. 

### 2. Serving static content, globally
- 정적 컨텐츠를 글로벌을 대상으로 서비스하고 싶다. 이 컨텐츠는 `S3` 내에 저장된다.
- 전 세계를 대상으로 서비스되는 `CDN` 서비스인 `CloudFront`를 이용해 배포한다.
- 클라이언트는 `CloudFront`의 엣지 로케이션과 통신을 하게 되고 엣지 로케이션은 `S3`에게 받은 데이터를 캐시로 저장하게 된다.

![image](https://user-images.githubusercontent.com/97398071/235868358-62ccee0b-430d-4b8c-aa40-9214cb7e7d17.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. Serving static content, globally, securely
- 보안을 위해 `S3`의 오리진 액세스를 `CloudFront`에서만 접속 가능하도록 제한한다.
- 이를 위해 `CloudFront`만 인가하도록 버킷 정책을 추가한다. 이렇게 되면 `S3`에 바로 접근하는 사용자는 인가를 받지 못하고 `S3` 버킷은 보호된다.

![image](https://user-images.githubusercontent.com/97398071/235868434-42922e78-082f-4a8d-94a0-79dec5438604.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 4. Adding a public serverless REST API
- `REST API`는 `Amazon API Gateway`, 람다, `DynamoDB`를 통해 생성한다.
- 읽기가 많이 발생하니 `DynamoDB`와 람다 사이에 `DAX`를 캐시 레이어로 사용한다.

![image](https://user-images.githubusercontent.com/97398071/235868511-77a999a9-49c5-4b22-99ef-0af21f079ecb.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/

### 5. Leveraging DynamoDB Global Tables
- 글로벌로 서빙할 때 지역에 따라 발생하는 지연을 줄이기 위해 `DynamoDB`의 글로벌 데이터베이스를 사용하는 것도 좋다.
- 전체 인프라와 이 구성의 속도를 향상시켜주는 좋은 방법이다.

![image](https://user-images.githubusercontent.com/97398071/235868654-a7bd1c84-75aa-46e0-8874-648d069a0fcf.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/

### 6. User Welcome email flow
- `DynamoDB`의 변경 사항을 전송하기 위해 `DynamoDB Stream`을 이용하고, 스트림은 람다 함수를 호출한다.
- 이 람다 함수는 `IAM Roles`를 부여받아 `Amazon SES`를 사용할 수 있게 해 준다.
- 람다 함수가 `AWS SDK`를 이용해 `Amazon SES`에 메일 송신을 요청하고, `Amazon SES`는 이메일을 발송하는 구조이다.

![image](https://user-images.githubusercontent.com/97398071/235868726-4d482104-3390-49a6-adc2-886f2bce579c.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/

### 7. Thumbnail Generation flow
- 클라이언트에서 `S3` 버킷으로 바로 업로드할 수도 있고, `CloudFront OAI`를 이용할 수도 있다.
- `CloudFront OAI`를 거치는 구조를 `S3 Transfer Acceleration`이라고 한다.
- `S3`에 파일이 추가되면 람다 함수를 호출하게 한다. `S3`는 람다를 호출할 수 있다.
- 람다는 섬네일을 생성하고 섬네일을 `S3` 버켓에 넣는데 다른 버킷에 넣을 수 있다.
- `S3`는 `SQS`와 `SNS`를 호출할 수도 있다. 원하는 작업을 수행하면 된다.

![image](https://user-images.githubusercontent.com/97398071/235868839-55dc6231-85c8-4b00-9710-56743537c694.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/

### 8. AWS Hosted Website Summary
- We’ve seen static content being distributed using CloudFront with S3  
→ 정적 컨텐츠를 `CloudFront`와 `S3`로 배포하는 방법을 확인했다.

- The REST API was serverless, didn’t need Cognito because public  
→ 서버리스인 `REST API`를 생성하는 법을 살펴봤다.

- We leveraged a Global DynamoDB table to serve the data globally  
→ 데이터를 글로벌로 서빙하기 위해 `DynamoDB`를 추가했다.

- (we could have used Aurora Global Database)  
→ `Aurora` 데이터베이스를 사용할수도 있었지만, 이 사례는 서버리스이므로 `DynamoDB`를 선택한 것이다.

- We enabled DynamoDB streams to trigger a Lambda function  
→ `DynamoDB streams`을 사용해 람다의 트리거로 사용했다.

- The lambda function had an IAM role which could use SES  
→ 호출되는 람다 함수에 `IAM Roles`를 부여해서 `SES`를 이용할 수 있도록 했다.

- SES (Simple Email Service) was used to send emails in a serverless way  
→ `SES`는 서버리스 방식으로 이메일을 발송할 때 사용한다.

- S3 can trigger SQS / SNS / Lambda to notify of events  
→ `S3`는 이벤트 알림을 위해 `SQS`, `SNS`, 람다를 호출할 수 있다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---