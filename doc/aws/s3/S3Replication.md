## Amazon S3 – Replication
### 1. Amazon S3 – Replication (CRR & SRR)
- Must enable Versioning in source and destination buckets  
→ 복제를 위해서는 가장 먼저 소스 버킷과 복제 대상 버킷의 버전 관리 기능을 활성화해야 한다.

- `S3`의 복제에는 두 가지 종류가 있다.
~~~ 
- Cross-Region Replication (CRR)
→ 교차 리전으로 복제, 소스 버킷과 복제 대상 버킷 리전이 달라야 한다.

- Same-Region Replication (SRR)
→ 같은 리전으로 복제, 소스 버킷과 복제 대상 버킷 리전이 같아야 한다.
~~~

- Buckets can be in different AWS accounts  
→ 버킷은 서로 다른 `AWS` 계정간에도 사용할 수 있다.

- Copying is asynchronous  
→ 복제는 비동기식으로 이루어지며 백그라운드에서 실행된다.

- Must give proper IAM permissions to S3  
→ 복제 기능이 정상적으로 실행되려면 `S3`에 올바른 `IAM` 권한, 즉 읽기와 쓰기 권한을 `S3`에 부여해야 한다.

- Use cases:
~~~
- CRR – compliance, lower latency access, replication across accounts
→ 법규나 내부 체제를 관리하거나 데이터가 다른 리전에 있어 발생할 수 있는 지연 시간을 줄일 목적으로 사용한다.

- SRR – log aggregation, live replication between production and test accounts
→ 다수의 S3 버킷간의 로그를 통합할 때 개발 환경이 별도로 존재해 운영 환경과 개발 환경간의 실시간 복제를 필요로 할 때 사용한다.
~~~

- After you enable Replication, only new objects are replicated  
→ 복제를 활성화한 후에는 새로운 객체만 복제 대상이 된다.

- Optionally, you can replicate existing objects using S3 Batch Replication  
→ 기존의 객체를 복제하려면 `S3 배치 복제, S3 Batch Replication` 기능을 사용해야 한다.
~~~
- Replicates existing objects and objects that failed replication
→ 기존 객체부터 복제에 실패한 객체까지 복제할 수 있는 기능이다.
~~~

- For DELETE operations
~~~
- Can replicate delete markers from source to target (optional setting)
→ 소스 버킷에서 대상 버킷으로 삭제 마커를 복제할 수 있다.

- Deletions with a version ID are not replicated (to avoid malicious deletes)
→ 버전 ID가 존재하는 객체를 삭제하는 경우에는 버전 ID가 복제되지 않는다. 이는 영구적인 삭제이다. 악의적인 사용자가 한 버킷에서 다른 버킷으로 삭제 마커를 복제하면 안 되기 때문이다.
~~~

- There is no “chaining” of replication  
→ 체이닝 복제는 불가능하다.

- If bucket 1 has replication into bucket 2, which has replication into bucket 3, Then objects created in bucket 1 are not replicated to bucket 3  
→ 1번 버킷이 2번 버킷에 복제되어 있고 2번 버킷이 3번 버킷에 복제되어 있다고 해서 1번 버킷의 객체가 3번 버킷으로 복제되지는 않는다.

- `S3` → `Bucket` → `Origin Bucket` 선택 → `Management` → `Replication rules`  
→ 복제 규칙을 생성할 수 있다. `Destination`에 복제에 사용할 버킷을 설정한다. 설정을 완료하면 활성화된 시점 이후에 새로 업로드되는 객체만 복제된다.

- 대상 버킷의 소스 버킷에서 복제 활성화 이전의 객체를 복사하려면 `S3` 배치 작업 처리를 실행해야 한다.
- 복제 설정 시 `Delete marker replication` 옵션이 있다. 기본적으로 삭제 마커는 복제되지 않지만 복제되도록 설정하는 기능이다. 중요하다.
- 오리진 버킷에서 영구 객체를 삭제해도 복제 버킷에서는 삭제되지 않는다.

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---