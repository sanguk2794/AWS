## Security Groups
### 1. Introduction to Security Groups
- Security Groups are the fundamental of network security in AWS  
→ 보안 그룹은 `AWS 클라우드`에서 네트워크 보안을 실행하는데 핵심이 된다.

- They control how traffic is allowed into or out of our EC2 Instances.  
→ 보안 그룹을 통해 `EC2` 인스턴스에 들어오고 나가는 트래픽을 제어할 수 있다.

- Security groups only contain rules  
→ 보안 그룹은 허용 규칙만을 포함한다.

- Security groups rules can reference by IP or by security group  
→ `IP Address` 또는 다른 보안 그룹을 참조해 규칙을 생성할 수 있다.

- Security groups are acting as a “firewall” on EC2 instances
~~~
- Access to Ports
→ Port 액세스를 제어할 수 있다.

- Authorised IP ranges – IPv4 and IPv6
→ 인증된 IP 주소의 범위를 확인해 IPv4인지 IPv6인지 확인할 수 있다.

- Control of inbound network (from other to the instance)
→ 외부에서 인스턴스로 들어오는 인바인드 네트워크를 통제할 수 있다.

- Control of outbound network (from the instance to other)
→ 인스턴스에서 외부로 나가는 아웃바운드 네트워크를 통제할 수 있다.
~~~

![Security Group Diagram](https://user-images.githubusercontent.com/97398071/231209003-3206de7e-2207-4c3e-9fad-45e08c7b1150.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 보안 그룹은 `Type`과 프로토콜, 트래픽이 인스턴스에서 통과하는 위치인 포트 범위, `IP Address`의 범위인 소스 등을 포함한다.  
→ 허용되지 않은 소스로부터의 접근은 방화벽이 차단하기 때문에 외부에서 액세스가 불가능하고 액세스 시 타임아웃이 발생한다.

![image](https://user-images.githubusercontent.com/97398071/231207724-7d957894-0ffb-4b84-93e2-5ba10172e6fa.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Can be attached to multiple instances  
→ 보안 그룹과 인스턴스는 다대다 관계이다. 보안 그룹을 여러 인스턴스에 연결할 수 있으며 여러 인스턴스를 보안 그룹에 연결할 수 있다.

- Locked down to a region / VPC combination  
→ 보안 그룹은 지역과 `VPC`의 결합으로 통제되어 있다. 그래서 지역을 변경하면 새로운 보안 그룹을 생성하거나 다른 `VPC`를 생성해야 한다.

- Does live “outside” the EC2 – if traffic is blocked the EC2 instance won’t see it  
→ 보안 그룹은 `EC2` 인스턴스의 외부에 존재한다. 그래서 트래픽이 차단되면 `EC2` 인스턴스는 확인할 수 없다. `EC2`에서 실행하는 애플리케이션이 아니라 `EC2` 외부의 방화벽인 것이다.

- It’s good to maintain one separate security group for SSH access  
→ `SSH` 액세스에 대해서 별도의 보안 그룹을 유지하는 것이 좋다. 보통 `SSH` 액세스는 가장 복잡하므로 보안 그룹의 설정에는 주의를 기울여야 한다.

- If your application is not accessible (time out), then it’s a security group issue  
→ 타임 아웃으로 `EC2`에 접근할 수 없으면 이것은 보안 그룹의 문제이다.

- If your application gives a “connection refused“ error, then it’s an application error or it’s not launched  
→ 하지만, 연결 거부 오류가 발생하면 보안 그룹은 실행됐고 트래픽은 통과했지만 애플리케이션에 문제가 생겼거나 실행되지 않는 등 문제가 발생한 것이다.

- All inbound traffic is blocked by default, All outbound traffic is authorised by default  
→ 기본적으로 모든 인바운드 트래픽은 거부되고 모든 아웃바운드 트래픽은 허용된다.

### 2. Referencing other security groups Diagram
- 보안 그룹 설정을 통한 인바운드 트래픽 제어 시나리오이다.
~~~
- EC2 인스턴스에 보안 그룹 1이라는 보안 그룹을 설정했다. 이 보안 그룹의 기본적인 인바인드 규칙은 보안 그룹 1의 인바운드를 보안 그룹 2에 허용하는 것이다.
- 인바운드 규칙으로 보안 그룹 2의 연결을 허용했다. 따라서 보안 그룹 2를 통한 연결은 허용된다.
- 인바운드 규칙으로 보안 그룹 3의 연결을 명시적으로 허용하지 않았다. 따라서 보안 그룹 3을 통한 연결은 거부된다.
~~~

![Referencing other security groups Diagram](https://user-images.githubusercontent.com/97398071/231212524-46b0725c-2ac9-4c96-9176-fa975e6afa4a.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. Classic Ports to know
- 22 = SSH (Secure Shell) - log into a Linux instance
- 21 = FTP (File Transfer Protocol) – upload files into a file share

- 22 = SFTP (Secure File Transfer Protocol) – upload files using SSH  
→ `SSH`는 다른 시스템으로 파일을 전송할 수 있는 파일 전송 프로토콜 역할도 수행한다.

- 80 = HTTP – access unsecured websites
- 443 = HTTPS – access secured websites

- 3389 = RDP (Remote Desktop Protocol) – log into a Windows instance  
→ 원격 데스크톱 프로토콜이다. 윈도우 인스턴스에 로그인할 때 사용된다. 22번 포트는 리눅스용이며 3389번 포트는 윈도우 인스턴스용인 것이다.

### 4. Practice Network and Security Groups
![Security Groups](https://user-images.githubusercontent.com/97398071/231216317-8e9b3281-0687-4468-8966-0ea27b3a8d54.png)

- `Network & Security` → `Security Groups`  
→ 보안 그룹을 확인할 수 있으며, 각 보안 그룹의 인바인드 규칙, 아웃바인드 규칙을 설정할 수 있다.

![Inbound Rule](https://user-images.githubusercontent.com/97398071/231217169-40d482f6-bf18-47da-b0a4-65942e215c14.png)

- `SSH`를 시도하거나 `HTTP` 쿼리를 실행하는데 타임아웃이 발생한다면 그건 100% `EC2` 보안 그룹의 설정 문제이다.

---
#### ▶ Reference
- [[네트워크] SSH란?](https://hanamon.kr/네트워크-ssh란/)
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---