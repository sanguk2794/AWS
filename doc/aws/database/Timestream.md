## Amazon Timestream
### 1. 시계열 데이터
- `시계열 데이터, time-series data`는 일정한 시간 동안 수집된 일련의 순차적으로 정해진 데이터 셋의 집합이다.
- 시계열 데이터의 특징은 시간에 관해 순서가 매겨져 있다는 점과, 연속한 관측치는 서로 상관관계를 갖고 있다는 것이다.
- 시계열 데이터의 분석 목적은 시계열이 갖고 있는 법칙성을 발견해 이를 모형화하고, 또 추정된 모형을 통하여 미래의 값을 예측하는 것에 있다.

### 1. Amazon Timestream
- Fully managed, fast, scalable, serverless time series database  
→ 시계열 데이터베이스이다. 완전 관리형이며 빠르고 확장성 있는 서버리스 서비스이다.

- Automatically scales up/down to adjust capacity  
→ 데이터베이스의 용량을 자동으로 확장하거나 축소할 수 있다.

- Store and analyze trillions of events per day  
→ 매일 수조 건의 이벤트를 저장 및 분석할 수 있다.

- 1000s times faster & 1/10th the cost of relational databases  
→ 시계열 데이터가 있을 때에는 관계형 데이터베이스보다 시계열 데이터베이스를 사용하는 것이 훨씬 빠르고 저렴하다.

- Scheduled queries, multi-measure records, SQL compatibility  
→ 쿼리를 예약하고 다중 척도 레코드를 얻을 수 있다. `SQL`과도 호환된다.

- Data storage tiering: recent data kept in memory and historical data kept in a cost-optimized storage  
→ 최신 데이터는 메모리에 저장되고 과거 데이터는 비용 효율적인 스토리지에 저장된다.

- Built-in time series analytics functions (helps you identify patterns in your data in near real-time)  
→ 시계열 분석 기능이 있어 거의 실시간으로 데이터를 분석하고 패턴을 찾을 수 있다.

- Encryption in transit and at rest  
→ `AWS`의 다른 데이터베이스들처럼 전송 중 암호화와 데이터 암호화를 지원한다.

- Use cases: IoT apps, operational applications, real-time analytics, …  
→ `IoT apps`, 운영 애플리케이션, 실시간 분석 등에 사용된다.

- Amazon Timestream – Architecture
~~~
- 시계열 데이터베이스는 AWS IoT에서 데이터를 받을 수 있고, Kinesis Data Streams의 데이터를 람다를 통해 받을 수도 있다. Prometheus, telegraf와도 통합이 가능하다.
- Timestream와 연결 가능한 것은 Amazon QuickSight, Amazon SagaMaker, Grafana, JDBC 커넥션 등과 연결 가능하다.
~~~

![image](https://user-images.githubusercontent.com/97398071/235907249-1f308732-9082-41ce-8bf4-1b0c6c21b0e5.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 한 스타트업에서 기후 변화로 인한 산불 발생을 줄이기 위해 새로운 프로젝트를 진행하고 있습니다. 업체에서는 개발 중인 센서를 숲 전체에 뿌려 산불이 발생하기 전에 이를 감지하는 데 도움이 될만 한 온도, 습도, 기압과 같은 데이터를 수집하려고 합니다. 업체는 매초 다양한 수치를 읽어 저장할 수 있는 수천 개의 센서를 생산할 예정입니다. 센서는 화재 발생 가능성을 예측할 수 있도록 읽어들인 수치를 저장하고 빠르게 분석할 수 있어야 합니다. 이런 경우 데이터 저장에 사용할 수 있는 `AWS` 서비스는 무엇입니까?  
→ `Amazon Timestream`

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---