## Auto Scaling Group
### 1. What’s an Auto Scaling Group?
- In real-life, the load on your websites and application can change  
→ 웹사이트나 애플리케이션을 배포한 후 웹사이트 방문자 수의 증감에 따라 로드가 달라질 수 있다.

- In the cloud, you can create and get rid of servers very quickly  
→ 클라우드에서는 `EC2` 인스턴스 생성 `API` 호출을 통해 서버를 빠르게 생성하고 종료할 수 있다. 이를 자동화하고 싶을 때 생성하는 것이 바로 `ASG`이다.

- The goal of an Auto Scaling Group (ASG) is to:
~~~
- Scale out (add EC2 instances) to match an increased load
→ 증가한 로드에 맞춰 인스턴스의 개수를 추가하는 스케일 아웃을 수행한다.

- Scale in (remove EC2 instances) to match a decreased load
→ 감소한 로드에 맞춰 인스턴스를 감소시키는 스케일 인을 수행한다.

- Ensure we have a minimum and a maximum number of EC2 instances running
→ 인스턴스의 최대 및 최소 개수를 보장하기 위해 사용하는 매개변수를 설정할 수 있다.

- Automatically register new instances to a load balancer
→ 로드 밸런서와 페어링하는 경우 ASG에 속한 모든 인스턴스가 로드 밸런서에 연결된다.

- Re-create an EC2 instance in case a previous one is terminated (ex: if unhealthy)
→ 한 인스턴스가 비정상이면 이 인스턴스를 종료하고 새로운 인스턴스를 생성한다.
~~~

- ASG are free (you only pay for the underlying EC2 instances)  
→ `ASG`는 무료이며, `EC2` 인스턴스와 같은 생성된 하위 리소스에 대한 비용만 지불하면 된다.

- 아래 예제의 경우의 인스턴스 최소 용량은 2, 희망 용량은 4, 최대 용량은 7이다.

![image](https://user-images.githubusercontent.com/97398071/233827202-837056f7-8d29-45df-aee2-9fcfa43cbe4a.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Auto Scaling Group in AWS With Load Balancer  
→ `ASG`는 로드 밸런서와 함께 작동한다. `ELB`는 생성된 인스턴스에 트래픽을 즉시 분산하여 사용자가 로드 밸런싱된 웹사이트에 액세스가 가능하도록 한다.
여기에 더해, `ELB`는 상태 확인을 통해 인스턴스의 상태를 확인하고 `ASG`로 전달할 수 있다. 따라서, 로드 밸런서가 비정상이라고 판단하는 인스턴스를 `ASG`가 종료할 수 있으 매우 편리하다.

![image](https://user-images.githubusercontent.com/97398071/233827270-61a8b40f-8d24-4bab-a78b-7248b04a1be1.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. Auto Scaling Group Attributes
- A Launch Template (older “Launch Configurations” are deprecated)  
→ 인스턴스 속성을 기반으로 `ASG`를 생성하기 위해서는 시작 템플릿을 생성해야 한다. 이 시작 템플릿에는 `EC2` 인스턴스를 시작하는 방법에 대한 정보들이 포함되어 있다. 이 정보들은 `EC2` 인스턴스를 생성할 때 지정한 매개변수들과 매우 유사하다.
~~~
- AMI + Instance Type
- EC2 User Data
- EBS Volumes
- Security Groups
- SSH Key Pair
- IAM Roles for your EC2 Instances
- Network + Subnets Information
- Load Balancer Information
~~~

- Min Size / Max Size / Initial Capacity  
→ `ASG`의 최소, 최대, 초기 용량을 정의해야 한다.

- Scaling Policies  
→ 스케일링 정책을 정의해야 한다.

### 3. Auto Scaling - CloudWatch Alarms & Scaling
- It is possible to scale an ASG based on CloudWatch alarms  
→ `CloudWatch` 경보를 통한 스케일 인 및 스케일 아웃이 가능하다. 

- An alarm monitors a metric (such as Average CPU, or a custom metric)  
→ 경보를 위한 지표로써 평균 `CPU` 등 원하는 사용자 지정 지표를 지정할 수 있다. 이 경보가 울리는 것이 트리거가 되어 `ASG`의 스케일링 활동을 유발하는 것이다. 경보에 의해 내부적으로 자동적인 스케일링이 이루어지기 때문에 `ASG`라는 이름이 붙었다.

- Based on the alarm:
~~~
- We can create scale-out policies (increase the number of instances)
- We can create scale-in policies (decrease the number of instances)
~~~

![image](https://user-images.githubusercontent.com/97398071/233827977-4257fa58-a052-4bc3-975f-a315888df5fc.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 4. Create ASG
- 실행중인 인스턴스가 없도록 전원 `Terminate`를 수행한다.

- `EC2` → `Auto Scaling` → `Auto Scaling Groups` → `Create Auto scaling group`

- `Launch Template` → `Create Launch Template`  
→ 기동 템플릿 이름, 설명, `AMI`, 인스턴스 타입, `Key pair`, `Security groups`, `EBS`, `User Data` 등을 설정한 후 `Create Launch Template` 버튼을 누르면 된다.

- 생성한 기동 템플릿을 선택한 후 아래의 설정들을 추가 선택한다.
~~~
- VPC, subnet, Instance Type 
→ Instance Type에는 Spot, reservation, on-demand 등이 포함된다.

- Load balancing
→ 로드 밸런싱은 기본적으로 선택이지만 ASG에 로드 밸런서를 설정하지 않는 경우는 거의 없다.

- Health check
→ ASG에서 인스턴스를 자동으로 제거할 수 있다.

- Group size
→ 최소, 최대, 희망 용량를 선택할 수 있다.

- Scaling policy
~~~

- `Create Auto scaling group`

![image](https://user-images.githubusercontent.com/97398071/233828975-a52eecf7-948a-4c4e-ac98-93fcb95fa462.png)

- `ASG`가 `EC2` 인스턴스를 생성하기를 결정하면 `Activity notification`에 로그가 남기에 나중에 확인이 가능하다.

![image](https://user-images.githubusercontent.com/97398071/233829110-c2059709-42d8-4ea0-ae95-e85f574c35dc.png)

### 5. Auto Scaling Groups Policies
- `ASG`의 스케일링 정책에는 두 가지 정책이 있다. `Dynamic Scaling Policies`와 `Predictive Scaling Policies`이 그것이다.

#### 1. Dynamic Scaling Policies
- 동적 스케일링 정책에는 대상 추적 스케일링, 단순과 단계 스케일링, 예약된 작업 스케일링의 세 가지 유형이 있다.

- Target Tracking Scaling  
→ 대상 추적 스케일링 정책은 가장 단순하고 설정하기도 쉽다. 예를 들어 모든 `ASG`의 대상 그룹 내 인스턴스들의 평균 `CPU` 사용률을 추적하여 이 수치가 40%대에 머무르게 설정할 수 있다.
~~~
- Most simple and easy to set-up
- Example: I want the average ASG CPU to stay at around 40%

- 대상 추적 스케일링 정책은 휴지 기간이 만료될 때까지 기다리지 않고 즉시 조정 활동을 트리거할 수 있다.
~~~

- Step Scaling
~~~
- 단계 스케일링 정책은 경고를 설정할 때 한 번에 추가할 유닛의 수와 한 번에 제거할 유닛의 수를 단계별로 설정할 수 있다.

- When a CloudWatch alarm is triggered (example CPU > 70%), then add 2 units
- When a CloudWatch alarm is triggered (example CPU < 30%), then remove 1
→ CloudWatch 경보를 설정하고 전체 ASG에 대한 CPU 사용률이 70%를 초과하는 경우 두 유닛 추가하고 CPU 사용량이 30% 이하로 떨어지는 경우 유닛 하나를 제거하는 등의 설정을 추가해야 한다.

- 단계 조정 정책은 휴지 기간이 만료될 때까지 기다리지 않고 즉시 조정 활동을 트리거할 수 있다.
~~~

- Simple Scaling  
~~~
- 단순과 단계 스케일링은 경고를 설정할 때 한 번에 추가할 유닛의 수와 한 번에 제거할 유닛의 수를 단계별로 설정할 수 있다.

- When a CloudWatch alarm is triggered (example CPU > 70%), then add 2 units
- When a CloudWatch alarm is triggered (example CPU < 30%), then remove 1
→ CloudWatch 경보를 설정하고 전체 ASG에 대한 CPU 사용률이 70%를 초과하는 경우 두 유닛 추가하고 CPU 사용량이 30% 이하로 떨어지는 경우 유닛 하나를 제거하는 등의 설정을 추가해야 한다.

- 단순 조정 정책을 통해 조정 활동을 수행하기 위해 휴지 기간이 만료될 때까지 기다려야 한다.
~~~

- Scheduled Actions Scaling  
~~~
- 예약된 작업은 예정된 사용 패턴을 바탕으로 스케일링을 수행할 때 사용한다. 
- 예를 들어 금요일마다 큰 이벤트가 있다면 트래픽 증가에 대비해 금요일마다 자동으로 스케일 아웃을 수행한다.

- Anticipate a scaling based on known usage patterns
- Example: increase the min capacity to 10 at 5 pm on Fridays
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

### 6. Scaling Cool-downs
- After a scaling activity happens, you are in the cooldown period (default 300 seconds)  
→ 스케일링 정책 중 또 다른 중요한 내용에는 스케일링 휴지가 있다. 스케일링 작업이 끝날때마다 인스턴스의 추가 또는 삭제를 막론하고 기본적으로 300초의 휴지 기간을 갖는 것이다.

- During the cooldown period, the ASG will not launch or terminate additional instances (to allow for metrics to stabilize)  
→ 휴지 기간에는 `ASG`가 추가 인스턴스를 실행 또는 종료할 수 없다. 이는 지표를 이용하여 새로운 인스턴스가 안정화될 수 있도록 하며 새로운 지표의 양상을 살펴보기 위함이다.

- Advice: Use a ready-to-use AMI to reduce configuration time in order to be serving request fasters and reduce the cooldown period  
→ 즉시 사용 가능한 `AMI`를 이용하여 인스턴스 구성 시간을 단축하고 이를 통해 요청을 좀 더 신속히 처리하는 것이 좋다. `EC2` 인스턴스의 구성에 할애되는 시간이 적으면 활성화 시간이 빨라지므로 휴지 기간 또한 단축할 수 있다.

### 7. Stress Check in Amazon Linux 2
- 해당 커맨드를 `EC2 Instance Connect` 또는 `SSH`를 통해 실행한다. 그러면 `CPU` 사용률이 증가하므로 `ASG`의 테스트가 가능하다.
~~~ shell
$ sudo amazon-linux-extras install epel -y
$ sudo yum install stress -y

$ stress -c 4
~~~

### 7. 인스턴스의 기본 종료 정책
- 기본 종료 순서는 다음과 같다.
~~~
- 여러 가용 영역에 인스턴스가 있는 경우 인스턴스가 가장 많은 가용 영역과 축소로부터 보호되지 않는 인스턴스가 하나 이상 있는 가용 영역을 선택합니다. 이 인스턴스 수가 있는 가용 영역이 둘 이상인 경우 가용 영역을 선택한다. 
- 선택한 가용 영역에서 가장 오래된 시작 구성을 사용하는 비보호 인스턴스를 선택한다. 그러한 인스턴스가 하나 있으면 종료한다.
- 위 기준에 따라 종료할 인스턴스가 여러 개인 경우 다음 청구 시간에 가장 가까운 비보호 인스턴스를 결정한다.
- 다음 청구 시간에 가장 가까운 비보호 인스턴스가 둘 이상인 경우 이러한 인스턴스 중 하나를 무작위로 선택한다.
~~~

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
