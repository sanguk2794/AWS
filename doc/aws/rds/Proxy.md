## Amazon RDS Proxy
### 1. Amazon RDS Proxy
- Fully managed database proxy for RDS  
→ `VPC` 내에 `RDS`를 배포할 수 있던 것과 같이, 완전 관리형 `RDS` 프록시도 배포할 수 있다. `RDS`에 바로 액세스하면 된다.

- Allows apps to pool and share DB connections established with the database  
→ `RDS` 프록시를 사용하면 애플리케이션이 `DB` 내에서 `DB` 연결 풀을 형성하고 공유할 수 있다.
애플리케이션을 `RDB` 인스턴스에 일일이 연결하는 대신 프록시에 연결하면 프록시가 하나의 풀에 연결을 모아 `RDS` 인스턴스로 가는 연결이 줄어든다.

- Improving database efficiency by reducing the stress on database resources (e.g., CPU, RAM) and minimize open connections (and timeouts)  
→ 연결이 많은 경우 `CPU`, `RAM` 등 `DB` 리소스의 부담을 줄여 `DB`의 효율성을 향상시킬 수 있고 `DB`에 개방된 연결과 시간초과를 최소화할 수 있다. 중요한 내용이다.

- Serverless, autoscaling, highly available (multi-AZ)  
→ `RDS` 프록시는 완전한 서버리스로 오토 스케일링이 가능해 용량을 관리할 필요가 없고 가용성이 높다. 다중 `AZ` 또한 제공한다.

- Reduced RDS & Aurora failover time by up 66%  
→ `RDS` 인스턴스에 장애가 발생하면 기본 인스턴스가 아닌 대기 인스턴스로 실행되어 장애 조치 시간을 66%까지 줄일 수 있다.
메인 `RDS` 인스턴스에 애플리케이션을 모두 연결하고 장애 조치를 각자 처리하게 하는 대신 장애 조치와 무관한 `RDS` 프록시에 연결하는 것이다.

- Supports RDS (MySQL, PostgreSQL, MariaDB) and Aurora (MySQL, PostgreSQL)  
→ `RDS` 프록시는 `MySQL`, `PostgreSQL`, `MariaDB`용 `RDS`를 지원하며, `MySQL`, `PostgreSQL`용 `Aurora`를 지원한다.

- No code changes required for most apps  
→ 애플리케이션의 코드를 변경하지 않아도 되고 `RDS` 인스턴스나 `Aurora DB`에 연결하는 대신 `RDS` 프록시에 연결하기만 하면 된다.

- Enforce IAM Authentication for DB, and securely store credentials in AWS Secrets Manager  
→ 데이터베이스에 `IAM` 인증을 강제함으로써 `IAM` 인증을 통해서만 `RDS` 인스턴스에 연결하도록 할 수 있다.
이 때 자격 증명은 `AWS Secrets Manager` 서비스에 안전하게 저장된다. `IAM` 인증을 강제하고 싶다면 `RDS` 프록시를 사용해야 한다.

- RDS Proxy is never publicly accessible (must be accessed from VPC)  
→ `RDS` 프록시는 `public access`가 불가능하다. `VPC` 내에서만 액세스 가능하다.
`RDS` 프록시는 코드 조각을 실행하는 `Lambda` 함수와 함께 사용하면 굉장히 유용하게 사용할 수 있다.

- 정리하면 다음과 같다. `RDS` 프록시를 사용하면 `RDS` 데이터베이스 인스턴스의 연결을 최소화할 수 있고, 장애 조치 시간을 최대 66%까지 감소시킬 수 있다.
데이터베이스에 `IAM` 인증을 강제하는데 사용하고 자격 증명은 `Secret Manager` 서비스에 안전히 저장된다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
