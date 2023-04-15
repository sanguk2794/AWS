## EC2 Hibernate
### 1. EC2 Hibernate
- 인스턴스를 중지하면 `EBS` 디스크 데이터는 다시 시작할 때까지 그대로 유지된다.
- 이 때, 인스턴스를 종료한다면 루트 볼륨이 삭제되게 했다면 인스턴스도 삭제된다.
- 하지만 그렇게 설정하지 않은 볼륨은 인스턴스가 종료되더라도 그대로 남는다.

- On start, the following happens:
→ 인스턴스 기동에는 다음과 같은 과정이 포함된다. 
~~~
First start: the OS boots & the EC2 User Data script is run
→ OS가 부트되고, User Data Script가 실행된다.

Following starts: the OS boots up 
→ OS 부팅이 완료된다.

Then your application starts, caches get warmed up, and that can take time!
→ 애플리케이션이 실행되고 캐시가 구성되므로, 과정이 끝날 때까지 다소 시간이 걸린다.
~~~

- 절전 모드의 특징은 다음과 같다.
~~~
The in-memory (RAM) state is preserved → The instance boot is much faster! (the OS is not stopped / restarted)
→ RAM의 인메모리 상태가 그대로 보존되므로 빠른 인스턴스 부팅이 가능하다.
이는 운영체제를 중지하거나 다시 시작하지 않고 그대로 멈춰 뒀기 때문이다.
 
- Under the hood: the RAM state is written to a file in the root EBS volume
- The root EBS volume must be encrypted
→ 절전 모드가 되면 RAM에 기록되어 있던 인메모리의 상태는 루트 경로의 `EBS` 볼륨에 기록된다.
따라서, EBS 볼륨을 암호화해야 하며, 볼륨 용량도 RAM을 저장하기 충분한 사이즈여야 한다. 
~~~

![image](https://user-images.githubusercontent.com/97398071/232237746-e7ed2d35-8c0d-45ce-8226-d2f2965eabfe.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 절전 모드를 실행시키면 실행 중인 인스턴스는 중지 상태로 전환되고 `RAM`의 내용은 `EBS Volumn`에 덤프된다.
그리고 `RAM`은 사라진다. 

- 재기동시 `EBS Volumn`에 덤프했던 `RAM` 정보를 그대로 가져와 인스턴스 메모리로 가져간다.
이렇게 하면 `EC2` 인스턴스를 중지한 적 없는 것 처럼 사용이 가능하다.

- Use cases:
~~~
- Long-running processing
→ 오래 실행되는 프로세서를 가지고 있고 중지하지 않을 때
- Saving the RAM state
→ 램 상태를 저장하고 싶을 때
- Services that take time to initialize
→ 서비스 초기화가 많은 시간이 걸릴 때, 이를 무시하고 빠르게 재부팅이 하고 싶을 때 
~~~

- 절전 모드를 지원하는 제품군이 많다.
~~~
C3, C4, C5, I3, M3, M4, R3, R4, T2, T3, ...
~~~

- 인스턴스 `RAM` 사이즈의 최대 크기는 `150GB`이다. 단, 이 사이즈는 이후 정책 변경에 의해 바뀔 수 있다.
- not supported for bare metal instances
- AMI – Amazon Linux 2, Linux AMI, Ubuntu, RHEL, CentOS & Windows
→ 윈도우, 리눅스 등의 여러 운영체제에서 사용할 수 있다.

- Root Volume – must be EBS, encrypted, not instance store, and large
→ 루트 볼륨, 즉 `EBS`에만 저장이 가능하며, 암호화가 필요하며 덤프된 램을 포함할 만큼의 충분한 용량이 필요하다.

- Available for On-Demand, Reserved and Spot Instances
→ 온디맨드, 예약, 스팟 같은 모든 종류의 인스턴스에 사용 가능하다.

- An instance can NOT be hibernated more than 60 day
→ 절전 모드는 최대 60일까지 사용할 수 있다. 이 기간은 이후 정책 변경에 따라 달라질 수 있다.

- `EC2` → `Instance` → `Launch an instance` → `details` → `Stop - Hibernate behavior`
→ 이 설정을 활성화하면 `EC2` 인스턴스를 절전모드로 둘 수 있게 된다. 밑의 경고 문구는 절전 모드를 활성화하는 경우 루트 볼륨에 `RAM`을 저장할 수
있는 공간이 충분한지, 그리고 루트 `EBS` 볼륨이 암호화되었는지 확인하라고 되어 있다.

![image](https://user-images.githubusercontent.com/97398071/232238334-13d347ff-42e0-4363-ac08-57aca50fe81a.png)

- 암호화 설정은 `EC2` → `Instance` → `Launch an instance` → `Configure storage` → `Advanced` → `EBS Volumns` → `Encrypted`에서 가능하다. 

- 메모리는 스토리지 용량과 메모리 용량을 비교하면 된다.

- 실제 절전모드에 들어갔는지 체크는 인스턴스 가동시간을 확인할 수 있는 `uptime` 커맨드를 사용하면 된다. 
`uptime`의 출력 시간이 1분 이상이 되면 인스턴스를 중지하고 다시 기동해 `uptime` 커맨드를 실행시키자.
~~~ shell script
$ uptime
23:30:50 up 0 min, 1 users ...
-- ...
23:31:50 up 1 min, 1 users ...

-- 재기동 후
$0 uptime
23:35:50 up 5 min, 1 users ...
~~~

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
