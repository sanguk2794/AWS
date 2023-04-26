## Elastic Beanstalk
### 1. Developer problems on AWS
- 개발자로서 인프라를 관리하고 코드를 배포하는 것은 복잡하다. 모든 데이터와 로드 밸런서 등을 구성하고 싶지는 않을 것이다.
~~~
- Managing infrastructure
- Deploying Code
- Configuring all the databases, load balancers, etc
~~~

- Scaling concerns  
→ 물론 모든 것들이 확장 가능해야 한다.

- Most web apps have the same architecture (ALB + ASG)  
→ 대부분의 웹 애플리케이션은 로드 밸런서와 오토 스케일링 그룹이 있는 동일한 아키텍쳐를 가진다.

- All the developers want is for their code to run!  
→ 개발자로서 원하는 것은 코드가 잘 실행되는 것 뿐이다. 다른 것들에 대해서는 걱정하고 싶지 않다.

- Possibly, consistently across different applications and environments  
→ 또, 여러 프로그래밍 언어로 개발할 때 여러 다른 애플리케이션과 환경이 있다면 아마도 애플리케이션을 배포하는 단 하나의 방법만을 원할 것이다.

- 이러한 때 사용할 수 있는 것이 `Elastic Beanstalk`이다.

### 2. Elastic Beanstalk – Overview
- Elastic Beanstalk is a developer centric view of deploying an application on AWS  
→ `Beanstalk`는 `AWS`에서 애플리케이션을 배포하는 개발자 중심의 관점이다.

- It uses all the component’s we’ve seen before: EC2, ASG, ELB, RDS, …  
→ 단 하나의 인터페이스로 지금까지 봤었던 `EC2`, `ASG`, `ELB`, `RDS` 등 모든 구성 요소를 재사용하는 것이다.

- Managed service  
→ 이 모든 것을 배포하는 관리 서비스가 되어 준다.
~~~
- Automatically handles capacity provisioning, load balancing, scaling, application health monitoring, instance configuration, …
→ 용량 프로비저닝, 로드 밸런서의 구성, 확장, 애플리케이션 상태 모니터링, 인스턴스 구성 등을 처리해 주는 것이다.

- Just the application code is the responsibility of the developer
→ 개발자로써 신경써야 할 것은 코드 그 자체일 뿐이다.
~~~
- We still have full control over the configuration  
→ 여전히 각각의 구성 요소를 완전히 제어할 수 있지만 `Beanstalk`에서는 하나의 인터페이스로 통합되어 있다.

- Beanstalk is free but you pay for the underlying instances  
→ `Beanstalk` 서비스 자체는 무료이지만 `Beanstalk`가 활용할 기본 인스턴스들이나 `ASG`, `ELB` 등에 대한 비용은 지불해야 한다.

- 플랫폼을 선택하고 애플리케이션 코드를 선택한 후, 애플리케이션 실행에 필요한 모든 기능이 포함된 완전 관리형 플랫폼이 생성된다.
`EC2` 인스턴스, `ELB`, `ASG` 등의 설정을 일일히 구성할 필요가 없다.

### 3. Elastic Beanstalk – Components
- Application: collection of Elastic Beanstalk components (environments, versions, configurations, …)  
→ `Beanstalk`의 구성 요소 중 하나는 애플리케이션이다. 이는 환경과 버전 그리고 구성을 가진 `Beanstalk` 구성 요소들의 묶음으로 구성되어 있다.

- Application Version: an iteration of your application code  
→ 애플리케이션 버전은 애플리케이션 코드의 반복이다.

- Environment  
→ 환경은 특정 애플리케이션 버전을 실행하는 리소스의 모음이다.
~~~
- Collection of AWS resources running an application version (only one application version at a time)
→ 환경 내에는 단 하나의 애플리케이션 버전만 존재할 수 있다.

- Tiers: Web Server Environment Tier & Worker Environment Tier
→ Beanstalk에서는 서로 다른 두 개의 티어를 가질 수 있다. 웹 서버 환경 티어와 작업자 환경 티어가 있다.

- You can create multiple environments (dev, test, prod, …)
→ 생각하는 어떤 환경이든 다수의 환경을 만들 수 있다.
~~~

![image](https://user-images.githubusercontent.com/97398071/234600966-f294556f-c4a4-47ee-b3bf-de58e524f242.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 애플리케이션을 개발 중에 있으며, 최소 비용을 사용해 이 애플리케이션을 `Elastic Beanstalk`으로 배포하려 합니다. 이를 위해서는, 애플리케이션을 .................. 에서 실행해야 합니다.  
→ 단일 인스턴스 모드, 단일 인스턴스 모드는 하나의 `EC2` 인스턴스와 하나의 `Elastic IP`를 생성한다.

- 애플리케이션을 `Elastic Beanstalk`으로 배포하던 도중, 배포 프로세스가 극도로 느리다는 것을 알게 되었습니다. 로그를 검토한 결과, 종속성이 매 배포 당 각 EC2 인스턴스로 리졸브된다는 사실을 발견했습니다. 영향을 최소화하면서 배포 프로세스를 빠르게 하려면 어떻게 해야 할까요?  
→ 종속성을 포함하는 `Golden AMI`를 생성해 그 이미지를 `EC2` 인스턴스 실행에 사용

#### 1. Web Server Tier vs. Worker Tier
- `Web Server Tier`는 클라이언트가 직접 접근한다. 
- 이와 달리 `Worker Tier`는 이와 다르게 클라이언트가 직접 `EC2` 인스턴스에 접근하지 않는다. 메시지 대기열인 `SQS` 대기열을 사용한다.
- 훌륭한 점은 `Web Server Tier`에서 `SQS` 대기열로 메시지를 푸시하도록 함으로써 `Web Server Tier`와  `Worker Tier`가 함께 할 수 있다는 점이다.

![image](https://user-images.githubusercontent.com/97398071/234601459-3b144fe7-3bd4-40c1-aae5-4458e24c0275.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 4. Elastic Beanstalk – Supported Platforms
- Go
- Java SE
- Java with Tomcat
- .NET Core on Linux
- .NET on Windows Server
- Node.js
- PHP
- Python
- Ruby
- Packer Builder
- Single Container Docker
- Multi-container Docker
- Preconfigured Docker
- If not supported, you can write your custom platform (advanced)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
