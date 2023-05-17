## Amazon ECS
### 1. Amazon ECS
- ECS = Elastic Container Service  
→ `Amazon Elastic Container Service, Amazon ECS`는 컨테이너화된 애플리케이션을 쉽게 배포, 관리, 스케일링할 수 있도록 도와주는 완전 관리형 컨테이너 오케스트레이션 서비스이다.

### 2. Amazon ECS - Launch Type
- 컨테이너를 실행하는 데 사용할 수 있는 시작 유형에는 `EC2` 시작 유형과 `Fargate`의 두 가지 유형이 존재한다.
~~~
- EC2 시작 유형
→ 클러스터에서 EC2 인스턴스를 구성하고 배포하여 컨테이너를 실행한다.

- Fargate
→ 서버리스로 인프라를 관리할 필요 없이 컨테이너를 실행할 수 있다.
~~~

#### 1. Amazon ECS - EC2 Launch Type
- EC2 시작 유형은 클러스터에서 EC2 인스턴스를 구성하고 배포하여 컨테이너를 실행한다. 즉, Amazon EC2 인스턴스에서 컨테이너화된 애플리케이션을 실행할 수 있다.

- EC2 Launch Type: you must provision & maintain the infrastructure (the EC2 instances)  
→ `EC2 Launch Type`을 사용하면 같이 기동되는 `EC2` 인스턴스 등 인프라의 프로비저닝과 유지보수를 직접 수행해야 한다.

- Each EC2 Instance must run the ECS Agent to register in the ECS Cluster  
→ 각각의 `EC2` 인스턴스에 `ECS Agent`를 실행해야 한다. 이 `EC2` 인스턴스는 `ECS Cluster` 위에서 동작한다.

![image](https://user-images.githubusercontent.com/97398071/235605621-cdaec1d5-a659-4e23-9938-1e51aa599562.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 2. Amazon ECS – Fargate Launch Type
- You do not provision the infrastructure (no EC2 instances to manage)  
→ 인프라를 프로비저닝하지 않아 관리할 서버가 없다.

- It’s all Serverless!  
→ 서버리스이다. 서버리스는 서버를 관리하지 않는다는 의미이지 서버가 없다는 의미는 아니다.

- You just create task definitions  
→ 태스크 정의를 생성하는 것만으로 충분하다. 새 도커 컨테이너를 실행하면 어디서 실행되는지 알리지 않고 그냥 실행된다.

- To scale, just increase the number of tasks. Simple - no more EC2 instances  
→ 태스크 수를 늘리는 것 만으로 확장이 가능하다. 시험에서는 `EC2` 시작 유형보다 서버 관리가 쉽기 때문에 서버리스인 `Fargate`를 사용하라는 문제가 자주 등장한다.

### 3. Amazon ECS – IAM Roles for ECS
- EC2 Instance Profile (EC2 Launch Type only):  
→ `EC2` 시작 유형의 `ECS`를 사용할 경우에만 설정한다. `EC2 Instance Profile`을 사용하면 `EC2` 인스턴스에 `IAM` 역할을 전달할 수 있다.
~~~
- Used by the ECS agent
→ EC2 인스턴스 프로파일을 생성한다. ECS Agent만이 EC2 인스턴스의 프로파일을 사용한다.

- Makes API calls to ECS service
→ EC2 인스턴스의 프로파일을 이용해 API를 호출한다.

- Send container logs to CloudWatch Logs
→ 인스턴스에 저장된 ECS 서비스가 CloudWatch에 컨테이너 로그를 송신한다.

- Pull Docker image from ECR
→ ECR로부터 도거 이미지를 가져온다.

- Reference sensitive data in Secrets Manager or SSM Parameter Store
→ Secrets Manager나 SSM Parameter Store에서 민감 데이터를 참고하기도 한다.
~~~

- ECS Task Role:  
→ `EC2` 시작 유형, `Fargate` 시작 유형 전부 설정해야 한다. 태스크 `IAM` 역할이다. `IAM` 설정이니 햇갈리지 말자.
~~~
- Allows each task to have a specific role
→ 두 개의 태스크가 있다면 각자에 특정 역할을 맡길 수 있다.

- Use different roles for the different ECS Services you run
→ 역할을 다르게 하는 이유는 역할이 각자 다른 서비스에 연결할 수 있게 하기 위해서이다.

- Task Role is defined in the task definition
→ ECS 서비스의 태스크 정의에서 태스크 역할을 정의한다.
~~~

- `EC2` 인스턴스 프로파일의 역할과 `ECS` 태스크 역할의 차이점을 이해하는 것은 중요하다.  
→ `ECS` 태스크 역할은 `Amazon ECS` 컨테이너 에이전트 및 `Fargate` 에이전트에 사용자를 대신하여 `AWS API` 호출을 수행할 권한을 부여할 때 사용한다. 태스크의 요구 사항에 따라 태스크 실행 `IAM` 역할이 필요하다.

### 4. Amazon ECS – Load Balancer Integrations
- `ELB`를 통해 `EC2` 인스턴스에 접근하는 것과 비슷하게 `ELB`를 통해 `Task`에 접근할 수 있다.

- `HTTP`나 `HTTPS` 엔드 포인트로 태스크를 활용할 때 로드밸런서를 사용할 수 있다.

- Application Load Balancer supported and works for most use cases  
→ `ALB`는 대부분 사용사례를 지원하는 매우 좋은 옵션이다.

- Network Load Balancer recommended only for high throughput / high performance use cases, or to pair it with AWS Private Link  
→ 처리량이 매우 많거나 높은 성능이 요구될 때 권장된다. 추후에 언급될 `AWS Private Link`와 사용할 때도 권장된다.

- Elastic Load Balancer supported but not recommended (no advanced features – no Fargate)  
→ 구세대의 `ELB`는 사용할 수는 있지만 권장하지 않는다. 고급 기능들을 지원하지 않으며 `Fargate`에 연결할 수도 없기 때문이다.

![image](https://user-images.githubusercontent.com/97398071/235696796-a0a93eff-15d7-467d-9a8b-3cc3cf6fa6c6.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 5. Amazon ECS – Data Volumes (EFS)
- 데이터를 지속하기 위해 `Data Volumes`가 필요하다. 그리고 `ECS`의 `Data Volumes`으로 가장 많이 사용되는 것은 `EFS`이다.

- Mount EFS file systems onto ECS tasks  
→ `EC2` 태스크에 파일 시스템을 마운트해서 데이터를 공유하려고 한다.

- Works for both EC2 and Fargate launch types  
→ `NFS`는 `EC2`와 `Fargate` 시작 유형에 모두 호환된다.

- Tasks running in any AZ will share the same data in the EFS file system  
→ 어느 `AZ`에서 실행되는 태스크든 `EFS`에 연결되어 있다면 데이터를 공유할 수 있고 원한다면 파일 시스템을 통해 다른 태스크와도 연결할 수 있다.

- Fargate + EFS = Serverless  
→ `Fargate`로 `ECS` 태스크를 실행하고 파일 시스템 지속성을 위해 `Amazon EFS`를 사용하는게 가장 좋은 이용 사례이다.

- Use cases: persistent multi-AZ shared storage for your containers  
→ `EFS`와 `ECS`를 함께 사용해서 다중 `AZ`에서 공유하는 컨테이너, 영구 스토리지를 만들 수 있다.

- Note: Amazon S3 cannot be mounted as a file system  
→ `S3`는 파일 시스템으로 마운트될 수 없다.

### 6. ECS Service Auto Scaling
- Automatically increase/decrease the desired number of ECS tasks  
→ 태스크의 수를 자동으로 늘리거나 줄일 수 있다.

- Amazon ECS Auto Scaling uses AWS Application Auto Scaling  
→ 세 개의 지표에 대해 확장이 가능하다.
~~~
- ECS Service Average CPU Utilization
→ ECS 서비스의 CPU 사용률

- ECS Service Average Memory Utilization - Scale on RAM
→ ECS 서비스의 RAM 등 메모리 사용률

- ALB Request Count Per Target – metric coming from the ALB
→ ALB 관련 지표인 타겟 당 요청수
~~~

- Target Tracking – scale based on target value for a specific CloudWatch metric  
→ 대상 추적 스케일링은 특정 타겟을 추적한다.

- Step Scaling – scale based on a specified CloudWatch Alarm  
→ 단계 스케일링은 `CloudWatch`의 알람을 기준으로 스케일링을 설정한다.

- Scheduled Scaling – scale based on a specified date/time (predictable changes)  
→ 예약 스케일링은 미리 `ECS` 스케일링을 설정한다.

- ECS Service Auto Scaling (task level) ≠ EC2 Auto Scaling (EC2 instance level)  
→ `EC2` 시작 유형이라면 태스크 레벨에서의 `ECS` 서비스 확장이 `EC2` 인스턴스 클러스터의 확장과 다르다는 것을 인지해야 한다.

- Fargate Auto Scaling is much easier to setup (because Serverless)  
→ 그렇기에 `Fargate`를 사용하는 것이 `ASG` 설정시에도 훨씬 편리하다.

### 7. EC2 Launch Type – Auto Scaling EC2 Instances
- Accommodate ECS Service Scaling by adding underlying EC2 Instances  
→ `EC2` 시작 타입을 `ECS` 타입으로 설정했을 경우 오토 스케일링을 구현하기 위해 추가적인 설정이 필요하다.

- Auto Scaling Group Scaling
~~~
- Scale your ASG based on CPU Utilization, Add EC2 instances over time
→ CPU 사용률에 따라 ASG를 확장하도록 설정하면 CPU 사용률이 급등할 때 EC2 인스턴스를 추가할 수 있다.
~~~

- ECS Cluster Capacity Provider - 이 쪽이 추천된다.
~~~
- Used to automatically provision and scale the infrastructure for your ECS Tasks
→ ECS 클러스터 용량 공급자는 아주 똑똑하다. 새 태스크를 실행할 용량이 부족하면 자동으로 ASG를 확장한다.

- Capacity Provider paired with an Auto Scaling Group
→ 용량 공급자는 ASG와 함께 사용된다.

- Add EC2 Instances when you’re missing capacity (CPU, RAM…)
→ RAM이나 CPU가 모자랄 때 자동으로 EC2 인스턴스를 추가해준다.
~~~

- `Fargate` 유형이라면 외부 `ECS Cluster Capacity Provider`에 대한 설정이 필요하지 않다.

![image](https://user-images.githubusercontent.com/97398071/235706440-5d46824b-0676-46a2-bba9-e87e1beefe2e.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 8. ECS Solution Architect
#### 1. ECS tasks invoked by Event Bridge
- 이 아키텍처는 도커 컨테이너를 이용해 `S3` 버킷으로부터 이미지나 객체를 처리하는 서버리스 아키텍처이다.
~~~
- 사용자가 S3 버켓에 이미지를 업로드한다. 이 때 S3와 통합되어 있는 Amazon EventBridge를 통해 ECS 태스크를 실행한다. 
Amazon EventBridge는 ECS 태스크를 실행하는 규칙을 생성할 수 있다.

- ECS 태스크가 생성되고 ECS의 태스크 역할이 주어진다. 이 태스크는 객체를 받아 처리하고 Amazon DynamoDB에 결과를 보내게 된다.
~~~
![image](https://user-images.githubusercontent.com/97398071/235706995-55aaee52-99d0-4196-8592-0da36156b700.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 2. ECS tasks invoked by Event Bridge Schedule
- 이 아키텍처는 도커 컨테이너를 이용해 한 시간마다 배치 처리를 하는 아키텍처이다.
~~~
- Amazon EventBridge에 한 시간마다 트리거되는 규칙적인 일정을 생성한다.

- 이를 받은 Fargate는 ECS 태스크를 실행한다. 한 시간마다 Fargate 클러스터에 새 태스크가 생성된다.

- 태스크에는 Amazon S3에 접근 가능한 ECS 태스크 역할을 생성한다. 처리 결과를 S3에 전송한다.
~~~

![image](https://user-images.githubusercontent.com/97398071/235708185-2258797a-f888-43a9-9dc5-35840e19f3aa.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 3. ECS – SQS Queue Example
~~~
- ECS 서비스에 두 개의 태스크가 있고, 이 두 태스크는 SQS 대기열에서 메세지를 가져와서 처리한다.

- 여기에 ECS 서비스 오토 스케일링을 활성화하면 SQS 대기열에 메세지가 많아질수록 ECS 서비스에 태스크가 많아진다.
~~~

![image](https://user-images.githubusercontent.com/97398071/235709130-be563dcc-6645-484d-b480-2645d1d556e0.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
