## Blocking an IP address
### 1. Blocking an IP address
- `EC2` 인스턴스를 사용한 간단한 솔루션 아키텍처이다.
~~~
- 클라이언트 차단을 원한다면 VPC 레벨에서의 VPC의 NACL 방어가 첫 번째 방어선이 된다.
→ 이 네트워크 ACL에서는 차단 규칙을 만들 수 있다. 클라이언트 IP를 아주 간단하고 빠르며 저렴하게 차단 가능하다.

- 애플리케이션이 글로벌이라면 보안 그룹도 도움이 되지 않는다.
→ EC2 인스턴스의 보안 그룹에는 차단 규칙을 만들 수 없기 때문이다. 오직 허용 규칙만 설정만 가능하다.

- 선택적으로 방화벽 소프트웨어를 EC2 인스턴스에서 실행해 클라이언트의 요청을 거절할 수 있다.
→ 요청이 EC2 인스턴스에 도착했기에 요청을 처리하는 CPU 비용이 발생한다.
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/2d9ed7ec-d1c4-4622-930c-4e4afeee4e7a)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. Blocking an IP address – with an ALB
- `EC2` 인스턴스 앞에 `ALB`를 두고 있는 솔루션 아키텍처이다.
~~~
- 이전 아키텍쳐와 비교했을 때 다른점은 총 2개의 보안 그룹을 가진다는 것이다. 
→ EC2 인스턴스 보안 그룹에 더해 ALB 보안 그룹이 추가되었다.

- 이 경우 EC2 보안 그룹은 ALB의 보안 그룹을 허용하도록 구성되어야 한다.

- 이전 아키텍처와 동일하게 NACL 방어는 유효하다.
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/e502d1a3-cc9c-404a-beed-15e292ea0fd1)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. Blocking an IP address – with an NLB
- 마찬가지로 클라이언트로부터 `IP` 주소를 하나만 차단하려면 방어를 위해 `NACL`을 사용해야 한다.
- 네트워크 로드 밸런서는 보안 그룹이 없고 모든 트래픽이 지나가기 때문이다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/d4172a81-74f7-47f6-9cc2-8aa8de4f73cf)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 4. Blocking an IP address – ALB + WAF
- `ALB` 구조에서 추가적으로 `WAF`를 설치해 `IP`를 차단할 수 있다. 
- `WAF`는 추가 서비스이며 방화벽 서비스이기 때문에 조금 비싸다. 하지만 `IP` 주소에 대한 복잡한 필터링이 가능하며 규칙을 만들어서 클라이언트로부터 동시에 많은 요청을 받지 않도록 할 수 있기 때문에 보안이 더 강력해진다.
- `WAF`는 클라이언트와 `ALB` 사이의 서비스가 아니라 `ALB`에 설치되는 서비스이다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/5d68f9c0-12ba-4d8d-9e7f-5d132584866e)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 5. Blocking an IP address – ALB, CloudFront WAF
- `EC2` 인스턴스 앞에 `ALB`를 두고 `ALB` 앞에 `CloudFront`를 두는 아키텍처이다.

- `ALB`는 모든 엣지 로케이션으로부터 오는 `CloudFront`의 공용 `IP`를 허용해야 한다. 이 목록은 온라인에서 확인할 수 있다.

- `CloudFront`에서 클라이언트를 막으려면 두 가지 방법이 있다.
~~~
- CloudFront의 지리적 제한 기능을 이용해서 CloudFront에서 클라이언트의 국가를 차단할 수 있다.
- 특정 IP 하나만 막고 싶다면 CloudFront에 설치된 WAF나 웹 애플리케이션 방화벽을 사용해서 IP 주소를 필터링할 수 있다.
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/fe2b6ab4-cc36-4638-9f00-3ba16b70bdfc)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 솔루션 아키텍트인 여러분은 기업을 위해 다음과 같은 `AWS` 서비스를 포함하는 아키텍처를 구축했습니다. `CloudFront`, 웹 애플리케이션 방화벽(`AWS WAF`), `AWS Shield`, `Application Load Balancer` 및 오토 스케일링 그룹이 관리하는 `EC2` 인스턴스. 이 기업에서는 가끔 악성 요청을 수신하고 있으며, 이 `IP` 주소를 차단하고자 합니다. 이 아키텍처에 따르면, 다음 중 어느 서비스에서 이 작업을 수행해야 할까요?
→ `AWS WAF`


---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---