## Amazon S3 Security
### 1. Object Encryption
- You can encrypt objects in S3 buckets using one of 4 methods  
→ `S3` 버킷 내의 객체를 암호화하는 유형에는 네 가지가 있다.

- Server-Side Encryption (SSE)  
→ 객체를 암호화하는 유형 4가지 중 3가지 유형은 서버 측에서 암호화를 수행한다. 이를 서버측 암호화라고 한다.
~~~
- Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3) – Enabled by Default
→ SSE-S3는 Amazon S3 키를 사용해서 암호화 키를 관리하는 서비스 암호화 유형이다.

- Server-Side Encryption with KMS Keys stored in AWS KMS (SSE-KMS)
→ SSE-KMS는 KMS 키를 사용해서 암호화 키를 관리하는 서비스 암호화 유형이다.

- Server-Side Encryption with Customer-Provided Keys (SSE-C)
→ SSE-C는 고객이 제공하는 키를 사용해서 암호화 키를 관리하는 서비스 암호화 유형이다. 이 경우에는 스스로 암호화 키를 제공해야 한다.
~~~

- Client-Side Encryption  
→ 클라이언트 측 암호화는 클라이언트 측에서 모든 것을 암호화한 다음에 `Amazon S3`에 업로드하는 방식이다.

- It’s important to understand which ones are for which situation for the exam  
→ 시험에서는 어느 것이 어떤 상황에 해당하는지 이해하는 것이 중요하다.

- `S3` 버킷에 있는 모든 파일이 기본적으로 암호화되게 하고 싶습니다. 이를 달성하기 위한 최적의 방법은 무엇입니까?  
→ 기본 암호화를 활성화한다.

### 2. Amazon S3 Encryption – Server Side Encryption
#### 1. Amazon S3 Encryption – SSE-S3
- Encryption using keys handled, managed, and owned by AWS  
→ 이 경우 `AWS`에서 처리, 관리 및 소유하는 키로 암호화를 진행한다. 사용자는 이 키에 접근할 수 없다.

- Object is encrypted server-side  
→ 객체는 `AWS`에 의해 서버 측에 암호화된다.

- Encryption type is AES-256  
→ `AES-256` 보안 유형으로 암호화가 이루어진다.

- Must set header "x-amz-server-side-encryption": "AES256"  
→ 헤더에 `x-amz-server-side-encryption: AES256`를 설정해서 `SSE-S3` 메커니즘으로 객체를 암호화하도록 `Amazon S3`에 요청해야 한다.

![image](https://user-images.githubusercontent.com/97398071/235283410-631a6ff8-06f9-435e-88e2-4359841866cc.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 2. Amazon S3 Encryption – SSE-KMS
- Encryption using keys handled and managed by AWS KMS (Key Management Service)  
→ `AWS`와 `S3` 서비스가 소유한 키에 의존하는 대신 `KMS, Key Manager Service`로 자신의 키를 관리하고 이 키로 암호화하는 방식이다.

- Object is encrypted server side  
→ 객체는 `AWS`에 의해 서버 측에 암호화된다.

- KMS advantages: user control + audit key usage using CloudTrail  
→ `KMS`를 사용하면 사용자가 키를 제어할 수 있다는 장점이 있다. `KMS` 내에서 직접 키를 생성하고 `CloudTrail`을 사용해서 사용량을 감시할 수 있다.

- `CloudTrail`은 누군가가 `KMS`에서 키를 사용했을 때 `AWS`에서 발생하는 모든 일을 기록하는 역할을 담당하는 서비스이다.

- Must set header "x-amz-server-side-encryption": "aws:kms"  
→ 헤더에 `x-amz-server-side-encryption: aws:kms`를 설정해서 `SSE-KMS` 메커니즘으로 객체를 암호화하도록 `Amazon S3`에 요청해야 한다.

![image](https://user-images.githubusercontent.com/97398071/235283509-c074401e-d592-4a87-bcfa-00c388f8ab8f.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 근무하고 있는 회사에서 데이터를 암호화해 S3에 저장하고 싶어 합니다. 회사에서는 암호화 키를 `AWS`에 저장하고 관리하는 것은 상관없지만, 암호화 키의 순환 정책에 대한 제어권은 유지하고 싶어합니다. 이럴 경우 ....................를 사용할 것을 권장합니다.  
→ `SSE-KMS`

#### 3. SSE-KMS Limitation
- If you use SSE-KMS, you may be impacted by the KMS limits  
→ `SSE-KMS` 사용시에는 몇 가지 제약이 존재한다. `Amazon S3`에서 파일을 업로드하고 다운로드할 때 `KMS` 키를 사용해야 하기 때문이다.

- When you upload, it calls the GenerateDataKey KMS API  
→ `KMS` 키에는 `GenerateDataKey`같은 자체 `API`가 있다.

- When you download, it calls the Decrypt KMS API  
→ 복호화할 때는 복호화 `API`를 호출한다. 따라서 `KMS` 서비스의 `API` 호출을 수행해야 한다.

- Count towards the KMS quota per second (5500, 10000, 30000 req/s based on region)  
→ 각 `API` 호출은 초당 `KMS` 할당량에 포함된다. `KMS`는 리전에 따라 초당 5500 ~ 30000개의 요청을 처리할 수 있다.

- You can request a quota increase using the Service Quotas Console  
→ `Service Quotas Console, 서비스 할당량 콘솔`로 한도를 늘릴 수 있다.

- 처리량이 매우 높은 `S3` 버킷이 있고 모든 파일이 `KMS` 키로 암호화되었다면 스로틀링 오류 등 문제가 발생할 수 있다. 중요하다.

#### 4. Amazon S3 Encryption – SSE-C
- Server-Side Encryption using keys fully managed by the customer outside of AWS  
→ 키가 `AWS` 외부에서 관리되지만 `AWS`로 키를 보내기 때문에 서버측 암호화에 포함된다.

- Amazon S3 does NOT store the encryption key you provide  
→ 하지만 제공된 암호화 키는 `Amazon S3`에 저장되지 않고 사용 후 폐기된다.

- HTTPS must be used  
→ 이 경우 `Amazon S3`로 키를 전송하기 때문에 `HTTPS`를 사용해야 한다.

- Encryption key must provided in HTTP headers, for every HTTP request made  
→ 요청을 전송할 때마다 `HTTP` 헤더에 키를 포함하여 전송해야 한다.

![image](https://user-images.githubusercontent.com/97398071/235283857-b86284ca-ecbb-4897-8cdf-b7a229d04c10.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. Amazon S3 Encryption – Client Side Encryption
- Use client libraries such as Amazon S3 Client-Side Encryption Library  
→ 클라이언트 라이브러리로 더 쉽게 구현할 수 있다. `Amazon S3 Client-Side Encryption Library`가 라이브러리의 대표적 예시이다.

- Clients must encrypt data themselves before sending to Amazon S3  
→ 데이터를 `Amazon S3`로 보내기 전에 클라이언트가 직접 암호화를 수행해야 한다.

- Clients must decrypt data themselves when retrieving from Amazon S3  
→ 데이터 복호화 또한 `Amazon S3` 외부의 클라이언트에서 수행해야 한다.

- Customer fully manages the keys and encryption cycle  
→ 클라이언트가 전적으로 키와 암호화의 주기를 관리하는 것이다.

![image](https://user-images.githubusercontent.com/97398071/235284055-32b2fd69-31d7-4542-a7af-cde8d37252b6.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 클라이언트 암호화는 `AWS`가 인식할 수 없다.

### 4. Amazon S3 – Encryption in transit (SSL/TLS)
- Encryption in-flight is also called SSL/TLS  
→ 전송 중 암호화는 `SSL` 또는 `TLS`라고도 불린다.

- Amazon S3 exposes two endpoints:  
→ `Amazon S3` 버킷에는 두 개의 엔드포인트가 있다.
~~~
- HTTP Endpoint – non encrypted
→ `HTTP Endpoint`는 암호화되지 않은 엔드포인트이다.

- HTTPS Endpoint – encryption in flight
→ `HTTPS Endpoint`는 전송 중 암호화 엔드포인트이다.
~~~

- HTTPS is recommended  
→ 데이터를 안전하게 전송하려면 전송 중 암호화를 사용하는 `HTTPS` 연결이 추천된다.

- HTTPS is mandatory for SSE-C  
→ `SSE-C` 유형의 매커니즘을 사용할 때에는 키를 같이 전송하기 때문에 반드시 `HTTPS` 프로토콜을 사용해야 한다.

- Most clients would use the HTTPS endpoint by default  
→ 대부분의 클라이언트에서는 `HTTPS` 엔드포인트가 기본값으로 되어 있다.

### 5. Create Encrypted Bucket
- `Amazon S3` - `Bucket` - `Objects` - `Upload` - `Properties` - `Server-side encryption`

![image](https://user-images.githubusercontent.com/97398071/235284394-a6c69347-9835-4b28-8fcd-ed4dce8d00ae.png)

- `SSE-KMS` 키를 선택할 경우 2가지 종류의 `KMS` 키 타입을 확인할 수 있다.  
→ `Choose from your AWS KMS keys`를 선택하면 자신의 `KMS` 키를 사용할 수 있다. 이 때 물론 `KMS` 키를 먼저 생성해야 하며 비용이 발생한다.
또는 `ARN, Amazon Resource Name`을 입력할 수도 있다. `ARN`은 계정마다 자동적으로 주어져 있다. 

![image](https://user-images.githubusercontent.com/97398071/235284460-dc462c04-0ede-4874-aa2f-7f7a90062d20.png)

- 암호화는 객체의 특정 버전 `ID`에 적용된다.

- 버킷의 기본 암호화를 지정할 때에는 `Amazon S3` - `Bucket` - `Properties` - `Default encryption`에서 기본 암호화를 활성화하면 된다.

![image](https://user-images.githubusercontent.com/97398071/235284575-af5d51a9-f972-40c8-8f91-a2cff65e0b8d.png)

- `SSE-C`는 콘솔에서 사용할 수 없다. 반드시 `AWS CLI`, `SDK` 또는 `Amazon S3 Rest API`를 사용해야 한다.

### 6. Amazon S3 – Default Encryption vs. Bucket Policies
- SSE-S3 encryption is automatically applied to new objects stored in S3 bucket  
→ `SSE-S3` 암호화는 `S3` 버킷에 저장되는 객체에 대해 자동적으로 암호화를 적용한다.

- Optionally, you can “force encryption” using a bucket policy and refuse any API call to PUT an S3 object without encryption headers (SSE-KMS or SSE-C)  
→ 암호화를 강제할 수 있는 한 가지 방법은 버킷 정책을 사용하는 것이다. 버킷 정책을 사용해서 지정된 암호화 헤더가 없는 `S3` 객체를 버킷에 넣는 `API`의 호출을 거부한다.

- 버킷 정책의 생김새는 아래와 같다. 이 중 왼쪽 정책은 `SSE-S3` 암호화를 강제로 적용한다. 헤더가 `AES256`이 아니라면 호출을 거부하도록 구성되어 있다.

![image](https://user-images.githubusercontent.com/97398071/235284781-821591e8-a0d7-48bc-afed-ed9c6b489927.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Note: Bucket Policies are evaluated before “Default Encryption”  
→ 일반적으로 버킷 정책은 항상 기본 암호화전에 평가된다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
