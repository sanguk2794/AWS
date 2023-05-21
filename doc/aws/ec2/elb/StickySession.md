## Sticky Sessions
### 1. Sticky Sessions (Session Affinity)
- 고정 세션, 세션 밀접성이라고 한다.

- It is possible to implement stickiness so that the same client is always redirected to the same instance behind a load balancer  
→ 고정 세션을 실행하면 로드 밸런서에 2개의 요청을 수행하는 하나의 클라이언트의 요청에 응답하기 위해 동일한 백엔드 인스턴스를 호출한다.

![image](https://user-images.githubusercontent.com/97398071/233820808-b836ed3a-5ec1-4ba0-8f13-fff4bd1df74f.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- This works for Classic Load Balancers & Application Load Balancers  
→ `CLB`, `ALB`에서 동작한다.

- The “cookie” used for stickiness has an expiration date you control  
→ 쿠키를 요청에 포함시켜 전송한다. 쿠키 정보는 고정성과 만료 기간을 포함한다.

- Use case: make sure the user doesn’t lose his session data  
→ 사용자 로그인과 같이 중요한 정보를 취하는 세션 데이터를 잃지 않기 위해 동일한 백엔드 인스턴스에 연결시키는 것이다.

- Enabling stickiness may bring imbalance to the load over the backend EC2 instances  
→ 고정 세션을 사용하면 백엔드 `EC2` 인스턴스의 부하에 불균형을 초래할 수 있다. 일부 인스턴스가 고정 사용자를 갖게 되기 때문이다.

### 2. Sticky Sessions – Cookie Names
- 고정 세션에는 2가지 유형의 쿠키가 존재한다. `Application-based Cookie`와 `Duration-based Cookie`가 그것이다.

- Application-based Cookie  
~~~ 
- 애플리케이션 기반 쿠키는 `Custom cookie`와 `Application cookie`의 2가지로 나뉜다.

- Application cookie
→ 로드밸런서가 자동으로 생성해 준다. ALB의 쿠키 이름은 AWSALBAPP이다.

- Custom cookie
→ 커스텀 쿠키는 대상으로 생성된 사용자 정의 쿠키로 애플리케이션에서 생성되며 애플리케이션에 필요한 모든 사용자 정의 속성을 포함할 수 있다.
Application cookie, Duration-based Cookie와 동일한 이름을 가진 쿠키(AWSALB, AWSALBAPP or AWSALBTG)를 선언해서는 안 된다.
~~~

- Duration-based Cookies  
→ 로드 밸런서에서 자동 생성하는 쿠키로 `AWSALB`, `AWSELB`와 같은 이름을 가진다. 설정한 시간이 지나면 쿠키가 만료되며 그 기간이 로드 밸런서 자체에서 생성된다.
 
- `Elastic Block Store` → `Target Groups` → 대상 그룹 클릭 → `Actions` → `Edit attributes` → `Stickiness` 

![image](https://user-images.githubusercontent.com/97398071/233821152-71532200-05d4-4a6c-a94a-8734b78e5d07.png)

출처 → [AWS Sticky Session チュートリアル](https://qiita.com/Uking/items/7663b07efa588a08709d)

- `Application cookie`의 경우 애플리케이션에서 로드 밸런서로 보낼 쿠키의 기간만을 설정해주며, `Custom Cookie`의 경우 쿠키의 기간과 함께 이름을 설정해야 한다.

- 이후 애플리케이션의 요청에 `Cookie`가 추가되는 것을 확인할 수 있다. 이 쿠키의 만료 기간은 지정한 기간 이후가 된다.

---
#### ▶ Reference
- [Sticky Session](https://www.imperva.com/learn/availability/sticky-session-persistence-and-cookies/)
- [AWS Sticky Session チュートリアル](https://qiita.com/Uking/items/7663b07efa588a08709d)
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
