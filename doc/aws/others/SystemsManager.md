## SSM Systems Manager
### 1. Systems Manager – SSM Session Manager
- Allows you to start a secure shell on your EC2 and on-premises servers  
→ `EC2` 인스턴스와 온프레미스 서버에서 보안 셸을 시작할 수 있다.

- No SSH access, bastion hosts, or SSH keys needed  
→ `SSH` 액세스, 배스천 호스트, `SSH` 키가 필요없다.

- No port 22 needed (better security)  
→ `EC2` 인스턴스의 포트 22는 닫혀 있다. `EC2` 인스턴스에 보안 셸을 설정하기 위해 `SSH`를 실행할 필요가 없기 때문이다.

- 사용자는 세션 관리자 서비스를 통해 EC2 인스턴스에 액세스할 수 있습니다

- Supports Linux, macOS, and Windows  
→ `Linux`, `MacOS`, `Windows`를 지원한다. 

- Send session log data to S3 or CloudWatch Logs  
→ 로그 데이터를 `Amazon S3` 또는 `CloudWatch Logs`로 보낼 수 있다.

- 먼저 `EC2` 인스턴스를 생성해야 한다. 그리고 이 인스턴스에 대한 `IAM` 역할 설정이 필요하다.
- `AWS System Manager`로 이동한다. `Session Manager`에서 세션을 생성한다.
- `AWS` 콘솔에서 직접 보안 쉘에 연결할 수 있다. 이 때, 인바운드 규칙이 없더라도 인스턴스에 접근 가능해 보안이 뛰어나다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/71017d86-8eda-443c-8144-0de8deab08df)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 총 3가지 방법으로 인스턴스에 접근 가능하다.
~~~
1. SSH - 22번 포트를 열어야 한다.
2. EC2 Instance Connect - 22번 포트를 열어야 한다.
3. SSM System Manager - 인바운드 규칙을 설정하지 않아도 괜찮다. 단, IAM 역할 설정이 필요하다.
~~~

### 2. Systems Manager – Run Command
- Execute a document (= script) or just run a command  
→ `Run Command`는 스크립트 또는 명령 한 개를 실행하는데 사용된다. 매우 간단히 명령어를 실행할 수 있다.

- Run command across multiple instances (using resource groups)  
→ 이 명령어들은 리소스 그룹을 사용하는 여러 인스턴스에서 동시에 실행된다.

- No need for SSH  
→ 이 명령들을 실행할 때는 `SSH`가 필요하지 않다. 세션 관리자에서 본 것과 같은 메커니즘을 사용한다.

- Command Output can be shown in the AWS Console, sent to S3 bucket or CloudWatch Logs  
→ 그리고 실행 명령으로 실행된 명령의 결과는 모두 `S3` 또는 `CloudWatch` 로그로 전송된다.

- Send notifications to SNS about command status (In progress, Success, Failed, …)  
→ 진행 중, 성공, 실패 등 모든 업데이트를 `SNS`로 전송할 수 있다.

- Integrated with IAM & CloudTrail  
→ 또한 보안을 위해 `IAM`과 완벽하게 통합된다.

- Can be invoked using EventBridge  
→ `EventBridge`로 실행 명령을 직접 실행할 수도 있다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/fec2e029-4453-4b65-ab42-0020f24a9217)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. Systems Manager – Patch Manager
- Automates the process of patching managed instances  
→ 인스턴스 관리 과정 패칭을 자동화하는 데 사용된다.

- OS updates, applications updates, security updates  
→ 운영 체제 및 애플리케이션 업데이트 그리고 보안 업데이트를 적용할 수 있다.

- Supports EC2 instances and on-premises servers  
→ `EC2` 인스턴스와 온프레미스 서버도 지원한다.

- Supports Linux, macOS, and Windows  
→ `Linux`, `Mac`, `Windows`를 지원한다.

- Patch on-demand or on a schedule using Maintenance Windows  
→ 유지 관리 기간을 사용하여 패치 일정을 정할 수 있다.

- Scan instances and generate patch compliance report (missing patches)  
→ 패치 관리자 내에서 인스턴스를 스캔하여 패치 규정 준수 보고서를 생성할 수 있다. 모든 인스턴스가 올바르게 패치되었는지 확인하고 패치가 누락된 경우가 있는지 찾을 수 있다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/085e2b64-749e-4e1f-8cf4-4f48f04ddf96)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 4. Systems Manager – Maintenance Windows
- Defines a schedule for when to perform actions on your instances  
→ 인스턴스에서 작업을 수행할 일정을 정의하는 데 사용된다.

- Example: OS patching, updating drivers, installing software, …  
→ `OS` 패치, 드라이버 업데이트 및 소프트웨어 설치 또는 원하는 작업을 수행하기 위해 유지 관리 기간을 정의할 수 있다.

- Maintenance Window contains
~~~
- Schedule
- Duration
- Set of registered instances
- Set of registered tasks
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/a2c66748-4ab1-4b41-b849-4e3074609a8b)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 5. Systems Manager - Automation
- Simplifies common maintenance and deployment tasks of EC2 instances and other AWS resources  
→ `Automation`는 `EC2` 인스턴스 또는 기타 `AWS` 리소스에서의 명령 유지 관리 및 배포 작업을 단순화하는 데 사용된다.

- Examples: restart instances, create an AMI, EBS snapshot  
→ `Automation`를 사용하면 인스턴스들을 한 번에 다시 시작하거나, `AMI`를 생성하거나 `EBS` 스냅샷을 생성하는 등의 작업을 자동화할 수 있다.

- Automation Runbook – SSM Documents to define actions preformed on your EC2 instances or AWS resources (pre-defined or custom)  
→ `Automation Runbook`은 `EC2` 인스턴스 또는 `AWS` 리소스에 동일한 문서로 표시된 미리 정의된 작업이다.

- Can be triggered using:
~~~
- Manually using AWS Console, AWS CLI or SDK
- Amazon EventBridge
- On a schedule using Maintenance Windows
- By AWS Config for rules remediations
- 콘솔, SDK, CLI, Amazon EventBridge, 유지 관리 기간, AWS config를 통합해 사용 가능하다. 자동화의 트리거로 설정할 수 있다.
~~~

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---