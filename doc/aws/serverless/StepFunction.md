## AWS Step Functions
### 6. AWS Step Functions
- Build serverless visual workflow to orchestrate your Lambda functions  
→ `Step Functions`는 서버리스 워크플로를 시각적으로 구성할 수 있는 기능으로 주로 람다 함수를 오케스트레이션하는데 사용된다.
그래프를 만들고 각 그래프 단계별로 해당 단계의 결과에 따라 다음으로 수행하는 작업이 무엇인지 정의한다. 복잡한 워크플로를 `AWS`에서 실행시킬 수 있는 편리한 도구이다.

- Features: sequence, parallel, conditions, timeouts, error handling, …  
→ `Step Functions`이 제공하는 기능으로는 시퀀싱, 병행 실행, 조건 설정, 타임아웃, 에러 처리 등이 있다.

- Can integrate with EC2, ECS, On-premises servers, API Gateway, SQS queues, etc…  
→ 람다 함수뿐만 아니라 `EC2`, `ECS`, 온프레미스 서버, `API Gateway`, `SQS` 등 다양한 `AWS` 서비스를 워크플로에 넣을 수 있다.

- Possibility of implementing human approval feature  
→ 워크플로에 사람이 개입해서 승인을 해야만 진행되는 단계를 설정할 수 있다. 예를 들어 어떤 지점에 이르면 사람이 결과를 확인하고 승인을 하면 다음 단계로 넘어가고 아니면 워크플로가 멈춰 실패하게 된다.

- Use cases: order fulfillment, data processing, web applications, any workflow  
→ 주문 이행이나 데이터 처리, 웹 애플리케이션 등 구성하기 복잡한 워크플로를 시각적으로 구성하려고 할 때 사용한다.

![image](https://user-images.githubusercontent.com/97398071/235828284-b49bc2f7-4992-49d8-a46c-d8ead60d1540.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- `AWS` 서비스(예: `Lambda`)를 사용해 서버리스 워크플로를 구축할 수 있으며 사람의 승인을 지원하는 `AWS` 서비스는 무엇입니까?  
→ `AWS Step Functions`

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---