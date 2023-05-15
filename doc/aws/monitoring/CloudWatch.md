## Amazon CloudWatch
### 1. Amazon CloudWatch Metrics
- CloudWatch provides metrics for every services in AWS  
→ `CloudWatch`는 `AWS`의 모든 서비스에 대한 지표를 제공한다. 따라서 계정에서 일어나는 모든 일을 모니터링할 수 있다.

- Metric is a variable to monitor (CPUUtilization, NetworkIn…)  
→ 지표는 모니터링할 변수이다. `CPU` 사용률, 네트워크 사용량, `S3` 버킷 사이즈 등이 포함된다.

- Metrics belong to namespaces  
→ 지표는 각각 다른 네임스페이스에 저장되며 서비스당 이름 공간은 하나이다.

- Dimension is an attribute of a metric (instance id, environment, etc…)  
→ 지표의 속성으로 측정 기준이 있는데 `CPU` 사용률에 대한 지표는 특정 인스턴스나 특정 환경 등과 관련이 있을 것이다.

- Up to 10 dimensions per metric  
→ 지표 당 최대 측정 기준은 10개이다.

- Metrics have timestamps  
→ 지표는 시간을 기반으로 하므로 타임스탬프가 꼭 있어야 한다.

- Can create CloudWatch dashboards of metrics  
→ 지표를 통해 `CloudWatch` 대시보드를 생성할 수 있다.

- Can create CloudWatch Custom Metrics (for the RAM for example)  
→ `CloudWatch`에 커스텀 지표를 만들 수 있다. 예를 들면 `EC2` 인스턴스로부터 메모리 사용량을 추출할 수 있다. 이는 굉장히 전형적인 사례이다.

### 2. CloudWatch Metric Streams
- Continually stream CloudWatch metrics to a destination of your choice, with near-real-time delivery and low latency.  
→ `CloudWatch` 지표는 `CloudWatch` 외부로 스트리밍할 수 있다. `CloudWatch` 지표를 원하는 대상으로 지속적으로 스트리밍하면 거의 실시간으로 전송되고 지연 시간도 짧아진다.

- Amazon Kinesis Data Firehose (and then its destinations)  
→ `CloudWatch` 지표를 `Amazon Kinesis Data Firehose`로 거의 실시간으로 스트리밍할 수 있다.

- `Amazon Kinesis Data Firehose`에서 원하는 곳으로 지표 데이터를 전송할 수 있다.
~~~
- Amazon Kinesis Data Firehose에서 S3 버킷으로 지표를 보내 Athena를 사용하면 지표를 분석할 수 있다.
- Amazon Kinesis Data Firehose에서 Redshift를 사용해 지표로 데이터 웨어하우스를 만들 수 있다.
- Amazon Kinesis Data Firehose에서 Amazon OpenSearch를 사용해 대시보드를 생성할 수 있다.
~~~

- 3rd party service provider: Datadog, Dynatrace, New Relic, Splunk, Sumo Logic…  
→ 타사 서비스에도 `CloudWatch` 지표를 전송할 수 있다.

- Option to filter metrics to only stream a subset of them  
→ 모든 이름공간에 속한 지표를 스트림할 수 있으며 몇몇 이름공간의 지표로 필터링도 가능하다.

![image](https://user-images.githubusercontent.com/97398071/236108668-c9071109-adc5-48d4-9239-7d3b2e6871b5.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. CloudWatch Logs
- Log groups: arbitrary name, usually representing an application  
→ 각 로그 그룹 내에는 로그 스트림이 있다.

- Log stream: instances within application / log files / containers  
→ 로그 스트림은 애플리케이션 내 인스턴스나 다양한 로그 파일명 또는 컨테이너 등을 나타낸다.

- Can define log expiration policies (never expire, 30 days, etc..)  
→ 로그 만료일을 정할 수 있다. 로그가 영원히 만료되지 않도록 설정도 가능하다.

- CloudWatch Logs can send logs to:
~~~
- 로그를 아래 서비스들로 내보낼 수 있다.
- Amazon S3 (exports)
- Kinesis Data Streams
- Kinesis Data Firehose
- AWS Lambda
- ElasticSearch
~~~

#### 1. CloudWatch Logs - Sources
- SDK, CloudWatch Logs Agent, CloudWatch Unified Agent  
→ `SDK`, `CloudWatch Logs Agent`, `CloudWatch Unified Agent`의 로그를 전송할 수 있다.

- Elastic Beanstalk: collection of logs from application  
→ `Elastic Beanstalk`를 통해 실행되는 애플리케이션의 로그를 전송할 수 있다.

- ECS: collection from containers  
→ `ECS` 컨테이너의 로그를 전송할 수 있다.

- AWS Lambda: collection from function logs  
→ 람다 함수의 로그를 전송할 수 있다.

- VPC Flow Logs: VPC specific logs  
→ `VPC` 메타데이터 또는 네트워크 트래픽 로드를 전송할 수 있다.

- API Gateway  
→ 받은 모든 요청에 대한 로그를 전송할 수 있다.

- CloudTrail based on filter  
→ 필터링한 로그를 전송할 수 있다.

- Route53: Log DNS queries  
→ 모든 `DNS` 쿼리를 로그로 전송할 수 있다.

#### 2. CloudWatch Logs Metric Filter & Insights
- CloudWatch Logs can use filter expressions
~~~
- CloudWatch Logs에서 필터 표현식을 사용할 수 있다.

- For example, find a specific IP inside of a log
→ 로그 내 특정 IP를 찾을 수 있다.

- Or count occurrences of “ERROR” in your logs
→ ERROR 문구를 포함하는 로그를 찾을 수 있다.
~~~

- Metric filters can be used to trigger CloudWatch alarms  
→ 지표 필터를 통해 출현 빈도를 계산하는 지표를 생성한 후 이 지표를 기반으로 `CloudWatch alarms`를 생성할 수 있다.

- CloudWatch Logs Insights can be used to query logs and add queries to CloudWatch Dashboards  
→ `CloudWatch Logs Insights` 기능을 통해 로그를 쿼리하고 이 쿼리를 대시보드에 바로 추가할 수 있다.

### 4. CloudWatch Logs – S3 Export
- Log data can take up to 12 hours to become available for export  
→ `CloudWatch`에서 `S3`로 로그 데이터를 내보내는 데 최대 12시간이 소요될 수 있다.

- The API call is CreateExportTask  
→ `API` 호출은 `CreateExportTask`이다.

- Not near-real time or real-time… use Logs Subscriptions instead  
→ 실시간이 아니다. 

### 5. CloudWatch Logs Subscriptions
- `구독 필터, Subscription Filter`는 `CloudWatch Logs`에 적용하여 로그 데이터를 원하는 목적지로 보낼 수 있다.

- 목적지에는 람다 함수, `Kinesis Data Streams`, `Kinesis Data Firehose` 등이 포함된다.

![image](https://user-images.githubusercontent.com/97398071/236108936-3e7fb3e6-0fe1-47ee-b1c2-0572f225106c.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 6. CloudWatch Logs Aggregation Multi-Account & Multi Region
- `CloudWatch Logs`로 여러 계정과 리전의 로그를 집계할 수 있다. 이 때에도 `Subscription Filter`를 사용한다.

![image](https://user-images.githubusercontent.com/97398071/236109001-680a7c8e-480b-46a9-b104-106e576a9424.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 7. CloudWatch Logs for EC2
- `CloudWatch`에서 `EC2` 인스턴스의 로그와 지표를 받아 표시할 수 있다.

- By default, no logs from your EC2 machine will go to CloudWatch  
→ 기본적으로, `EC2` 인스턴스에서 `CloudWatch`로 어떤 로그도 옮겨지지 않는다.

- You need to run a CloudWatch agent on EC2 to push the log files you want  
→ 그렇기에 `EC2` 인스턴스에 `CloudWatch Logs agent`라는 작은 프로그램을 실행시켜 원하는 로그 파일을 푸시해야 한다.

- Make sure IAM permissions are correct  
→ 로그를 보낼 수 있게 `IAM` 역할이 설정되어 있어야 한다.

- The CloudWatch log agent can be setup on-premises too  
→ `CloudWatch Logs agent`는 온프레미스 환경에서도 사용할 수 있다. 즉, `EC2` 인스턴스 환경이 아니더라도 로그를 푸시할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/236109121-ee3afb7f-20b4-47b1-8eb1-683eb479d721.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 8. CloudWatch Logs Agent & Unified Agent
- For virtual servers (EC2 instances, on-premises servers…)  
→ `EC2` 인스턴스나 온프레미스 서버와 같은 가상 서버를 위한 프로그램이다.

- `CloudWatch Logs agent`에는 `CloudWatch Logs Agent`와 `CloudWatch Unified Agent`의 두 종류가 존재한다.
- CloudWatch Logs Agent
~~~
- Old version of the agent
→ 더 오래된 버전이다.

- Can only send to CloudWatch Logs
→ 오직 로그만 전송한다.
~~~

- CloudWatch Unified Agent
~~~
- Collect additional system-level metrics such as RAM, processes, etc…
→ 프로세스나 RAM과 같은 추가적인 시스템 단계 지표를 수집한다.

- Collect logs to send to CloudWatch Logs
→ 로그를 전송한다. 지표와 로그를 둘 다 사용하기 때문에 통합 에이전트인 것이다.

- Centralized configuration using SSM Parameter Store
→ SSM Parameter Store를 이용해서 에이전트를 쉽게 구성할 수 있다. 모든 통합 에이전트를 대상으로 중앙 집중식 환경 구성을 할 수 있다.
~~~

#### 1. CloudWatch Unified Agent – Metrics
- Collected directly on your Linux server / EC2 instance  
~~~
- EC2 인스턴스 또는 온프레미스 리눅스 서버에 설치하면 로그와 더불어 지표를 수집할 수 있다. 
- CPU (active, guest, idle, system, user, steal)
- Disk metrics (free, used, total), Disk IO (writes, reads, bytes, iops)
- RAM (free, inactive, used, total, cached)
- Netstat (number of TCP and UDP connections, net packets, bytes)
- Processes (total, dead, bloqued, idle, running, sleep)
- Swap Space (free, used, used %) → 스와핑 공간은 메모리처럼 사용된다.
~~~

- 핵심은 기본 `EC2` 인스턴스 모니터링보다 `CloudWatch Unified Agent`가 더 세부적이고 많은 지표를 수집한다는 것이다.

### 9. CloudWatch Alarms
- Alarms are used to trigger notifications for any metric  
→ 지표를 통해 알림을 트리거할 때 사용한다.

- Various options (sampling, %, max, min, etc…)  
→ `sampling`, `%`, `max`, `min` 등 다양한 옵션을 추가해 복잡한 경고를 정의할 수도 있다.

- Alarm States:
~~~
- 알림에는 세 가지 상태가 존재한다.

- OK
→ 트리거되지 않았음

- INSUFFICIENT_DATA
→ 상태를 결정할 데이터가 부족함

- ALARM
→ 임계값이 위반되어 알람이 보내짐
~~~

- Period:
~~~
- 알림이 지표를 평가하는 기간을 말한다.

- Length of time in seconds to evaluate the metric
→ 짧게 설정할 수도 있고, 길게 설정할 수도 있다.

- High resolution custom metrics: 10 sec, 30 sec or multiples of 60 sec
→ 고해상도 사용자 지정 지표에도 적용될 수 있다.
~~~

#### 1. CloudWatch Alarm Targets
- 경보의 주 대상은 세 가지이다.

- Stop, Terminate, Reboot, or Recover an EC2 Instance  
→ 첫 번째는 `EC2` 인스턴스의 동작이다. 인스턴스를 멈추거나 삭제하거나 재시작하거나 복구하는 등의 동작을 포함한다.

- Trigger Auto Scaling Action  
→ 두 번째는 오토 스케일링의 동작이다. 스케일 아웃과 스케일 인 등이 포함된다.

- Send notification to SNS (from which you can do pretty much anything)  
→ 세 번째는 `SNS` 서비스에 알람을 전송하는 것이다.

#### 2. EC2 Instance Recovery, CloudWatch Alarm Actions
- Status Check:
~~~
- Instance status = check the EC2 VM
- System status = check the underlying hardware
→ 상태 점검에는 VM 점검을 위환 상태 점검과 하드웨어 점검을 위한 시스템 상태 점검이 포함된다.
~~~

- `CloudWatch` 경보 액션을 사용하면 특정 `EC2` 인스턴스를 모니터링하다가 경보가 위반되는 경우 `EC2` 인스턴스 복구를 실행해 `EC2` 인스턴스를 다른 호스트로 옮기는 등의 작업을 수행할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/236632802-8db10359-6ea7-4dae-acf1-3d6154168c9b.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Recovery: Same Private, Public, Elastic IP, metadata, placement group  
→ 복구할 때에는 동일한 사설, 공개, 탄력적 `IP` 주소와 동일한 메타데이터, 동일한 배치 그룹을 가지게 된다. 

#### 3. CloudWatch Alarm: good to know
- Alarms can be created based on CloudWatch Logs Metrics Filters  
→ `CloudWatch Logs` 지표 필터에 기반해 경보를 생성할 수 있다.

- To test alarms and notifications, set the alarm state to Alarm using CLI  
→ 경보 알림 생성에는 `set-alarm-state`라는 `CLI` 호출을 사용한다. 특정 임계값에 도달하지 않아도 경보를 트리거하고 싶을 때 유용하다.
~~~ shell
$ aws cloudwatch set-alarm-state --alarm-name "myalarm" --state-value ALARM --state-reason "testing purposes"
~~~

- 경보 알림시 `SNS`에 메세지를 전송할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/236633005-d801269e-1da8-4053-884f-26b20fa544ec.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---