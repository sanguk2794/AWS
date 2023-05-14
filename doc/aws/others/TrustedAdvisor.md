## Trusted Advisor
### 1. Trusted Advisor
- No need to install anything – high level AWS account assessment  
→ 사용자는 계정을 분석하고 권장사항을 제공받기 위해 `AWS`로부터 높은 수준의 계정 평가를 받으려고 한다.

- Analyze your AWS accounts and provides recommendation:
~~~
- Trusted Advisor가 계정 평가에 사용된다. 이는 사용자 계정에서 다섯 가지 범주의 문제를 분석한다.

- Cost Optimization → 비용 최적화
- Performance → 성능
- Security → 보안
- Fault Tolerance → 내결함성 
- Service Limits → 서비스 할당량
~~~

- Core Checks and recommendations – all customers  
→ 첫 번째 티어는 모든 고객에게 핵심 검사와 권장사항을 제시한다. 이는 계정을 개선하기 위해 무엇을 할 수 있는지에 관한 정보를 전달하는 기본 검사이다.

- Can enable weekly email notification from the console  
→ 콘솔에서 직접 `Trusted Advisor`의 주간 이메일 알림을 받기 때문에 매주 원활한 추적이 가능하다.

- Full Trusted Advisor – Available for Business & Enterprise support plans
~~~
- Trusted Advisor의 기능을 전부 사용하고 싶다면 비즈니스나 기업의 지원 플랜이 필요하다.

- Ability to set CloudWatch alarms when reaching limits
→ 전체 기능을 사용하면 서비스 제한에 도달할 경우 CloudWatch 경보를 설정할 수 있다.

- Programmatic Access using AWS Support API
→ AWS 지원 API를 통해 Trusted Advisor로 프로그래매틱 액세스할 수 있습니다
~~~

- 시험에 관련하여 기억할 것은 비즈니스 지원 플랜이 있어야 `Trusted Advisor`의 전체 기능에 액세스를 할 수 있고, 그전까지는 핵심 검사만 가능하다 것과 비즈니스와 기업 지원 플랜을 사용할 경우 `Trusted Advisor`에 프로그래매틱 액세스할 수 있다는 것이다.

### 2. Trusted Advisor Checks Examples
- Cost Optimization: 
~~~
- low utilization EC2 instances, idle load balancers, under-utilized EBS volumes… 
→ 비용 최적화 검사에서는 활용도 낮은 EC2 인스턴스와 작동되지 않는 로드 밸런서 또는 활용도 낮은 EBS 볼륨 등을 확인할 수 있다.

- Reserved instances & savings plans optimizations
→ 그리고 예약 인스턴스 및 절감형 플랜 최적화를 표시한다.
~~~

- Performance: 
~~~
- High utilization EC2 instances, CloudFront CDN optimizations
→ 성능 검사의 경우 활용률이 매우 높은 EC2 인스턴스에 대한 정보를 얻을 수 있는데 과사용 상태일 수 있기 때문이다. CloudFront CDN optimizations도 마찬가지이다.

- EC2 to EBS throughput optimizations, Alias records recommendations 
→ EC2를 EBS에 연결하여 얻을 수 있는 성능 및 최적화를 보여주고 DNS에 대한 Alias 레코드 권장 사항을 보여준다.
~~~

- Security: 
~~~
- MFA enabled on Root Account, IAM key rotation, exposed Access Keys 
→ 보안 검사를 통해 루트 계정의 MFA 활성화 여부를 확인할 수 있으며 최근에 IAM 키가 교체되었는지 혹은 IAM 액세스 키가 노출되었는지 확인할 수 있다.

- S3 Bucket Permissions for public access, security groups with unrestricted ports 
→ S3 버킷 권한에 관련한 보안 문제를 보여준다. 버킷에 공용 액세스가 있는지  SSH의 보안 그룹이 제한받지 않는 포트를 가지고 있는지 등을 확인할 수 있다.
~~~

- Fault Tolerance: 
~~~
- EBS snapshots age, Availability Zone Balance 
→ EBS 스냅샷 수명과 서로 다른 AZ의 균형 등을 확인할 수 있다.

- ASG Multi-AZ, RDS Multi-AZ, ELB configuration… 
→ 오토 스케일링 그룹과 RDS, ELB가 모두 다중 AZ인지 아닌지 알려준다.
~~~

- Service Limits  
→ 특정 서비스가 할당량에 도달했는지 알 수 있다. 이를 활용하여 실제로 도달하기 전에 서비스 할당량을 늘릴 수 있다.
 
출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---