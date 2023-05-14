## Micro Services architecture
### 1. Micro Services architecture
- 정확히 서버리스는 아니다.

- We want to switch to a micro service architecture  
→ 마이크로서비스 아키텍처로 전환하고자 한다.

- Many services interact with each other directly using a REST API  
→ 많은 서비스가 `REST API`를 통해 상호작용을 수행한다.

- Each architecture for each micro service may vary in form and shape  
→ 마이크로서비스의 아키텍처는 모습과 형태가 달라 뭐든 원하는 대로 작성할 수 있다.

- We want a micro-service architecture so we can have a leaner development lifecycle for each service  
→ 마이크로서비스 아키텍처를 쓰는 이유는 서비스 개발 수명 주기를 줄이고자 함이다. 각 서비스가 독립적으로 확장하며 독립적인 코드 레포지토리를 보유할 수 있다.

### 2. Micro Services Environment
- 유저는 `HTTPS`를 통해 `ELB`와의 통신을 수행한다.
- `ELB`는 `ECS`를 거쳐 `DynamoDB`와 통신한다.
- 마이크로서비스는 보통 `DNS` 이름이나 `URL`이 존재한다. 이를 위해 `Route 53`을 사용한다.
- 이상이 첫번째 서비스이다.

- 두 번째 서버로 `API Gateway`와 람다, `ElastiCache`를 사용하는 전형적인 서버리스 아키텍처 서비스를 개발한다.
- 이 두 번째 서비스는 첫 번째 서비스와 상호작용이 가능하다. `ELB`에 람다 함수가 호출을 보내 첫 마이크로서비스에서 정보를 얻고 응답을 생성한다.

- 그리고 역시 `ELB`를 사용하는 세 번쩨 서비스를 개발한다. 세 번째 서비스는 전형적인 아키텍처에 해당한다.
- `EC2` 인스턴스가 결정하기 전에 두 번째 마이크로서비스를 호출해야 한다.

![image](https://user-images.githubusercontent.com/97398071/235874558-3aab0bb6-4486-4ef9-9816-273dedd02041.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. Discussions on Micro Services
- You are free to design each micro-service the way you want  
→ 각 마이크로서비스는 원하는 대로 설계할 수 있다. 

- Synchronous patterns: API Gateway, Load Balancers  
→ 마이크로서비스에는 두 가지 패턴이 있다. `API Gateway`, `Load Balancers`는 다른 마이크로서비스에 `HTTPS` 호출을 보내기 적합한 구조이다.

- Asynchronous patterns: SQS, Kinesis, SNS, Lambda triggers (S3)  
→ 비동기식 패턴도 있다. `SQS`, `Kinesis`, `SNS`, `Lambda triggers` 등으로 작동하는 경우이다. 상호작용 후 어떤 일이 일어날지 개의치 않는다.

- Challenges with micro-services:
~~~
- repeated overhead for creating each new microservice
→ 마이크로 서비스의 문제는 새로운 마이크로서비스를 생성할 때마다 오버헤드가 발생한다는 점이다.

- issues with optimizing server density/utilization
→ 서버 밀도나 사용률을 최적화하는데 어려움을 겪기도 한다.

- complexity of running multiple versions of multiple microservices simultaneously
→ 여러 버전의 마이크로서비스를 가동하려면 복잡하다.

- proliferation of client-side code requirements to integrate with many separate services.
→ 여러 서비스와 통합하려다 클라이언트 코드의 요구사항이 급증하기도 한다.
~~~

- Some of the challenges are solved by Serverless patterns:
~~~
- API Gateway, Lambda scale automatically and you pay per usage
→ 하지만 위의 문제는 서버리스 패턴으로 어느정도 해결 가능하다. API Gateway나 람다는 자동으로 확장한다. 그러므로 서버 사용률을 걱정할 필요가 없어진다.

- You can easily clone API, reproduce environments
→ API Gateway에서 API를 쉽게 복제하고 재생산할 수 있다.

- Generated client SDK through Swagger integration for the API Gateway
→ API Gateway를 위한 Swagger 통합으로 클라이언트 SDK의 생성이 가능하다.
~~~

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---