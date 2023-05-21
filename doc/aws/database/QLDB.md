## Amazon QLDB
### 1. Amazon QLDB
- QLDB stands for ”Quantum Ledger Database”  
→ `QLDB`는 `Quantum Ledger Database`의 약자이다.

- A ledger is a book recording financial transactions  
→ `ledger, 원장`은 금융 트랜잭션을 기록하는 장부이다. 따라서 `QLDB`는 금융 트랜잭션 원장을 갖게 된다.

- Fully Managed, Serverless, High available, Replication across 3 AZ  
→ 완전 관리형 데이터베이스이며 서버리스이고 가용성이 높고, 3개의 가용 영역에 걸쳐 데이터를 복제할 수 있다.

- Used to review history of all the changes made to your application data over time  
→ 애플리케이션의 데이터의 시간에 따른 모든 변경 내용을 검토하는데 사용된다.

- Immutable system: no entry can be removed or modified, cryptographically verifiable  
→ 불변 시스템이다. 데이터베이스에 무언가를 쓰면 삭제하거나 수정할 수 없다. 정말로 아무것도 삭제되지 않았는지 확인하기 위해 암호화 서명을 하기도 한다. 금융 트랜잭션에서 유용하다. 어떤 금융 데이터도 데이터베이스에서 사라지게 하고 싶지 않을 가능성이 높기 때문이다.

- 2-3x better performance than common ledger blockchain frameworks, manipulate data using SQL  
→ 일반 원장 블록체인 프레임워크보다 2 ~ 3배 더 나은 성능을 얻을 수 있다. 또, `SQL`을 사용해 데이터를 관리할 수도 있다.

- Difference with Amazon Managed Blockchain: no decentralization component, in accordance with financial regulation rules  
→ `Amazon` 관리형 블록체인이라는 다른 데이터베이스도 있다. `QLDB`와 관리형 블록체인의 차이점은, 관리형 블록체인은 탈중앙화 요소가 있는 것에 비해 `QLDB`에는 탈중앙화 개념이 없다는 것이다. `QLDB`에는 중앙 권한 구성요소만 존재한다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---