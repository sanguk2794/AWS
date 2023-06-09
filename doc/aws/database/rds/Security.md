## RDS & Aurora Security
### 1. RDS & Aurora Security
- At-rest encryption:  
~~~
- RDS 및 Aurora에 저장된 데이터를 암호화할 수 있다.

- Database master & replicas encryption using AWS KMS – must be defined as launch time
→ KMS를 사용해 마스터와 모든 복제본의 암호화가 이루어지며 이는 DB를 처음 실행할 때 정의된다.

- If the master is not encrypted, the read replicas cannot be encrypted
→ 어떤 이유에서든 마스터 DB를 암호화하지 않았다면 읽기 전용 복제본을 암호화할 수 없다.

- To encrypt an un-encrypted database, go through a DB snapshot & restore as encrypted
→ 또한 암호화되어 있지 않은 기존 DB를 암호화하려면 암호화되지 않은 DB의 스냅샷을 가져와서 암호화된 DB의 형태로 DB 스냅샷을 복원해야 한다.
~~~

- In-flight encryption: TLS-ready by default, use the AWS TLS root certificates client-side  
→ `RDS` 및 `Aurora`의 각 `DB`는 기본적으로 전송 중 데이터 암호화 기능을 갖추고 있다. 따라서 클라이언트는 `AWS` 웹사이트에서 제공하는 `AWS`의 `TLS` 루트 인증서를 사용해야 한다.

- IAM Authentication: IAM roles to connect to your database (instead of username/pw)  
→ `DB` 인증시 `RDS`와 `Aurora`는 사용자 이름과 패스워드라는 전통적인 조합을 사용할 수 있으며 `IAM Role`을 통해 `DB`에 접속할 수도 있다. `IAM Role`을 통해 접속할 수 있는 `DB`는 `Aurora`와 `PostgreSQL`, `MySQL`로 제한된다.

- `IAM`  인증은 `MySQL` 및 `PostgreSQL`에서 작동한다. 인증 토큰을 활용한다. 데이터베이스 접속을 위한 `IAM` 사용자를 생성해야 하는데 이는 `AWS` 제공 플러그인에 의해 처리된다.

- Security Groups: Control Network access to your RDS / Aurora DB  
→ 보안 그룹을 통해 `DB`에 대한 네트워크 액세스를 통제할 수 있다.

- No SSH available except on RDS Custom  
→ `RDS`와 `Aurora`는 관리형 서비스이기 때문에 `SSH`로 인스턴스에 액세스할 수 없다. 다만 `RDS Custom`은 예외이다.

- Audit Logs can be enabled and sent to CloudWatch Logs for longer retention  
→ 감사 로그가 있어서 시간에 따라 `RDS` 및 `Aurora`에서 어떤 쿼리가 생성되는지 확인할 수 있다. 이 로그는 시간이 지나면 자동 삭제된다. 장기간 보관하고 싶다면 `CloudWatch Logs`로 로그 데이터를 전송해야 한다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---