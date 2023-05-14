## On-Premise strategy with AWS
### 1. On-Premise strategy with AWS
- Ability to download Amazon Linux 2 AMI as a VM (.iso format)
~~~
- Amazon Linux 2 AMI를 가상 머신으로 다운로드 할 수 있다. 이 파일은 ISO 형식이다.

- VMWare, KVM, VirtualBox (Oracle VM), Microsoft Hyper-V 
→ 이 ISO 형식의 파일은 Oracle VM과 Microsoft Hyper-V에 해당하는 VMWare, KVM, Virtual Box 등을 포함한다.
~~~

- VM Import / Export 
~~~
- VM 가져오기와 내보내기 기능이 있다.

- Migrate existing applications into EC2 
→ 기존의 VM과 애플리케이션을 EC2로 마이그레이션 할 수 있다.

- Create a DR repository strategy for your on-premises VMs 
→ 재해 복구 리포지토리 전략을 생성할 수 있다.

- Can export back the VMs from EC2 to on-premises
→ VM을 EC2에서 온프레미스 환경으로 다시 마이그레이션 할 수 있다.
~~~

- AWS Application Discovery Service 
~~~
- Gather information about your on-premises servers to plan a migration 
→ AWS Application Discovery Service는 온프레미스의 정보를 모아주고 마이그레이션을 계획할 수 있게 해 주는 서비스이다.

- Server utilization and dependency mappings 
→ 서버 사용량 정보와 종속성 매핑에 대한 정보를 제공해 준다.

- Track with AWS Migration Hub 
→ AWS Migration Hub를 사용해서 마이그레이션을 추적할 수 있다.
~~~

- AWS Database Migration Service (DMS) 
~~~
- replicate On-premise => AWS , AWS => AWS, AWS => On-premise 
→ DMS, AWS data Migration Service는 온프레미스에서 AWS로의 복제 뿐만 아니라 AWS에서 AWS로, AWS에서 온프레미스로의 복제를 허용한다.

- Works with various database technologies (Oracle, MySQL, DynamoDB, etc..) 
→ Oracle, MySQL DynamoDB 등 다양한 데이터베이스들과 함께 작동한다.
~~~

- AWS Server Migration Service (SMS) 
~~~
- Incremental replication of on-premises live servers to AWS
→ SMS, 즉 AWS Server Migration Service는 온프레미스의 라이브 서버들을 AWS로 증분 복제할 때 사용된다.
~~~

- 온프레미스에서 `AWS`로의 마이그레이션 방법은 다음과 같다. 이름정도만 알고 있으면 된다. 
~~~
- 온프레미스와 EC2에 대한 VM 가져오기와 내보내기
- AWS Application Discovery Service와 같은 마이그레이션 서비스
- Migration Hub DMS
- SMS
~~~

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---