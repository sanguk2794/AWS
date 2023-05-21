## Connection Draining
### 1. Connection Draining
- Feature naming  
→ `CLB`를 사용할 경우에는 연결 드레이닝이라고 부르며, `ALB`나 `NLB`를 사용하는 경우에는 등록 취소 지연이라고 부른다.
~~~
- Connection Draining – for CLB
- Deregistration Delay – for ALB & NLB
~~~

![image](https://user-images.githubusercontent.com/97398071/233824213-a024abe5-0b10-4f74-a965-f6671945c7eb.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Time to complete “in-flight requests” while the instance is de-registering or unhealthy  
→ 인스턴스가 등록 취소 혹은 비정상적인 상태에 있을 때 인스턴스에 어느 정도의 시간을 주어 활성 요청을 완료할 수 있도록 하는 기능이다.

- Stops sending new requests to the EC2 instance which is de-registering  
→ 연결이 드레이닝되면 `ELB`는 등록 취소 중인 `EC2` 인스턴스로 새로운 요청을 보내지 않는다.

- Between 1 to 3600 seconds (default: 300 seconds) and Can be disabled (set value to 0)  
→ 연결 드레이닝의 파라미터는 매개변수로 표시할 수 있으며 기본 값은 300, 즉 5분이다. 이 값을 0으로 하면 비활성화된다.

- Set to a low value if your requests are short  
→ 짧은 요청의 경우에는 낮은 값으로 설정하는 것이 좋다. 그래야 `EC2` 인스턴스가 빠르게 드레이닝되고 그 후에 오프라인 상태가 되어 교체 등의 작업을 할 수 있기 때문이다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---