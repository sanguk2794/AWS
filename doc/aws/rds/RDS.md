## RDS
### 1. Amazon RDS Overview
- RDS stands for Relational Database Service  
→ `RDS`는 `Relational Database Service`, 관계형 데이터베이스의 약자이다.

- It’s a managed DB service for DB use SQL as a query language.  
→ `SQL`을 쿼리로 사용하는 데이터베이스용 관리형 데이터베이스 서비스이다.

- It allows you to create databases in the cloud that are managed by AWS  
→ 클라우드의 `RDS` 서비스에 `DB`를 생성할 수 있고 `AWS`가 `DB`를 관리해준다. 다음 종류의 `DB`를 지원한다.
~~~
- Postgres
- MySQL
- MariaDB
- Oracle
- Microsoft SQL Server
- Aurora (AWS Proprietary database)
~~~

### 2. Advantage over using RDS versus deploying DB on EC2
- `EC2` 인스턴스에 자체 `DB` 서비스를 배포하지 않고 `RDS`를 사용하는 이유는 다음과 같다.

- RDS is a managed service:  
→ `RDS`는 관리형 서비스이다. 데이터베이스 뿐만 아니라 다양한 서비스를 제공해 준다.
~~~
- Automated provisioning, OS patching
→ 데이터베이스 프로비저닝과 기본 운영체제의 패치가 자동화되어 있다.

- Continuous backups and restore to specific timestamp (Point in Time Restore)!
→ 지속적으로 백업이 생성되므로 특정 시점으로 복원할 수 있다.

- Monitoring dashboards
→ 데이터베이스의 성능을 대시보드에 모니터링할 수 있다.

- Read replicas for improved read performance
→ 읽기 전용 복제본을 활용해 읽기 성능을 개선할 수 있다.

- Multi AZ setup for DR (Disaster Recovery)
→ 재해 복구를 목적으로 하는 다중 AZ를 설정할 수 있다.

- Maintenance windows for upgrades
→ 유지 관리 기간에 업그레이드를 할 수 있다.

- Scaling capability (vertical and horizontal)
→ 인스턴스의 유형을 통한 수직 확장과 읽기 전용 복제본 추가를 통한 수평 확장이 모두 가능하다.

- Storage backed by EBS (gp2 or io1)
→ 파일 스토리지는 EBS에 구성된다.
~~~

- BUT you can’t SSH into your instances  
→ 한 가지 단점은 RDS 인스턴스에 SSH 액세스가 불가능하다는 점이다.

### 3. RDS Storage Auto Scaling
- Helps you increase storage on your RDS DB instance dynamically, When RDS detects you are running out of free database storage, it scales automatically  
→ `RDS` 데이터베이스를 사용하면서 공간이 부족해질 경우 `RDS Storage Auto Scaling` 기능이 활성화되어 있으면 `RDS`가 이를 감지해서 자동으로 스토리지를 확장해 준다.

- Avoid manually scaling your database storage  
→ 데이터베이스를 수동으로 확장하는 작업을 피할 수 있다. 스토리지를 늘리기 위해 `DB`를 중지시키는 등의 작업이 불필요하다.

- You have to set Maximum Storage Threshold (maximum limit for DB storage)  
→ 이를 위해 최대 스토리지 용량의 임계값을 설정해주어야 한다.

- Automatically modify storage if:  
~~~
- Free storage is less than 10% of allocated storage 
→ 남은 공간이 10% 이하가 되었을 때

- Low-storage lasts at least 5 minutes 
→ 스토리지 부족 상태가 5분 이상 지속될 때

- 6 hours have passed since last modification 
→ 지난 수정으로부터 6시간 이상 지났을 때
~~~

- Useful for applications with unpredictable workloads  
→ 워크로드를 예측할 수 없는 애플리케이션에서 굉장히 유용하다.

- Supports all RDS database engines (MariaDB, MySQL, PostgreSQL, SQL Server, Oracle)  
→ 이는 모든 `RDS` 데이터베이스 엔진에서 제공되는 기능이다.

### 4. RDS Read Replicas for read scalability
- `RDS` 읽기 전용 복제본과 다중 `AZ`의 차이를 이해하고 각각의 사용 사례를 아는 것은 중요하다.

- Up to 5 Read Replicas  
→ 읽기 전용 복제본을 최대 5개까지 생성할 수 있다.

- Within AZ, Cross AZ or Cross Region  
→ 이들은 동일한 `AZ` 또는 가용 영역이나 리전을 거쳐서 생성될 수 있다.

- Replication is ASYNC, so reads are eventually consistent  
→ 마스터 `RDS` 데이터베이스의 인스턴스와 두 읽기 전용 복제본 사이에서 비동기식 복제가 발생한다. 비동기식이란 결국 읽기가 일관적으로 유지된다는 것을 의미한다.

- Replicas can be promoted to their own DB  
→ 읽기 전용 복제본을 데이터베이스로 승격시킬 수 있다. 승격한 `RDS`는 자체적인 생애주기를 갖게 된다.

- Applications must update the connection string to leverage read replicas  
→ 화면 상단의 주요 애플리케이션에 있는 모든 연결을 업데이트해야 하며, 이를 통해 `RDS` 클러스터 상의 읽기 전용 복제본 전체 목록을 활용할 수 있다.

#### 1. RDS Read Replicas – Use Cases
- You have a production database that is taking on normal load  
→ 평균적인 로드를 가지고 있는 생산 데이터베이스가 있다. 

- You want to run a reporting application to run some analytics  
→ 데이터를 기반으로 보고와 분석을 실시하고자 한다.

- You create a Read Replica to run the new workload there  
→ 보고 애플리케이션을 메인 `RDS`에 연결하면 오버로드가 발생하고 생산 애플리케이션의 속도가 느려지게 된다. 이를 피하기 위해 읽기 전용 복제본을 생성해 워크로드에 활용할 수 있다.

- The production application is unaffected  
→ 이 경우, 생산 애플리케이션은 성능에 전혀 영향을 받지 않는다.

- Read replicas are used for SELECT (=read) only kind of statements (not INSERT, UPDATE, DELETE)  
→ 단, 읽기 전용 복제본이 있을 경우 `SELECT` 명령문만을 사용해야 한다는 것을 명심해야 한다.

![image](https://user-images.githubusercontent.com/97398071/233844430-26321dd7-5a70-45c0-82f7-0d78825c9330.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 2. RDS Read Replicas – Network Cost
- In AWS there’s a network cost when data goes from one AZ to another  
→ `AWS`는 `AZ`에서 다른 `AZ`로 데이터가 이동할 때 비용이 발생한다. 하지만 예외가 존재하며 이 예외는 보통 관리형 서비스에서 나타난다.

- For RDS Read Replicas within the same region, you don’t pay that fee  
→ `RDS` 읽기 전용 복제본은 관리형 서비스이다. 동일한 리전 내라면 다른 `AZ` 내로 이동시키더라도 비용이 발생하지 않는다.
하지만 서로 다른 리전에 복제본이 존재하는 경우에는 네트워크에 대한 복제 비용이 발생한다.

![image](https://user-images.githubusercontent.com/97398071/233844620-b3f9827f-dc28-486d-ac9b-2366a7093188.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 5. RDS Multi AZ (Disaster Recovery)
- `RDS Multi AZ`는 주로 재해 복구에 사용된다. 읽기 전용 복제본과의 차이를 제대로 이해해야 한다. 
- SYNC replication  
→ 동기적으로 복제된다는 것은 마스터 데이터베이스의 모든 변경 사항이 대기 인스턴스에도 그대로 복제된다는 것을 의미한다.

- One DNS name – automatic app failover to standby  
→ 마스터 데이터베이스에 문제가 생기더라도 스탠바이 데이터베이스에 자동으로 장애 조치가 수행된다.

- Increase availability  
→ 이를 통해 가용성을 높일 수 있기 때문에 다중 `AZ`라고 불린다.

- Failover in case of loss of AZ, loss of network, instance or storage failure  
→ `Multi-AZ`는 전체 `AZ` 또는 네트워크가 손실될 때에 대비한 장애 조치이며, 마스터 데이터베이스의 인스턴스 또는 스토리지에 장애가 발생할 때에 대비한 보험이다.

- No manual intervention in apps  
→ 앱에 수동으로 조치를 취할 필요가 없다. 장애 조치가 필요하면 스탠바이가 마스터로 승격되는 과정이 자동으로 이루어진다.

- Not used for scaling  
→ 스탠바이 데이터베이스는 단지 대기 목적 하나만을 수행한다. 그 누구도 이 데이터베이스를 읽거나 쓸 수 없다.

- Note:The Read Replicas be setup as Multi AZ for Disaster Recovery (DR)  
→ 원하는 경우에는 읽기 전용 복제본을 다중 `AZ`로 설정할 수 있다. 흔히 나오는 문제이다 

### 6. RDS – From Single-AZ to Multi-AZ
- `RDS`를 단일 `AZ`에서 다중 `AZ`로 전환할 수 있다.

- Zero downtime operation (no need to stop the DB)  
→ 이 작업에는 타임아웃이 전혀 없다. 즉, 단일 `AZ`에서 다중 `AZ`로 전환할 때에 데이터베이스를 중지할 필요가 없는 것이다.

- Just click on “modify” for the database  
→ 데이터베이스 수정을 클릭하고 다중 `AZ` 기능을 활성화시키는 것만으로도 충분하다.
이를 통해 `RDS` 데이터베이스 인스턴스는 마스터를 갖고 동기식 복제본인 스탠바이 데이터베이스를 확보한다.  

- The following happens internally:  
→ 내부적으로 발생하는 일은 다음과 같다.
~~~
- A snapshot is taken
→ 기본 데이터베이스의 RDS가 자동으로 스냅샷을 생성한다.

- A new DB is restored from the snapshot in a new AZ
→ 이 스냅샷은 새로운 스탠바이 데이터베이스에 복원된다.

- Synchronization is established between the two databases
→ 두 데이터베이스 간 동기화가 설정되므로, 스탠바이 RDS가 메인 RDS의 내용을 모두 수용하여 다중 AZ 설정 상태가 된다.
~~~

### 7. Create Database
- `Amazon RDS` → `Database` → `Create Database` 클릭

![image](https://user-images.githubusercontent.com/97398071/233845389-5c7a3f42-3ec3-4eba-93a4-e22317f0b5f1.png)

- 데이터베이스 엔진을 선택할 수 있다. 일곱 가지의 엔진 타입 중 하나를 선택해야 한다.

![image](https://user-images.githubusercontent.com/97398071/233845471-45ea5600-0b15-4c83-9fc8-d380128d0407.png)

- 템플릿을 설정할 수 있다. 상용 환경에서 사용할 것인지 테스트, 개발 환경에서 사용할 것인지 선택한다.
- 가용성 및 내구성에 대해 설정할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/233845717-6fea887e-b58a-4012-9366-9776322676f0.png)
 
- 인스턴스 구성과 스토리지를 설정할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/233845782-844e2ef0-c4a2-4901-b9d1-ee8f279a3dc9.png)

- `EC2` 컴퓨팅 리소스와 연결 여부를 선택할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/233845850-4236ed16-c9b4-4aca-8ca4-53c08f31b7a1.png)

- `VPC security group`을 설정할 수 있다.
- 데이터베이스 인증에는 세 가지 매커니즘이 있다. 
~~~
- 암호 인증 
→ 사용자 이름과 비밀번호를 사용

- 암호 및 IAM 데이터베이스 인증(다중 AZ DB 클러스터에서 사용할 수 없음)
→ AWS IAM 사용자와 역할을 통해 데이터베이스 암호와 사용자 자격 증명을 사용하여 인증

- 암호 및 Kerberos 인증(다중 AZ DB 클러스터에서 사용할 수 없음)
→ 권한이 부여된 사용자가 Kerberos 인증을 사용
~~~

![image](https://user-images.githubusercontent.com/97398071/233846013-700b6f96-49a2-49f4-b006-7a2e591571f9.png)

- 최소 0일부터 최대 35일까지 백업 기간을 설정할 수 있다. 장기 보관을 하고 싶다면 `Log Exports`를 통해 감사 로그를 내보내면 된다.
- 삭제 방지를 체크하면 실수로 데이터베이스가 삭제되는 것을 방지할 수 있다.
- 데이터베이스 접속에 실패할 경우 데이터베이스가 `public`으로 설정되었는지, 보안 그룹 설정이 `IP` 주소를 허용하는지 확인해보자.
- 생성한 데이터베이스에 상세에서 읽기 전용 복제본을 생성할 수 있다. 데이터베이스의 읽기 용량이 커지므로 매우 유용하다.

![image](https://user-images.githubusercontent.com/97398071/233846518-7e49dfc0-43c9-483e-a6ed-f23daaa27055.png)
 
- `RDB`에 관련된 `CPU` 사용률을 포함한 전반적인 지표를 `CloudWatch`에서 확인 가능하다.
- `RDB`를 특정 시점으로 복원하거나 다른 리전으로 스냅샷을 마이그레이션하는 것도 가능하다.

### 8. RDS Custom
- `RDS`에서는 기저 운영체제나 사용자 지정 기능에 액세스 불가능하다. 그러나 `RDS Custom`에서는 가능하다.
- Managed Oracle and Microsoft SQL Server Database with OS and database customization  
→ `RDS Custom`은 `Oracle`과 `Microsoft SQL Server`, 두 유형의 데이터베이스에서만 사용할 수 있다.

- RDS: Automates setup, operation, and scaling of database in AWS  
→ `RDS`를 통해 `AWS`에서의 데이터베이스 자동화 설정, 운영 그리고 다양한 스케일링 장점을 얻을 수 있다.

- Custom: access to the underlying database and OS so you can  
→ 기저 데이터베이스와 운영 체제에 액세스할 수 있게 되어 내부 설정 구성, 패치 적용 그리고 네이티브 기능 활성화가 가능하며 
`SSH` 또는 `SSM` 세션 관리자를 사용해서 `RDS` 뒤에 있는 기저 `EC2` 인스턴스에 액세스할 수 있다.
~~~
- Configure settings
- Install patches
- Enable native features
- Access the underlying EC2 Instance using SSH or SSM Session Manager
~~~

- De-activate Automation Mode to perform your customization, better to take a DB snapshot before  
→ 사용자 지정 설정을 사용하려면 `RDS`가 수시로 자동화, 유지 관리 또는 스케일링과 같은 작업을 수행하지 않도록 자동화를 꺼두는 것이 좋다.
또, 기저 인스턴스에 접근이 가능해져 문제가 쉽게 발생할 수 있기 때문에 데이터베이스 스냅샷을 만들어 두는 것이 좋다. 그러지 않으면 오류가 발생했을 때 복구가 어려울 수 있다.

- RDS vs. RDS Custom  
→ `RDS`는 데이터베이스 전체를 관리한다. 운영 체제와 나머지는 `AWS`서 관리하고 사용자는 아무 것도 하지 않아도 된다.
반면, `RDS Custom`은 `Oracle` 및 `Microsoft SQL Server`에서만 사용할 수 있으며, 기저 운영체제와 데이터베이스에 대한 관리자 권한 전체를 갖게 된다.
~~~
- RDS: entire database and the OS to be managed by AWS
- RDS Custom: full admin access to the underlying OS and the database
~~~

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
