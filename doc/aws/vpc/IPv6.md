## IPv6
### 1. IPv6 in VPC
- IPv4 cannot be disabled for your VPC and subnets  
→ `VPC`와 서브넷에서 `IPv4`를 비활성화하는 것은 불가능하다.

- You can enable IPv6 (they’re public IP addresses) to operate in dual-stack mode  
→ `VPC`와 서브넷에서 `IPv6`를 활성화할 수 있다. 공용 `IP` 주소이며 듀얼 스택 모드로 작동한다.

- Your EC2 instances will get at least a private internal IPv4 and a public IPv6  
→ `EC2` 인스턴스가 `VPC`에서 실행되면 최소 사설 내부 `IPv4` 하나와 공용 `IPv6` 하나를 얻는다.

- They can communicate using either IPv4 or IPv6 to the internet through an Internet Gateway  
→ `IPv4`나 `IPv6`를 사용하면 인터넷 게이트웨이를 통해 인터넷에 통신할 수 있다.

### 2. IPv6 Troubleshooting
- So, if you cannot launch an EC2 instance in your subnet  
→ 만약 `IPv6`가 활성화된 `VPC`가 있는데 서브넷에서 `EC2` 인스턴스를 실행할 수 없다고 한다면 실제로 인스턴스가 `IPv6`을 받지 못해서가 아니다.

- It’s because there are no available IPv4 in your subnet  
→ 진짜 원인은 서브넷에 이용 가능한 `IPv4`가 없기 때문이다.

- Solution: create a new IPv4 CIDR in your subnet  
→ 솔루션은 서브넷에 `IPv4 CIDR`를 생성하는 것이다.

- `IPv4` 공간이 전부 소진된다면 사용자가 새 `EC2` 인스턴스를 생성할 시 오류가 발생한다.  
→ `EC2` 인스턴스에 `IPv4` 주소가 반드시 할당되어야 하기 때문이다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---