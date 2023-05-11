## AWS DataSync
### 1. AWS DataSync
- Move large amount of data to and from  
~~~
- DataSync는 데이터를 동기화하며 이를 통해 대용량의 데이터를 한 곳에서 다른 곳으로 옮기기 위해 사용된다.
- On-premises / other cloud to AWS (NFS, SMB, HDFS, S3 API…) – needs agent
→ 온프레미스나 AWS의 다른 클라우드로 데이터를 옮길 수 있다. 이 때 서버를 NFS, SMB, HDFS, S3 API 등을 통해 연결해야 하며 에이전트가 있어야 한다. 

- AWS to AWS (different storage services) – no agent needed
→ AWS 서비스에서 다른 AWS 서비스로 데이터를 옮길 수 있다. 이 경우에는 에이전트가 필요 없다.
~~~

- Can synchronize to:
~~~
- Amazon S3 (any storage classes – including Glacier)
→ Glacier를 포함한 모든 스토리지 클래스에 동기화할 수 있다.
- Amazon EFS
- Amazon FSx (Windows, Lustre, NetApp, OpenZFS...)
~~~
- Replication tasks can be scheduled hourly, daily, weekly  
→ 복제 작업 스케줄을 지정하면 일정에 맞춰서 데이터가 자동으로 동기화된다. 단, 지연이 발생할 수 있다.

- File permissions and metadata are preserved (NFS POSIX, SMB…)  
→ 파일 권한과 메타데이터의 저장 기능이 있다. `NFS POSIX` 파일 시스템, `SMB` 권한 을 준수한다. 중요하다.

- 서버 메시지 블록(Server Message Block, SMB)은 도스나 윈도우에서 파일이나 디렉터리 및 주변 장치들을 공유하는데 사용되는 메시지 형식이다.

- One agent task can use 10 Gbps, can setup a bandwidth limit  
→ 네트워크 성능을 초과하고 싶지 않을 경우 대역폭에 제한을 걸 수 있다.

- 솔루션 아키텍트가 한 스타트업의 온프레미스 서버를 `AWS`로 이전하는 작업을 계획하고 있습니다. 현재 회사의 인프라는 공유 `NFS` 스토리지에 호스팅된 여러 개의 서버와 `30TB`의 데이터로 구성되어 있습니다. 아키텍트는 `Amazon S3`를 사용해 데이터를 호스팅하기로 결정했습니다. 데이터를 온프레미스에서 `S3`로 효율적으로 이전할 수 있는 `AWS` 서비스는 무엇입니까?  
→ `AWS DataSync`

- `S3` 버킷에 있는 다량의 데이터를 EFS 파일 시스템으로 옮기는 데 가장 적합한 `AWS` 서비스는 무엇입니까?  
→ `AWS DataSync`

- 사내 서버에 있는 중요 파일과 문서를 주기적으로 `AWS`에 동기화시켜 복사본을 따로 보관하고자 합니다. 이럴 경우에 도움을 줄 수 있는 `AWS` 서비스는 무엇입니까?  
→ `AWS DataSync`

#### 1. AWS DataSync NFS / SMB to AWS
- 아래는 `SMB` 혹은 `NFS` 프로토콜을 사용하는 온프레미스 파일을 `AWS`로 동기화하는 시나리오이다.
~~~
- 온프레미스 서버에 SMB 혹은 NFS 서버가 존재한다. 이를 `AWS`로 동기화하려고 한다.
- 온프레미스 서버에 AWS DataSync Agent를 다운로드한다. AWS DataSync Agent를 사용하면 암호화를 거쳐 AWS DataSync 서비스에 연결할 수 있다.
- S3 버킷, EFS, FSx 등 어떤 스토리지와도 동기화가 가능하다. 
- AWS에서 다시 온프레미스로의 동기화를 실행할 수도 있다.
- DataSync를 사용하고자 하지만 네트워크 용량이 부족한 경우에는 AWS Snowcone 장치를 활용할 수 있다. Snowcone 장치에는 DataSync Agent가 사전에 설치되어 있다.
~~~

![image](https://user-images.githubusercontent.com/97398071/235355622-15d99a75-3533-48e8-9acf-bf8638c82575.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 2. AWS DataSync Transfer between AWS storage services
- `DataSync`를 이용하면 `AWS` 서비스 간 동기화가 가능하다. 이 때 메타데이터가 유지된다는 것은 중요하다.

![image](https://user-images.githubusercontent.com/97398071/235355911-395183e0-ec1b-48cd-9b44-b4e520a0c033.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
