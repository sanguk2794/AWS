## Storage Comparison
### 1. Storage Comparison
- S3: Object Storage  
→ `S3`는 객체 스토리지로 대부분의 `AWS` 서비스와 연결할 수 있다.

- S3 Glacier: Object Archival  
→ `S3 Glacier`는 해당 객체를 아카이브할 때 사용한다.

- EBS volumes: Network storage for one EC2 instance at a time  
→ 하나의 `EC2` 인스턴스에만 스토리지를 연결할 때에는 `EBS` 볼륨을 사용한다. 단, `io1`, `io2` 모델은 `Multi-attach`기능을 지원한다.

- Instance Storage: Physical storage for your EC2 instance (high IOPS)  
→ `Instance Storage`는 `EC2` 인스턴스에서 고성능 물리 스토리지를 필요로 할 때 사용 가능한 스토리지이다.

- EFS: Network File System for Linux instances, POSIX filesystem  
→ `EFS`는 인스턴스가 `NFS` 프로토콜을 기반으로 하며, `Multi AZ`간의 마운트가 필요하거나 `POSIX` 파일 시스템을 사용할 때 적용 가능하다.

- FSx for Windows: Network File System for Windows servers  
→ `FSx for Windows`는 `Windows` 서버 파일 시스템을 필요로 하는 경우 사용한다.

- FSx for Lustre: High Performance Computing Linux file system  
→ `FSx for Lustre`는 고성능 연산 `Linux` 파일 시스템이며, `Lustre`와 호환 가능한 경우에 사용한다. 

- FSx for NetApp ONTAP: High OS Compatibility  
→ `FSx for NetApp ONTAP`는 높은 운영체제 호환성을 가진 `NFS`가 필요할 때 사용한다.

- FSx for OpenZFS: Managed ZFS file system  
→ `FSx for OpenZFS` 관리형 `ZFS` 파일 시스템이 필요할 때 사용한다.

### 2. Connect on-premise to AWS storage
- Storage Gateway: S3 & FSx File Gateway, Volume Gateway (cache & stored), Tape Gateway
~~~
- S3 & FSx File Gateway
→ 온프레미스와 S3 또는 FSx 파일을 동기화할 수 있다.

- Volume Gateway (cache & stored)
→ 온프레미스 서버에 볼륨을 마운트할 수 있다.

- Tape Gateway
→ 테이프 형식으로 백업을 실행한다.
~~~

- Transfer Family: FTP, FTPS, SFTP interface on top of Amazon S3 or Amazon EFS  
→ `Amazon S3`나 `EFS`에서 `FTP`, `SFTP`, `FTPS` 인터페이스를 필요로 하는 경우 사용한다.

- DataSync: Schedule data sync from on-premises to AWS, or AWS to AWS  
→ 온프레미스에서 `AWS`, `AWS`에서 `AWS`로의 스케줄 동기화가 가능하다.

- Snowcone / Snowball / Snowmobile: to move large amount of data to the cloud, physically  
→ 데이터를 옮기는 데 쓸 네트워크 용량이 없거나 물리적으로 대용량의 데이터를 옮겨야 할 때 사용한다. `Snowcone`은 `DataSync Agent`가 사전 설치되어 온다는 특징이 있다.

- Database: for specific workloads, usually with indexing and querying

### 3. Storage Types
- `Amazon EBS` 등 블록 스토리지  
→ 블록 스토리지는 데이터를 일정한 크기의 덩어리(블록)로 나누어 저장하는 방식이다. 
이렇게 나누어진 각각의 블록은 고유한 주소를 가지고 있으며, 이 주소를 통하여 블록들을 재구성하여 데이터를 불러올 수 있다.

- `Amazon EFS`, `Amazon FSx` 등 파일 스토리지  
→ 파일 스토리지는 파일과 폴더의 계층구조로 이루어진 방식이다.

- `Amazon S3`, `Amazon Glacier` 등 객체 스토리지  
→ 오브젝트 스토리지는 오브젝트라는 개별 데이터 단위로 데이터를 저장한다. 오브젝트는 비디오, 오디오뿐 아니라 텍스트, 기타 다른 파일 유형 등의 모든 데이터를 포괄한다.
파일 스토리지와는 다르게 계층구조 없는 평면(flat) 구조로 데이터를 저장한다. 그만큼 접근이 쉽고 빠르며 확장성이 높다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
