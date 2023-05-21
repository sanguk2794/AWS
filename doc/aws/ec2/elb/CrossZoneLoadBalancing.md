## Cross-Zone Load Balancing
### 1. Cross-Zone Load Balancing
- 교차 영역 로드 밸런싱을 사용하면, `AZ`별로 인스턴스 개수가 불균형하더라도 각각의 로드 밸런서 인스턴스가 모든 가용 영역에 등록된 모든 인스턴스에 부하를 고르게 분배한다.

![image](https://user-images.githubusercontent.com/97398071/233821505-84c4104b-d4f0-41d6-a640-0489372f2d39.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 만약 교차 영역 로드 밸런싱을 사용하지 않는다면 탄력적 로드 밸런서 노드의 인스턴스로 분산된다.

![image](https://user-images.githubusercontent.com/97398071/233821513-d7c8c271-5875-4261-be73-0d20ad91cb44.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 정답은 없으며, 쓰임새에 따라 선택하면 된다.

- Application Load Balance - Enabled by default (can be disabled at the Target Group level), No charges for inter AZ data  
→　`ALB`는 교차 영역 로드 밸런싱이 기본적으로 활성화되어 있으며, `target group` 설정에서 이 설정을 비활성화할 수 있다. 그리고 데이터를 다른 `AZ`로 옮기는 데 비용이 들지 않는다. 일반적으로 `AWS`에서는 데이터를 다른 `AZ`로 옮길 때 비용을 지불해야 한다.

- Network Load Balancer & Gateway Load Balancer - Disabled by default, You pay charges ($) for inter AZ data if enabled  
→ `NLB`, `GWLB`는 교차 영역 로드 밸런싱이 기본적으로 비활성화되어 있으며, 활성화하려면 비용을 지불해야 한다.

- Classic Load Balancer - Disabled by default, No charges for inter AZ data if enabled  
→ `CLB`는 교차 영역 로드 밸런싱이 기본적으로 비활성화되어 있으나, 활성화 시 비용이 들지 않는다.

- `EC2` → `Elastic Block Store` → `Load Balancer` → 대상 로드 밸런서를 클릭 → `Attributes`  
→ `Cross-zone load balancing` 활성 여부를 확인할 수 있다.

- `EC2` → `Elastic Block Store` → `Load Balancer` → 대상 로드 밸런서를 클릭 → `Attributes` → `Edit` → `Cross-zone load balancing`
→ `Cross-zone load balancing` 활성 여부를 변경할 수 있다.

- 표로 정리하자면 다음과 같다.

| ELB  | 기본 활성 | 비용 지불 |
|------|-------|-------|
| CLB  | ☓     | ☓     |
| ALB  | ○     | ☓     |
| NLB  | ☓     | ○     |
| GWLB | ☓     | ○     |

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---