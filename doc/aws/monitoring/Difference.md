## CloudWatch vs CloudTrail vs Config
### 1. CloudWatch vs CloudTrail vs Config
- 자주 출제된다.

- CloudWatch 
~~~
- Performance monitoring (metrics, CPU, network, etc…) & dashboards 
→ 지표, CPU, 네트워크 등의 성능 모니터링과 대시보드를 만드는 데 사용된다.

- Events & Alerting 
→ 이벤트 알림을 받을 수 있다.

- Log Aggregation & Analysis 
→ 로그 집계 및 분석 도구도 사용할 수 있다.
~~~

- CloudTrail 
~~~
- Record API calls made within your Account by everyone 
→ 계정 내에서 만든 API의 모든 호출을 기록한다.

- Can define trails for specific resources 
→ 특정 리소스에 대한 추적을 정의할 수 있다.

- Global Service 
→ 글로벌 서비스이다.
~~~

- Config 
~~~
- Record configuration changes 
→ 구성 변경을 기록할 수 있다.

- Evaluate resources against compliance rules 
→ 규정 준수 규칙에 따라 리소스를 평가할 수 있다.

- Get timeline of changes and compliance
→ 변경과 규정 준수에 대한 타임라인을 UI로 시각화해준다.
~~~

### 2. For an Elastic Load Balancer
- CloudWatch:
~~~
- Monitoring Incoming connections metric
→ 들어오는 연결의 수를 지표로 확인할 수 있다.

- Visualize error codes as % over time
→ 오류 코드 수를 시간에 따라 비율로 시각화할 수 있다.

- Make a dashboard to get an idea of your load balancer performance
→ 로드 밸런서의 성능을 볼 수 있는 대시보드를 생성할 수 있다.
~~~

- Config:
~~~
- Track security group rules for the Load Balancer
→ 로드 밸런서에 대한 보안 그룹 규칙을 추적해 누구도 수상하게 행동하거나 무언가 변경하지 못하게 할 수 있다.

- Track configuration changes for the Load Balancer
→ 로드 밸런서의 구성 변경을 추적할 수 있다.

- Ensure an SSL certificate is always assigned to the Load Balancer (compliance)
→ SSL 인증서가 로드 밸런서에 항상 할당되어 있어야 한다는 규칙을 만들어 암호화되지 않은 트래픽이 로드 밸런서에 접근하지 못하도록 할 수 있다.
~~~

- CloudTrail:
~~~
- Track who made any changes to the Load Balancer with API calls
→ API를 호출하여 로드 밸런서를 변경했는지 추적할 수 있다. 만약 누군가가 보안 그룹 규칙을 바꾸거나 SSL 인증서를 바꾼다면 CloudTrail을 통해 확인할 수 있다.
~~~

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---