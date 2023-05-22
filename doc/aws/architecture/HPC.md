## High Performance Computing (HPC)
### 1. High Performance Computing (HPC)
- The cloud is the perfect place to perform HPC  
→ 클라우드는 고성능 컴퓨팅을 실행하기에 최적화되어 있다.

- You can create a very high number of resources in no time  
→ 많은 리소스를 즉각적으로 생성할 수 있기 때문이다.

- You can speed up time to results by adding more resources  
→ 리소스를 추가해서 결과 추출 시간을 단축할 수 있다.

- You can pay only for the systems you have used  
→ 또 사용량만큼만 비용을 지불하면 된다. 작업이 끝나고 나면 전체 인프라를 제거해서 요금이 청구되지 않게 할 수 있다.

- Perform genomics, computational chemistry, financial risk modeling, weather prediction, machine learning, deep learning, autonomous driving  
→ 유전체학이나 컴퓨터화학 금융 위험 모델링, 기상 예측, 머신 러닝, 딥 러닝 자율 주행 등에 사용된다.

### 2. Data Management & Transfer
- `HPC`를 위한 데이터 관리 및 전송에 도움을 주는 서비스들이 있다.

- AWS Direct Connect : Move GB/s of data to the cloud, over a private secure network  
→ `AWS Direct Connect`를 통해 프라이빗 보안 네트워크를 통해 초당 `GB`의 속도로 클라우드로 데이터를 전송할 수 있다.

- Snowball & Snowmobile : Move PB of data to the cloud  
→ `Snowball`과 `Snowmobile`을 사용하면 물리적 라우팅을 통해 클라우드로 `PB` 단위 데이터를 옮길 수 있다. 주로 대용량 전송에 사용된다.

- AWS DataSync : Move large amount of data between on-premises and S3, EFS, FSx for Windows  
→ `DataSync`는 `DataSync 에이전트`를 설치해 대용량의 데이터를 전송한다. 온프레미스, `NFS`, `SMB` 시스템에서 `S3`, `EFS`나 `Windows용 FSx`로 전송할 수 있다.

### 3. Compute and Networking
- EC2 Instances:
~~~
- CPU optimized, GPU optimized
→ 실행하려는 작업에 따라 CPU나 GPU에 최적화된 인스턴스를 선택해야 한다.

- Spot Instances / Spot Fleets for cost savings + Auto Scaling
→ 스팟 인스턴스나 스팟 플릿(Fleet)을 사용해 비용을 크게 절약하고 수행하는 계산을 기반으로 플릿을 오토 스케일링할 수 있다.
~~~

- EC2 Placement Groups: Cluster for good network performance  
→ `EC2` 인스턴스가 서로 통신해야 하거나 분배된 형태로 동작할 경우 클러스터 유형의 `EC2` 배치 그룹을 사용하면 최고의 네트워크 성능을 발휘한다. 모두 같은 `AZ` 내에 존재하기 때문이다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/ef4ecb90-0bd0-474b-b2e8-d95e30be8c4d)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- EC2 Enhanced Networking (SR-IOV)
~~~
- EC2 Enhanced Networking는 SR-IOV라고도 불린다.

- Higher bandwidth, higher PPS (packet per second), lower latency
→ 넓은 대역폭, 높은 PPS가 제공되므로 지연 시간이 짧아진다.

- Option 1: Elastic Network Adapter (ENA) up to 100 Gbps
→ ENA를 사용하면 네트워크 속도를 100Gbps까지 올릴 수 있다. 중요하다.

- Option 2: Intel 82599 VF up to 10 Gbps – LEGACY
→ Intel의 82599VF라는 걸 사용해 최대 10Gbps까지 올릴 수 있다. 과거 버전의 ENA이다.
~~~

- Elastic Fabric Adapter (EFA)
~~~
- Improved ENA for HPC, only works for Linux
→ HPC, 고성능 컴퓨팅을 위해 개선된 ENA이다. Linux에서만 사용 가능하다.

- Great for inter-node communications, tightly coupled workloads
→ 노드 간 소통이나 밀집된 워크 로드 처리에 뛰어난 성능을 보여준다.

- Bypasses the underlying Linux OS to provide low-latency, reliable transport
→ Linux 인스턴스가 있고 많은 워크로드를 처리해야 할 경우 EFA를 사용하면 OS를 우회하여 보다 높은 네트워크 성능을 얻을 수 있다.
~~~

- 시험에서 흔히 물어보는 것은 `ENA`와 `EFA`와 `ENI` 등의 차이점이다.

- 고성능 컴퓨팅(`HPC`)을 수행하기 위해 `EC2` 인스턴스를 클러스터 배치 그룹으로 배포했습니다. `EC2` 인스턴스들 간의 네트워크 성능을 최대화하려 합니다. 무엇을 사용해야 할까요?
→ `Elastic Fabric Adapter (EFA)`

### 4. Storage
- 데이터를 저장하는 방법이다.

- Instance-attached storage:
~~~
- 인스턴스가 연결된 스토리지를 사용할 수 있다.

- EBS: scale up to 256,000 IOPS with io2 Block Express
→ EBS의 경우 io2 Block Express로 256,000 IOPS까지 확장 가능하다.

- Instance Store: scale to millions of IOPS, linked to EC2 instance, low latency
→ 인스턴스 스토어의 경우 수백만의 IOPS까지 확장할 수 있다. EC2와 연결되어 하드웨어에 있고 지연 시간이 짧지만 인스턴스가 망가지면 손상될 수 있다.
~~~

- Network storage:
~~~
- Amazon S3: large blob, not a file system
→ 대용량 블롭 데이터를 저장할 때 사용된다. 쉽게 말하면 큰 객체 저장을 위해 사용된다.

- Amazon EFS: scale IOPS based on total size, or use provisioned IOPS
→ EFS를 사용하면 IOPS가 파일 시스템의 전체 크기에 따라 확장된다. 프로비저닝된 IOPS 모드를 써서 EFS에서 높은 IOPS를 얻기도 한다.

- Amazon FSx for Lustre:
- HPC optimized distributed file system, millions of IOPS
- Backed by S3
→ HPC 전용 파일 시스템이다. HPC에 최적화되어 수백만의 IOPS를 제공하며 백엔드에서 S3로 제공된다.
~~~

### 5. Automation and Orchestration
- 자동화 및 오케스트레이션에 대한 내용이다.

- AWS Batch
~~~
- AWS Batch supports multi-node parallel jobs, which enables you to run single jobs that span multiple EC2 instances.
→ AWS Batch를 사용하면 다중 노드 병렬 작업을 수행할 수 있고 여러 EC2 인스턴스에 걸쳐 작업할 수 있다.

- Easily schedule jobs and launch EC2 instances accordingly
→ AWS Batch를 사용하면 작업 예약이 가능하며, AWS Batch 서비스로 관리되어 EC2 인스턴스 실행이 쉬워진다.
~~~

- AWS ParallelCluster
~~~
- Open-source cluster management tool to deploy HPC on AWS
→ AWS ParallelCluster는 오픈 소스 클러스터 관리 도구로 HPC를 AWS에 배포할 때 사용된다.

- Configure with text files
→ 텍스트 파일로 설정한다.

- Automate creation of VPC, Subnet, cluster type and instance types
→ 그리고 VPC와 서브넷 및 클러스터 타입, 인스턴스 타입 생성을 자동화할 수 있다.

- Ability to enable EFA on the cluster (improves network performance)
→ AWS ParallelCluster는 EFA와 함께 사용한다. 클러스터 상에서 EFA를 활성화하는 매개변수가 텍스트 파일에 존재한다. 중요하다.
~~~

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---