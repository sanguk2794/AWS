## Disaster Recovery
### 1. Disaster Recovery Overview
- 재해 복구를 다루는 문제가 시험에 잘 나올 뿐만 아니라, 솔루션 아키텍트라면 재해 복구를 꼭 알아둬야 한다. 아주 중요하다.

- Any event that has a negative impact on a company’s business continuity or finances is a disaster  
→ 여기서 재해란 회사의 사업 지속이나 재정에 부정적인 영향을 미치는 이벤트를 말이다.

- Disaster recovery (DR) is about preparing for and recovering from a disaster  
→ 재해 복구는 이러한 재해에 대비하고 재해 발생 시 복구하는 작업을 의미한다.

- What kind of disaster recovery?
~~~
- On-premise => On-premise: traditional DR, and very expensive
→ 우선 온프레미스 간 재해 복구 실행이 가능하다. 전형적인 재해 복구 유형으로 비용이 아주 많이 든다.

- On-premise => AWS Cloud: hybrid recovery
→ 아니면 클라우드를 사용할 수도 있다. 온프레미스를 기본 데이터 센터로 두고 재해 발생 시 클라우드를 사용하는 방식을 하이브리드 복구라고 한다.

- AWS Cloud Region A => AWS Cloud Region B
→ 모두 클라우드에 있는 경우에 재해 복구를 수행하면 완전 클라우드 유형이 된다.
~~~

- Need to define two terms:
~~~
- 핵심 용어 두 개는 다음과 같다.

- RPO: Recovery Point Objective
→ RPO는 복구 시점 목표를 의미한다.

- RTO: Recovery Time Objective
→ RTO는 복구 시간 목표를 의미한다.
~~~

### 2. RPO and RTO
- `RPO, 복구 시점 목표`는 얼마나 자주 백업을 실행할지 시간상 어느 정도 과거로 되돌릴 수 있는지를 결정하는 것을 말한다.  
→ 재해가 일어나면 `RPO`와 재해 발생 시점 사이에 데이터 손실이 발생한다. `RPO`는 한 시간, 1분 등 원하는 대로 설정할 수 있다. 데이터 손실을 얼마나 감수할 지 설정하는 것이다.

- 반면 `RTO`는 재해 발생 후 복구 시간을 결정한다.  
→ 재해 발생 시점과 `RTO`의 시간 차는 애플리케이션 다운타임이 된다. `RTO`는 한 시간, 1분 등 원하는 대로 설정할 수 있다. 서버 다운을 얼마나 감수할 지 설정하는 것이다.

- `RPO`와 `RTO`의 시간 간격이 짧을수록 비용이 높아지기 때문에 `RPO`와 `RTO`의 적정선을 찾아 최적화는 아주 중요하다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/62c8c7f0-5c3c-4d50-a5c5-5c61f913b9f1)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. Disaster Recovery Strategies
- Backup and Restore  
→ 백업 및 복구가 있다.

- Pilot Light  
→ 파일럿 라이트가 있다.

- Warm Standby  
→ 웜 대기가 있다.

- Hot Site / Multi Site Approach  
→ 핫 사이트 또는 다중 사이트 접근이 있다.

- 백업 및 복구는 `RTO`가 작은 반면 파일럿 라이트와 웜 대기 다중 사이트는 `RTO`가 빠르다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/a1de0b1c-573c-42f9-8aba-9f2026cecf55)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 1. Backup and Restore (High RPO)
- 기업 데이터 센터와 `AWS Cloud` 및 `S3` 버킷이 있다고 가정했을 때, 시간에 따라 데이터를 백업하고 싶다면 `AWS Storage Gateway`를 사용할 수 있다.
 
- 수명 주기 정책을 만들어 비용 최적화 목적으로 `Glacier`에 데이터를 입력할 수 있다.

- `AWS Snowball`을 사용해 일주일에 한 번씩 많은 양의 데이터를 `Glacier`로 전송할 수도 있다.  
→ `Snowball`을 사용하면 `RPO`는 대략 일주일이 되는데, 데이터 센터의 데이터가 전부 사라진다면 `Snowball` 장치로 일주일에 한 번만 보냈기에 한 주 치 데이터를 통째로 잃게 된다.

- 클라우드의 AMI, 스냅샷을 백업에 활용할 수도 있다.
~~~
- RPO - EBS 볼륨과 Redshift, RDS 등 AWS Cloud를 정기적으로 스냅샷을 예약하고 백업해 두면 스냅샷을 만드는 간격에 따라 RPO가 달라진다.

- RTO - 재해가 발생하면 모든 데이터를 복구해야 하므로 AMI를 사용해서 EC2 인스턴스를 다시 만들고 스냅샷을 통해 Amazon RDS, EBS 볼륨, Redshift 등을 복원해야 한다.
→ 데이터 복구는 시간이 오래 걸리므로 RTO도 커진다.

- COST - 값이 저렴하기 때문에 백업 및 복구 전략으로 자주 사용된다. 재해 발생 시 인프라를 재생산할 수 있으므로 백업 저장 비용 외에는 따로 돈이 들지 않기 때문이다.
~~~

- 정리하자면 백업 및 복구는 아주 쉽고 비용이 저렴한 대신 `RPO`와 `RTO`가 높다.

- ![image](https://github.com/sanguk2794/AWS/assets/97398071/01c4c3a7-a8de-451d-9162-3d55fafd70ff)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 2. Disaster Recovery – Pilot Light
- A small version of the app is always running in the cloud  
→ 애플리케이션 축소 버전이 클라우드에서 항상 실행된다. 예를 들면 크리티컬 데이터베이스에서 `RDS`로 데이터를 계속 복제한다면 언제든 실행할 수 있는 `RDS` 데이터베이스가 확보된다.

- Useful for the critical core (pilot light)  
→ 이 애플리케이션의 축소 버전이 보통 크리티컬 시스템이 되는데 이를 파일럿 라이트라고 한다.

- Very similar to Backup and Restore  
→ 백업 및 복구와 아주 비슷하다.

- Faster than Backup and Restore as critical systems are already up  
→ 하지만, 크리티컬 시스템이 작동하고 있기 때문에 복구할 때 여타 시스템만 재기동하면 되므로 상대적으로 `RTO`가 빠르다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/e91fe254-d55a-4cc0-ac42-e5076119e133)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 3. Warm Standby
- Full system is up and running, but at minimum size  
→ 웜 대기는 시스템 전체를 실행하되 최소한의 규모로 가동해서 대기하는 방법이다.

- Upon disaster, we can scale to production load  
→ 재해가 발생하면 프로덕션 로드로 확장할 수 있다.

- 비용이 더 드는대신 `RPO`와 `RTO`는 줄어든다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/c474253c-195f-42fe-9ab1-dab4fa1457be)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 4. Multi Site / Hot Site Approach
- 핫 사이트는 실행중인 대기 상태 사이트를 원격지에 실행해 두고 해당 사이트의 데이터를 최신으로 유지한다.

- 멀티 사이트는 복수개의 프로덕션 환경을 원격지에 실행해 모두 액티브 상태로 둔 채 실시간 서비스를 제공한다.

- Very low RTO (minutes or seconds) – very expensive  
→ 몇 분, 몇 초 정도로 `RTO`가 낮지만 비용이 굉장히 많이 든다.

- Full Production Scale is running AWS and On Premise  
→ `AWS`와 온프레미스에서 완전 프로덕션 스케일을 가진다. 이미 실행 중인 핫 사이트가 있기 때문에 `Route 53`가 기업 데이터 센터와 `AWS Cloud`에  요청을 라우팅할 수 있는데 이를 액티브-액티브 유형 설정이라고 한다.

- ![image](https://github.com/sanguk2794/AWS/assets/97398071/a205c7a7-fba7-4235-ac8b-7e0369424dce)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 5. Disaster Recovery Tips
- Backup 
~~~
- EBS Snapshots, RDS automated backups / Snapshots, etc… 
→ 백업에는 EBS 스냅샷과 RDS로 자동화된 스냅샷과 백업 등을 사용한다.

- Regular pushes to S3 / S3 IA / Glacier, Lifecycle Policy, Cross Region Replication 
→ S3, S3 IA, Glacier 등에 스냅샷을 규칙적으로 푸시할 수 있다. 백업이 다른 리전에 있길 바란다면 리전 간 복제도 가능하다.

- From On-Premise: Snowball or Storage Gateway 
→ 온프레미스에서 클라우드로 데이터 공유 시에는 Snowball과 Storage Gateway가 유용하다.
~~~

- High Availability 
~~~
- Use Route53 to migrate DNS over from Region to Region 
→ 고가용성을 위해서는 Route 53를 사용해 DNS를 다른 리전으로 옮기면 충분하다.

- RDS Multi-AZ, ElastiCache Multi-AZ, EFS, S3 
→ 다중 AZ를 실행하는 기술도 이용할 수 있다. RDS 다중 AZ, 일래스티 캐시 다중 AZ EFS, S3 등이 포함된다.

- Site to Site VPN as a recovery from Direct Connect 
→ 기업 데이터 센터에서 AWS로 연결할 때 Direct Connect를 사용할 경우 연결이 끊겼을 때를 대비하여 Site-to-Site VPN을 네트워크 복구 옵션으로 사용할 수 있다.
~~~

- Replication 
~~~
- RDS Replication (Cross Region), AWS Aurora + Global Databases 
→ RDS 리전 간 복제를 사용하거나 오로라 글로벌 데이터베이스를 사용할 수 있다.

- Database replication from on-premises to RDS 
→ 온프레미스 데이터베이스를 RDS로 복제한다면 데이터베이스 복제 소프트웨어를 쓸 수 있다.

- Storage Gateway 
→ Storage Gateway를 사용할 수 있다.
~~~

- Automation 
~~~
- CloudFormation / Elastic Beanstalk to re-create a whole new environment 
→ 아시다시피 CloudFormation과 일래스틱 Beanstalk를 사용하면 클라우드에 새로운 환경을 빠르게 재생산할 수 있다.

- Recover / Reboot EC2 instances with CloudWatch if alarms fail 
→ CloudWatch를 사용한다면 CloudWatch 경보가 실패했을 때 EC2 인스턴스를 복구하거나 다시 시작할 수 있다.

- AWS Lambda functions for customized automations 
→ AWS Lambda는 사용자 맞춤 자동화에 유용하다. AWS 인프라 전체를 자동화할 때 효과적이다. 여러분이 재해 복구 전체를 자동화하는 데 성공한다면 아주 큰일을 해낸 것이다.
~~~

- Chaos 
~~~
- 재해 대응을 확인하기 위해서는 직접 재해를 만들어서 대처해 봐야 한다.

- Netflix has a “simian-army” randomly terminating EC2
- 요즘 AWS에서 자주 인용되는 사례는 넷플릭스이다. 모든 걸 AWS에서 실행하고 Simian Army를 만들어 EC2 인스턴스를 무작위로 종료하고 동작을 확인한다. 어떤 장애가 발생해도 인프라가 무사하도록 무작위 서비스를 종료하는 것이다.
~~~

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---