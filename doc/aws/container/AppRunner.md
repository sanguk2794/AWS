## AWS App Runner
### 1. AWS App Runner
- Fully managed service that makes it easy to deploy web applications and APIs at scale  
→ 완전 관리형 서비스로 규모에 따라 웹 애플리케이션, `API`의 배포를 돕는다.

- No infrastructure experience required  
→ 이 서비스로 누구나 `AWS`에 배포할 수 있다. 인프라나 컨테이너 소스 코드 등을 전혀 알 필요가 없다.

- Start with your source code or container image  
→ 소스 코드나 `Docker` 컨테이너 이미지를 보유하고 있다면 원하는 구성을 설정하는 것만으로 많은 것이 자동으로 이루어진다. 구성에는 `CPU`와 용량 등이 포함된다.

- Automatically builds and deploy the web app  
→ `App Runner` 서비스를 통해 자동적으로 빌드와 디플로이가 이루어지고 컨테이너가 생성되어 해당 앱이 배포된다. 배포된 이후에는 `URL`을 통해 바로 액세스할 수 있다.

- Automatic scaling, highly available, load balancer, encryption  
→ 오토 스케일링이 가능하고 가용성이 높으며 로드 밸런싱 및 암호화 기능을 지원한다.

- VPC access support  
→ 애플리케이션, 즉 컨테이너가 `VPC`에 액세스할 수 있다.

- Connect to database, cache, and message queue services  
→ 데이터베이스, 캐시, `SQS` 등에 연결할 수 있다.

- Use cases: web apps, APIs, microservices, rapid production deployments  
→ 사용 사례로는 빨리 배포해야 하는 웹 앱, `API` 그리고 마이크로 서비스가 있다.

![image](https://user-images.githubusercontent.com/97398071/235719760-6ca3987d-1885-44e8-b84a-cba21bbba03f.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---