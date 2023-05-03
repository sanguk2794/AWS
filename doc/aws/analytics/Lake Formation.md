## AWS Lake Formation
### 1. AWS Lake Formation
- `AWS Lake Formation`는 데이터 레이크 생성을 돕는다.

- Data lake = central place to have all your data for analytics purposes  
→ 데이터 레이크는 데이터 분석을 위해 모든 데이터를 한 곳으로 모아주는 중앙 집중식 저장소이다.

- Fully managed service that makes it easy to setup a data lake in days  
→ `Lake Formation`은 데이터 레이크 생성을 수월하게 해 주는 완전 관리형 서비스이다. 보통 수개월이 걸리는 작업을 며칠만에 완료할수도 있다.

- Discover, cleanse, transform, and ingest data into your Data Lake  
→ `Lake Formation`는 데이터 검색, 정제, 변환, 주입을 돕는다. 

- It automates many complex manual steps (collecting, cleansing, moving, cataloging data, …) and de-duplicate (using ML Transforms)  
→ 데이터 수집, 정제나 카탈로깅, 복제같은 복잡한 수작업을 자동화하고 기계 학습 변환 기능으로 중복제거를 수행한다.

- Combine structured and unstructured data in the data lake  
→ 데이터 레이크에는 정형 데이터와 비정형 데이터 소스를 결합할 수 있다.

- Out-of-the-box source blueprints: S3, RDS, Relational & NoSQL DB…  
→ 블루프린트를 제공할 수 있다. 내장된 블루프린트는 데이터를 데이터 레이크로 이전하는 것을 도와주며, `S3`, `RDS`, `Relational & NoSQL DB` 등에서 지원된다.

- Fing-grained Access Control for your applications (row and column-level)  
→ `Lake Formation`을 설정하는 이유는 모든 데이터를 한곳에서 처리하는 것 외에도 애플리케이션에서 행, 열 수준의 세분화된 액세스 제어를 할 수 있기 때문이다.

- Built on top of AWS Glue  
→ `Lake Formation`은 `AWS Glue` 위에 빌드되는 계층이긴 하지만 `Glue`와 직접 상호작용하지는 않는다.

![image](https://user-images.githubusercontent.com/97398071/235967188-5ea469b7-166e-4418-bb8f-cba748366462.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- `RDS`, `S3` 버킷에 데이터가 저장되어 있고, 데이터에 대한 분석 작업을 수행할 수 있도록 `AWS Lake Formation`을 데이터 레이크로 사용해 수집, 이동 및 분류하고 있습니다. 회사에는 빅데이터 개발자와 `ML` 엔지니어가 아주 많기 때문에 민감 정보가 포함되었을 수도 있는 일부 데이터에 대해서는 접근을 제한하려고 합니다. 이런 경우 무엇을 사용해야 합니까?  
→ `AWS Lake Formation` 세분화된 액세스 제어

### 2. AWS Lake Formation: Centralized Permissions Example
- `Lake Formation`을 사용하는 이유는 바로 중앙화된 권한이다. 중요하다.
- 회사가 분석에 `Athena`와 `QuickSight`를 사용할 때 사용자는 허용된 데이터만 볼 수 있어야 하고 읽기 권한이 있어야 한다.
- 데이터 소스는 `S3`, `RDS`, `Aurora` 등이다.
- `Athena`에 보안을 설정하거나 `QuickSight`에서 사용자 수준의 보안을 설정할 수 있다. `S3` 버킷 정책이나 `IAM` 정책에 보안 설정을 추가할 수도 있다.
- 이렇게 보안을 관리할 곳이 많아지면 엉망이 되버린다. 이를 해결할 수 있는 방법이 `Lake Formation`이다. 액세스 제어 기능과 열 및 행 수준 보안이 있기 때문이다.

![image](https://user-images.githubusercontent.com/97398071/235967351-fe22ab2c-9eac-4bfa-916e-69179492276e.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---