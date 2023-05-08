## SSM Parameter Store
### 1. SSM Parameter Store
- Secure storage for configuration and secrets  
→ `AWS Systems Manager`의 기능인 `Parameter Store`는 구성 및 암호 관리를 위한 계층적 스토리지이다. `Parameter Store`에는 암호, 데이터베이스 문자열, `AMI` `ID`, 라이선스 코드와 같은 데이터를 파라미터 값으로 저장할 수 있다.

- Optional Seamless Encryption using KMS  
→ `KMS`를 이용해 구성을 암호화할지 선택할 수 있다.

- Serverless, scalable, durable, easy SDK  
→ 서버리스이며 확장과 내구성이 뛰어나다. 또, `SDK`를 사용하기 쉽다.

- Version tracking of configurations / secrets  
→ 파라미터를 업데이트할 때 구성과 암호의 버전을 추적할 수 있다.

- Security through IAM  
→ `IAM`을 통해 보안이 제공된다.

- Notifications with Amazon EventBridge  
→ `Amazon EventBridge`로 알람을 받을 수도 있다.

- Integration with CloudFormation  
→ `CloudFormation`과 통합이 가능하다. `CloudFormation`이 `Parameter Store`의 파라미터를 스택의 입력 파라미터로 활용할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/236845686-7f6b0f41-6122-4ac8-ad96-3d8e7438dd7d.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- `CLI`, `Lambda`를 통해 계층 구조의 파라미터에 접근할 수 있다.

### 2. SSM Parameter Store Hierarchy
- `Parameter Store`에 계층 구조의 파라미터를 저장할 수 있다. 원하는 방식으로 파라미터를 구조화할 수 있는 것이다.
- 구조화를 통해 `IAM` 정책을 간소화하여 애플리케이션이 `/my-department/` 혹은` my-app/` 등 특정 경로에만 액세스할 수 있도록 설정할 수 있다.
- `AWS`에서 발행하는 퍼블릭 파라미터를 사용할 수 있다. 예를 들어 특정 리전에서 `Amazon Linux 2`의 최신 `AMI`를 찾으려 할 때 `Parameter Store`를 `API` 호출을 대신해 사용할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/236845897-fc80762d-06cb-40fa-ba07-23c06b160ada.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. Parameters Policies (for advanced parameters)
- `Parameters Policies`는 `advanced parameters`에서만 사용할 수 있다.

- Allow to assign a TTL to a parameter (expiration date) to force updating or deleting sensitive data such as passwords  
→ 파라미터 정책을 통해 만료기간을 뜻하는 `TTL`을 파라미터에 할당할 수 있다. 사용자가 비밀번호 등의 민감한 정보를 업데이트 또는 삭제하도록 강제한다.

- Can assign multiple policies at a time  
→ 한 번에 여러 정책을 할당할 수 있다.

- 파라미터를 삭제하는 만료 정책의 시나리오는 다음과 같다.
~~~
- 타임스탬프에서 파라미터를 반드시 삭제해야 한다고 가정할 때, 파라미터가 만료되기 15일 전에 알람을 받도록 설정할 수 있다.
- 이를 위해 EventBridge와 통합한다. 그러면 EventBridge에서 알림을 받을 수 있다. 
- 가끔 파라미터를 변경하고 싶을 수 있다. 이를 위해 20일간 파라미터에 업데이트가 없으면 알림을 받도록 설정할 수 있다.
~~~

![image](https://user-images.githubusercontent.com/97398071/236848585-d914ccfb-b396-477b-995f-a4555532b8fc.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---