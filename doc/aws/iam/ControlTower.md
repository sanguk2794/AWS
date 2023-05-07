## AWS Control Tower
### 1. AWS Control Tower
- Easy way to set up and govern a secure and compliant multi-account AWS environment based on best practices  
→ `AWS Control Tower` 서비스를 사용하면 모범 사례를 기반으로 안전하고 규정을 준수하는 다중 계정 `AWS` 환경을 손쉽게 설정하고 관리할 수 있다.

- AWS Control Tower uses AWS Organizations to create accounts  
→ `Control Tower` 서비스는 다중 계정 생성을 위해 `AWS Organization` 서비스를 사용한다.

- Benefits:
~~~
- Automate the set up of your environment in a few clicks
→ 클릭 몇 번으로 환경을 자동으로 설정할 수 있으며, 자동 구성이 가능하다.

- Automate ongoing policy management using guardrails
→ 가드레일을 사용하면 지속적이고 자동화된 정책 관리가 가능하다.

- Detect policy violations and remediate them
→ 정책 위반을 감지하고 자동으로 교정할 수 있다.

- Monitor compliance through an interactive dashboard
→ 대화형 대시보드를 통해 모든 계정의 전반적인 규정 준수를 모니터링할 수 있다.
~~~

### 2. AWS Control Tower – Guardrails
- Provides ongoing governance for your Control Tower environment (AWS Accounts)  
→ 가드레일을 사용하면 `Control Tower` 환경 내의 모든 계정에 관한 거버넌스를 얻을 수 있다.

- 가드레일에는 두 가지 유형이 있다.
~~~
- Preventive Guardrail – using SCPs (e.g., Restrict Regions across all your accounts)
→ 예방 가드레일은 계정을 보호하기 위해 AWS Organizations 서비스의 서비스 제한 정책인 SCP를 사용해 모든 계정에 적용한다.

- Detective Guardrail – using AWS Config (e.g., identify untagged resources)
→ 탐지 가드레일은 규정을 준수하지 않는 것을 탐지한다. 규정 준수에 대한 탐지는 AWS Config를 사용한다. 규정을 준수하지 않는 대상에 대한 SNS 주제를 트리거 해서 관리자로서 알림을 받거나 Lambda 함수를 실행해 Lambda 함수가 자동으로 문제를 교정하도록 처리할 수 있다.
~~~

![image](https://user-images.githubusercontent.com/97398071/236679818-ebdfc49e-6bb7-4a92-90f5-b8a366716e74.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---