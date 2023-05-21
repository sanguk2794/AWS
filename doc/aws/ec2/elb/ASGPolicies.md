## Auto Scaling Groups Policies
### 1. Auto Scaling Groups Policies
- `ASG`의 스케일링 정책에는 동적 스케일링 정책과 예측 스케일링 정책의 두 가지 정책이 있다. 

### 2. Dynamic Scaling Policies
- 동적 스케일링 정책에는 대상 추적 스케일링, 단순과 단계 스케일링, 예약된 작업 스케일링의 세 가지 유형이 있다.

- Target Tracking Scaling  
→ 대상 추적 스케일링 정책은 가장 단순하고 설정하기도 쉽다. 예를 들어 모든 `ASG`의 대상 그룹 내 인스턴스들의 평균 `CPU` 사용률을 추적하여 이 수치가 40%대에 머무르게 설정할 수 있다.
~~~
- 대상 추적 스케일링 정책은 휴지 기간이 만료될 때까지 기다리지 않고 즉시 조정 활동을 트리거할 수 있다.

- Most simple and easy to set-up
- Example: I want the average ASG CPU to stay at around 40%
~~~

- Step Scaling
~~~
- 단계 스케일링 정책은 경고를 설정할 때 한 번에 추가할 유닛의 수와 한 번에 제거할 유닛의 수를 단계별로 설정할 수 있다.
- 단계 조정 정책은 휴지 기간이 만료될 때까지 기다리지 않고 즉시 조정 활동을 트리거할 수 있다.

- When a CloudWatch alarm is triggered (example CPU > 70%), then add 2 units
- When a CloudWatch alarm is triggered (example CPU < 30%), then remove 1
→ CloudWatch 경보를 설정하고 전체 ASG에 대한 CPU 사용률이 70%를 초과하는 경우 두 유닛 추가하고 CPU 사용량이 30% 이하로 떨어지는 경우 유닛 하나를 제거하는 등의 설정을 추가해야 한다.
~~~

- Simple Scaling
~~~
- 단순과 단계 스케일링은 경고를 설정할 때 한 번에 추가할 유닛의 수와 한 번에 제거할 유닛의 수를 단계별로 설정할 수 있다.
- 단순 조정 정책을 통해 조정 활동을 수행하기 위해 휴지 기간이 만료될 때까지 기다려야 한다.

- When a CloudWatch alarm is triggered (example CPU > 70%), then add 2 units
- When a CloudWatch alarm is triggered (example CPU < 30%), then remove 1
→ CloudWatch 경보를 설정하고 전체 ASG에 대한 CPU 사용률이 70%를 초과하는 경우 두 유닛 추가하고 CPU 사용량이 30% 이하로 떨어지는 경우 유닛 하나를 제거하는 등의 설정을 추가해야 한다.

~~~

- Scheduled Actions Scaling
~~~
- 예약된 작업은 예정된 사용 패턴을 바탕으로 스케일링을 수행할 때 사용한다. 

- Anticipate a scaling based on known usage patterns
- Example: increase the min capacity to 10 at 5 pm on Fridays
→ 금요일마다 큰 이벤트가 있다면 트래픽 증가에 대비해 금요일마다 자동으로 스케일 아웃을 수행한다.
~~~

#### 2. Predictive Scaling
- 예측 스케일링을 통해 `AWS` 내 오토 스케일링 서비스를 활용하여 지속적으로 예측을 생성할 수 있다. 로드를 보고서 다음 스케일링을 예측하는 것이다. 과거 로드를 분석해 예측이 생성되고 해당 예측을 바탕으로 사전에 스케일링 작업이 예약된다.

![image](https://user-images.githubusercontent.com/97398071/233829911-a7a3c3d9-a028-4600-a28e-0e9d0e5fc7b7.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 3. Good metrics to scale on
- 따라서 `Predictive Scaling`을 위해서는 스케일링의 기반이 될 훌륭한 지표가 필요하다. 물론 이는 애플리케이션의 목적과 작동 방식에 달라진다. 하지만, 대표적인 것들은 역시 존재하며 다음과 같다.
~~~
- CPUUtilization: Average CPU utilization across your instances
→ 일반적으로 인스턴스에 요청이 갈 때마다 일종의 연산이 수행되어야 한다. CPU 사용률이 높을수록 인스턴스가 잘 사용되고 있다는 의미이다.

- RequestCountPerTarget: to make sure the number of requests per EC2 instances is stable
→ EC2 인스턴스는 한 번에 대상별로 1000개의 요청까지만 최적으로 작동한다. 따라서 요청의 수 또한 스케일링의 지표로 사용할 수 있다.

- Average Network In / Out (if you’re application is network bound)
→ 업로드와 다운로드가 많아 EC2 인스턴스에 대해 해당 네트워크에서 병목 현상이 발생할 것으로 판단된다면 평균 네트워크 입출력을 기반으로 스케일링을 수행할 수 있다.

- Any custom metric (that you push using CloudWatch)
→ 물론 다른 지표를 기준으로 사용하는 것도 가능하다.
~~~

- `EC2` → `Auto Scaling` → `Auto scaling group` → 오토 스케일링 그룹 선택 → `Automatic scaling`  
→ 스케일링 정책을 확인할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/233830457-01eb44de-0b88-41e0-8cc7-9193e9cd918d.png)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---