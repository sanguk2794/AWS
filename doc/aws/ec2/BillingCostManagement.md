## Billing & Cost Management
IAM 관리자로 설정했다고 하더라도, `Billing & Cost Management Dashboard`에 접근하려면 권한의 변경이 필요하다.
그러므로, 루트 계정으로 접속하도록 하자.

- `IAM Users and Role Acceess to Billing Information` 항목으로 이동해 편집 버튼을 누르고 
`Active IAM Access`를 활성화하면 관리자인 `IAM` 사용자가 청구 데이터에 접근할 수 있게 되며,
이 곳에서 월별 요금을 확인할 수 있다. 

### 1. 요금 확인
- `Bills` → `Try the new Bills page experience` → `Charges by service`
→ 여기에서 어떤 서비스가 어떤 리전에서 얼마의 요금을 유발했는지 확인할 수 있다.

- `Free Tier` 
→ 무료 등급을 넘어갔을 때 추가되는 항목, 여기로 이동하면 모든 서비스의 무료 등급 사용량을 확인할 수 있으며, 현재 사용량도 확인할 수 있다.
그래서 무료 등급이 `100%`에 도달한 경우, 곧 `100%`에 도달할 경우 등을 즉각적으로 알 수 있다.

### 2. Budget
- `Budget` 
→ 예산을 생성해서 비용을 추적하고 한도에 도달하기 직전에 알람을 받을 수 있다.

- `Templates - new` → `Zero spend budget`
→ `AWS` 무료 등급 한도를 초과하면 알려주는 예산을 생성할 때 사용한다. 
이렇게 설정해 두면 비용이 유발되자마자 이메일로 알람을 받을 수 있다.

- `Templates - new` → `Monthly cost budget`
→ 클라우드 예산 금액을 설정하면 실제 지출액의 `85%` 또는 `100%`에 도달하거나 예상되는 지출액이 `100%`에 도달할 것으로 예상될 때 알림을 받을 수 있다.

- 이를 통해 미래의 비용 또한 추적할 수 있다. 