## NACL
### 1. NACL
- NACL = Network Access Control List  
→ 내클이라 발음한다.

- `NACL`은 서브넷 단위이고, `Security Group`은 인스턴스 단위이다. 따라서 외부 통신의 경우 `NACL`과 `Security Group`을 모두 거쳐야 하고 내부 통신의 경우 `Security Group`만 거친다.

- `NACL`은 요청 정보를 따로 저장하지 않아 응답 트래픽도 제어를 해야한다. 이를 `Stateless`라고 한다.

- `Security Group`은 요청 정보를 저장하여 응답하므로 응답 트래픽 제어를 하지 않는다. 이를 `Stateful`이라고 한다.

- NACL are like a firewall which control traffic from and to subnets  
→ `NACL`은 서브넷을 오가는 트래픽을 제어하는 방화벽과 비슷한 역할을 한다.

- One NACL per subnet, new subnets are assigned the Default NACL  
→ 서브넷마다 하나의 `NACL`이 있고 새로운 서브넷에는 항상 기본 `NACL`이 할당된다.

- You define NACL Rules:
~~~
- Rules have a number (1-32766), higher precedence with a lower number
→ NACL의 범위는 1부터 32,766까지이다. 숫자가 낮을수록 우선순위가 높다.

- First rule match will drive the decision
→ 첫 번째 규칙 비교가 결과를 결정한다.

- Example: if you define #100 ALLOW 10.0.0.10/32 and #200 DENY 10.0.0.10/32, the IP address will be allowed because 100 has a higher precedence over 200
→ 예를 들어 CIDR에서 ALLOW를 정의하고 같은 CIDR인 특정 IP를 DENY로 정의하면 ALLOW는 100이고 DENY가 200이기 때문에 IP 주소는 허용된다.

- The last rule is an asterisk (*) and denies a request in case of no rule match
→ 마지막 규칙은 아스타리스크이다. 일치하는 규칙이 없을 경우 모든 요청을 거부한다.

- AWS recommends adding rules by increment of 100
→ AWS는 규칙을 100씩 추가하는 것을 권장한다. 그 사이에 규칙을 추가하기 편하기 때문이다.
~~~

- Newly created NACLs will deny everything  
→ 새로 만들어진 `NACL`은 기본적으로 모든 트래픽을 거부한다.

- NACL are a great way of blocking a specific IP address at the subnet level  
→ `NACL`은 서브넷 수준에서 특정한 `IP` 주소를 차단하는데 적합하다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/a77adf66-381d-4fce-a124-df4d66b77c8c)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. Default NACL
- 기본 `NACL`은 시험에서 정말 중요한 부분이다.
- Accepts everything inbound/outbound with the subnets it’s associated with  
→ 기본 `NACL`는 연결된 서브넷을 가지고 인바운드와 아웃바운드의 모든 트래픽을 허용하는 특수성을 가진다. `NACL`이 모든 트래픽을 허용하지 않으면 `AWS`를 시작할 때 디버깅을 해야 할 것이다. 기본 `NACL`은 매우 개방적이다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/6c60aea2-1ab4-48d2-87ee-64ebc31b5574)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Do NOT modify the Default NACL, instead create custom NACLs  
→ 기본 `NACL`을 수정하지 않는 것이 권장된다. 필요하다면 사용자 정의 `NACL`을 생성할 수 있다.

- 시험에서 기본 `NACL`이 기본 서브넷과 연결되어 있다고 한다면 모든 트래픽이 허용된다는 뜻이다.

### 3. Ephemeral Ports
- Ephemeral Ports = 임시 포트, 이 포트는 임시라서 클라이언트와 서버 간의 연결이 유지되는 동안만 열려 있다.

- For any two endpoints to establish a connection, they must use ports  
→ 클라이언트와 서버가 연결되면 포트를 사용해야 한다.

- Different Operating Systems use different port ranges, examples:  
→ `OS`에 따라 포트 범위가 달라진다.
~~~
- IANA & MS Windows 10 → 49152 – 65535
→ Windows 10을 사용하면, 49152에서 65,535까지가 임시 포트에 할당된다.

- Many Linux Kernels → 32768 – 60999
→ Linux를 사용할 경우 범위는 32768에서 60999까지가 임시 포트에 할당된다.
~~~

- 임시 포트 할당 시나리오는 다음과 같다. 임시 포트를 제대로 구성하는 것은 정말 중요하다.
~~~
- 클라이언트가 웹 서버에 연결되며 웹 서버에는 고정 `IP` 및 고정 포트가 있다. 그리고 한 번의 요청 또는 연결에만 임시 포트 50105를 개방하려고 한다.
- 클라이언트가 서버로 요청을 전송한다. 클라이언트에서 서버로 전송된 요청에는 목적지 IP 정보와 서버가 연결할 목적지 포트, 요청과 관련된 페이로드가 포함된다. 이 때 임시 포트인 50105를 사용한다.
- 웹 서버가 응답한다. 여기에는 소스 IP와 소스 포트 정보, 목적지 IP, 응답 페이로드가 포함된다. 이 때 클라이언트가 전송했던 임시 포트인 50105를 재사용한다.
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/fc77d127-94c6-4374-bd11-ced39d3bba97)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 1. 페이로드
페이로드는 전송되는 데이터를 뜻한다. 페이로드는 전송의 근본적인 목적이 되는 데이터의 일부분으로 그 데이터와 함께 전송되는 헤더와 메타데이터와 같은 부분은 제외한다.

### 4. NACL with Ephemeral Ports
- `NACL`의 또 다른 특징은 다중 `NACL` 및 서브넷이 있다면 각 `NACL` 조합이 `NACL` 내에서 허용되어야 한다는 것이다. `CIDR` 사용 시, 서브넷이 고유의 `CIDR`을 갖기 때문이죠

- `NACL`에 서브넷을 추가하면 `NACL` 규칙도 업데이트해서 연결 조합이 가능한지 확인해야만 한다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/fa487d16-afbd-4f15-ae7d-4ab08e41bf71)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 5. Security Group vs. NACLs
| Security Group                                                             | NACL                                                                                                      |
|----------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| Operates at the instance level                                             | Operates at the subnet level                                                                              |
| Supports allow rules only                                                  | Supports allow rules and deny rules                                                                       |
| Stateful: return traffic is automatically allowed, regardless of any rules | Stateless: return traffic must be explicitly allowed by rules (think of ephemeral ports)                  |
| All rules are evaluated before deciding whether to allow traffic           | Rules are evaluated in order (lowest to highest) when deciding whether to allow traffic, first match wins |
| Applies to an EC2 instance when specified by someone                       | Automatically applies to all EC2 instances in the subnet that it’s associated with                        |

- `NACL`은 서브넷 단위에서 작동하고 `Security Group`은 인스턴스 단위에서 작동한다.
- 보안 그룹은 허용 규칙을 지원하지만 `NACL`은 허용과 거부 규칙을 모두 지원한다.
- 보안 그룹은 상태 유지이기에 규칙과 무관하게 `return` 트래픽을 허용한다. 심지어 아웃바운드 룰을 삭제해도 제대로 작동한다. 그리고 `NACL`은 무상태이기 때문에 인바운드와 아웃바운드 규칙이 매번 평가되고 트래픽 허용 여부를 설정한다.
- 보안 그룹에서는 모든 규칙이 평가되고 트래픽 허용 여부를 결정하지만 `NACL`에서는 가장 높은 우선순위를 가진 것이 평가의 기준이 된다.
- 보안 그룹은 `EC2` 인스턴스에 적용되지만 `NACL`은 연결된 서브넷의 모든 EC2 인스턴스에 적용된다.

---
#### ▶ Reference
- [[AWS] NACL vs Security Group (Stateless와 Stateful 차이)](https://honglab.tistory.com/153)
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---