## Amazon EKS
### 1. Amazon EKS Overview
- `Amazon EKS`를 이용해서 `AWS`에 컨테이너를 실행할 수 있다.

- Amazon EKS = Amazon Elastic Kubernetes Service  
→ `Amazon EKS`는 `Amazon Elastic Kubernetes Service`의 약자이다.

- It is a way to launch managed Kubernetes clusters on AWS  
→ `AWS`에 관리형 `Kubernetes` 클러스터를 실행할 수 있도록 해주는 서비스이다.

- Kubernetes is an open-source system for automatic deployment, scaling and management of containerized (usually Docker) application  
→ `Kubernetes`는 오픈 소스 시스템으로 `Docker`로 컨테이너화한 애플리케이션의 자동 배포, 확장, 관리를 지원한다.

- It’s an alternative to ECS, similar goal but different API  
→ 컨테이너를 실행한다는 목적은 `ECS`와 비슷하지만, 사용하는 `API`가 다르다. `ECS`는 오픈소스가 아닌 반면, `Kubernetes`는 오픈소스이고 여러 클라우드 제공자가 사용하므로 표준화를 기대할 수 있다.

- EKS supports EC2 if you want to deploy worker nodes or Fargate to deploy serverless containers  
→ `EKS`에는 `EC2` 시작 모드와 `Fargate` 모드의 두 가지 실행 모드가 있다. 

- Use case: if your company is already using Kubernetes on-premises or in another cloud, and wants to migrate to AWS using Kubernetes  
→ 회사가 온프레미스나 클라우드에서 `Kubernates`나 `Kubernates API`를 사용 중일 때 `Kubernates` 클러스터를 관리하기 위해 `Amazon EKS`를 사용한다.

- Kubernetes is cloud-agnostic (can be used in any cloud – Azure, GCP…)  
→ `Kubernetes`는 `cloud-agnostic`하다. `cloud-agnostic`하다는 것은 클라우드를 가리지 않는다는 의미이다. 따라서 클라우드 또는 컨테이너 간 마이그레이션을 실행하는 경우 간단한 솔루션이 될 수 있다.

- `ECS`와 비슷하나 이름에서 `Pod`가 관련되면 이는 `Amazon Kubernates`와 관련된 것이다.

- `EKS` 포드가 실행되는 `EKS` 노드는 `ASG`로 관리할 수 있다.

- `ECS`와 유사하게 `EKS` 서비스나 `Kubernates` 서비스를 노출할 때에는 퍼블릭 또는 프라이빗 로드 밸런서를 설정해 웹에 연결해야 한다.

![image](https://user-images.githubusercontent.com/97398071/235714518-3bb219ab-cb0d-42e0-87e6-a7a155552c9b.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. Amazon EKS – Node Types
- Managed Node Groups
~~~
- Creates and manages Nodes (EC2 instances) for you
→ 관리형 노드 그룹은 AWS로 노드, 즉 EC2 인스턴스를 생성하고 관리한다.

- Nodes are part of an ASG managed by EKS
→ 노드는 EKS 서비스로 관리되는 ASG 그룹의 일부이다.

- Supports On-Demand or Spot Instances
→ 온디맨드 인스턴스와 스팟 인스턴스를 지원한다.
~~~

- Self-Managed Nodes
~~~
- Nodes created by you and registered to the EKS cluster and managed by an ASG
→ 자체 관리형 노드 그룹은 직접 노드를 생성하고 EKS 클러스터에 등록한 다음 ASG 그룹의 일부로 관리해야 한다.

- You can use prebuilt AMI - Amazon EKS Optimized AMI
→ 사전 빌드된 AMI인 Amazon EKS 최적화 AMI를 사용하면 시간을 절약할 수 있다.

- Supports On-Demand or Spot Instances
→ 온디맨드 인스턴스와 스팟 인스턴스를 지원한다.
~~~

- AWS Fargate
~~~
- No maintenance required; no nodes managed
→ 노드를 원치 않을 경우 Amazon EKS가 지원하는 Fargate 모드를 선택한다. 유지 관리가 필요없고 노드를 관리하지 않아도 괜찮고 Amazon EKS에서 컨테이너만 실행하면 된다.
~~~

- `Amazon EKS`는 다음 중 ………………..를 제외한 나머지 노드 타입을 지원합니다.  
→ `AWS Lambda`

### 3. Amazon EKS – Data Volumes
- Need to specify StorageClass manifest on your EKS cluster  
→ `Amazon EKS` 클러스터에 데이터 볼륨을 연결하려면 `EKS` 클러스터에 스토리지 클래스 매니패스트를 지정해야 한다.

- Leverages a Container Storage Interface (CSI) compliant driver  
→ `EBS`, `FSx`, `EFS` 등 스토리지와 연결하기 위한 `컨테이너 스토리지 인터페이스, CSI`라는 규격 드라이버를 지원하는데 시험에 나올만한 키워드이다.

- Support for…
~~~
- Amazon EBS
- Amazon EFS (works with Fargate) - Fargate에서 동작한다.
- Amazon FSx for Lustre
- Amazon FSx for NetApp ONTAP
~~~

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---