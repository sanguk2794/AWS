## AWS Directory Services
### 1. What is Microsoft Active Directory (AD)?
- Found on any Windows Server with AD Domain Services  
→ `Microsoft AD`는 `AD` 도메인 서비스를 사용하는 모든 `Windows` 서버에 사용되는 소프트웨어이다. 

- 기본적으로 `AD`는 사용자가 마이크로소프트 IT 환경에서 업무를 수행하는 데 도움을 주는 데이터베이스이자 서비스 집합이다.

- Database of objects: User Accounts, Computers, Printers, File Shares, Security Groups  
→ `AD`는 객체의 데이터베이스이며 사용자 계정, 컴퓨터 프린터, 파일 공유, 보안 그룹이 객체가 될 수 있다. 전체 `Microsoft` 생태계에서 관리되는 온프레미스의 모든 사용자는 `Microsoft Active Directory`의 관리 대상이 된다.

- Centralized security management, create account, assign permissions  
→ 중앙 집중식 보안 관리로 계정 생성, 권한 할당 등의 작업이 가능하다.

- Objects are organized in trees  
→ 모든 객체는 트리로 구성된다.

- A group of trees is a forest  
→ 트리의 그룹을 포레스트라고 한다.

![image](https://user-images.githubusercontent.com/97398071/236678454-44f50a74-7518-4980-8def-aa89b1fae173.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. AWS Directory Services
- 세 가지 서비스가 있다. 깊게 볼 필요는 없지만 차이점은 이해해야 한다.

- AWS Managed Microsoft AD  
~~~
- Create your own AD in AWS, manage users locally, supports MFA
→ AWS에 자체 액티브 디렉토리를 생성하고 로컬에서 관리할 수 있으며 MFA를 지원한다. 

- Establish “trust” connections with your on- premises AD
→ 독립 실행형 Active Directory로 사용자가 있는 온프레미스 AD와 신뢰 관계를 구축할 수 있다. 

- AWS 관리형 AD의 인증된 사용자가 AWS 관리형이 아닌 계정을 사용할 때 온프레미스 AD에서 계정을 검색할 수 있다.
- 반대로 온프레미스 AD 사용자가 AWS 계정을 사용해 온프레미스 AD에서 인증하려 할 때도 신뢰 관계에 의거해 AWS AD에서 검색할 수 있다.
- 즉, 온프레미스와 AWS AD간에 사용자가 공유되는 셈이다.
~~~

- AD Connector  
~~~
- Directory Gateway (proxy) to redirect to on- premises AD, supports MFA
→ 디렉토리 게이트웨이로 온프레미스 AD에 리다이렉트한다. MFA를 지원한다.

- Users are managed on the on-premises AD
→ 사용자는 온프레미스 AD에서만 관리된다.
~~~

- Simple AD  
~~~
- AD-compatible managed directory on AWS
→ AD 호환 관리형 디렉토리는 Microsoft 디렉터리를 사용하지 않으며 온프레미스 AD와도 결합되지 않는다.

- Cannot be joined with on-premises AD
→ 온프레미스 AD가 없으나 AWS 클라우드에 액티브 디렉토리가 필요한 경우 독립형인 Simple AD 서비스를 사용한다.
~~~

- 온프레미스에서 사용자를 프록시한다면 `AD Connector`를 선택하면 된다.
- `AWS 클라우드`에서 사용자를 관리하고 `MFA`를 사용해야 할 때는 `AWS Managed Microsoft AD`를 선택하면 된다.
- 온프레미스가 없을 때는 `Simple AD`를 선택하면 된다.

![image](https://user-images.githubusercontent.com/97398071/236678504-f09e620e-1e1e-4c56-b6a3-beecd5727086.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. IAM Identity Center – Active Directory Setup
- `IAM Identity Center`와 `AD`를 통합할 수 있다.

- Connect to an AWS Managed Microsoft AD (Directory Service)
~~~
- Integration is out of the box
→ Directory 서비스를 통해 AWS 관리형 AD 연결한다면 IAM Identity Center를 AWS 관리형 Microsoft AD에 통합하고 연결하도록 설정하는 것으로 충분하다.

- 모든 AWS 계정을 다중 계정 설정으로 통합할 수 있다. Organizations를 사용하여 모든 기능이 켜진 새 조직을 생성하고 그 아래에 하위 계정을 생성해야 한다.
~~~

![image](https://user-images.githubusercontent.com/97398071/236678808-6ec3f21e-af72-43f0-8282-e0fb3de1efb8.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Connect to a Self-Managed Directory
~~~
- 온프레미스에 자체 관리형 디렉터리가 있는 경우의 연결 방법은 두 가지가 있다. 

- Create Two-way Trust Relationship using AWS Managed Microsoft AD
→ AWS 관리형 Microsoft AD를 사용해 양방향 신뢰 관계를 구축한 다음 IAM Identity Center를 통합한다. 클라우드에 있는 액티브 디렉터리에서 사용자를 관리하고 싶을 때 적합한 솔루션이다.

- Create an AD Connector
→ AD 커넥터를 사용해 프록시를 구축한 다음 IAM Identity Center를 통합한다. API 호출만 프록시해 사용하고 싶을 때 적합한 솔루션이다.
~~~

![image](https://user-images.githubusercontent.com/97398071/236678829-7198ca07-0a5f-431b-aa3d-92aeb08113df.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 4. Active Directory Federation Service
- `Federation`
→ 페더레이션은 신뢰가 설정된 도메인의 컬렉션이다. 신뢰 수준은 다른 경우도 있지만 일반적으로 인증 및 권한 부여가 포함된다.

- `Active Directory Federation Service`는 사용자 계정 및 응용 프로그램이 완전히 다른 네트워크나 조직에 있는 경우에도 완벽한 단일 프롬프트 액세스를 제공해주는 `ID` 액세스 솔루션이다.
- `Active Directory`에 저장된 온프레미스 자격 증명을 사용하여 두 환경의 리소스에 액세스하기를 원할 때 `Active Directory Federation Service`를 사용하여 `SAML 2.0` 기반 페더레이션을 설정해 사용할 수 있다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---