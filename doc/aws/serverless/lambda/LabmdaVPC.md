## Lambda with RDS Proxy
### 1. Lambda by default
- By default, your Lambda function is launched outside your own VPC (in an AWS-owned VPC)  
→ 기본적으로 람다 함수를 시작하면 `VPC`의 외부에서 시작된다.

- Therefore, it cannot access resources in your VPC (RDS, ElastiCache, internal ELB…)  
→ 그러므로 `VPC`에 내부의 리소스를 액세스할 수 없다. `RDS`, `ElastiCache`, `internal ELB` 등 서비스에 람다 함수가 액세스할 수 없는 것이다.

- 단, 인터넷의 퍼블릭 `API`에 액세스하는 것은 가능하다. 아래 사진에서 `DynamoDB`에 액세스 가능한 이유는 `DynamoDB`가 `AWS` 클라우드의 퍼블릭 리소스이기 때문이다.

![image](https://user-images.githubusercontent.com/97398071/235754470-bed6000e-a52b-4502-9207-a3876b928bb8.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. Lambda in VPC
- 람다 함수가 `VPC` 내부의 리소스를 액세스하지 못하는 문제를 해결할 수 있다. 특정 `VPC` 내에서 람다 함수를 실행하는 것이 그것이다.

- You must define the VPC ID, the Subnets and the Security Groups  
→ `VPC ID` 설정과 람다를 실행하려는 서브넷 지정, 람다 함수에 보안 그룹 추가가 필요하다.

- Lambda will create an ENI (Elastic Network Interface) in your subnets  
→ 설정을 완료하면 서브넷 내부 리소스를 `ENI`를 통해 액세스할 수 있게 된다.

![image](https://user-images.githubusercontent.com/97398071/235755230-f6419e90-f305-4838-af2c-e1979f044988.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. Lambda with RDS Proxy
- `VPC` 내에서 람다를 사용하는 대표적인 사용 사례는 `RDS` 프록시이다.

- If Lambda functions directly access your database, they may open too many connections under high load  
→ `RDS` 데이터베이스에 직접 액세스했을 때 람다 함수의 수가 너무 많이 생성되었다 사라지길 반복하면 개방된 연결이 너무 많아서 `RDS` 데이터베이스의 로드가 상승해 시간 초과등의 문제가 발생할 수 있다.

- RDS Proxy  
→ 이를 해결하는 방법은 `RDS` 프록시를 시작하는 것이다. 프록시를 통해 연결을 한 곳으로 모아 `RDS` 데이터베이스 인스턴스 연결의 수를 줄일 수 있다.

- `RDS` 프록시에는 세 가지 장점이 있다.
~~~
- Improve scalability by pooling and sharing DB connections
→ 데이터베이스 연결의 풀링과 공유를 통해 확장성을 향상시킨다.

- Improve availability by reducing by 66% the failover time and preserving connections
→ 장애가 발생할 경우 장애 조치 시간을 66%까지 줄여 가용성을 향상시키고 연결을 보존한다. 이는 RDS와 Aurora에 모두 적용된다.

- Improve security by enforcing IAM authentication and storing credentials in Secrets Manager
→ RDS 프록시 수준에서 IAM을 강제하여 보안을 높일 수 있고, 자격 증명은 Secrets Manager에 저장된다.
~~~

- The Lambda function must be deployed in your VPC, because RDS Proxy is never publicly accessible  
→ 람다 함수를 `RDS` 프록시에 연결할 수 있으려면 `VPC` 내에서 람다 함수를 시작해야 한다. `RDS` 프록시 또한 퍼블릭 액세스가 불가능하다.

![image](https://user-images.githubusercontent.com/97398071/235755390-60318b91-6c27-4fc9-9c0b-94b8e9ccaeac.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)


---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---