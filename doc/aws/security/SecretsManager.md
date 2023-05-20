## AWS Secrets Manager
### 1. AWS Secrets Manager
- Newer service, meant for storing secrets  
→ 암호를 저장하는 최신 서비스이다. 수명 주기 동안 암호를 쉽게 교체, 관리 및 검색할 수 있게 해 주는 서비스이다.

- Capability to force rotation of secrets every X days  
→ `Secrets Manager`는 `X`일마다 강제로 암호를 교체하는 기능이 있어 보다 효율적으로 암호 관리를 할 수 있다.

- Automate generation of secrets on rotation (uses Lambda)  
→ 교체할 암호를 강제로 생성하거나 자동화할 수 있다. 이를 위해 새 암호를 생성할 `Lambda` 함수를 정의해야 한다.

- Integration with Amazon RDS (MySQL, PostgreSQL, Aurora)  
→ `RDS` 및 `Aurora`가 제공하는 다양한 서비스와 아주 잘 통합된다. 데이터베이스에 접근하기 위한 사용자 이름과 비밀번호를 `AWS Secrets Manager`에 저장해 사용할 수 있다는 것이다.

- Secrets are encrypted using KMS  
→ 암호는 `KMS` 서비스를 통해 암호화된다. 

- Mostly meant for RDS integration  
→ `RDS`와 `Aurora`의 통합 혹은 암호에 대한 내용이 시험에 나오면 `AWS Secrets Manager`를 떠올릴 수 있어야 한다.

### 2. AWS Secrets Manager – Multi-Region Secrets
- Replicate Secrets across multiple AWS Regions  
→ 복수 `AWS` 리전에 암호를 복제할 수 있다. 이 때, `AWS Secrets Manager` 서비스가 기본 암호와 동기화된 읽기 전용 복제본을 유지한다.

- Secrets Manager keeps read replicas in sync with the primary Secret  
→ 두 리전이 있을 때 기본 리전에 암호를 하나 만들면 보조 리전에 동일한 암호가 복제된다.

- Ability to promote a read replica Secret to a standalone Secret  
→ 한 리전에서 문제가 발생하면 암호 복제본을 독립 실행형 암호로 승격시킬 수 있다.

- 여러 리전에 암호가 복제되므로 다중 리전 앱을 구축하고 재해 복구 전략을 짤 수 있다.

![image](https://user-images.githubusercontent.com/97398071/236855878-597795a3-65a7-4519-915a-cf785be49174.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---