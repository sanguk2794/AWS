## IAM
### 1. IAM
- IAM = Identity and Access Management, Global service  
→ `IAM`은 리전 선택이 필요하지 않은 글로벌 서비스이다. 

### 2. IAM: Users & Groups
- Root account created by default, shouldn’t be used or shared  
→ 기본으로 생성되는 루트 계정은 모든 권한을 가지고 있기 때문에 위험하다. 그렇기에 이 루트 계정은 오직 계정을 생성할 때에만 사용해야 한다. 

- Users are people within your organization, and can be grouped  
→ 하나의 유저는 조직 내 한 사람에 해당된다. 필요하다면 유저들을 그룹으로 묶을 수 있다.

- Groups only contain users, not other groups  
→ 그룹에는 오직 유저만 배치할 수 있다. 서브 그룹의 설정이 불가능하다.

- Users don’t have to belong to a group, and user can belong to multiple groups  
→ 그룹에 포함되지 않은 유저를 생성할 수 있다. 단, 이는 별로 추천되지 않는다. 이와 반대로 한 사용자를 여러 그룹에 포함시키는 것은 가능하다.

![Group and user](https://user-images.githubusercontent.com/97398071/228144081-a46cf07b-47d2-4019-a7a9-2065a8a8364b.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 1. Create Users and Groups
- `Access management` -> `Users` -> `Add users` 버튼을 클릭한다.

![Add users](https://user-images.githubusercontent.com/97398071/228142985-dc012d60-9730-4da3-8c48-5da10ee22b25.png)

- `Set user details`  
→ 유저명과 패스워드를 설정한다.

![image](https://user-images.githubusercontent.com/97398071/228145516-7a267064-c6c6-4325-88d9-64122230a15a.png)

- `Set permission`  
→ 권한을 설정한다. 유저를 그룹에 포함시킬 수 있다.

![Set permission](https://user-images.githubusercontent.com/97398071/228145474-d34fabc5-be79-44cc-a982-14fa698c3618.png)

- 이를 위해 그룹을 생성한다. 만약 그룹을 먼저 생성했다면 유저를 해당 그룹에 포함시킬 수 있다.

- `Add tags(optional)`  
→ `AWS`에서는 어디에서도 태그를 사용할 수 있는데, 유저의 접근을 추적, 제어할 수 있도록 도와주는 정보이다.
`Key`와 `Value`의 쌍으로 이루어져 있으며 `Department:Engineering`과 같은 형식으로 부서를 기입해 둔다거나 필요한 다른 정보를 기입하는 데 사용한다.
그룹, 계정 말고도 태그를 기입할 수 있는 곳은 많기 때문에 그냥 추가적인 정보를 기입하는 곳이라고 생각하면 된다.

- `[userName]_credentials.csv` 다운로드  
→ 유저의 자격 증명 정보가 들어있다. 타인을 위해 유저를 생성한 경우에는 특정 메일 주소로 유저 정보를 보낼 수 있다.

- `Register account alias`  
→ 대시보드에서 AWS 계정 별칭을 변경한다. 이후 유저 로그인을 위한 로그인 `URL`을 복사할 수 있다.

![Register account alias](https://user-images.githubusercontent.com/97398071/228144957-38df41f5-9dbd-4d47-bc2b-f4f22350aec3.png)

### 3. IAM: Permissions
- `User`와 `Group`을 생성하는 이유는 다른 인원들이 허가받은 범위 내의 `AWS` 서비스를 이용할 수 있도록 허용하기 위해서이다. 서비스를 사용할 것을 허용하려면 이들에게 권한을 부여해야 한다.

- Users or Groups can be assigned JSON documents called policies. These policies define the permissions of the users  
→ `IAM`에서는 `Policies`, 또는 `IAM Policies`라고 불리는 `JSON` 타입의 문서를 통해 권한을 지정한다. 이 파일은 프로그래머가 아니더라도 이해할 수 있도록 이해하기 쉬운 영어로 구성되어 있다.

- In AWS you apply the Least privilege principle: don’t give more permissions than a user needs  
→ `AWS`는 `Least privilege principle, 최소 권한의 원칙`을 적용한다. 유저가 꼭 필요한 것 이상의 권한을 주지 않는 것이다.
유저가 허가받지 않은 서비스를 사용할 수 있도록 허용한다면 유저가 너무 많은 서비스를 실행하여 큰 비용이 발생하거나 보안 문제를 야기할 수 있다.

#### 1. Policies
- `AWS`에서는 정책을 통해 권한을 부여한다.

![IAM Policies inheritance](https://user-images.githubusercontent.com/97398071/228144281-e39737c0-54d4-4538-b92d-34bd81f4019c.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 유저 개개인에게 권한을 적용하기 위해 사용되는 정책을 `Inline Policy`라고 한다. 유저가 그룹에 속해있던 속해있지 않던 `Inline Policy`는 적용이 가능하다.

- 정책의 구조와 정책 명명법에 대해서는 잘 익혀둬야 한다. `AWS`에서 자주보게 되는 형식이기 때문이다. `JSON` 형식으로 이루어져 있다.
~~~
1. Version : policy language version, always include "2012-10-17"
→ 정책 언어의 버전이다. 보통은 `2012-10-17`이다.

2. Id : an identifier for the policy (optional)
→ 정책을 식별하기 위한 식별자이다. 선택 사항이다.

3. Statement : one or more individual statements (required)
→ Statement(문장)는 하나일 수도 있고 여러개일 수도 있다. 이 문장에는 아주 중요한 부분들이 있다.

3-1. Sid : an identifier for the statement (optional)
→ 문장 ID이다. 문장의 식별자로 사용되며, 선택 사항이다.

3-2. Effect : whether the statement allows or denies access (Allow, Deny)
→ 문장이 특정 API에 접근하는 것을 허용할 것인지, 거부할 것인지 선언할 때 사용한다.

3-3. Principle : account/user/role to which this policy applied to
→ 특정 정책이 적용될 유저, 계정 또는 역할로 구성된다.

3-4. Action : list of actions the policy allows or denies
→ Effect에 기반해 허용 및 거부할 API 목록을 설정한다.

3-5. Resource : list of resources to witch the actions applied to
→ 적용될 Action의 리소스 목록이다.

3-6. Condition : conditions for when this policy is in effect (optional)
→ Statement가 언제 적용될지를 결정한다. 선택 사항이다.
~~~

- 해당 내용은 `EC2 Describe`, `Elastic Load Balancing Describe`, `CloudWatch Describe`를 허용하고 있다.
~~~ JSON
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:Describe*",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "elasticloadbalancing:Describe*",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:ListMetrics",
                "cloudwatch:GetMetricStatistics",
                "cloudwatch:Describe*"
            ],
            "Resource": "*"
        }
    ]
}
~~~

### 4. IAM User interface
- `Users` → `Permissions` → `Permissions policies`  
→ 권한을 부여할 수 있다.

![Permissions policies](https://user-images.githubusercontent.com/97398071/228144568-73179fcb-caf7-4616-ad0c-bac70254dda1.png)

- `Policies`  
→ `AWS`의 모든 정책이 있다. 권한의 상세 정보를 확인할 수 있으며, `Create policy` 버튼을 눌러 새로운 정책을 생성할 수 있다.

![Policies](https://user-images.githubusercontent.com/97398071/228144650-c22ae1b6-33a5-45e0-8876-104e56f92768.png)

- 관리자 계정 권한으로 설정한 `AdministratorAccess`는 모든 작업과 리소스가 허용된 상태임을 `JSON` 설정을 통해 확인할 수 있다.

![Policies details](https://user-images.githubusercontent.com/97398071/228144725-228ecb56-bc16-4300-8b11-a229dfd251ff.png)

### 5. IAM – Account protection 
#### 1. Password Policy
- `AWS`에는 유저의 정보가 침해당하지 않도록 보호하기 위한 여러 방어 메커니즘들이 있으며, 그 중 하나는 비밀번호 정책을 사용하는 것이다. 비밀번호 정책을 이용하면 계정에 대한 치명적인 공격을 막아내는데 큰 도움이 된다.

- Strong passwords = higher security for your account  
→ 비밀번호가 강력해질수록 계정의 보안이 철저해진다. 

- In AWS, you can setup a password policy   
→ `AWS`에서는 다양한 옵션을 이용해 비밀번호 정책을 설정할 수 있다.
~~~
- Set a minimum password length 

- Require specific character types
→ including uppercase letters, lowercase letters, numbers, non-alphanumeric characters

- Allow all IAM users to change their own passwords
→ 사용자들의 비밀번호 변경을 허용 또는 금지할 수 있다.

- Require users to change their password after some time (password expiration)
→ 일정 시간이 지나면 비밀번호를 만료시켜 새로운 비밀번호를 설정할 것을 요구할 수 있다.

- Prevent password re-use
→ 이전에 사용했던 비밀번호의 재사용을 불가능하게 설정할 수 있다.
~~~

- `Account settings` → `Change password policy`에서 패스워드 정책을 설정할 수 있다.

![Set password policy button](https://user-images.githubusercontent.com/97398071/228145152-83c4ce02-3d23-4989-b451-e20c17b1a681.png)

#### 2. Multi-Factor Authentication - MFA
- 다요소 인증은 패스워드 정책과 함께 `AWS`가 제공하는 계정 보호를 위한 방어 메커니즘 중 하나이다. `AWS`에서는 `MFA`를 필수적으로 사용할 것을 권장한다.

- MFA = password you know + security device you own  
→ 비밀번호와 함께 보안장치를 사용하도록 강제하는 것이다. 그냥 비밀번호보다 훨씬 안전하다.

- if a password is stolen or hacked, the account is not compromised  
→ 해킹을 당해 비밀번호가 누출되었다고 하더라도 물리적 장치가 없으므로 계정이 침해당하지 않는다.

- MFA devices options in AWS
###### 1. Virtual MFA device → Google Authenticator, Authy etc...
~~~
- Google Authenticator 
→ 하나의 휴대전화에서만 사용이 가능하다.

- Authy 
→ 여러 장치에서 사용이 가능하다. 또, 하나의 장치에서도 토큰을 여러 개 지원한다. 즉, 루트 계정, 유저 계정, 또 다른 유저 계정들을 원하는 수만큼 등록할 수 있다.
~~~

###### 2. Universal 2nd Factor (U2F) Security Key → YubiKey by Yubico (3rd party)
- Yubico is support for multiple root and IAM users using a single security key  
→ 물리적인 키이며, 하나의 보안 키로 여러 루트 계정과 IAM 유저 계정을 지원하기 때문에 하나의 키로 충분하다.

![MFA device_1](https://user-images.githubusercontent.com/97398071/228144495-46f70ffd-491f-4cea-9c44-e03ef21fec08.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

###### 3. Hardware Key Fob MFA Device → Provided by Gemalto (3rd party)  
- 물리적인 키이다.

###### 4. Hardware Key Fob MFA Device for AWS GovCloud (US) → Provided by SurePassID (3rd party)  
- 물리적인 키이다. 만약 미국 정부의 클라우드인 `AWS GovCloud`를 사용하는 경우에 필요한 특수한 키 팝이다.

![MFA device_2](https://user-images.githubusercontent.com/97398071/228144542-506029f5-6893-49bb-9d08-2d31662a8623.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- `My Security Cridential` → `Multi-factor authentification (MFA)` → `Activate MFA`  
→ `MFA` 설정이 가능하다. 만약 가상 `MFA` 장치를 활용한다면 안드로이드와 아이폰에서 사용 가능한 `Virtual MFA Application`의 리스트를 확인한 후
하나의 애플리케이션을 선택하면 된다.

### 6. IAM - Roles for Services
- Some AWS service will need to perform actions on your behalf  
→ 몇몇 `AWS` 서비스는 사용자를 대신하여 작업을 수행해야 한다.

- To do so, we will assign permissions to AWS services with IAM Roles  
→ 이를 위해서는 `AWS` 서비스에 권한을 부여해야 한다.

- 이 때 사용하는 것이 `IAM Role`이다. `IAM Role`은 실제 유저가 아닌 `AWS` 서비스에 의해 사용되도록 만들어져 있다.
예를 들어 `EC2 Instance` 서비스에서 `AWS`의 다른 어떤 서비스에 접근하기 위해 부여하는 것이 `IAM Role`이다.

- Common roles: EC2 Instance Roles, Lambda Function Roles, Roles for CloudFormation

#### 1. Create Role
- `IAM` → `Role` → `Create role`

![Create role button](https://user-images.githubusercontent.com/97398071/228143895-7717112c-cc55-48ee-b05c-c3f9b23edf33.png)

- `Select trusted entity` → `Use case`  
→ 신뢰할 수 있는 객체 유형을 선택한다. 여러 유형이 있는데, 대표적으로 `AWS Service`, `AWS Account`, `Web identity` 등이 있다.
이 중 가장 중요한 것은 `AWS Service Role`을 생성할 수 있다는 것이다. 
`AWS Service Role` 생성의 가장 흔한 활용 사례는 `EC2 instance` 또는 `Lambda function`에 대한 `Role`을 생성하는 것이다.

![AWS service role](https://user-images.githubusercontent.com/97398071/228143072-888db893-22a5-4ede-909a-65b01778c218.png)

### 7. IAM Security Tools
- IAM Credentials Report (account-level) 
~~~
- a report that lists all your account's users and the status of their various credentials
→ IAM 자격 증명 보고서를 생성할 수 있다. 보고서는 계정에 있는 유저와 다양한 자격 증명의 상태를 포함한다.
~~~

- IAM Access Advisor (user-level)
~~~
- Access advisor shows the service permissions granted to a user and when those services were last accessed.
→ IAM 엑세스 관리자를 통해 유저에게 부여된 서비스 권한과 해당 서비스에 마지막으로 액세스한 시간을 확인할 수 있다.

- You can use this information to revise your policies.
→ 해당 기능을 통해 어떤 권한을 사용하지 않는지 알 수 있다. 액세스 관리자를 확인해 권한을 조정하면 최소 권한의 원칙을 지킬 수 있다.
~~~

#### 1. Download Credential Report
- `IAM` → `Credential report` → `Download report` 버튼을 눌러 보고서를 다운로드 가능하다.  
→ 해당 파일은 `CSV` 파일 형식이며 사용자가 언제 생성되었는지, 비밀번호가 활성화되었는지, 비밀번호를 마지막으로 언제 사용했는지,
마지막으로 언제 변경되었는지, 비밀번호 변경 주기를 활성화한 경우 다음 주기는 언제인지, `MFA`가 활성화되었는지, 
액세스 키가 설정되었는지 등을 확인할 수 있다. 보안 측면에서 어떤 사용자를 주목해야 할지 확인이 가능하다.

![Download credential report](https://user-images.githubusercontent.com/97398071/228143971-cdc3bf09-aa88-418f-81ed-51731bb1ed9b.png)

#### 2. Get Access Advisor Information
- `IAM` → `Users` → `Detail` → `Access Advisor`

![Access Advisor](https://user-images.githubusercontent.com/97398071/228142600-048fe641-cc00-4d8c-b479-764b94ed6749.png) 

- `AWS Health APIs and Notifications`  
→ 개인화된 헬스 대시보드는 계정이 어떤 알림이 있는지 확인하는 서비스이며 로그인 시 자동으로 호출된다. 

### 8. IAM Guidelines & Best Practices
- Don’t use the root account except for AWS account setup
- One physical user = One AWS user
- Assign users to groups and assign permissions to groups
- Create a strong password policy
- Use and enforce the use of Multi Factor Authentication (MFA)
- Create and use Roles for giving permissions to AWS services
- Use Access Keys for Programmatic Access (CLI / SDK)
- Audit permissions of your account with the IAM Credentials Report
- Never share IAM users & Access Keys

### 9. IAM Section – Summary
- Users → mapped to a physical user, has a password for AWS Console
- Groups → contains users only 
- Policies → JSON document that outlines permissions for users or groups
- Roles → for EC2 instances or AWS services
- Security → MFA + Password Policy
- Access Keys → access AWS using the CLI or SDK
- Audit → IAM Credential Reports & IAM Access Advisor

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
