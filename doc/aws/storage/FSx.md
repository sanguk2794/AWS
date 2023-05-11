## Amazon FSx
### 1. Amazon FSx – Overview
- Launch 3rd party high-performance file systems on AWS  
→ `AWS`에서 서드파티의 파일 시스템을 실행시킬 때 사용한다.

- Fully managed service  
→ 완전 관리형 서비스이다. 

- 실행 가능한 서드파티 파일 시스템의 종류는 다음과 같다.
~~~
- FSx for Lustre
- FSx for Windows File Server
- FSx for NetApp ONTAP
- FSx for OpenZFS
~~~

![image](https://user-images.githubusercontent.com/97398071/235351197-98d763d2-2373-4c67-8b23-2a5277d91375.png)

### 2. Amazon FSx for Windows (File Server)
- FSx for Windows is a fully managed Windows file system share drive  
→ 완전 관리형 `Window` 파일 시스템의 공유 드라이브이다.

- Supports SMB protocol & Windows NTFS  
→ 윈도우를 사용하기 때문에 `SMB` 프로토콜과 `Windows NTFS`를 지원한다.

- Microsoft Active Directory integration, ACLs, user quotas  
→ `Microsoft Active Directory` 통합을 지원한다.

- Can be mounted on Linux EC2 instances  
→ `Lunux EC2` 인스턴스에도 마운트할 수 있다. 중요하다.

- Supports Microsoft's Distributed File System (DFS) Namespaces (group files across multiple FS)  
→ `Microsoft`의 분산 시스템인 `DFS` 기능을 이용해서 파일 시스템을 그룹화할 수 있다. 온프레미스의 `Windows` 파일 서버와 `FSx for Windows File Server`를 결합할 수 있다.

- Scale up to 10s of GB/s, millions of IOPS, 100s PB of data
- Storage Options:
~~~
- SSD – latency sensitive workloads (databases, media processing, data analytics, …)
→ 데이터베이스, 미디어 처리, 데이터 분석 등 지연시간이 짧아야 하는 워크로드에 사용된다.

- HDD – broad spectrum of workloads (home directory, CMS, …)
→ 비용이 더 저렴하다.
~~~

- Can be accessed from your on-premises infrastructure (VPN or Direct Connect)  
→ 프라이빗 연결을 사용하면 온프레미스 인프라에서도 액세스가 가능하다.

- Can be configured to be Multi-AZ (high availability)  
→ 다중 `AZ`에 구성할 수 있다.

- Data is backed-up daily to S3  
→ 모든 데이터는 재해 복구 목적으로 `Amazon S3`에 매일 백업된다.

- 여러분의 `EC2 Windows` 서버는 `Windows`의 보안 메커니즘을 준수하며, `Microsoft Active Directory`와 통합된 네트워크 파일 시스템을 마운트하여 일부 데이터를 공유해야 합니다. 어떤 방법을 추천할 수 있을까요?  
→ `Windows`용 `Amazon FSx`

### 3. Amazon FSx for Lustre
- Lustre is a type of parallel distributed file system, for large-scale computing  
→ `Lustre`는 분산 파일 시스템으로, 대규모의 연산에 사용된다.

- The name Lustre is derived from “Linux” and “cluster“  
→ `Lustre`는 `Linux`와 `cluster`를 합친 단어이다.

- Machine Learning, High Performance Computing (HPC)  
→ 러신 머닝과 `HPC`에 주로 사용된다.

- Video Processing, Financial Modeling, Electronic Design Automation  
→ 동영상 처리, 금융 모델링, 전자 설계 자동화 등의 애플리케이션에서 사용된다.

- Scales up to 100s GB/s, millions of IOPS, sub-ms latencies
- Storage Options:
~~~
- SSD – low-latency, IOPS intensive workloads, small & random file operations
→ 낮은 지연시간을 가진다. 워크로드가 많거나 크기가 작은 무작위 파일 작업이 많을 때 유용하다.

- HDD – throughput-intensive workloads, large & sequential file operations
→ 처리량이 많은 워크로드의 집합 또는 각각의 파일의 사이즈가 크고 처리 순서를 지키는 파일 작업이 이어진다면 유용하다.

~~~

- Seamless integration with S3  
→ `S3` 통합이 가능하다. 
~~~
- Can “read S3” as a file system (through FSx)
→ FSx로 S3를 파일 시스템처럼 읽어들일 수 있다. 

- Can write the output of the computations back to S3 (through FSx)
→ FSx의 연산 출력값을 Amazon S3에서 사용할 수 있다. 중요하다.
~~~

- Can be used from on-premises servers (VPN or Direct Connect)  
→ `VPN` 또는 직접 연결을 통해 온프레미스 서버에서 사용할 수 있다.

- 고성능 컴퓨팅과 전산 유전학 연구를 수행하기 위해 `IOPS`를 최대화해 줄 분산 `POSIX` 준수 파일 시스템이 필요한 상황입니다. 이 파일 시스템은 수백만 개의 `IOPS`로 손쉽게 스케일링할 수 있어야 합니다. 어떤 방법을 추천할 수 있을까요?  
→ `Amazon FSx for Lustre`

#### 1. FSx Lustre - File System Deployment Options
- `Lustre`의 파일 시스템 배포 옵션에는 스크래치 파일 시스템과 영구 파일 시스템이 있다.

- Scratch File System
~~~
- Temporary storage
→ 임시 스토리지이다.

- Data is not replicated (doesn’t persist if file server fails)
→ 데이터가 복제되지 않는다. 기저 서버가 오작동하면 파일이 모두 유실될 수 있다.

- High burst (6x faster, 200MBps per TiB)
→ High burst를 사용할 수 있다. 최적화 기능이다.

- Usage: short-term processing, optimize costs
→ 단기 처리 데이터에 사용된다. 데이터 복제가 없어 비용 최적화가 가능하다.
~~~

![image](https://user-images.githubusercontent.com/97398071/235350784-00ce8d63-0d59-4762-9bea-c79c30e938f4.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Persistent File System
~~~
- Long-term storage
→ 장기 스토리지이다.

- Data is replicated within same AZ
→ 동일한 가용 영역 내에 데이터가 복제된다.

- Replace failed files within minutes
→ 기저 서버가 오작동했을 때 몇분 내에 해당 파일이 교체된다.

- Usage: long-term processing, sensitive data
→ 민감한 데이터의 보존, 장기 처리 등에 사용된다.
~~~

![image](https://user-images.githubusercontent.com/97398071/235350803-1b4fc1da-a90d-4f9f-aed5-96dcb47118e3.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- `FSx` 파일 시스템에 있는 다음 배포 옵션 중에서 `AZ` 내에 복사된 장기 스토리지를 제공하는 것은 무엇인가요?  
→ 영구 파일 시스템, 이는 데이터가 동일한 AZ 내에서 복제되는 장기 스토리지를 제공하며, 실패한 파일들은 수 분 내로 교체된다.

### 4. Amazon FSx for NetApp ONTAP
- Managed NetApp ONTAP on AWS  
→ 파일 관리 시스템이다.

- File System compatible with NFS, SMB, iSCSI protocol  
→ `NFS`, `SMB`, `iSCSI` 프로토콜과 호환된다.

- Move workloads running on ONTAP or NAS to AWS  
→ 온프레미스 시스템의 `ONTAP`이나 `NAS`에서 실행중인 워크로드를 `AWS`로 옮길 수 있다.

- Works with:  
→ 다양한 운영체제를 지원한다.
~~~
- Linux
- Windows
- MacOS
- VMware Cloud on AWS
- Amazon Workspaces & AppStream 2.0
- Amazon EC2, ECS and EKS
~~~

- Storage shrinks or grows automatically  
→ 스토리지는 자동으로 확장, 축소된다.

- Snapshots, replication, low-cost, compression and data de-duplication  
→ 스냅샷과 복제 기능을 제공한다. 비용이 적게 들고 데이터의 중복 제거가 가능하다.

- Point-in-time instantaneous cloning (helpful for testing new workloads)  
→ 지정 시간 복제 기능이 있다. 새 워크로드 등을 테스트할 때 용이하다. 스테이징 파일 시스템을 둘 수 있기 때문이다.

- `Amazon FSx for NetApp ONTAP`는 다음 중 ………………를 제외한 나머지 프로토콜과 호환됩니다.  
→ `FTP`

### 5. Amazon FSx for OpenZFS
- Managed OpenZFS file system on AWS  
→ `AWS`의 관리형 파일 시스템이다.

- File System compatible with NFS (v3, v4, v4.1, v4.2)  
→ 여러 버전의 `NFS` 프로토콜과 호환이 가능하다.

- Move workloads running on ZFS to AWS  
→ `ZFS`에서 실행되는 워크로드를 `AWS`로 옮길 때 사용된다.

- Works with:  
→ 다양한 운영체제를 지원한다.
~~~
- Linux
- Windows
- MacOS
- VMware Cloud on AWS
- Amazon Workspaces & AppStream 2.0
- Amazon EC2, ECS and EKS
~~~

- Up to 1,000,000 IOPS with < 0.5ms latency  
→ 성능이 상당히 좋다.

- Snapshots, compression and low-cost  
→ 스냅샷과 복제 기능을 제공한며 비용이 적게 든다. `ONTAP`과 다른 점은 데이터 중복 제거 기능이 없다는 점이다.

- Point-in-time instantaneous cloning (helpful for testing new workloads)  
→ `ONTAP`과 동일하게 지정 시간 복제 기능이 있다.

- 온프레미스 ZFS 파일 시스템을 `AWS`로 마이그레이션 하는데 가장 적합한 `AWS` 서비스는 무엇입니까?  
→ `Amazon FSx for OpenZFS`

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
