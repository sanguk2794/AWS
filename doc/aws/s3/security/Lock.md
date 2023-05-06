## Lock
### 1. S3 Glacier Vault Lock
- Adopt a WORM (Write Once Read Many) model  
→ `WORM` 모델을 채용하기 위해 `Glaciar Vault`를 잠글 수 있다. `WORM`은 한 번 쓰고 여러 번 읽는다는 뜻이다.

- Create a Vault Lock Policy - Lock the policy for future edits (can no longer be changed or deleted)  
→ 누구도 변경하거나 삭제할 수 없도록 객체를 `S3 Glacier Vault`에 넣고 수정하거나 삭제할 수 없도록 잠그는 것이다. 관리자나 `AWS` 서비스를 삭제해도 마찬가지다.

- Helpful for compliance and data retention  
→ 규정 준수와 데이터 보존에 유용하다.

- 회사에는 규정상 백업된 데이터베이스를 4년 동안 보관해야 한다는 의무가 있습니다. 백업된 데이터가 지워지지 않도록 보관하려면 어떤 구성을 사용해야 합니까?  
→ 볼트 잠금 정책이 적용된 `Glacier Vault`

### 2. S3 Object Lock
- `S3` 객체 잠금을 활성화하려면 먼저 버저닝을 활성화해야 한다.

- Adopt a WORM (Write Once Read Many) model  
→ `WORM` 모델을 채용할 수 있다.

- Block an object version deletion for a specified amount of time  
→ 잠금은 버킷 내의 모든 객체에 각각 적용할 수 있으며 특정 객체 버전이 특정 시간동안 삭제되는 것을 차단할 수 있다.

- 보존 모드에는 2가지가 존재한다. 두 가지 보존모드의 차이점은 중요하다.

- Retention mode - Compliance:  
→ 규정 준수 모드는 `S3 Glacier Vault Lock`과 매우 유사하다.
~~~
- Object versions can't be overwritten or deleted by any user, including the root user
→ 루트를 포함한 그 누구도 객체 버전을 덮어쓰거나 삭제할 수 없다.

- Objects retention modes can't be changed, and retention periods can't be shortened
→ 보존 기간 중 보존 모드의 변경, 보존 기간의 단축이 불가능하다.  
~~~

- Retention mode - Governance:  
→ 거버넌스 모드는 규정 준수 모드보다 조금 더 유연성이 필요할 때 사용한다.
~~~
- Most users can't overwrite or delete an object version or alter its lock settings
→ 대부분의 유저는 객체 버전을 덮어쓰거나 삭제하거나 로그 설정을 변경할 수 없다. 

- Some users have special permissions to change the retention or delete the object
→ 하지만 관리자 등 IAM을 통해 권한을 부여받은 유저는 보존 기간을 변경하거나 객체를 삭제할 수 있다.
~~~

- Retention Period: protect the object for a fixed period, it can be extended  
→ 두 보존 모드는 모두 보존 기간을 설정해야 한다. 이 기간은 원하는 만큼 연장할 수 있다.

- Legal Hold:  
→ 객체에 법적 보존 상태를 설정할 수 있다. 
~~~
- protect the object indefinitely, independent from retention period
→ 법적 보존을 설정하면 S3 버킷 내 모든 객체를 영구적으로 보호한다. 대개 재판에서 사용될 수 있는 중요한 객체에 법적 보존을 설정한다.

- can be freely placed and removed using the s3:PutObjectLegalHold IAM permission
→ s3:PutObjectLegalHold IAM 권한을 가진 사용자는 어떤 객체에서든 법적 보존을 설정하거나 제거할 수 있다.
~~~

- 회사에서 S3 버킷에 데이터와 파일을 저장하고 있습니다. 이 파일 중 일부는 회사 규정 준수 정책에 따라 정해진 기간 동안 보관해야 하며 덮어쓰거나 삭제되지 않도록 보호해야 합니다. 이런 경우에 사용할 수 있는 S3 기능은 무엇입니까?  
→ `S3` 객체 잠금 - 규정 준수 모드

- `S3` 객체 잠금 설정 중에서 객체 또는 객체 버전을 무기한 덮어쓰거나 삭제할 수 없도록 방지하고, 수동으로 삭제할 수 있는 기능만을 제공하는 설정은 무엇입니까?
→ 법적 보존

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
