## Amazon OpenSearch Service
### 1. Amazon OpenSearch Service
- Amazon OpenSearch is successor to Amazon ElasticSearch  
→ `Amazon OpenSearch Service`는 `Amazon ElasticSearch`의 후속 서비스이다. 라이선스 문제로 이름이 변경되었다.

- In DynamoDB, queries only exist by primary key or indexes…  
→ `DynamoDB`는 기본 키나 데이터베이스의 인덱스로만 데이터를 쿼리할 수 있다. 

- With OpenSearch, you can search any field, even partially matches  
→ 이에 반해 `Amazon OpenSearch Service`는 부분적으로 일치하는 필드를 포함해 모든 필드를 검색할 수 있다.

- It’s common to use OpenSearch as a complement to another database  
→ `Amazon OpenSearch Service`는 애플리케이션에서 검색 기능을 제공할 때 많이 사용되고 일반적으로 다른 데이터베이스를 보완하는데 사용된다.

- OpenSearch requires a cluster of instances (not serverless)  
→ `Amazon OpenSearch Service`를 생성하고 사용하려면 인스턴스의 클러스터를 생성해야 한다. 서버리스 서비스가 아니다.

- Does not support SQL (it has its own query language)  
→ 자체 쿼리 언어가 있어 `SQL`을 지원하지 않는다.

- Ingestion from Kinesis Data Firehose, AWS IoT, and CloudWatch Logs  
→ `Kinesis Data Firehose`, `AWS IoT`, `CloudWatch Logs`나 사용자 지정 애플리케이션의 데이터를 주입할 수 있다.

- Security through Cognito & IAM, KMS encryption, TLS  
→ `Cognito`와 `IAM`과 통합해 제공하는 보안을 통해 저장 데이터 암호화와 전송 중 암호화가 가능하다.

- Comes with OpenSearch Dashboards (visualization)  
→ `OpenSearch Dashboards`로 `OpenSearch` 데이터를 시각화할 수 있다.

### 2. OpenSearch patterns: DynamoDB
- 굉장히 흔한 패턴이다.
- 데이터가 포함된 `DynamoDB`가 있고 사용자가 이 데이터를 삽입, 삭제, 업데이트한다.
- 모든 데이터를 `DynamoDB Stream`으로 전송하면 람다 함수가 받아 `Amazon OpenSearch`에 실시간으로 데이터를 삽입한다.
- 이러한 과정을 통해 애플리케이션은 특정 항목을 찾을 수 있게 된다. 예를 들면 항목의 이름을 부분 검색해 항목 `ID`를 알아낼 수 있다.
- 항목 `ID`를 얻은 후에는 `DynamoDB`를 호출해 `DynamoDB` 테이블에서 전체 항목을 검색한다.

![image](https://user-images.githubusercontent.com/97398071/235952286-c58c6467-c1dc-4ffa-8c00-ce0f13ffd1d2.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. OpenSearch patterns: CloudWatch Logs
- `CloudWatch Logs`로 데이터를 주입할 수도 있다.
- 실시간으로 데이터를 전송하는 `Subscription Filter`를 사용하여 람다 함수에 데이터를 전송하고, 람다 함수는 `Amazon OpenSearch`에 실시간으로 데이터를 전송한다.
- 아니면 `CloudWatch Logs`에서 `Subscription Filter`으로 데이터를 송신한 다음 `Kinesis Data Firehose`로 `Subscription Filter`의 데이터를 읽고 `Amazon OpenSearch`에 거의 실시간으로 데이터를 주입할 수도 있다.

![image](https://user-images.githubusercontent.com/97398071/235952484-fb202412-0cca-4b73-af43-438db72832f1.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 4. OpenSearch patterns: Kinesis Data Streams & Kinesis Data Firehose
- `Kinesis Data Streams`의 스트림을 `Amazon OpenSearch`로 송신할 때 두 가지 전략이 있다.
- 첫 번째 전략은 `Kinesis Data Firehose`이다. `Kinesis Data Firehose`는 근 실시간 서비스이다. 람다 함수를 사용해 일부 데이터를 변환한 다음에 `Amazon OpenSearch`로 데이터를 송신한다.
- 두 번째 전략은 `Kinesis Data Streams`에서 람다 함수를 생성하고 커스텀 코드를 작성해서 람다 함수가 `Amazon OpenSearch`에 실시간으로 데이터를 쓰게 하는 것이다.

![image](https://user-images.githubusercontent.com/97398071/235952701-4482e5da-2221-4491-862a-fba49e912589.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---