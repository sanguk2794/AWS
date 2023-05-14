## Instantiating Applications quickly
### 1. Instantiating Applications quickly
- When launching a full stack (EC2, EBS, RDS), it can take time to:  
→ 풀 스택을 실행하면 애플리케이션 설치, 데이터 삽입 및 복구, 설정, 애플리케이션 실행에 매우 긴 시간이 걸림. 
~~~
- Install applications
- Insert initial (or recovery) data
- Configure everything
- Launch the application
~~~

- We can take advantage of the cloud to speed that up!  
→ 속도를 높이기 위해 클라우드의 장점을 활용할 수 있다.

- EC2 Instances:
~~~
- Use a Golden AMI: Install your applications, OS dependencies etc.. beforehand and launch your EC2 instance from the Golden AMI
→ Golden AMI는 애플리케이션과 OS 종속성 등 모든 것을 사전에 설치하고 그것으로부터 AMI를 생성하는 것을 말한다. 
이후로는 `EC2` 인스턴스들을 Golden AMI로부터 직접 실행하면 된다. Golden AMI는 클라우드에서 아주 흔한 패턴이다.

- Bootstrap using User Data: For dynamic configuration, use User Data scripts
→ 인스턴스를 부트스트랩할 수 있게 해준다. 그러나 이 방식은 매우 느리다. EC2 인스턴스가 다른 인스턴스가 이미 했던 완전히 같은 일을 반복하기 때문이다.

- Hybrid: mix Golden AMI and User Data (Elastic Beanstalk)
→ Elastic Beanstalk을 활용해 Golden AMI와 User Data가 하이브리드 작동하도록 할 수 있다. 
Elastic Beanstalk는 AMI에 User Data를 추가하는 하이브리드와 동일한 원칙을 적용하기 때문이다.
~~~

- RDS Databases: Restore from a snapshot: the database will have schemas and data ready!  
→ 스냅샷으로부터 복구할 수 있다. 그러면 데이터베이스에서 스키마와 데이터가 준비된다.

- EBS Volumes: Restore from a snapshot: the disk will already be formatted and have data!  
→ 스냅샷으로부터 `EBS` 볼륨을 복구할 수 있다. 스냅샷은 이미 적절히 포맷되어 있으며, 필요한 데이터를 전부 가지고 있다.

- 솔루션 아키텍트로서, 여러분은 복잡한 `ERP` 소프트웨어 스위트를 `AWS Cloud`로 이전하려 합니다. 오토 스케일링 그룹이 관리하는 한 세트의 `Linux EC2` 인스턴스에 소프트웨어를 호스팅할 계획입니다. 소프트웨어가 `Linux` 기기를 준비하는 데에는 보통 한 시간 이상이 걸립니다. 스케일 아웃이 발생할 경우, 설치 과정을 빠르게 만들기 위해서는 어떤 방법을 추천할 수 있을까요?  
→ `Golden AMI` 사용

#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---