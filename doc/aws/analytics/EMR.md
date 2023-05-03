## Amazon EMR
### 1. Amazon EMR
- EMR stands for “Elastic MapReduce”  
→ `EMR`은 `Elastic MapReduce`의 약자이다.

- EMR helps creating Hadoop clusters (Big Data) to analyze and process vast amount of data  
→ `EMR`은 빅데이터 작업을 위한 하둡 클러스터 생성에 사용된다. 방대한 양의 데이터를 분석하고 처리할 수 있다. 하둡 클러스터와 빅데이터가 `EMR`의 키워드이다.
 
- The clusters can be made of hundreds of EC2 instances  
→ 하둡 클러스터는 프로비저닝해야 하며 수백 개의 `EC2` 인스턴스로 구성될 수 있다.

- EMR comes bundled with Apache Spark, HBase, Presto, Flink…  
→ `EMR`은 빅데이터 전문가가 사용하는 여러 도구가 함께 제공된다. 

- EMR takes care of all the provisioning and configuration  
→ `EMR`이 상기 서비스에 대한 프로비저닝과 구성을 대신 처리해준다.

- Auto-scaling and integrated with Spot instances  
→ 전체 클러스터를 자동 확장할 수 있고 스팟 인스턴스와 통합되므로 가격 할인 혜택을 받을수도 있다.

- Use cases: data processing, machine learning, web indexing, big data  
→ 데이터 처리와 기계학습, 웹 인덱싱 등에 사용되며 모든 작업은 하둡, `Spark`, `Hbase`같은 빅데이터 관련 기술을 사용한다.

- ………………………는 데이터 엔지니어와 분석가가 클러스터를 운영하거나 관리할 필요 없이 `Apache Spark`, `Hive`, `Presto`와 같은 오픈 소스 빅데이터 프레임워크로 구축된 애플리케이션을 쉽고 경제적으로 실행할 수 있게 해줍니다.
→ `Amazon EMR`

### 2. Amazon EMR – Node types & purchasing
- `EMR`은 `EC2` 인스턴스의 클러스터로 구성되며, 여러 노드 유형이 있다.

- Master Node: Manage the cluster, coordinate, manage health – long running  
→ 마스터 노드는 클러스터를 관리하고 다른 모든 노드의 상태를 조정하며 장기 실행된다.

- Core Node: Run tasks and store data – long running  
→ 태스크를 실행하고 데이터를 저장한다. 장기 실행된다.

- Task Node (optional): Just to run tasks – usually Spot  
→ 테스트만 실행하는 노드이다. 대게 스팟 인스턴스를 사용하며 태스크 노드의 사용은 선택 사항이다.

- Purchasing options:
~~~
- On-demand: reliable, predictable, won’t be terminated
→ 온디맨드 EC2 인스턴스 유형을 사용하면 신뢰할 수 있고 예측 가능한 유형의 워크로드를 얻을 수 있다.

- Reserved (min 1 year): cost savings (EMR will automatically use if available)
→ 예약 인스턴스를 사용하면 최소 1년을 사용해야 한다. 하지만 비용을 크게 절약할 수 있다. 가능한 경우에 `EMR`은 자동으로 예약 인스턴스를 사용한다. 
장기 실행해야 하는 마스터노드와 코어노드에 적합한 인스턴스 가격 옵션이다.

- Spot Instances: cheaper, can be terminated, less reliable
→ 스팟 인스턴스는 종료될 수 있으므로 신뢰도는 떨어지지만 저렴하다. 따라서 스팟 인스턴스는 태스크 노드에 활용하는 것이 좋다.
~~~

- Can have long-running cluster, or transient (temporary) cluster  
→ `EMR`에서 배포할 때에는 장기 실행 클러스터에서 예약 인스턴스를 사용하거나 임시 클러스터를 사용해 특정 작업을 수행하고 분석 완료 후에 삭제할 수 있다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---