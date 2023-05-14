## Creating a highly available EC2 instance
### 1. Creating a highly available EC2 instance
- `EC2` 인스턴스는 기본적으로 하나의 가용 영역에서 실행된다. 그렇기 때문에 가용성이 그리 높지 않지만 아키텍처 설계를 통해 가용성을 높일 수 있다.

- 웹 서버를 가동하고 있는 공용 `EC2` 인스턴스가 있고 웹 서버에 액세스 한다고 가정한다.
~~~
- EC2 인스턴스에 탄력적 IP를 연결하고 사용자가 탄력적 IP를 통해 웹사이트에 곧장 액세스하게 설정해야 한다.
- 그러면 사용자는 EC2 인스턴스에서 직접 작업할 수 있고 웹 서버에서 결과를 받을 수 있다.
- 일이 잘못될 경우를 대비해 대기 인스턴스를 만들어 EC2 인스턴스의 가용성을 높일 수 있다.
→ 무언가가 잘못되었다는 것을 알려면 모니터링이 필수이다. 

- 이미 알고 있는 이벤트에 기반해 CloudWatch Events나 경보를 만들면 인스턴스의 상태를 확인할 수 있다.
→ 만약 웹 서버가 있다면 CPU가 100%까지 올라갈 경우를 대비해 CPU를 모니터링하는 CloudWatch 경보를 설정해야 할 것이다.

- 경보나 CloudWatch Events를 통해 람다 함수를 실행시킬 수 있다.
→ 인스턴스가 실행되지 않았을 경우 새 인스턴스를 실행하거나, 대기 인스턴스에 탄력적 IP를 연결하는 등의 실행이 가능하다.

- 사용자는 탄력적 IP를 통해 아키텍처와 소통하기 때문에 무슨 일이 일어나는지 알 수 없다. 백엔드에서 진행되기 때문이다.

- 탄력적 IP가 다른 인스턴스에 연결되면 이 IP는 원래 인스턴스에서 분리된다. 한 탄력적 IP는 한 인스턴스에만 연결될 수 있기 때문이다.

- 이전 EC2 인스턴스는 중지하거나 종료하는 것이 바람직하다.
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/1dbbb12f-a180-4805-a03c-a53fc2dd0e43)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. Creating a highly available EC2 instance With an Auto Scaling Group
- `ASG`를 사용하면 `CloudWatch 경보`나 `Events`가 필요없다. 한 인스턴스가 종료되는 것을 `ASG`가 확인하면 설정한 대로 새 EC2 인스턴스를 생성해주기 때문이다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/cc93ce66-1596-45fd-a51e-8902cb2f63a7)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. Creating a highly available EC2 instance With ASG + EBS
- `EC2` 인스턴스를 상태 유지하게 하고 `EBS` 볼륨을 할당할 수 있다.

- 인스턴스가 종료될 경우 `ASG`는 수명 주기 후크를 사용한다.
→ 이 수명 주기 후크를 사용하면 EBS 볼륨에서 스냅샷을 얻을 수 있는 스크립트를 생성할 수 있다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/6d57d44d-bcd1-4587-9dde-7d8abfcbb1b1)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---