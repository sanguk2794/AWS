## MGN
### 1. AWS Application Discovery Service
- 온프레미스 서버나 데이터 센터의 데이터를 클라우드로 마이그레이션할 때에는 마이그레이션 계획이 필요하다.  
→ `AWS Application Discovery` 서비스로 마이그레이션을 계획하는 것이다. 
 
- Plan migration projects by gathering information about on-premises data centers  
→ 사내 데이터 센터에 대한 정보를 수집하여 마이그레이션 프로젝트 계획에 사용할 수 있다.

- Server utilization data and dependency mapping are important for migrations  
→ 서버를 스캔하고 마이그레이션에 중요한 서버 설치 데이터 및 종속성 매핑에 대한 정보를 수집하면 어떻게 마이그레이션을 수행할지, 무엇을 먼저 마이그레이션해야 할 지 파악할 수 있다.

- 마이그레이션에는 두 가지 방법이 있다.

- Agentless Discovery (AWS Agentless Discovery Connector)
~~~
- Agentless Discovery는 Connector를 사용한다.

- VM inventory, configuration, and performance history such as CPU, memory, and disk usage
→ 가상 머신, 구성, CPU와 메모리 및 디스크 사용량과 같은 성능 기록에 대한 정보를 제공한다.
~~~

- Agent-based Discovery (AWS Application Discovery Agent)
~~~
- System configuration, system performance, running processes, and details of the network connections between systems
→ 가상 머신 내에서 더 많은 업데이트와 정보를 얻을 수 있다. 예를 들어, 시스템 구성, 성능, 실행 중인 프로세스, 시스템 사이의 네트워크 연결에 대한 세부 정보 등을 얻을 수 있다. 종속성 매핑을 얻는 데 도움을 준다.
~~~

- Resulting data can be viewed within AWS Migration Hub  
→ 이 모든 결과 데이터는 `AWS Migration Hub`라는 서비스에서 확인할 수 있다.

- `Application Discovery` 서비스는 이동해야 할 항목과 그것들이 내부적으로 어떻게 상호 연결되어 있는지 파악하기에 정말 유용하다.  

### 2. AWS Application Migration Service (MGN)
- 온프레미스에서 `AWS`로의 마이그레이션에 사용하는 가장 간단한 방법은 `MGN, AWS Application Migration Service`을 사용하는 것이다.
 
- The “AWS evolution” of CloudEndure Migration, replacing AWS Server Migration Service (SMS)  
→ 예전에는 클라우드 마이그레이션이라고 불렸었다. 이름이 바뀌었다.

- Lift-and-shift (rehost) solution which simplify migrating applications to AWS  
→ 리호스팅을 할 수 있다. `Lift-and-shift` 솔루션이라고도 불린다. 

- Converts your physical, virtual, and cloud-based servers to run natively on AWS  
→ 물리적, 가상, 또는 클라우드에 있는 다른 서버를 `AWS` 클라우드 네이티브로 실행되도록 변환한다.

- Supports wide range of platforms, Operating Systems, and databases  
→ 이 방법은 광범위한 플랫폼, 운영 체제, 데이터베이스를 지원한다.

- Minimal downtime, reduced costs  
→ 다운타임이 짧고 비용도 절감된다. 서비스가 자동으로 수행하기에 관련 엔지니어를 고용할 필요가 없기 때문이다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/e0a14905-7726-4fc3-bf27-076f95af6b5b)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---