## KMS
### 1. AWS KMS (Key Management Service)
- `KMS`는 `AWS`의 키 관리 서비스이다.

- Anytime you hear “encryption” for an AWS service, it’s most likely KMS  
→ `AWS` 서비스로 암호화한다고 하면 `KMS` 암호화일 확률이 크다.

- AWS manages encryption keys for us  
→ `KMS` 서비스를 사용하면 `AWS`에서 암호화 키를 관리한다.

- Fully integrated with IAM for authorization  
→ `KMS`는 권한 부여를 위해 `IAM`과 완전히 통합된다.

- Easy way to control access to your data  
→ `KMS`로 암호화한 데이터에 관한 액세스를 더 쉽게 제어할 수 있다.

- Able to audit KMS Key usage using CloudTrail  
→ `CloudTrail`을 통해 키를 사용하기 위해 호출된 모든 `API`를 감시할 수 있다. 중요하다.

- Seamlessly integrated into most AWS services (EBS, S3, RDS, SSM…)  
→ 대부분의 `AWS` 서비스에 `KMS`를 원활하게 사용할 수 있다.

- Never ever store your secrets in plaintext, especially in your code!
~~~
- 암호 데이터는 절대로 평문으로 저장해서는 안된다. 

- KMS Key Encryption also available through API calls (SDK, CLI)
→ KMS를 사용할 때에는 API를 호출하거나 CLI, SDK를 사용한다.

- Encrypted secrets can be stored in the code / environment variables
→ 암호화된 파일은 코드나 환경변수에 저장된다.
~~~

### 2. KMS Keys Types
- `KMS`의 키 유형에는 대칭키 유형과 비대칭키 유형의 2가지가 있다.

- Symmetric (AES-256 keys)
~~~
- Single encryption key that is used to Encrypt and Decrypt
→ 대칭 KMS 키에는 데이터의 암호화와 복호화에 단일 암호화 키를 사용한다.

- AWS services that are integrated with KMS use Symmetric CMKs
→ KMS와 통합된 모든 AWS 서비스는 대칭 키를 사용한다.

- You never get access to the KMS Key unencrypted (must call KMS API to use)
→ KMS 대칭키를 생성, 사용할 때 키에 대한 직접적 액세스는 불가능하다. 키를 활용하려면 KMS API를 호출해야 한다.
~~~

- Asymmetric (RSA & ECC key pairs)
~~~
- Public (Encrypt) and Private Key (Decrypt) pair
→ 데이터 암호화에 사용하는 퍼블릭 키와 데이터 복호화에 사용하는 프라이빗 키, 두 종류의 키가 존재한다. 

- Used for Encrypt/Decrypt, or Sign/Verify operations
→ 암호화 및 복호화, 작업 할당 및 검증에 사용된다.

- The public key is downloadable, but you can’t access the Private Key unencrypted
→ 이 경우에는 KMS에서 퍼블릭 키를 다운로드할 수 있다. 하지만 프라이빗 키에는 액세스가 불가능하다. 프라이빗 키를 활용하려면 KMS API를 호출해야 한다.

- Use case: encryption outside of AWS by users who can’t call the KMS API
→ KMS API 키에 액세스할 수 없는 사용자가 AWS 클라우드 외부에서 암호화를 수행할 때 사용한다.
~~~

### 3. AWS KMS (Key Management Service)
- Three types of KMS Keys:
~~~
- AWS Managed Key: free (aws/service-name, example: aws/rds or aws/ebs)
→ AWS 관리형 키는 aws/rds나 aws/ebs와 같은 이름이 지정된다. 무료로 사용할 수 있으며 특정 서비스에 관련한 저장 데이터 암호화에 사용할 수 있다.

- Customer Managed Keys (CMK) created in KMS: $1 / month
→ 고객 관리형 키는 KMS를 사용해 생성 가능하고 키 하나에 매달 1달러의 비용이 청구된다.

- Customer Managed Keys imported (must be 256-bit symmetric key): $1 / month
→ 자체 키 구성 요소를 KMS에 임포트할 수 있다. 고객 관리형 키와 동일하게 매달 1달러의 비용이 청구된다.
~~~

- + pay for API call to KMS ($0.03 / 10000 calls)  
→ 더불어, `KMS`로 호출하는 모든 `API` 요청에 대해서는 비용을 지불해야 한다.

- Automatic Key rotation:
~~~
- AWS-managed KMS Key: automatic every 1 year
→ AWS 자동 관리형 키를 사용하면 키가 자동으로 교체되며 그 주기는 1년이다.

- Customer-managed KMS Key: (must be enabled) automatic every 1 year
→ 고객 관리형 KMS 키를 사용하는 경우에는 반드시 자동 교체를 활성화해야 한다. 자동 교체를 활성화하면 고객 관리형 키가 자동으로 교체되며 그 주기는 1년이다. 이 교체 빈도를 변경할 수는 없다.

- Imported KMS Key: only manual rotation possible using alia
→ 자체 키를 KMS에 임포트한다면 수동으로 키를 교체해야 한다. 이 키를 올바르게 교체하려면 반드시 KMS 키 별칭을 사용해야 한다.
~~~

### 4. Copying Snapshots across regions
- 스냅샷을 교차 리전에 복제할 수 있다.
~~~
- `KMS` 키로 암호화된 `EBS` 볼륨이 있고 리전은 `eu-west-2`라고 가정한다. 이 `EBS` 볼륨을 다른 리전으로 복제하기 위한 시나리오이다.
- EBS 볼륨의 스냅샷을 생성한다. 암호화된 스냅샷에서 스냅샷을 생성하면 생성된 스냅샷 또한 동일한 KMS 키로 암호화된다.
- 다른 리전으로 스냅샷을 복사하려면 다른 KMS 키를 사용해서 스냅샷을 다시 암호화해야 하는데 이 부분은 AWS가 자동으로 처리해준다. 다만, 동일한 KMS 키가 서로 다른 리전에 있을수는 없다.
- 스냅샷을 ap-southeast-2 리전의 자체 EBS 볼륨으로 복원한다. 
~~~

![image](https://user-images.githubusercontent.com/97398071/236682658-eaa0049e-4787-4755-8d53-7d7bafe426c5.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 5. KMS Key Policies
- Control access to KMS keys, “similar” to S3 bucket policies  
→ `KMS` 키에 관한 액세스를 제어한다. `S3` 버킷 정책과 비슷하다.

- Difference: you cannot control access without them  
→ 하지만 `S3` 버킷 정책과 차이점이 있는데 `KMS` 키에 `KMS` 키 정책이 없으면 누구도 액세스할 수 없다는 것이다.

- 이와 관련해 2가지 유형의 `KMS` 키 정책이 있다.

- Default KMS Key Policy:
~~~
- Created if you don’t provide a specific KMS Key Policy
→ 기본 정책은 사용자 지정 키 정책을 제공하지 않는 경우에 생성된다.

- Complete access to the key to the root user = entire AWS account
→ 기본 정책은 계정의 모든 사람이 키에 액세스할 수 있도록 허용한다.
~~~

- Custom KMS Key Policy:
~~~
- Define users, roles that can access the KMS key
→ 제어를 조금 더 특정하고자 할 때에는 사용자 지정 키 정책을 사용한다.

- Define who can administer the key
→ 키 관리자를 정의할 수 있다.

- Useful for cross-account access of your KMS key
→ KMS 키에 관한 교차 계정 액세스 시 매우 유용하다.
~~~

### 6. Copying Snapshots across accounts
- Create a Snapshot, encrypted with your own KMS Key (Customer Managed Key)  
→ 계정 간 스냅샷을 복사할 때 사용한다. 자체 `KMS`로 암호화된 스냅샷을 생성하면 고객 키 관련 정책을 연결해야 하므로 고객 관리형 키가 된다.

- Attach a KMS Key Policy to authorize cross-account access  
→ 교차 계정 액세스 권한 부여를 위해 `KMS` 키 정책을 연결한다.

- Share the encrypted snapshot, (in target) Create a copy of the Snapshot, encrypt it with a CMK in your account  
→ 암호화된 스냅샷을 대상 계정에 공유하면 대상 계정에서는 스냅샷 복제본을 생성하고 해당 대상 계정에서 다른 고객 관리형 키로 암호화한다.

- Create a volume from the snapshot  
→ 새로운 볼륨을 생성하면 복사가 끝난다.

![image](https://user-images.githubusercontent.com/97398071/236682945-405fba90-847f-47ef-bcba-2690f61b3162.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 7. KMS Multi-Region Keys
- `KMS`에는 다중 리전 키를 둘 수 있다. 한 리전의 기본 키가 다른 리전으로 복제되며, 이 때의 키 `ID`는 동일하다.

![image](https://user-images.githubusercontent.com/97398071/236682983-5310491d-459f-4d63-a5b3-d86c2f9b274d.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Identical KMS keys in different AWS Regions that can be used interchangeably  
→ 다른 AWS 리전에서 사용할 수 있는 `KMS` 키 세트를 서로 교차해서 사용할 수 있다. 한 리전에서 암호화한 다음 다른 리전에서 복호화할 수 있다는 의미이다.

- Multi-Region keys have the same key ID, key material, automatic rotation…  
→ 다중 리전 키는 동일한 키 `ID`와 동일한 키 구성 요소를 가진다.

- No need to re-encrypt or making cross-Region API calls  
→ 다중 리전 키의 핵심은 한 리전에서 암호화하고 다른 리전에서 복호화해 다음 리전으로 복제할 때나 교차 리전 `API` 호출을 실행할 때 데이터를 재암호화하지 않아도 된다는 것이다.

- KMS Multi-Region are NOT global (Primary + Replicas)  
→ `KMS` 다중 리전 키를 전역으로 사용할 수 없다. 복제본에 있는 것에 불과하다.

- Each Multi-Region key is managed independently  
→ 각 다중 리전 키는 자체 키 정책 등으로 독립적으로 관리된다. 따라서 특정 사용 사례를 제외하고는 다중 리전 키 사용이 권장되지 않는다.

- Use cases: global client-side encryption, encryption on Global DynamoDB, Global Aurora  
→ 전역 클라이언트 측 암호화에 사용한다. 한 리전에서 클라이언트 측 암호화를 하고 다른 리전에서 클라이언트 측 복호화를 하는 것이다.

### 8. DynamoDB Global Tables and KMS Multi-Region Keys Client-Side encryption
- We can encrypt specific attributes client-side in our DynamoDB table using the Amazon DynamoDB Encryption Client  
→ 중요한 것은 전체 테이블을 암호화하는 것이 아니라는 점이다. 저장 데이터 암호화이므로 테이블의 속성을 암호화하여 특정 클라이언트만 사용할 수 있게 된다.

- `DynamoDB` 전역 테이블과 `KMS` 다중 리전 키를 이용해 클라이언트 측 암호화하는 시나리오는 다음과 같다.
~~~
- us-east-1과 ap-southeast-2에 멀티 리전 키가 있다고 가정한다. us-east-1의 KMS 다중 리전 키의 복제본이 ap-southeast-2 리전에 저장되었다.
- 클라이언트 애플리케이션에서 DynamoDB에 데이터를 삽입할 때 다중 리전 키를 이용해 사회 보장 번호 등 중요한 일부 속성을 암호화한다.
- DynamoDB 테이블에 액세스할 수 있는 데이터베이스 관리자조차 사회 보장 번호 속성을 암호화하는 데 사용한 KMS 키에 액세스할 수 있는 권한이 없다면 해당 데이터에 대한 접근이 불가능하다.
- DynamoDB 테이블이 전역 테이블인 경우 해당 테이블의 데이터는 다른 리전으로 복제된다.
- 복제된 리전의 DynamoDB 데이터는 ap-southeast-2에 존재하는 다중 리전 키의 복제본을 통해 복호화가 가능하다.
~~~

- Using client-side encryption we can protect specific fields and guarantee only decryption if the client has access to an API key  
→ 클라이언트 측 암호화 기술을 사용하면 데이터의 특정 필드나 속성을 보호할 수 있다. 또, 전역 테이블 덕분에 데이터 뿐만 아니라 암호화 키도 같이 복제할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/236688852-88f6620e-296c-44f8-8b1c-e4969220c7c5.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 9. Global Aurora and KMS Multi-Region Keys Client-Side encryption
- We can encrypt specific attributes client-side in our Aurora table using the AWS Encryption SDK  
→ `AWS Encryption SDK`를 사용하여 `Aurora` 테이블의 클라이언트 측 특정 속성을 암호화할 수 있다.

- Combined with Aurora Global Tables, the client-side encrypted data is replicated to other regions  
→ `Aurora`는 전역 데이터베이스이므로 테이블이 전역으로 복제된다.

- If we use a multi-region key, replicated in the same region as the Global Aurora DB, then clients in these regions can use low-latency API calls to KMS in their region to decrypt the data client-side  
→ `Global Aurora DB`와 동일한 지역에서 복제된 다중 지역 키를 사용하는 경우, 로컬 `KMS API`를 호출해 데이터 클라이언트 측의 암호를 해독한다. 따라서, 지연 시간이 단축된다.

- Using client-side encryption we can protect specific fields and guarantee only decryption if the client has access to an API key, we can protect specific fields even from database admins  
→ `DB` 관리자로부터 데이터를 보호할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/236689588-f655e923-07e6-45c1-ba8d-e9aa6f872d5d.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
