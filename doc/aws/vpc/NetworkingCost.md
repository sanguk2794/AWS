## Networking Costs
### 1. Networking Costs in AWS per GB - Simplified
- 비용 청구는 다음과 같다.
~~~
- 같은 AZ 내에서 EC2 인스턴스로 들어가는 트래픽은 모두 무료이다.

- 다른 AZ 내의 EC2 인스턴스 간 통신을 위해 Public IP나 Elastic IP를 사용할 수 있다. 이 때 청구되는 비용은 GB당 2센트이다.
→ 두 인스턴스가 통신하려면 트래픽이 AWS 네트워크 밖으로 나갔다 다시 들어와야 하므로 AWS에서 비용을 청구하는 것이다. 대신 사설 IP를 사용하면 비용이 반으로 절감된다. 그 이유는 내부 AWS 네트워크를 사용하기 때문이다.

- 핵심은 인스턴스가 소통할 때 속도는 높이고 비용은 줄이려면 최대한 공용 IP보다는 사설 IP를 사용해야 한다는 점이다.
→ 동일한 리전 내 가용 영역 두 곳에 있는 인스턴스 두 개가 통신할 때 공용 IP는 사용하지 않는 것이 좋다.
~~~

- Use Private IP instead of Public IP for good savings and better network performance  
→ 공용 `IP`보다는 사설 `IP`를 사용해야 한다. 비용이 절약될 뿐만 아니라 네트워크 성능도 더 좋다.

- Use same AZ for maximum savings (at the cost of high availability)  
→ 최대한 비용을 절감하려면 같은 가용 영역을 사용해야한다. 하지만 비용이 절감되는 만큼 가용성이 떨어지기는 한다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/b672528d-e964-4147-861a-e4c2f955323f)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 보통 어떻게 하면 `RDS`의 읽기 전용 복제본을 가장 저렴하게 생성할까요? 같은 식으로 물어본다.  
→ 읽기 전용 복제본을 같은 `AZ`에 생성한다면 네트워크 비용 측면에서 볼 때 한 데이터베이스에서 다른 데이터베이스로 복제하는 데는 비용이 들지 않는다. 하지만 읽기 전용 복제본을 다른 `AZ`에 생성한다면 `GB`당 1센트를 지불해야 한다. 그러므로 가장 저렴하게 생성하는 방법은 같은 `AZ` 내에 읽기 전용 복제본을 생성하는 것이다.

### 2. Minimizing egress traffic network cost
- 아키텍처를 영리하게 구성해서 네트워킹 비용을 최적화하는 방법이다.

- Egress traffic: outbound traffic (from AWS to outside)  
→ `Egress traffic, 송신 트래픽`은 아웃바운드 트래픽이다. `AWS`에서 밖으로 나가는 트래픽이다.

- Ingress traffic: inbound traffic - from outside to AWS (typically free)  
→ `Ingress traffic, 수신 트래픽`은 인바운드 트래픽이고요 외부에서 `AWS`로 들어오는 트래픽으로 보통 무료이다.

- Try to keep as much internet traffic within AWS to minimize costs  
→ `AWS`로 데이터를 보내는 것은 대부분 무료이지만 밖으로 데이터를 내보내는 것은 비용이 든다. 그러므로 인터넷 트래픽을 최대한 `AWS` 내부에 두어 비용을 최소화해야 한다.

- `AWS` 내의 데이터를 데이터센터에 불러와야 한다면 가능한 한 많은 것을 `AWS` 서버 내에서 하는 것이 좋다. 송신 비용을 최소화해야 한다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/babd2934-0ce2-44d0-8037-6cf84eb623eb)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. S3 Data Transfer Pricing – Analysis for USA
- S3 ingress: free  
→ `S3` 버킷으로 들어가는 데이터는 수신 트래픽이므로 전부 무료이다.

- S3 to Internet: $0.09 per GB  
→ `Amazon S3`에서 인터넷을 통해 데이터를 내려받는다면 송신 트래픽 `GB`마다 9센트를 지불하게 된다.

- S3 Transfer Acceleration:
~~~
- Faster transfer times (50 to 500% better)
→ S3 전송 가속화를 사용한다면 전송 속도가 50-500% 정도 빨라진다.

- Additional cost on top of Data Transfer Pricing: +$0.04 to $0.08 per GB
→ 대신 데이터 전송에 GB당 4-8센트의 추가 비용이 발생한다.
~~~

- S3 to CloudFront: $0.00 per GB  
→ `S3`에서 `CloudFront`로 이동시키며 발생하는 트래픽은 비용이 청구되지 않는다.

- CloudFront to Internet: $0.085 per GB (slightly cheaper than S3)
~~~
- CloudFront에서 인터넷으로 전송할 때는 GB당 8.5센트가 청구된다. Amazon S3보다는 약간 저렴하다.

- Caching capability (lower latency)
→ 그 외에도 캐싱 기능을 얻게 되기 때문에 데이터 액세스 지연 시간이 짧아지며 요청을 보낼 때마다 비용이 절감된다.

- Reduce costs associated with S3 Requests Pricing (7x cheaper with CloudFront)
→ S3 버킷에 요청이 들어오면 비용을 부담해야 하는데 Amazon CloudFront로 들어오면 일곱 배나 저렴하다.
~~~

- S3 Cross Region Replication: $0.02 per GB  
→ `Amazon S3` 버킷의 리전 간 복제를 수행한다면 `GB`당 2센트가 청구된다.

### 4. Pricing: NAT Gateway vs Gateway VPC Endpoint
- `S3` 버킷에 프라이빗 서브넷 내의 `EC2` 인스턴스 2개를 연결하려고 할 때, `NAT Gateway`와 `Internet Gateway`를 사용할 수 있다.
~~~
- 이때 NAT Gateway 요금은 시간당 0.045달러에 더해 GB당 0.045달러가 청구된다. 
- 때문에 NAT Gateway에서 처리되는 시간, GB마다의 비용과 함께 S3로 리전 간 데이터 송신 비용이 청구된다.
~~~

- `VPC` 엔드 포인트를 사용하면 `Amazon S3` 버킷에 비공개로 데이터를 액세스할 수 있으므로 `GB`마다 1센트만 지불하면 된다.

- `VPC` 엔드 포인트를 설정하는 편이 훨씬 저렴하다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/4350b001-4d4b-43d3-a313-6dc0981821ed)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---