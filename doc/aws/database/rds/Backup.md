## Backup
### 1. RDS Backup
- Automated backups:
→ `RDS` 백업을 위한 첫 번째 방법은 `AWS`에서 제공하는 자동 백업이다.
~~~
- Daily full backup of the database (during the backup window)
→ 자동 백업은 RDS 서비스가 자동으로 매일 데이터베이스 유지 관리 시간에 데이터베이스 전체를 백업한다.

- Transaction logs are backed-up by RDS every 5 minutes
→ 거기에 더해 5분마다 트랜잭션 로그도 백업한다.

- => ability to restore to any point in time (from oldest backup to 5 minutes ago)
→ 따라서 이 자동 백업을 사용하면 5분 전 어떤 시점으로도 복구가 가능하다.

- 1 to 35 days of retention, set 0 to disable automated backups
→ 자동 백업 보유 기간은 1일부터 35일까지로 설정할 수 있다. 만약 이 기능을 비활성화하고 싶다면 0로 설정해서 자동 백업을 비활성화하면 된다.
~~~

- Manual DB Snapshots
→ `RDS` 백업을 위한 두 번째 방법은 수동 `DB` 스냅샷이다.
~~~
- Manually triggered by the user
→ 사용자가 수동으로 트리거해야 한다.

- Retention of backup for as long as you want
→ 원하는 만큼 오랫동안 보유할 수 있다는 장점이 있다.
~~~

- Trick: in a stopped RDS database, you will still pay for storage. If you plan on stopping it for a long time, you should snapshot & restore instead  
→ 원본 데이터베이스의 스냅샷을 만들고 원본 데이터베이스를 삭제하면 `RDS`를 중지시키는 것보다 훨씬 더 저렴한 가격으로 데이터베이스 정보를 보존할 수 있다. 다시 사용할 때가 되면 스냅샷을 복원해서 사용하면 된다.

### 2. Aurora Backups
- Automated backups  
→ `Aurora` 백업을 위한 첫 번째 방법은 `AWS`에서 제공하는 자동 백업이다.
~~~
- 1 to 35 days (cannot be disabled)
→ 자동 백업은 백업 데이터 보유 기간을 1일부터 35일까지로 설정할 수 있으며 비활성화가 불가능하다. RDS와 다른 점이다.

- point-in-time recovery in that timeframe 
→ 지정 시간 복구 기능이 있으며 정해진 시간 범위 내의 어느 시점으로도 복구가 가능하다.
~~~

- Manual DB Snapshots  
→ `Aurora` 백업을 위한 두 번째 방법은 수동 `DB` 스냅샷이다.
~~~
- Manually triggered by the user 
→ 사용자가 수동으로 트리거해야 한다.

- Retention of backup for as long as you want
→ 원하는 만큼 오랫동안 보유할 수 있다는 장점이 있다.
~~~

### 3. RDS & Aurora Restore options
- Restoring a RDS / Aurora backup or a snapshot creates a new database  
→ `RDS`와 `Aurora` 백업 또는 스냅샷을 복원 가능하다. 복원할 때마다 새로운 데이터베이스가 생성된다.

- Restoring MySQL RDS database from S3  
→ `S3`를 사용해 `MySQL RDS`를 복원할 수 있다.
~~~
- Create a backup of your on-premises database
→ 온프레미스 데이터베이스의 백업을 만든다.

- Store it on Amazon S3 (object storage)
→ 객체 스토리지인 Amazon S3에 위치시킨다.

- Restore the backup file onto a new RDS instance running MySQL
→ 새로운 RDS 인스턴스로 백업 파일을 복원한다.
~~~

- Restoring MySQL Aurora cluster from S3  
- `S3`를 사용해 `MySQL Aurora`를 복원할 수 있다.
~~~
- Create a backup of your on-premises database using Percona XtraBackup
→ Percona XtraBackup라는 소프트웨어를 사용해 온프레미스 데이터베이스를 외부로 다시 백업한다.

- Store the backup file on Amazon S3
→ Percona XtraBackup의 백업 파일이 Amazon S3로 전송된다.

- Restore the backup file onto a new Aurora cluster running MySQL
→ 백업 파일을 MySQL을 실행 중인 새 Aurora 클러스터로 복원시킨다.
~~~

### 4. Aurora Database Cloning
- Create a new Aurora DB Cluster from an existing one  
→ 기존의 데이터베이스로부터 새로운 `Aurora DB` 클러스터를 생성할 수 있다.

- Faster than snapshot & restore  
→ 실제 스냅샷을 만들고 복원하는 것보다 복제한 `Aurora`를 사용하는 것이 더 빠르다. 

- The new DB cluster uses the same cluster volume and data as the original but will change when data updates are made  
→ 새 데이터베이스는 같은 클러스터 볼륨을 사용하며 이것이 바로 더 빠른 이유이다. 그리고 시간의 흐름에 따라 새 데이터베이스로 업데이트되면 변경이 완료된다.

- Very fast & cost-effective  
→ 따라서, 데이터베이스 복제는 매우 빠르고 비용 면에서 효율적이다.

- Useful to create a “staging” database from a “production” database without impacting the production database  
→ 프로덕션 데이터베이스에 영향을 주지 않는다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---