## DocumentDB
### 1. DocumentDB
- Aurora is an “AWS-implementation” of PostgreSQL / MySQL …  
→ `Aurora`로 `AWS`에서 `PostgreSQL` 또는 `MySQL`의 대규모 클라우드 네이티브 버전을 구현하는 것과 비슷하다.

- DocumentDB is the same for MongoDB (which is a NoSQL database)  
→ `DocumentDB`는 `MongoDB`용 `Aurora`라고 생각하면 편하다. `MongoDB`는 `NoSQL` 데이터베이스이다. 따라서, `DocumentDB`도 `NoSQL` 데이터베이스이다.

- MongoDB is used to store, query, and index JSON data  
→ `MongoDB`는 `JSON` 데이터를 저장, 쿼리, 인덱싱하는데 사용된다.

- Similar “deployment concepts” as Aurora  
→ `Aurora`와 같은 배포 개념이 존재한다.

- Fully Managed, highly available with replication across 3 AZ  
→ 완전 관리형 데이터베이스로 가용성이 높다. 데이터는 3개의 가용 영역에 복제된다.

- DocumentDB storage automatically grows in increments of 10GB, up to 64 TB.  
→ 스토리지는 자동으로 `10GB` 단위로 최대 `64TB`까지 증가한다.

- Automatically scales to workloads with millions of requests per seconds  
→ `DocumentDB`는 초당 수백만 건의 요청이 있는 워크로드로 확장될 수 있도록 설계되었다. 만약 시험을 볼 때 `MongoDB` 키워드를 확인한다면 `DocumentDB`를 떠올리면 된다. 그리고 만약 `NoSQL` 관련 내용이 보인다면 `DocumentDB`와 더불어 `DynamoDB`를 떠올려야 한다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---