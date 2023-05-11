## Amazon QuickSight
### 1. Amazon QuickSight
- Serverless machine learning-powered business intelligence service to create interactive dashboards  
→ `Amazon QuickSight`는 서버리스 머신 러닝 기반 `BI` 서비스이다. 대화형 대시보드를 생성해준다.

- Fast, automatically scalable, embeddable, with per-session pricing  
→ 웹사이트에 임베드할 수 있으며 세션당 비용을 지불한다.

- Use cases:
~~~
- Business analytics
→ 비즈니스 분석

- Building visualizations
→ 시각화 구현

- Perform ad-hoc analysis
→ 시각화된 정보를 통한 임시 분석

- Get business insights using data
→ 데이터를 활용한 비즈니스 인사이트 획득
~~~

- Integrated with RDS, Aurora, Athena, Redshift, S3…  
→ `RDS`, `Aurora`, `Athena`, `Redshift`, `S3` 등 다양한 데이터 소스에 연결할 수 있다.

- In-memory computation using SPICE engine if data is imported into QuickSight  
→ 인 메모리 연산 엔진인 `SPICE` 엔진을 사용한다. 이 엔진은 `Amazon QuickSight`로 데이터를 직접 가져올 때 사용되며 `Amazon QuickSight`가 다른 `DB`에 연결되어 있을때는 작동하지 않는다.

- Enterprise edition: Possibility to setup Column-Level security (CLS)  
→ `Amazon QuickSight`는 훌륭한 사용자 수준 기능을 제공한다. `Amazon QuickSight`의 엔터프라이즈 에디션에는 액세스 권한이 없는 사용자에게 일부 열이 표시되지 않도록 열 수준 보안을 설정할 수 있다.

- 한 전자 상거래 기업은 주문 내역, 고객 정보, 이익, 전년도 매출과 같은 모든 과거 데이터를 `Redshift` 클러스터에 호스팅하고 있습니다. 전년도 이익과 총매출액을 표시하는 대시보드 및 보고서를 생성해야 한다는 요구 사항이 있었기 때문에, 내년에도 같은 요구 사항이 있을 것으로 보입니다. `DevOps` 팀은 이와 같은 대시보드를 정의할 수 있고 기본적으로 `Redshift`와 통합이 가능한 AWS 서비스를 찾는 업무를 맡았습니다. 가장 적합한 `AWS` 서비스는 무엇입니까?  
→ `Amazon QuickSight`

### 2. QuickSight Integrations
- `Amazon QuickSight`는 `AWS`의 다양한 데이터 소스와 통합할 수 있다.
- `RDS`, `Aurora`, `Redshift`, `Athena`, `S3`, `OpenSearch`, `Timestream`...
- 타사 데이터소스와 통합도 가능하다. 단, `Amazon QuickSight`가 지원하는 서비스형 소프트웨어로 그 대상은 제한된다.
- 타사 데이터베이스와의 통합도 가능하다.
- `Amazon QuickSight`로 직접 `Excel`, `CSV`, `JSON` 등의 파일을 업로드할 수 있다.
- 시험에서는 `Amazon QuickSight`를 `Redshift`나 `Athena`와 함께 사용하는 문제가 자주 등장한다.

![image](https://user-images.githubusercontent.com/97398071/235960416-7e9f4c4d-fac7-45b1-b83f-2a8668ecbeeb.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. QuickSight – Dashboard & Analysis
- `Amazon QuickSight`에는 크게 세 개의 개념이 있다. 대시보드와 분석, 그리고 사용자가 그 것이다.

- Define Users (standard versions) and Groups (enterprise version)
~~~
- 스탠다드 버전에서는 사용자를 정의할 수 있고 그룹은 엔터프라이즈 버전에서만 사용할 수 있다.
- These users & groups only exist within QuickSight, not IAM !!
→ QuickSight의 사용자 그룹은 QuickSight 서비스 전용이다. IAM 사용자와는 다르다. IAM 사용자는 관리용으로만 사용된다.
~~~

- A dashboard…
~~~
- is a read-only snapshot of an analysis that you can share
→ 대시보드는 읽기 전용 스냅샷이며 분석 결과를 공유할 수 있다.

- preserves the configuration of the analysis (filtering, parameters, controls, sort)
→ 또 분석의 구성을 저장한다. 분석을 위해 설정한 필터 또는 매개변수 제어, 정렬 옵션 등이 저장되어 대시보드에 표시된다. 
~~~

- You can share the analysis or the dashboard with Users or Groups  
→ 특정 사용자 또는 그룹과 분석 결과나 대시보드를 공유할 수 있다.

- To share a dashboard, you must first publish it  
→ 일단 대시보드부터 게시해야 한다. 

- Users who see the dashboard can also see the underlying data  
→ 액세스 권한이 있는 사용자는 기본 데이터를 볼 수도 있다. 

- `Amazon QuickSight`는 분석 및 대시보드를 생성해야 하며 특정 사용자나 그룹과 공유할 수 있다. 

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---