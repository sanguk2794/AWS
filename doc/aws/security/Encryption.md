## Encryption
### 1. Why encryption? Encryption in flight (SSL)
- 매우 민감한 기밀 내용인 경우 전송 중 암호화를 사용한다. 네트워크 패킷이 탈취되더라도 복호화가 불가능하므로 보안이 유지된다.

- Data is encrypted before sending and decrypted after receiving  
→ 전송 중 암호화를 사용하면 데이터를 전송하기 전 암호화를 수행하고 서버가 데이터를 받으면 복호화를 수행한다. 전송하는 이와 서버만이 암호화와 복호화를 하는 방법을 알고 있다.

- SSL certificates help with encryption (HTTPS)  
→ `SSL` 인증서를 통해 암호화를 수행한다. `SSL`과 `HTTPS`를 기반으로 동작한다.

- Encryption in flight ensures no MITM (man in the middle attack) can happen  
→ 기본적으로 전송 중 암호화를 활성화하면 `MITM, 중간자` 공격으로부터 보호된다.

- 대부분의 프로그래밍 언어가 `SSL` 암호화와 복호화를 지원하는 라이브러리를 지원한다. 전송 중 암호화의 구현에 대해서는 걱정할 필요가 없다.

![image](https://user-images.githubusercontent.com/97398071/236682204-3c9f5440-2a17-4fe3-94b1-aba5137963ad.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. Why encryption? Server side encryption at rest
- Data is encrypted after being received by the server  
→ 서버 측 암호화를 사용하면 서버에 수신된 데이터를 암호화한 후 서버에 저장한다. 혹시라도 서버가 해킹을 당하면 데이터가 복호화 되어서는 안되기 때문이다.

- Data is decrypted before being sent  
→ 데이터는 클라이언트로 다시 전송되기 전에 복호화된다.

- It is stored in an encrypted form thanks to a key (usually a data key)  
→ 데이터 키라고 불리는 키 덕분에 데이터는 암호화된 형태로 저장된다.

- The encryption / decryption keys must be managed somewhere and the server must have access to it  
→ 암호 키와 해독 키는 주로 `KMS, Key Management Service` 같은 곳에 따로 관리된다.

- 서비스의 서버 자체가 암호화와 복호화를 관리한다. 사용자는 사용할 암호화 키를 지정할 뿐이다.

![image](https://user-images.githubusercontent.com/97398071/236682288-2891c42d-457c-428a-99ad-325fc9955901.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. Why encryption? Client side encryption
- Data is encrypted by the client and never decrypted by the server, Data will be decrypted by a receiving client  
→ 클라이언트 측 암호화는 클라이언트가 암호화와 복호화를 수행한다. 

- The server should not be able to decrypt the data  
→ 서버는 그 데이터를 절대 복호화할 수 없다.

- Could leverage Envelope Encryption  
→ 이를 위해 봉투 암호화를 활용할 수 있다. 

![image](https://user-images.githubusercontent.com/97398071/236682342-d64b938c-ad8c-4fc9-849a-ce7fa5e7a4ca.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 4. S3 Replication Encryption Considerations
- Unencrypted objects and objects encrypted with SSE-S3 are replicated by default  
→ 한 버킷에서 다른 버킷으로의 `S3` 복제를 활성화하면 암호화되지 않은 객체와 `SSE-S3`로 암호화된 객체가 복제된다.

- Objects encrypted with SSE-C (customer provided key) are never replicated  
→ `SSE-C`로 암호화된 객체는 복제되지 않는다. 그 이유는 항상 키를 제공할 수 없기 때문이다.

- For objects encrypted with SSE-KMS, you need to enable the option  
→ `SSE-KMS`로 암호화된 객체는 기본적으로 복제되지 않지만, 옵션을 활성화해 복제 대상에 포함시킬 수 있다.
~~~
- Specify which KMS Key to encrypt the objects within the target bucket
→ 이 때에는 어떤 KMS 키로 대상 버킷 내 객체를 암호화하는지 지정해야 한다.

- Adapt the KMS Key Policy for the target key
→ KMS 정책을 대상 키에 적용해야 한다.

- An IAM Role with kms:Decrypt for the source KMS Key and kms:Encrypt for the target KMS Key
→ S3 복제 서비스를 허용하는 IAM 역할을 생성해서 소스 버킷의 데이터를 먼저 복호화하도록 한 뒤 대상 KMS 키로 대상 버킷의 데이터를 다시 암호화한다.

- You might get KMS throttling errors, in which case you can ask for a Service Quotas increase
→ 이 때, KMS 스로틀링 오류가 발생할 수 있다. 이러한 경우 서비스 할당량 증가를 요청해야 한다. 스로틀링 오류가 발생하는 이유는 수많은 암호화와 복호화가 발생하기 때문이다.
~~~

- You can use multi-region AWS KMS Keys, but they are currently treated as independent keys by Amazon S3 (the object will still be decrypted and then encrypted)  
→ 공식 문서에 따르면 `S3` 복제에 다중 리전 키를 사용할 수 있으나 `Amazon S3` 서비스에서 독립 키로 취급되므로 복호화와 암호화는 그대로 수행된다.

### 5. AMI Sharing Process Encrypted via KMS
- `AMI`를 다른 계정과 공유할 수 있다. 시험에 출제되는 중요한 부분이다.

- AMI in Source Account is encrypted with KMS Key from Source Account  
→ `AMI`는 `KMS` 키로 암호화 되어 있다.

- A 계정의 `AMI`로 B 계정의 `EC2` 인스턴스를 기동할 수 있다. 그 방법은 다음과 같다.
~~~
- 먼저, 시작 권한으로 AMI 속성을 수정해야한다. 이 시작 권한은 B 계정에서 AMI를 시작하도록 허용한다.
- KMS 키를 공유해야 해야 하며, 일반적으로 키 정책으로 설정한다. B 계정에서 KMS 키를 사용할 수 있도록 허용한다.
- B 계정에서 KMS 키와 AMI를 모두 사용할 수 있는 충분한 권한을 가진 IAM 역할이나 IAM 사용자를 생성한다.
- 모두 완료된 후에는 AMI를 통해 EC2 인스턴스를 기동하면 된다.
- 인스턴스 기동시의 선택 사항으로 대상 계정에서 자체 계정의 볼륨을 재암호화하는 KMS 키를 이용해 전체를 재암호화할 수 있다.
~~~

![image](https://user-images.githubusercontent.com/97398071/236843927-34ff78f8-4bb5-4d0b-bf91-4082881ec16b.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---