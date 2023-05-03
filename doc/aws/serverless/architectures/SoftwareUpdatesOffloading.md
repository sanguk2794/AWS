## Software updates offloading
### 1. Software updates offloading
- We have an application running on EC2, that distributes software updates once in a while  
→ `EC2`에서 작동하는 애플리케이션이 있고 종종 소프트웨어 업데이트를 배포한다.

- When a new software update is out, we get a lot of request and the content is distributed in mass over the network. It’s very costly  
→ 소프트웨어가 새로 업데이트되면 요청을 많이 받게 된다. 그래서 컨텐츠는 네트워킹을 통해 다수에게 배포되는데 비용이 많이 든다.

- We don’t want to change our application, but want to optimize our cost and CPU, how can we do it?  
→ 애플리케이션을 변경하거나 아키텍팅을 다시 하는 일 없이 비용과 `CPU`를 최적화하고 싶을 때 `Software updates offloading`를 사용한다.

### 2. Our application current state
- 전형적인 `ELB`와 `ASG` 개발 애플리케이션이 있다. 이 애플리케이션은 다중 `AZ`에 걸쳐 작동된다.
- 간단하게 소프트웨어 업데이트 모두 `EFS`에 위치한다고 가정한다.

![image](https://user-images.githubusercontent.com/97398071/235878072-845fe576-cd58-4078-a28c-29ba8e9de1be.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. Easy way to fix things!
- 앞에 `CloudFront`를 사용하는 것만으로 많은 비용을 절감할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/235878185-420a3457-a0f7-47d5-bd4c-ece014cf1c95.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 4. Why CloudFront?
- No changes to architecture  
→ 아키텍처에는 변화가 없다.

- Will cache software update files at the edge  
→ 엣지에서 소프트웨어 업데이트 파일은 캐시에 저장된다.

- Software update files are not dynamic, they’re static (never changing)  
→ 동적이 아니라 정적이라서 절대 바뀌지 않는다.

- Our EC2 instances aren’t serverless, But CloudFront is, and will scale for us  
→ `EC2` 인스턴스는 서버리스가 아니지만 `CloudFront`는 서버리스라서 확장한다.

- Our ASG will not scale as much, and we’ll save tremendously in EC2  
→ `ASG`가 많이 확장하지 않아 `EC2`와 네트워크, `EFS` 비용을 크게 절감할 수 있다.

- We’ll also save in availability, network bandwidth cost, etc  
→ 또, 가용성을 확보할 수 있다. 

- Easy way to make an existing application more scalable and cheaper!  
→ 캐싱을 활용하는 정적 컨텐츠가 대부분이라면 `CloudFront`가 기존 애플리케이션의 확장성을 높이고 비용을 절감하는데 용이하다는 것은 기억해야 한다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---