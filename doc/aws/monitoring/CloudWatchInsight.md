## CloudWatch Insights
### 1. CloudWatch Container Insights
- Collect, aggregate, summarize metrics and logs from containers  
→ 컨테이너로부터 지표와 로그를 수집, 집계, 요약하는 서비스이다. `CloudWatch Container Insights`를 사용하면 컨테이너로부터 지표와 로그를 손쉽게 추출해서 세분화된 대시보드를 생성할 수 있다.

- Available for containers on…
~~~
- Amazon Elastic Container Service (Amazon ECS)
- Amazon Elastic Kubernetes Services (Amazon EKS)
- Kubernetes platforms on EC2
- Fargate (both for ECS and EKS)
~~~

- In Amazon EKS and Kubernetes, CloudWatch Insights is using a containerized version of the CloudWatch Agent to discover containers  
→ `CloudWatch Container Insights`를 `Amazon EKS`나 `Kubernetes`에서 사용하는 경우 컨테이너화된 버전의 `CloudWatch` 에이전트를 사용해야 컨테이너를 찾을 수 있다.

![image](https://user-images.githubusercontent.com/97398071/236637254-e308a3d5-7087-400a-ae99-69400ee23119.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. CloudWatch Lambda Insights 
- Monitoring and troubleshooting solution for serverless applications running on AWS Lambda  
→ `AWS Lambda`에서 실행되는 서버리스 애플리케이션을 위한 모니터링과 트러블 슈팅 솔루션이다.

- Collects, aggregates, and summarizes system-level metrics including CPU time, memory, disk, and network  
→ `CPU` 시간, 메모리, 디스크, 네트워크와 같은 정보를 포함한 시스템 수준의 지표를 수집, 집계, 요약할 수 있다.

- Collects, aggregates, and summarizes diagnostic information such as cold starts and Lambda worker shutdowns  
→ `cold starts`나 `Lambda worker shutdowns`와 같은 정보를 포함한 지표를 수집, 집계, 요약할 수 있다.

- Lambda Insights is provided as a Lambda Layer  
→ `Lambda` 함수 옆에서 실행되며 `Lambda Insights`라는 대시보드를 생성해 `Lambda` 함수의 성능을 모니터링한다는 것만 알면 된다.

- 서버리스 애플리케이션의 세부 모니터링이 필요할 때 사용한다.

### 3. CloudWatch Contributor Insights
- Analyze log data and create time series that display contributor data.  
→ `Contributor`의 데이터를 표시하는 시계열 데이터를 생성하고 로그를 분석하는 서비스이다.

- See metrics about the top-N contributors  
→ 상위 `N`개의 `contributors`를 확인할 수 있다.

- The total number of unique contributors, and their usage.  
→ 총 `contributors`의 수나 사용량을 확인할 수 있다.

- This helps you find top talkers and understand who or what is impacting system performance.  
→ 이 서비스는 네트워크의 상위 대화자를 찾고 시스템 성능에 영향을 미치는 대상을 파악할 수 있다.

- Works for any AWS-generated logs (VPC, DNS, etc..)  
→ `VPC` 로그, `DNS` 로그 등 `AWS`가 생성하는 모든 로그에서 동작한다.

- For example, you can find bad hosts, identify the heaviest network users, or find the URLs that generate the most errors.  
→ 예를 들면 불량 호스트를 식별해 낼 수 있다. 네트워크 로그나 `VPC` 로그를 확인해서 사용량이 가장 많은 네트워크 사용자를 찾을 수 있다. 또, `DNS` 로그에서는 오류를 가장 많이 생성하는 `URL`을 찾을 수 있다.

- 사용량이 많은 네트워크 사용자를 식별하는 방법은 다음과 같다. 상위 `N`명의 기고자, 기여자를 확인한다면 `CloudWatch Contributor Insights`를 생각하자.
~~~
- 모든 네트워크 요청에 대해 VPC 내에서 생성되는 로그인 VPC 플로우 로그가 CloudWatch Logs로 전달된다.
- CloudWatch Contributor Insights가 이를 분석한다.
- 이를 통해 VPC에 트래픽을 생성하는 상위 N개의 IP 주소를 찾을 수 있다.
- 찾은 주소를 기반으로 좋은 사용자인지 나쁜 사용자인지 판단한다.
~~~

![image](https://user-images.githubusercontent.com/97398071/236638215-9517f448-480a-4540-9579-3155c8e1e9e5.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- You can build your rules from scratch, or you can also use sample rules that AWS has created – leverages your CloudWatch Logs  
→ 규칙은 직접 생성하거나, `AWS`가 생성한 간단한 규칙을 활용할 수 있다.

- CloudWatch also provides built-in rules that you can use to analyze metrics from other AWS services.  
→ 내장된 규칙이 있어 다른 `AWS` 서비스에서 가져온 지표도 분석할 수 있다.

### 4. CloudWatch Application Insights
- Provides automated dashboards that show potential problems with monitored applications, to help isolate ongoing issues  
→ 모니터링하는 애플리케이션의 잠재적 문제와 진행중인 문제를 분리할 수 있도록 자동화된 대시보드를 제공한다.

- Your applications run on Amazon EC2 Instances with select technologies only(Java, .NET, Microsoft IIS Web Server, databases…)  
→ `Java`, `.NET` 등 웹 서버나 특정 데이터베이스를 선택해 선택한 기술로만 실행할 수 있다.

- And you can use other AWS resources such as Amazon EBS, RDS, ELB, ASG, Lambda, SQS, DynamoDB, S3 bucket, ECS, EKS, SNS, API Gateway…  
→ `EBS`, `RDC`, `ELB`, `ASG`, `Lambda`, `SQS`, `DynamoDB`, `S3` 버킷, `ECS 클러스터`, `EKS 클러스터`, `SNS` `API Gateway` 등의 `AWS` 리소스에 연결된다.

- Powered by SageMaker  
→ 자동화된 대시보드를 생성할 때 내부적으로 `SageMaker` 서비스가 활용된다. 

- Enhanced visibility into your application health to reduce the time it will take you to troubleshoot and repair your applications  
→ 이를 통해 애플리케이션의 상태 가시성을 높일 수 있어, 트러블 슈팅이나 애플리케이션 보수 시간을 줄일 수 있다.

- Findings and alerts are sent to Amazon EventBridge and SSM OpsCenter  
→ 발견된 문제와 알림은 모두 `Amazon EventBridge`와 `SSM OpsCenter`로 전달된다.

### 5. CloudWatch Insights and Operational Visibility
- 개괄적으로만 알면 된다.

- CloudWatch Container Insights
~~~
- ECS, EKS, Kubernetes on EC2, Fargate, needs agent for Kubernetes
- Metrics and logs
→ ECS, EKS, Kubernetes on EC2, Fargate로부터 오는 로그와 지표를 위한 것이며, Kubernetes를 사용한다면 실행할 에이전트가 필요하다.
~~~

- CloudWatch Lambda Insights
~~~
- Detailed metrics to troubleshoot serverless applications
→ AWS Lambda에서 실행되는 서버리스 애플리케이션의 트러블 슈팅을 위한 세부 지표를 제공한다.
~~~

- CloudWatch Contributors Insights
~~~
- Find “Top-N” Contributors through CloudWatch Logs
→ CloudWatch Logs를 통해 상위 N개의 기고자를 찾을 수 있다.
~~~

- CloudWatch Application Insights
~~~
- Automatic dashboard to troubleshoot your application and related AWS services
→ 자동화된 대시보드를 생성해 사용하는 애플리케이션과 관련된 AWS 서비스나 애플리케이션의 트러블 슈팅을 위한 세부 지표를 제공한다.
~~~




---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---