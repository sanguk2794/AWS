## IAM Permission Boundaries
### 1. IAM Permission Boundaries
- IAM Permission Boundaries are supported for users and roles (not groups)  
→ `Users`와 `Roles`만 지원하고 그룹은 지원하지 않는다.

- Advanced feature to use a managed policy to set the maximum permissions an IAM entity can get.  
→ `IAM Entity`의 최대 권한을 정의하는 고급 기능이다.

- 아래의 예제에서는 유저 생성에 대한 권한을 `IAM` 정책을 통해 부여받았음에도 불구하고 유저 생성이 불가능하다.  
→ 권한 경계에서 `IAM`에 대한 접근을 허용하지 않기 때문에 제한이 생기는 것이다.

![image](https://user-images.githubusercontent.com/97398071/236675057-a6259abc-88f3-4126-96a0-b2c0ef06f4c9.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Can be used in combinations of AWS Organizations SCP  
→ `IAM` 권한 경계는 `AWS Organizations SCP`와도 함께 사용할 수 있다. 

- 사용자에게 주어지는 권한은 `IAM Permission Boundaries`, `Identity-based policy`, `Organizations SCP`의 교집합이 된다.

- Use cases
~~~
- Delegate responsibilities to non administrators within their permission boundaries, for example create new IAM users
→ 관리자가 아닌 사용자에게 책임을 위임하기 위해 권한 경계 내에서 새 IAM 사용자를 생성

- Allow developers to self-assign policies and manage their own permissions, while making sure they can’t “escalate” their privileges (= make themselves admin)
→ 개발자가 권한을 스스로 부여하고 관리하는 상황에서 특권을 남용하는 것을 막기 위해 스스로를 관리자로 만들지 못 하게 설정

- Useful to restrict one specific user (instead of a whole account using Organizations & SCP)
→ 계정에 SCP를 적용해서 계정의 모든 사용자를 제한하지 않고 AWS Organizations의 특정 사용자만 제한
~~~

![image](https://user-images.githubusercontent.com/97398071/236675105-40191288-4c57-48e4-97c6-bcb0f3b5f03f.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. IAM Policy Evaluation Logic
- `IAM` 정책 평가 논리이다. 외울 필요는 없고 이해만 할 정도면 충분하다.
- 전체 흐름의 각 단계마다 평가가 이루어진다. 평가 중 명시적인 거부가 있다면 자동으로 거부된다. 모든 방면에서 거부되지 않고 허용됐을 때만 액세스가 성공한다.

![image](https://user-images.githubusercontent.com/97398071/236675589-77664833-bb35-428e-aa8e-0eaf3fce0c10.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. Example IAM Policy
- Can you perform sqs:CreateQueue?  
→ `sqs:CreateQueue`를 실행할 수 없다.

- Can you perform sqs:DeleteQueue?  
→ `sqs:DeleteQueue`를 실행할 수 없다. 명시적 거부가 하나라도 있으면 바로 결정이 거부된다.

- Can you perform ec2:DescribeInstances?  
→ `ec2:DescribeInstances`를 실행할 수 없다. 명시적 거부도 없지만 명시적 허용도 없기 때문에 해당 `IAM` 정책 내에서의 `ec2:DescribeInstances` 실행이 거부된다.

![image](https://user-images.githubusercontent.com/97398071/236675742-7253732b-700a-4859-886a-0b85f36e21b8.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---