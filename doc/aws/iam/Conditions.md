## IAM Conditions
### 1. IAM Conditions
- `IAM` 조건은 `IAM` 내부의 정책에 적용된다. 이는 사용자 정책일수도 있고 리소스 정책일수도 있으며 엔드포인트 정책일수도 있다.

- 접두사가 `aws`이면 글로벌 서비스이다. `ec2`가 있으면 `ec2` 인스턴스에 관련된 서비스이다.

- `aws:SourceIp`는 `API` 호출이 생성되는 클라이언트의 `IP`를 제한하는 데 사용되는 조건이다.  
→ 클라이언트가 해당 `IP` 범위에 포함되지 않는다면 `API`의 호출을 거부하도록 작성되어 있다.

- `aws:RequestedRegion`는 `API` 호출 리전을 제한한다.  
→ 예시에서는 `eu-central-1`, `eu-west-1` 리전에서 오는 `EC2`, `RDS`, `DynamoDB` 호출을 제한한다.

![image](https://user-images.githubusercontent.com/97398071/236672401-3091dd12-90d6-4238-ae1a-f882b34e9773.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- `ec2:ResourceTag`는 특정 `EC2` 인스턴스의 태그를 통해 동작의 허용, 거부 여부를 설정한다.  
→ `ResourceTag/Project`가 `DataAnalytics`일 때 모든 인스턴스의 시작과 종료를 허용한다.

- `aws:PrincipleTag`는 사용자를 확인해 동작의 허용, 거부 여부를 설정한다.  
→ 사용자가 `Department`가 `Data`일 때 모든 인스턴스의 시작과 종료를 허용한다.

- `aws:MultiFactorAuthPresent`는 `MFA`를 강제할 때 사용되는 조건이다.

![image](https://user-images.githubusercontent.com/97398071/236672441-861dab79-7ae5-4991-bed0-bf7d530ae64f.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 첫 번째 `Statement`는 `s3:::test`라는 `ARN`에 적용된다. 이는 버킷 수준의 권한이므로 버킷을 특정해야 한다.  
→ 버킷의 목록을 확인할 수 있다.

- 두 번째 `Statement`는 `s3:::test/*`라는 `ARN`에 적용된다. 버킷 내의 객체에 적용되도록 `ARN`의 뒤에 `*`를 추가했다. 중요하다.  
→ `s3:GetObject`, `s3:PutObject`, `s3:DeleteObject`가 허용된다.

![image](https://user-images.githubusercontent.com/97398071/236672469-541ba759-5e3a-4a76-a14d-553ae43d849e.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. Resource Policies & aws:PrincipalOrgID
- aws:PrincipalOrgID can be used in any resource policies to restrict access to accounts that are member of an AWS Organization  
→ `aws:PrincipalOrgID`는 `AWS Organizations`의 멤버 계정에만 리소스 정책이 적용되도록 제한한다. 조직 내의 멤버 계정만 `S3` 버킷에 액세스할 수 있도록 설정하고 있다.

![image](https://user-images.githubusercontent.com/97398071/236673403-c834a8dc-b2ce-4abb-9c0b-364e15087d20.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---