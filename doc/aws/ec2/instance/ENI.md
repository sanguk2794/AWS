## ENI
### 1. Elastic Network Interfaces
- Logical component in a VPC that represents a virtual network card  
→ 가상 네트워크 카드를 나타내는 `VPC`의 논리적 구성 요소이다. `ENI`는 `EC2` 인스턴스가 네트워크에 액세스할 수 있게 해준다.

- The ENI can have the following attributes:
~~~
- Primary private IPv4, one or more secondary IPv4
→ 주요 사설 IPv4와 하나 이상의 보조 IPv4를 가질 수 있다. 또, EC2에 보조 ENI를 얼마든지 연결할 수 있다.

- One Elastic IP (IPv4) per private IPv4 and public IPv4
→ 각각의 ENI는 사설 IPv4당 Elastic IPv4를 갖거나 혹은 하나의 공용 IPv4를 가질 수 있으므로 사설 및 공용 IP가 한 개씩 제공된다.

- One or more security groups
→ ENI에 하나 이상의 보안 그룹을 연결할 수 있다.

- A MAC address
→ 맥 주소를 연결할 수 있다.
~~~

- You can create ENI independently and attach them on the fly (move them) on EC2 instances for fail over  
→ `EC2` 인스턴스와 독립적으로 `ENI`를 생성해 인스턴스에 즉시 연결할 수 있다.

- Bound to a specific availability zone (AZ)  
→ `ENI`는 특정 가용 영역에만 바인딩된다.

- 하나의 인스턴스에서 문제가 생겼을 때 `ENI`를 옮겨서 사설 `IP`를 이동시킬 수 있다. 그러면 사설 `IP`가 문제 인스턴스에서 문제가 발생하지 않은 두 번째 인스턴스로 연결되므로 장애 조치에 매우 유용하다.

![image](https://user-images.githubusercontent.com/97398071/232232936-cc5ed9fe-1cda-493f-827b-82278fb89e5a.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 네트워크 인터페이스는 인스턴스의 `Network` → `Network Instance` 탭에서 확인이 가능하다.  
→ 각 인스턴스에는 기본적으로 하나의 네트워크 인터페이스가 있다.

![image](https://user-images.githubusercontent.com/97398071/232233225-4270f098-6906-471c-88bd-a351d47af171.png)

- 네트워크 인터페이스는 `EC2` → `Network Interface` 탭에서 확인 가능하다.

![image](https://user-images.githubusercontent.com/97398071/232233392-c3b49581-8882-4920-85a5-3c5b7ca6f19c.png)

- 네트워크 인터페이스 생성 화면은 아래와 같다.

![image](https://user-images.githubusercontent.com/97398071/232233504-867fa52b-920e-45fb-a46e-3106dfb6526b.png)

- 생성 후 `Attach`를 통해 인스턴스를 연결할 수 있다.
- 인스턴스를 종료하면 직접 생성한 `ENI`는 남아 있지만 인스턴스 생성시 자동 생성된 `ENI`는 자동으로 삭제된다.
- `ENI`를 사용하면 사설 `IPv4` 주소를 더 많이 활용할 수 있다. 네트워킹 작업이 수월해져서 복잡한 사용 사례에 많은 도움이 된다.
- `ENI`는 비용이 발생하지 않는다.

### 2. 고가용성 달성을 위한 방법
- 고가용성을 달성하려면 인스턴스 장애 시 트래픽을 리디렉션할 수 있는 기능이 필요하며, 이를 위한 3가지 옵션이 존재한다. 결론부터 말하자면 `ELB`를 사용해야 한다. 진정한 고가용성을 실현 가능하다.

- `ELB` 사용  
→ 고가용성을 제공하기 위해 선호되는 방법이다.

- 탄력적 `IP` 주소 리디렉션  
→ 탄력적 `IP` 주소를 사용하면 여러 가용 영역에 걸쳐 있는 인스턴스들을 지정할 수 있다. 실패를 감지하고 탄력적 `IP`를 다시 연결할 때 직접 작업이 필요하다.

- `ENI` 재할당  
→ 모든 `EC2` 인스턴스는 기본 `ENI`를 보유하며, 선택적으로 추가 `ENI`를 보유할 수 있다. 트래픽을 보조 `ENI` 로 보낸 다음 해당 보조 `ENI`를 다른 인스턴스로 이동시킬 수 있다. 이는 탄력적 `IP` 주소 재할당과 유사하다.

- `ENI`는 탄력적 `IP`가 가지고 있는 문제들과 더불어 동일한 `AZ` 내에서만 다시 할당할 수 있다는 추가적 문제까지 있으므로 사용이 권장되지 않는다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
- [AWS: Elastic IP vs ENI](https://stackoverflow.com/questions/36608349/aws-elastic-ip-vs-eni)
---