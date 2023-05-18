## Amazon S3 - Security
### 1. Amazon S3 - Security
- User-Based  
→ 사용자 기반일 경우 사용자에게 특정 `IAM` 정책이 주어진다. 이 정책은 어떤 `API` 호출이 특정 `IAM` 사용자를 위해 허용되어야 하는지를 승인한다.
~~~
- IAM Policies – which API calls should be allowed for a specific user from IAM
~~~

- Resource-Based  
→ 리소스를 보안 기반으로 사용할 수도 있다. 가장 일반적인 방법은 버킷 정책을 사용하는 것이다.
~~~
- Bucket Policies – bucket wide rules from the S3 console - allows cross account
→ S3 버킷 정책을 사용할 수 있다. 이 정책은 S3 콘솔에서 직접 할당할 수 있는 전체 버킷 규칙이다.

- Object Access Control List (ACL) – finer grain (can be disabled)
→ 객체 액세스 제어 목록이다. 이 목록은 보다 세밀한 보안이며 비활성화가 가능하다.

- Bucket Access Control List (ACL) – less common (can be disabled)
→ 버킷 수준에서 살펴보면 버킷 ACL이 있는데 훨씬 덜 일반적이다. 이 역시 비활성화할 수 있다.
~~~

- Note: an IAM principal can access an S3 object if  
→ `IAM` 권한 또는 리소스 정책을 허용하는 경우에만 `S3` 객체에 접근할 수 있다.
~~~
- The user IAM permissions ALLOW it OR the resource policy ALLOWS it
- AND there’s no explicit DENY
~~~

- Encryption: encrypt objects in Amazon S3 using encryption keys  
→ 암호키를 사용해 객체를 암호화할 수 있다.

#### 1. S3 Bucket Policies
- JSON based policies
→ 버킷 정책은 `S3`의 보안 설정에 사용되는 `JSON` 기반의 정책이다.

- `Resource` 블럭은 여러 개 선언이 가능하다.
~~~
- Resources: buckets and objects
→ 이 정책이 적용되는 버킷과 객체를 선택한다.

- Effect: Allow / Deny
→ 작업을 허용할지 거부할지 여부를 선택한다.

- Actions: Set of API to Allow or Deny
→ 허용하거나 거부할 수 있는 API의 집합을 설정한다.

- Principal: The account or user to apply the policy to
→ 원칙을 통해 허용할 사용자를 설정한다. 아스타리스크를 사용하면 모든 사용자를 허용한다.
~~~

![image](https://user-images.githubusercontent.com/97398071/234629633-88b8c385-afcf-4983-8e9e-3f26bbfb1033.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Use S3 bucket for policy to:
~~~
- Grant public access to the bucket
→ 버킷에 대한 공개 액세스를 허용할 수 있다.

- Force objects to be encrypted at upload
→ 업로드 시 객체를 강제로 암호화할 수 있다.

- Grant access to another account (Cross Account)
→ 다른 계정에서의 액세스를 허용할 수 있다.
~~~

#### 2. Bucket Policies - Use Case
- 버킷 정책 설정을 통해 모든 유저에게의 공개 액세스를 허용할 수 있다.
- 버킷 정책 설정을 통해 `IAM` 유저에게 버킷 접근 권한을 부여하는 것으로 특정 `IAM` 유저에게 액세스를 허용할 수 있다.
- `EC2` 인스턴스가 있는 경우 `EC2` 인스턴스에서 `S3` 버킷으로의 액세스를 허용할 수 있다. 이 때에는 `IAM Role`을 활용해야 한다.
- 다른 계정에 있는 `IAM` 사용자에게 교차 계정 액세스를 허용하려면 버킷 정책을 사용해야 한다. 설정을 통해 `S3` 버킷으로 `API` 호출이 가능해진다.

#### 3. Bucket settings for Block Public Access
- These settings were created to prevent company data leaks  
→ 기업 데이터 유출을 방지하기 위한 추가 계층으로써 `AWS`가 개발했다.

- If you know your bucket should never be public, leave these on  
→ `S3` 버킷 정책을 설정하여 공개로 만들더라도 이 설정이 활성화되어 있다면 버킷은 결코 공개되지 않는다.
버킷을 공개하면 안 되는 경우, 이 설정을 그대로 두면 잘못된 `S3` 버킷 정책을 설정한 사람들에 대해 보안을 갖출 수 있다.

- Can be set at the account level  
→ `S3` 버킷 중 어느 것도 공개되서는 안 된다면 계정 수준에서 이를 설정하면 된다.

#### 4. Update Bucket Permission
- `Amazon S3` → `Buckets` → 버킷 선택 → `Permission`

- `Block all public access`  
→ 이 부분을 선택 해제하면 공개 액세스를 허용할 수 있다. 공개 버킷 정책을 설정하려는 경우에만 비활성화하도록 하자. 이 작업은 위험하다.

- `Edit bucket policy`  
→ 버킷 정책을 변경할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/234633374-51273174-c59a-4a99-9095-3d7b1ad1d805.png)

- [버킷 정책 생성기](https://awspolicygen.s3.amazonaws.com/policygen.html)를 통해 정책을 쉽게 작성할 수 있다.  
→ `ARN, Amazon Resource Name`을 입력할 때 맨 뒤에 `/*`를 추가해야 한다.

![image](https://user-images.githubusercontent.com/97398071/234634288-4cd7736f-fab5-4ef5-9d09-78908ffe0d0d.png)

- [정책 예제](https://docs.aws.amazon.com/ko_kr/AmazonS3/latest/userguide/example-bucket-policies.html)에서 여러 정책 예제를 확인할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/234633661-3cd693c0-e66b-4217-9725-b8f02ab4a2d5.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---