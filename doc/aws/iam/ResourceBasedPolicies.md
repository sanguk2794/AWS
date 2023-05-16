## Resource Based Policies
### 1. IAM Roles vs Resource Based Policies
- Cross account:
~~~
- 교차 계정이 API 호출을 수행할 수 있도록 설정하기 위한 방법에는 두 가지가 있다.

- attaching a resource-based policy to a resource (example: S3 bucket policy)
→ 리소스에 resource-based policy를 추가한다. S3 버킷 정책을 통해 사용자의 액세스를 허용한다.

- OR using a role as a proxy
→ 리소스에 액세스할 수 있도록 역할을 설정한다. IAM 역할을 통해 S3에 액세스를 수행한다.
~~~

![image](https://user-images.githubusercontent.com/97398071/236673573-d3ec0664-b390-4057-87a7-9962eaeab5c5.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- When you assume a role (user, application or service), you give up your original permissions and take the permissions assigned to the role  
→ 역할을 맡으면 기존의 권한을 모두 포기하고 해당 역할에 할당된 권한을 상속받는다.

- When using a resource-based policy, the principal doesn’t have to give up his permissions  
→ 리소스 기반 정책을 사용하면 본인이 역할을 맡지 않으므로 권한을 포기할 필요가 없다.

- Supported by: Amazon S3 buckets, SNS topics, SQS queues, etc…  
→ 리소스 기반 정책을 지원하는 `AWS` 서비스와 리소스가 점점 늘어나고 있다. 현재는 `Amazon S3` 버킷, `SNS`, `SQS`, `Lambda` 함수 등에서 설정 가능하다.

### 2. Amazon EventBridge – Security
- 리소스 기반 정책과 `IAM` 역할 설정의 차이는 `Amazon EventBridge`에서 사용할 때 가장 크게 드러난다.

- When a rule runs, it needs permissions on the target  
→ `Amazon EventBridge`는 작업을 수행할 때 대상에 대한 권한을 요구하는 규칙이 있다.

- Resource-based policy: Lambda, SNS, SQS, CloudWatch Logs, API Gateway…  
→ 리소스 기반 정책을 지원하는 대상에는 `Lambda`, `SNS`, `SQS`, `CloudWatch Logs`, `API Gateway` 등이 있다. 이 경우에는 대상 리소스의 리소스 기반 규칙을 설정해 `EventBridge`가 원하는 작업을 수행할 수 있도록 허용해야 한다.

- IAM role: Kinesis stream, Systems Manager Run Command, ECS task…  
→ `Kinesis Data Streams`, `Systems Manager Run Command`, `ECS` 태스크 등을 시작할 때에는 `IAM` 역할 설정이 필요하다.

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
