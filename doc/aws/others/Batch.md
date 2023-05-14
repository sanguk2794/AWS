## AWS Batch
### 1. AWS Batch
- Fully managed batch processing at any scale  
→ `Batch`는 완전 관리형 배치 처리 서비스로, 어떤 규모의 배치라도 처리할 수 있다.

- Efficiently run 100,000s of computing batch jobs on AWS  
→ `Batch` 서비스를 사용하면 `AWS`에서 수십만 개의 컴퓨팅 배치 작업을 매우 쉽고 효율적으로 실행할 수 있다.

- A “batch” job is a job with a start and an end (opposed to continuous)  
→ 배치 작업은 시작과 끝이 있는 작업이다. 결코 끝나지 않는 스트리밍 작업은 배치 작업에 포함되지 않는다.

- Batch will dynamically launch EC2 instances or Spot Instances  
→ `Batch` 서비스는 배치 작업을 로드하기 위해 동적으로 `EC2` 인스턴스 또는 스팟 인스턴스를 기동한다.

- AWS Batch provisions the right amount of compute / memory  
→ `Batch`는 배치 대기열을 처리할 수 있도록 적절한 양의 컴퓨팅 및 메모리를 프로비저닝한다.

- You submit or schedule batch jobs and AWS Batch does the rest!  
→ 배치 작업을 배치 대기열에 올리거나 예약하기만 하면 `Batch` 서비스가 나머지 작업을 수행한다.

- Batch jobs are defined as Docker images and run on ECS  
→ 배치 잡에는 도커 이미지와 `ECS` 서비스에서 실행되는 작업 정의가 포함된다. `ECS`에서 실행될 수 있는 것이라면 배치에서도 실행될 수 있다.

- Helpful for cost optimizations and focusing less on the infrastructure  
→ 작업에 필요한 `EC2` 인스턴스 또는 스팟 인스턴스의 적절한 수를 자동으로 조정해주기 때문에 비용이 최적화되고 인프라에 신경을 덜 쓸 수 있다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/983219e7-1505-4fcc-a968-26ea32ed5f7d)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. Batch vs Lambda 
- Lambda: 
~~~
- Time limit 
→ Lambda에는 15분의 시간 제한이 있다.

- Limited runtimes 
→ 선택 가능한 프로그래밍 언어에 제한이 있다.

- Limited temporary disk space 
→ 작업을 실행하는 임시 디스크 공간이 제한되어 있다.

- Serverless 
→ 서버리스이다.
~~~

- Batch: 
~~~
- No time limit 
→ Batch는 EC2 인스턴스에 의존하기 때문에 시간 제한이 없다.

- Any runtime as long as it’s packaged as a Docker image 
→ 도커 이미지로 패키징하는 한, 런타임의 길이는 상관없다.

- Rely on EBS / instance store for disk space 
→ 스토리지는, EC2 인스턴스와 같이 제공되는 스토리지를 사용하면 된다. 이는 EBS 볼륨이 될 수도 있고 디스크 공간을 위한 EC2 인스턴스 스토어일 수도 있다.

- Relies on EC2 (can be managed by AWS)
→ 실제 EC2 인스턴스에 의존한다. 하지만 관리형 서비스이므로 오토 스케일링 등은 신경쓰지 않아도 괜찮다.
~~~

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---