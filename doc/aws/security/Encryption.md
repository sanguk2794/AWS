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
→ 암호 키와 해독 키는 주로 `KMS Key Management Service` 같은 곳에 따로 관리된다.

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

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---