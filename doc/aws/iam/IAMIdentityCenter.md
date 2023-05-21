## AWS IAM Identity Center
### 1. AWS IAM Identity Center (successor to AWS Single Sign-On)
- One login (single sign-on) for all your
~~~
- AWS accounts in AWS Organizations
→ AWS Organizations 내의 계정

- Business cloud applications (e.g., Salesforce, Box, Microsoft 365, …)
→ 비즈니스 클라우드 애플리케이션 내의 모든 AWS 계정에서 싱글 로그인이 가능하다.

- SAML2.0-enabled applications
→ SAML2.0 활성 애플리케이션

- EC2 Windows Instances
→ EC2 Windows 인스턴스에 대해서도 싱글 로그인을 제공한다.

- 한 번만 로그인하면 모든 것에 액세스할 수 있다는 건 정말 정말 편리하다. 시험에서 나온다면 여러 AWS 계정을 한 번에 로그인하는 것에 대해 출제된다.
~~~

- Identity providers
~~~
- 싱글 사인온을 위한 사용자 정보는 두 위치에 저장될 수 있다.

- Built-in identity store in IAM Identity Center
→ IAM 자격 증명 센터에 내장된 ID 저장소

- 3rd party: Active Directory (AD), OneLogin, Okta…
→ Active Directory (AD), OneLogin, Okta 등 서드파티 ID 공급자
~~~

- `ID` 저장소가 `SAML2.0`과 호환되지 않는 경우 사용자 지정 `ID` 브로커 애플리케이션을 구축하여 유사한 기능을 수행할 수 있다.
→ 온프레미스 사용자 지정 자격 증명 브로커 애플리케이션을 개발하고 `STS`를 사용하여 단기 `AWS` 자격 증명을 발급할 수 있다.

### 2. AWS IAM Identity Center
- `AWS IAM` 자격 증명 센터의 로그인 페이지에 접속하고 로그인을 수행한다.  
→ 이 때 `ID` 센터는 `AWS` 조직이나 `Windows EC2` 인스턴스, 비즈니스 클라우드 애플리케이션, 사용자 정의 `SAML2.0` 지원 애플리케이션 등과 통합된 상태여야 한다.

- 로그인을 하는 것으로 모든 항목에 액세스가 가능해지는 것은 아니다. 자격 증명 센터에서 권한 셋을 정의해서 어떤 사용자가 무엇에 액세스할 수 있는지 정의할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/236677616-7978e0c6-b523-4ae4-88e4-7f0364b99228.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. AWS IAM Identity Center - Fine-grained Permissions and Assignments
- Multi-Account Permissions
~~~
- Manage access across AWS accounts in your AWS Organization
→ 다중 계정 권한을 사용하면 조직에서 여러 계정에 대한 액세스를 관리할 수 있다.

- Permission Sets – a collection of one or more IAM Policies assigned to users and groups to define AWS access
→ 권한 셋을 사용하여 사용자를 그룹에 할당하는 하나 이상의 IAM 정책을 정의한다.

- IAM 자격 증명 센터를 통해 로그인해서 Dev 계정 또는 Prod 계정의 콘솔에 액세스할 수 있도록 다른 계정의 IAM 역할을 위임할 수 있다.
~~~

![image](https://user-images.githubusercontent.com/97398071/236677977-398ddf72-b33c-453d-898e-c48fcf4cc504.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Application Assignments
~~~
- SSO access to many SAML 2.0 business applications (Salesforce, Box, Microsoft 365, …)
→ 어떤 사용자 또는 그룹이 어떤 애플리케이션에 액세스할 수 있는지 정의할 수 있다.

- Provide required URLs, certificates, and metadata
→ 필요한 URL, 인증서, 메타데이터가 제공된다.
~~~

- Attribute-Based Access Control (ABAC)
~~~
- Fine-grained permissions based on users’ attributes stored in IAM Identity Center Identity Store
→ 사용자는 IAM 자격 증명 센터 스토어에 저장된 사용자 속성을 기반으로 세분화된 권한을 갖게 된다. 이를 가능하게 하는 것을 속성 기반 액세스 제어이다.

- Example: cost center, title, locale, …
→ 이를 사용하여 사용자를 비용 센터에 할당하거나, 주니어나 시니어와 같은 직함을 주거나 로케일을 주는 식으로 설정해 특정 리전에서만 액세스하도록 할 수 있다.

- Use case: Define permissions once, then modify AWS access by changing the attributes
→ 사용 사례는 IAM 권한 셋을 한 번만 정의하고 이 속성을 활용하여 사용자 또는 그룹의 AWS 액세스만 수정하는 것이다.
~~~

#### 1. 권한 세트
권한 집합은 하나 이상의 `IAM` 정책 컬렉션을 정의하는 사용자가 만들고 유지 관리하는 템플릿이다. 권한 집합을 사용하면 조직 내 사용자 및 그룹에 대한 `AWS` 계정 액세스 권한을 간단하게 할당할 수 있다. 예를 들어 `AWS RDS`, `DynamoDB` 및 `Aurora` 서비스를 관리하기 위한 정책이 포함된 `Database Admin` 권한 집합을 생성하고 이 단일 권한 집합을 사용하여 데이터베이스 관리자에게 `AWS` 조직 내 대상 `AWS` 계정 목록에 대한 액세스 권한을 부여할 수 있다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
