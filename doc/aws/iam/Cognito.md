## Amazon Cognito
### 1. Amazon Cognito
- Cognito = 인식

- Give users an identity to interact with our web or mobile application  
→ `Cognito`는 사용자에게 웹 및 모바일 앱과 상호 작용할 수 있는 자격 증명을 부여한다. 일반적으로 이 사용자들은 `AWS` 계정 외부에 존재한다. 모르는 사용자들에게 자격 증명을 부여해 사용자를 인식할 수 있다.

- `Cognito User Pools`, `Cognito Identity Pools` 두 종류의 하위 서비스가 있다.

- Cognito User Pools:
~~~
- Sign in functionality for app users
→ 앱 사용자에게 가입 기능을 제공하는 기능이다.

- Integrate with API Gateway & Application Load Balancer
→ API Gateway 및 애플리케이션 로드 밸런서와 원활히 통합된다.
~~~

- Cognito Identity Pools (Federated Identity):
~~~
- Provide AWS credentials to users so they can access AWS resources directly
→ 앱에 등록된 사용자에게 임시 AWS 자격 증명을 제공해서 일부 AWS 리소스에 직접 액세스할 수 있도록 한다.

- Integrate with Cognito User Pools as an identity provider
→ Cognito 사용자 풀과 원활히 통합된다.
~~~

- Cognito vs IAM: “hundreds of users”, ”mobile users”, “authenticate with SAML”  
→ `Cognito`의 사용자는 `AWS` 외부의 웹과 모바일 앱을 대상으로 한다. `수백 명의 사용자`, `모바일 사용자`, `SAML을 통한 인증`같은 키워드가 나오면 `Cognito`를 설명하는 것이다.

### 2. Cognito User Pools (CUP) – User Features
- Create a serverless database of user for your web & mobile apps  
→ 웹 및 모바일 앱을 대상으로 하는 서버리스 사용자 데이터베이스이다.

- Simple login: Username (or email) / password combination  
→ 사용자 이름 또는 이메일과 비밀번호의 조합으로 간단한 로그인 절차를 정의할 수 있다.

- Password reset  
→ 비밀번호 재설정이 가능하다.

- Email & Phone Number Verification  
→ 이메일 및 전화번호 검증이 가능하다.

- Multi-factor authentication (MFA)  
→ `MFA`를 설정 가능하다.

- Federated Identities: users from Facebook, Google, SAML…  
→ `Facebook`, `Google`과 통합할 수 있어 소셜 로그인도 가능하다.

### 3. Cognito User Pools (CUP) - Integrations
- CUP integrates with API Gateway and Application Load Balancer  
→ `Cognito User Pools`는 `API Gateway`나 애플리케이션 로드 밸런서와 통합된다. 중요하다.

- `API Gateway`를 사용하는 경우
~~~
- 사용자는 Cognito 사용자 풀에 접속해서 토큰을 받고 검증을 위해 토큰을 API Gateway에 전달한다.
- 확인이 끝나면 사용자 자격 증명으로 변환되어 백엔드의 Lambda 함수에 전달된다.
- Lambda 함수는 처리할 사용자가 인증된 구체적인 사용자라는 사실을 인식한다.
~~~

- `ALB`를 사용하는 경우
~~~
- 사용자는 Cognito 사용자 풀에 접속해서 토큰을 받고 검증을 위해 토큰을 ALB에 전달한다.
- 확인이 끝나면 요청을 백엔드로 리다이렉트하고 사용자의 자격 증명과 함께 추가 헤더를 전송한다.
- 백엔드 서버는 처리할 사용자가 인증된 구체적인 사용자라는 사실을 인식한다.
~~~

![image](https://user-images.githubusercontent.com/97398071/236675904-c0ea1f30-f056-4848-942d-4c2367b7902c.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 4. Cognito Identity Pools (Federated Identities)
- Get identities for “users” so they obtain temporary AWS credentials  
→ 임시 `AWS` 자격 증명을 이용하면 `API Gateway`나 애플리케이션을 통하지 않더라도 `AWS` 계정에 직접 액세스할 수 있다. 

- Users source can be Cognito User Pools, 3rd party logins, etc…  
→ 사용자는 `Cognito` 사용자 풀 내의 사용자가 될 수도 있고 타사 로그인이 될 수도 있다.

- Users can then access AWS services directly or through API Gateway  
→ 직접 또는 `API Gateway`를 통해 서비스에 액세스할 수 있다.

- The IAM policies applied to the credentials are defined in Cognito  
→ 자격 증명에 적용되는 `IAM` 정책은 `Cognito` 자격 증명 풀 서비스에 사전 정의되어 있다.

- They can be customized based on the user_id for fine grained control  
→ `user_id`를 기반으로 사용자를 정의해 세분화된 제어를 할 수 있다.

- Default IAM roles for authenticated and guest users  
→ 기본 `IAM` 역할을 정의할 수도 있다. 게스트 사용자나 특정 역할이 정의되지 않은 인증된 사용자는 기본 `IAM` 역할을 상속받게 된다.

### 5. Cognito Identity Pools – Diagram
- 웹이나 모바일 애플리케이션에서 `S3` 버킷 또는 `DynamoDB` 테이블에 직접 액세스하고 싶은 상황이다. 이 때 `Cognito` 자격 증명 풀을 사용한다.
- 웹, 모바일 앱이 로그인해서 토큰을 발급받는다. `Cognito` 사용자 풀에 대한 로그인, 소셜 자격 증명 제공자, `SAML`, `OpenID Connect` 중 하나이다.
- 받은 토큰을 `Cognito` 자격 증명 풀 서비스에 전달해서 토큰을 임시 `AWS` 자격 증명과 교환한다.
- `Cognito` 자격 증명 풀은 전달 받은 토큰이 올바른지, 즉 유효한 로그인인지 평가한다.
- 해당 사용자에게 적용되는 임시 `IAM` 정책을 생성한다. 임시 자격 증명으로 인해 `AWS`의 `S3` 버킷이나 `DynamoDB` 테이블에 직접 액세스가 가능해진다.

![image](https://user-images.githubusercontent.com/97398071/236675937-1f95f739-9852-45bc-8b72-4ebe9cb85584.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 6. Cognito Identity Pools - Row Level Security in DynamoDB
- `Cognito` 자격 증명 풀에 조건을 설정하는 방법으로 `DynamoDB`에 행 수준 보안을 설정할 수 있다.
- 아래 예제는 `DynamoDB`의 `LeadingKeys`가 `Cognito` 자격 증명 사용자 `ID`와 일치해야 한다는 조건이다.
- 이 정책이 적용된 사용자는 `DynamoDB` 테이블의 모든 항목을 읽고 쓸 수 있는 것이 아니라 이 조건을 통해 액세스를 얻은 항목에만 접근할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/236675962-2337d695-6618-4534-afb2-4a3683b1dd40.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---