## Amazon Forecast
### 1. Amazon Forecast
- 단순히 말하면 예측을 도와주는 기능이다.

- Fully managed service that uses ML to deliver highly accurate forecasts  
→ 완전 관리형 서비스이며 머신 러닝을 사용하여 매우 정확한 예측을 제공한다.

- Example: predict the future sales of a raincoat  
→ 예를 들면 미래의 우비 판매를 예측하는 데 사용하는 것이다.

- 50% more accurate than looking at the data itself  
→ 데이터 자체를 확인하는 것보다 50% 더 정확하다.

- Reduce forecasting time from months to hours  
→ 관리형 서비스는 예측 시간을 몇 달에서 몇 시간으로 줄여준다.

- Use cases: Product Demand Planning, Financial Planning, Resource Planning, …  
→ 예측이 필요한 것은 모두 사용 사례가 된다. 제품 수요 계획과 재무 계획, 자원 계획 등이 이에 포함된다.

- 우비 판매량 예측 시나리오
~~~
- 과거 시계열 데이터에 제품 특징, 가격, 할인, 웹사이트 트래픽, 상점 위치 등 데이터를 추가한다.
- 이 데이터를 `S3`에 업로드하고, 데이터를 기반으로 `Amazon Forecast`를 실행한다. 그러면 예측 모델이 생성되고 이 모델을 사용하여 미래의 우비 판매량을 예측한다.
~~~

![image](https://user-images.githubusercontent.com/97398071/236100946-45572362-b8b7-4497-88e6-fe5bfdded13b.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---