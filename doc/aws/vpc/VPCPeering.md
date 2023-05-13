## VPC Peering
### 1. VPC Peering
- Privately connect two VPCs using AWS’ network  
→ `VPC`는 `AWS`의 네트워크를 사용하여 연결된다.

- Make them behave as if they were in the same network  
→ 동일한 네트워크에 있는 것처럼 동작하도록 한다.

- VPC Peering connection is NOT transitive(must be established for each VPC that need to communicate with one another)  
→ 서로 다른 `VPC`가 통신하려면 `VPC Peering Connection`을 활성화해야 한다.

- Must not have overlapping CIDRs  
→ `VPC Peering Connection`을 활성화할 때, `VPC`의 `CIDR`는 중복되면 안된다. 연결했을 때 `CIDR`가 겹치면 통신을 할 수 없기 때문입니다

- You must update route tables in each VPC’s subnets to ensure EC2 instances can communicate with each other  
→ `VPC Peering Connection`을 활성화하는 것만으로 통신이 가능한 것은 아니다. `EC2` 인스턴스가 서로 통신할 수 있도록 각 `VPC`의 서브넷에서 `Route Table`의 설정을 변경해야 한다.

- 세 개의 `VPC`인 `A`, `B`, `C`를 `VPC Peering`을 통해 연결하는 시나리오는 다음과 같다. 굉장히 중요하다.
~~~
- A와 B 사이에 피어링 연결을 만들어 A와 B를 연결한다.
- B와 C 사이에 피어링 연결을 만들어 B와 C를 연결한다.
- A와 B, 그리고 B와 C가 연결되어 있다고 해서 A와 C가 서로 통신할 수 있는 것은 아니다. A와 C의 VPC 피어링 연결을 만들어 A와 C를 연결해야 한다.
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/c49e79ac-ae1f-41e8-9278-1d16998585cd)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. VPC Peering – Good to know
- You can create VPC Peering connection between VPCs in different AWS accounts/regions  
→ 같은 계정이나 같은 리전의 `VPC`가 아니더라도 연결이 가능하다.

- You can reference a security group in a peered VPC (works cross accounts – same region)  
→ 같은 리전의 계정 간 `VPC`에서도 보안 그룹을 참조할 수도 있다. `CIDR`이나 `IP`를 소스로 가질 필요가 없다는 것은 대단한 장점이다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/06552882-9ba9-4d3d-bba4-7e81777b7d9c)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- `VPC Peering`을 추가한 네트워크의 구성도는 아래와 같다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/14cdc27a-d6db-44ea-925d-81a8ac4827e4)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)
---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---