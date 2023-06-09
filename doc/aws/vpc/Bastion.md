## Bastion Hosts
### 1. Bastion Hosts
- The bastion is in the public subnet which is then connected to all other private subnets  
→ 배스천 호스트는 공인 `IP` 주소로 프로비저닝되고 `SSH`를 통해 액세스할 수 있는 인스턴스이다. 공인 `IP` 주소로 프로비저닝되기 위해 퍼블릭 서브넷 내에 배치된다.

- We can use a Bastion Host to SSH into our private EC2 instances  
→ 배스천 호스트를 통해 프라이빗 서브넷의 `EC2` 인스턴스에 `SSH`로 액세스할 수 있다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/785a661e-ecc1-4e7c-b8af-c61b5053cac4)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Bastion Host security group must allow inbound from the internet on port 22 from restricted CIDR, for example the public CIDR of your corporation  
~~~
- 배스천 호스트는 배스천 호스트 보안 그룹이라는 자체 보안 그룹을 가진다.
- 배스천 호스트를 위해서는 보안 그룹이 반드시 인터넷 액세스를 허용해야 한다.
- 하지만 모든 인터넷 액세스를 허용하면 보안상 위험이 크다. 기업의 퍼블릭 CIDR 액세스, 사용자의 인터넷 액세스 등 특정 IP만 액세스 가능하도록 범위를 제한하는 것이 추천된다.
~~~

- Security Group of the EC2 Instances must allow the Security Group of the Bastion Host, or the private IP of the Bastion host  
→ 프라이빗 서브넷의 `EC2` 인스턴스 보안 그룹에서는 반드시 `SSH` 액세스를 허용해야 한다. `EC2` 인스턴스에 배스천 호스트를 통해 접속해야 하기 때문이다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---