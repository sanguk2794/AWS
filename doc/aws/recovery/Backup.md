## AWS Backup
### 1. AWS Backup
- Fully managed service  
→ 완전 관리형 서비스이다.

- Centrally manage and automate backups across AWS services  
→ `AWS` 서비스의 백업을 백업을 중앙 집중식으로 관리 및 자동화할 수 있도록 도와준다.

- No need to create custom scripts and manual processes  
→ 사용자 지정 스크립트나 매뉴얼을 만들 필요가 없다.

- Supported services: 
~~~
- Amazon EC2 / Amazon EBS 
- Amazon S3 
- Amazon RDS (all DBs engines) / Amazon Aurora / Amazon DynamoDB 
- Amazon DocumentDB / Amazon Neptune 
- Amazon EFS / Amazon FSx (Lustre & Windows File Server) 
- AWS Storage Gateway (Volume Gateway) 
- 계속 늘어나고 있다.
~~~

- Supports cross-region backups  
→ 리전 간 백업을 지원하므로 한 곳의 재해 복구 전략을 다른 리전에 푸시할 수 있다.

- Supports cross-account backup  
→ 계정 간 백업을 지원한다. `AWS`에서 여러 계정을 사용할 경우에 도움이 된다.

- Supports PITR for supported services  
→ `Aurora`와 같은 `PITR, 지정 시간 복구`를 지원한다.

- On-Demand and Scheduled backups  
→ 온디맨드와 함께 예약된 백업을 지원한다.

- Tag-based backup policies  
→ 태그 기반 백업 정책이 있다. 예를 들어 프로덕션 태그가 지정된 리소스만 백업할 수 있다.

- You create backup policies known as Backup Plans
~~~
- Backup frequency (every 12 hours, daily, weekly, monthly, cron expression)
→ 백업 플랜(Plan)을 만들어 백업 빈도를 정의할 수 있다.

- Transition to Cold Storage (Never, Days, Weeks, Months, Years)
→ 백업을 콜드 스토리지로 이전할지 여부도 결정할 수 있다. 보내지 않거나 일, 주, 월, 년 단위로 기간을 정할 수 있다.

- Retention Period (Always, Days, Weeks, Months, Years)
→ 백업 보유 기간을 정할 수 있다. 계속 보유하거나 일, 주, 월, 년 단위로 기간을 정할 수 있다.
~~~

- 백업 플랜의 생성과 `AWS` 리소스 할당이 완료되면 데이터가 자동으로 `S3` 내의 지정된 내부 버킷에 백업된다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/e014513d-5f36-4b1f-a684-2ab82ba399f6)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. AWS Backup Vault Lock
- Enforce a WORM (Write Once Read Many) state for all the backups that you store in your AWS Backup Vault  
→ `WORM, Write Once Read Many` 정책을 시행하면 백업 볼트(Vault)에 저장한 백업을 삭제할 수 없게 된다.

- Additional layer of defense to protect your backups against: 
~~~
- 백업에 대한 추가 방어막을 제공한다고 보면 된다.

- Inadvertent or malicious delete operations 
→ 의도치 않거나 악의적인 삭제 작업을 막을 수 있다.

- Updates that shorten or alter retention periods
→ 백업 유지 기간의 축소 및 변경 작업을 방지한다.
~~~

- Even the root user cannot delete backups when enabled  
→ 이 기능이 활성화되면 루트 사용자 자신도 백업을 삭제할 수 없다. 그러므로 백업의 안정성이 강력하게 보장된다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---