## Internet Gateway
### 1. Internet Gateway
- Allows resources (e.g., EC2 instances) in a VPC connect to the Internet  
→ `Internet Gateway`를 사용하면 `VPC` 내의 퍼블릭 서브넷의 리소스에 인터넷을 연결할 수 있다.

- It scales horizontally and is highly available and redundant  
→ 수평적으로 확장 가능하며 가용성과 중복성이 높다.

- Must be created separately from a VPC  
→ `VPC`와 별개로 생성해야 한다.

- One VPC can only be attached to one IGW and vice versa  
→ `VPC`는 `Internet Gateway` 하나와 연결된다. `Internet Gateway` 또한 하나의 `VPC`와 연결된다. 즉 일대일 관계이다.

- Internet Gateways on their own do not allow Internet access… Route tables must also be edited!
→ `Internet Gateway`에 대한 설정만으로는 인터넷 액세스가 불가능하다. 실제 인터넷 연결을 위해서는 라우팅 테이블을 함께 수정해야 한다. 라우팅 테이블을 수정해서 `EC2` 인스턴스를 라우터에 연결하고 `Internet Gateway`에 연결하면 인터넷 연결이 가능해진다.

- 기본 라우팅 테이블이 존재한다. 기본 라우팅 테이블은 테이블 연결이 없는 서브넷에 연결된다.

- 하나의 라우팅 테이블을 여러 개의 서브넷에 할당 가능하다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/f9012137-4b99-47a5-8ccc-12d189ee40bd)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 1. 라우팅 테이블
`IP` 네트워크를 통해 이동하는 데이터 패킷이 어디로 향하게 될지 결정하는 규칙이다. 주로 테이블 형태로 표시된다. 라우팅 테이블에는 패킷이 대상을 향한 최상의 경로를 따라 전달하는데 필요한 정보가 포함되어 있다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---