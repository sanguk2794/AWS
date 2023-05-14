## CloudFormation
### 1. What is CloudFormation
- CloudFormation is a declarative way of outlining your AWS Infrastructure, for any resources (most of them are supported).  
→ `CloudFormation`은 `AWS` 인프라를 요약하는 선언적인 방법이다. 거의 대부분의 리소스가 지원된다.

- For example, within a CloudFormation template, you say:
~~~
- I want a security group
- I want two EC2 instances using this security group
- I want an S3 bucket
- I want a load balancer (ELB) in front of these machines
~~~

- Then CloudFormation creates those for you, in the right order, with the exact configuration that you specify  
→ `CloudFormation`은 정한 순서와 구성 그대로 이 모든 것을 자동으로 생성해 준다.

- `CloudFormation`은 스택을 생성한 후 스택 템플릿을 제공해야 한다. 스택 템플릿을 생성할 때의 리전은 반드시 `us-east-`이어야 한다. 그렇지 않으면 제대로 작동하지 않고 오류가 발생한다.
→ 순서를 알아서 이해할 정도로 충분히 똑똑한 서비스이다. 

- `CloudFormation`을 삭제할 경우 사용했던 리소스들도 자동으로 삭제한다. 하나하나 지우지 않아도 괜찮다.

### 2. Benefits of AWS CloudFormation
- Infrastructure as code
~~~
- No resources are manually created, which is excellent for control
→ 수동으로 리소스를 만들 필요가 없다. 관리가 편해진다.

- Changes to the infrastructure are reviewed through code
→ AWS 클라우드의 작동 방식을 변경할 때마다 코드 리뷰를 통해 검토해 준다. 
~~~

- Cost
~~~
- Each resources within the stack is tagged with an identifier so you can easily see how much a stack costs you
→ 비용적으로도 이득이다. 스택 내의 각 리소스는 스택 내에서 만들어진 다른 리소스들과 비슷하게 태그될 것이기 때문이다.

- You can estimate the costs of your resources using the CloudFormation template
→ CloudFormation 템플릿을 사용하여 리소스 비용을 쉽게 예측할 수 있다.

- Savings strategy: In Dev, you could automation deletion of templates at 5 PM and recreated at 8 AM, safely
→ CloudFormation으로 절약 전략을 세울 수 있다. 예를 들어 어떤 환경에서 오후 5시에 자동으로 모든 템플릿을 삭제했다가 오전 8시에 다시 생성하도록 설정할 수 있다. 리소스가 없는 시간 동안의 비용을 절감할 수 있다.
~~~

- Productivity
~~~
- Ability to destroy and re-create an infrastructure on the cloud on the fly
→ 인프라를 그때 그때 파괴하고 다시 생성할 수 있다.

- Automated generation of Diagram for your templates!
→ 템플릿을 위한 다이어그램을 만들 수 있다.

- Declarative programming (no need to figure out ordering and orchestration)
→ 선언적 프로그래밍이 가능하다. 따라서 EC2 인스턴스에서 DynamoDB 테이블을 먼저 만들어야 하는지, 한 번에 같이 해야 하는지 알 필요가 없다.
~~~

- Don’t re-invent the wheel
~~~
- Leverage existing templates on the web!
→ CloudFormation을 사용하면 하나하나 다 만들지 않아도 괜찮다. 그냥 웹에 존재하는 기존 템플릿을 활용하면 된다.

- Leverage the documentation
→ 문서를 활용할 수 있다. 
~~~

- Supports (almost) all AWS resources:
~~~
- Everything we’ll see in this course is supported
→ CloudFormation은 거의 모든 AWS 리소스를 지원한다.

- You can use “custom resources” for resources that are not supported
→ 지원되지 않는 리소스에 대해서는 사용자 지정 리소스라는 것을 사용할 수 있다.
~~~

### 3. CloudFormation Stack Designer
- `CloudFormation`을 시각적으로 볼 수 있다. 만들어진 것을 다이어그램으로 확인할 수 있는 것이다.

- Example: WordPress CloudFormation Stack  
→ 온라인의 `WordPress CloudFormation Stack`에서 `Designer`에서 확인하면 다음과 같은 다이어그램이 나온다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/17700a22-22ed-4a1a-961e-67150eee31c1)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- We can see all the resources  
→ 모든 리소스와 그 관계를 확인할 수 있다.

- We can see the relations between the components  
→ 시험 관점에서 보면, `CloudFormation`은 코드형 인프라가 있을 때 사용하게 된다. 아키텍처를 다른 환경, 다른 리전, 다른 `AWS` 계정에서 반복해야한다면 굉장히 편리하다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---