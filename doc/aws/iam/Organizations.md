## AWS Organizations
### 1. AWS Organizations
- Global service  
→ 글로벌 서비스이다.

- Allows to manage multiple AWS accounts  
→ 다수의 `AWS` 계정을 동시에 관리할 수 있게 해 준다.

- The main account is the management account  
→ 조직을 생성하면 조직의 메인 계정이 관리 계정이 된다.

- Other accounts are member accounts  
→ 조직에 가입한 기타 계정이나 조직에서 생성한 계정을 멤버 계정이라고 부른다.

- Member accounts can only be part of one organization  
→ 멤버 계정은 한 조직에만 소속된다.

- Consolidated Billing across all accounts - single payment method  
→ `Organizations`를 통해 계정의 비용을 통합해 결제할 수 있다. 관리 계정에 하나의 지불 방법만 설정해 두면 조직 전체의 비용을 지불할 수 있다.

- Pricing benefits from aggregated usage (volume discount for EC2, S3…)  
→ `Organizations`를 사용하면 조직 내 모든 계정에 대해 집계된 사용량에 기반한 비용 할인을 받을 수 있다. 모든 계정에서 `EC2` 또는 `Amazon S3`를 많이 사용한다면 모든 계정의 사용량이 합쳐져서 계산되므로 큰 할인 혜택을 받을 수 있다.

- Shared reserved instances and Savings Plans discounts across accounts  
→ 계정 간 `Reserved instance`와 `Savings plans`가 공유된다. 어떤 계정에서 사용하지 않는 예약 인스턴스가 있을 때 다른 계정이 해당 인스턴스를 사용할 수 있다. 중요하다.

- API is available to automate AWS account creation  
→ `Organizations`에는 계정 생성을 자동화할 수 있는 `API`가 있어 계정을 쉽게 생성할 수 있다.

- `루트 조직 단위, Root Organizational Unit(OU)`라는 개념이 존재한다. 이 안에 관리 계정이 있고, 관리 계정은 서브 `OU`를 생성할 수 있다. 서브 `OU` 내에 또 다른 서브 `OU`를 생성하는 것도 가능하다.

- 서브 유저가 `IAM` 루트 계정이라고 하더라도 접근을 제어할 수 있다. 예를 들어 루트 계정인 서브 유저의 `S3` 접근을 제한할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/236652907-6a1b0e96-eded-4834-9839-724589479597.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Advantages
~~~
- Multi Account vs One Account Multi VPC
→ 다수의 계정을 가지므로 다수의 VPC를 가진 단일 계정에 비해 보안이 뛰어나다. 계정이 VPC보다 독립적이기 때문이다.

- Use tagging standards for billing purposes
→ 청구 목적의 태그 기준을 적용할 수 있다.

- Enable CloudTrail on all accounts, send logs to central S3 account
→ 한 번에 모든 계정에 대해 CloudTrail을 활성화해서 모든 로그를 중앙 S3 계정으로 전송할 수 있다.

- Send CloudWatch Logs to central logging account
→ CloudWatch Logs를 중앙 로그 계정으로 전송할 수 있다.

- Establish Cross Account Roles for Admin purposes
→ 자동으로 관리 목적의 계정 간 역할을 수립할 수 있어 관리 계정에서 모든 멤버 계정을 관리할 수 있다.
~~~

- Security: Service Control Policies (SCP)
~~~
- 보안측에서도 큰 장점이 있다. Service Control Policies를 정의할 수 있기 때문이다.

- IAM policies applied to OU or Accounts to restrict Users and Roles
→ 이는 특정 OU 또는 계정에 적용되는 IAM 정책으로 해당 사용자와 역할 모두가 계정 내에서 할 수 있는 일을 제한할 수 있다.

- They do not apply to the management account (full admin power)
→ SCP는 모든 대상에 적용되나 영구적인 관리자 권한을 갖는 관리 계정에는 적용되지 않는다. 실수가 발생했을 때 누군가는 복구를 해야하기 때문이다.

- Must have an explicit allow (does not allow anything by default – like IAM)
→ SCP는 기본적으로 아무것도 허용하지 않는다. 구체적인 허용 항목을 설정해야 작동한다.
~~~

### 2. SCP Hierarchy

![image](https://user-images.githubusercontent.com/97398071/236653271-b61b8ff9-b9ee-4e9c-86ea-67c7ddffc6a4.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Management Account
~~~
- Can do anything (no SCP apply)
→ 일반적인 루트 OU에는 FullAWSAccess(전체 AWS 액세스) SCP가 적용된다. 모든 서비스를 사용할 수 있고, 모든 계정에 대한 전체 권한을 가진다.

- 관리 계정에 Athena 서비스 액세스를 제한하기 위해 DenyAccessAthena SCP를 적용하긴 했지만 관리 계정에는 SCP가 적용되지 않으므로 여전히 모든 것이 가능하다.
~~~

- Account A
~~~
- Can do anything
→ 계정 A는 루트 OU에 있으므로 많은 일을 할 수 있다. 이 예제에서는 Redshift 서비스 액세스 외의 모든 일을 할 수 있다.

- EXCEPT access Redshift (explicit Deny from OU)
→ 계정 A는 Prod OU에 속하기도 한다. Prod OU에 DenyRedshift SCP가 추가되어 있으므로, 계정 A는 Redshift에 액세스할 수 없다.

- AuthorizedRedshift SCP가 계정 A에 추가되어 있지만 정책에 명시적 거부가 하나라도 포함되면 기본적으로 액세스가 거부된다.
~~~

- Account B
~~~
- Can do anything
→ Redshift 및 Lambda 액세스 외의 모든 일을 할 수 있다.

- EXCEPT access Redshift (explicit Deny from Prod OU)
→ Prod OU로부터 DenyRedshift SCP를 상속한다.

- EXCEPT access Lambda (explicit Deny from HR OU)
→ HR OU로부터 DenyAWSLambda SCP를 상속한다.
~~~

- Account C
~~~
- Can do anything
→ Redshift 액세스 외의 모든 일이 가능하다.

- EXCEPT access Redshift (explicit Deny from Prod OU)
→ Prod OU로부터 DenyRedshift SCP를 상속한다.
~~~

### 3. SCP Examples - Blocklist and Allowlist strategies
- `SCP`에는 `차단 목록, Blocklist`과 `허용 목록, Allowlist`의 두 가지 전략이 있다.

- `Blocklist`는 계정이 사용하지 못하게 할 서비스를 지정한다. `Allow, *`로 모든 서비스의 모든 작업을 허용한 다음 `Deny` 문을 추가해 `DynamoDB`에 대한 액세스를 거부하는 식으로 사용한다.

- `Allowlist`는 계정이 사용할 수 있는 서비스만을 지정한다. 특정 `OU`나 계정에 `EC2`와 `CloudWatch`만을 허용하는 식으로 사용한다.

![image](https://user-images.githubusercontent.com/97398071/236653454-3376bf45-3ff3-4fa9-ab3e-cb1a6adc3b81.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---