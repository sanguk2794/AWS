## VPC Endpoints
### 1. VPC Endpoints
- `AWS`에서 `DynamoDB`와 같은 서비스를 이용하면 퍼블릭 액세스가 가능하다.  
→ 그 이유는 `DynamoDB`의 `VPC`가 `NAT Gateway`와 `Internet Gateway`를 통한 퍼블릭 액세스를 허용하고 있기 때문이다. 이 때의 트래픽은 공용 인터넷을 거친다.

- 해당하는 리전 내의 `CloudWatch`, `Amazon S3` 등의 서비스를 이용할 때 공용 인터넷을 경유하지 않는 프라이빗 액세스를 원할 수 있다. 이 때 `VPC Endpoints`를 사용하면 공용 인터넷을 거치지 않고도 인스턴스에 액세스할 수 있다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/efb87b3e-bf8b-447f-a235-f9b862084481)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 인스턴스들이 외부의 `SNS` 서비스에 액세스할 때의 시나리오이다. 공용 액세스의 단점을 확인할 수 있다.
~~~
- Private Subnet의 인스턴스들은 NAT Gateway → Internet Gateway를 거쳐 SNS 서비스에 액세스한다.
→ 이 방법은 비용이 많이 든다. NAT Gateway를 거칠 때 비용이 발생하기 때문이다. 

- Public Subnet의 인스턴스들은 Internet Gateway를 거쳐 SNS 서비스에 액세스한다.
→ Internet Gateway에서는 비용이 발생하지 않지만 공용 인터넷에서 여러 개의 홉을 거치므로 효율적이라고는 할 수는 없다.
~~~ 

- 하지만 `VPC Endpoints`를 거치도록 설정하면 프라이빗 서브넷에 있는 `EC2` 인스턴스에서 `SNS` 서비스에 접근할 때, `AWS` 네트워크 내에서만 통신이 이루어지도록 설정할 수 있다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/6bce2f91-35aa-4c9b-8fb8-14bd556ec8c2)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Every AWS service is publicly exposed(public URL)  
→ 모든 `AWS` 서비스는 퍼블릭 액세스가 허용되며 이를 위한 퍼블릭 `URL`을 가진다.

- VPC Endpoints (powered by AWS PrivateLink) allows you to connect to AWS services using a private network instead of using the public Internet  
→ `VPC Endpoints`를 사용하면 `AWS PrivateLink`를 통해 프라이빗으로 액세스하므로 `AWS`에 있는 모든 서비스에 액세스할 때 퍼블릭 인터넷을 거치지 않을 수 있다.

- They’re redundant and scale horizontally  
→ `VPC Endpoints`는 중복과 수평 확장이 가능하다.

- They remove the need of IGW, NATGW, … to access AWS Services  
→ `Internet Gateway`나 `NAT Gateway` 없이 `AWS` 서비스에 액세스할 수 있으므로 네트워크 인프라가 상당히 간단해진다.

- In case of issues:
~~~
- Check DNS Setting Resolution in your VPC
- Check Route Tables
→ 문제가 발생하면 VPC에서 DNS 설정의 해석을 확인하거나 라우팅 테이블을 확인해야 한다.
~~~

### 2. Types of Endpoint
- `VPC Endpoints`는 `PrivateLink`를 이용하는 인터페이스 엔드포인트와 게이트웨이 엔드포인트의 두 종류가 있다.

- Interface Endpoints (powered by PrivateLink)
~~~
- Provisions an ENI (private IP address) as an entry point (must attach a Security Group)
→ 인터페이스 엔드포인트는 ENI를 프로비저닝해야 한다. 이 때, ENI는 VPC의 프라이빗 IP 주소이자 AWS의 엔트리 포인트가 된다. ENI가 있으므로 반드시 보안 그룹을 연결해야 한다.

- Supports most AWS services
→ 대부분의 AWS 서비스를 지원한다.

- $ per hour + $ per GB of data processed
→ 시간 단위 또는 처리되는 데이터 단위로 비용이 청구된다.
~~~

- Gateway Endpoints
~~~
- Provisions a gateway and must be used as a target in a route table (does not use security groups)
→ 게이트웨이 엔드포인트는 특이하게 게이트웨이를 프로비저닝해야 한다. 그리고 이때의 게이트웨이는 반드시 라우팅 테이블의 대상이 되어야 한다.
IP 주소를 사용하거나 보안 그룹을 사용하지 않고 라우팅 테이블의 대상이 될 뿐인 것이다. 중요하다.

- Supports both S3 and DynamoDB
→ 게이트웨이 엔드포인트 대상에는 Amazon S3와 DynamoDB의 두 가지가 있다. 다른 서비스는 지원하지 않는다.

- Free
→ 무료이다.

- S3 게이트웨이 엔드포인트에 대한 버킷 정책 설정과 S3 버킷에 대한 게이트웨이 엔드포인트 정책 설정이 필수이다.
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/069f2376-da33-49fd-91ce-c13da3c6f9c3)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. Gateway or Interface Endpoint for S3?
- `Amazon S3` 또는 `DynamoDB`의 엔드포인트를 선택해야 한다면 게이트웨이 엔드포인트를 선택하는 것이 유리하다. 무료인 데다가 루트 테이블만 수정하면 되기 때문에 관리가 편리하기 때문이다.

- Interface Endpoint is preferred access is required from on-premises (Site to Site VPN or Direct Connect), a different VPC or a different region  
→ 게이트웨이 엔드포인트보다 인터페이스 엔드포인트가 권장되는 경우는 온프레미스에서 액세스해야 할 필요가 있을 때 뿐이다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/6719029b-623f-44e1-a089-cee118d43fd6)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
