## Amazon MQ
### 1. Amazon MQ
- SQS, SNS are “cloud-native” services: proprietary protocols from AWS  
→ `SQS`, `SNS`는 `AWS` 독점 기술이다. 

- Traditional applications running from on-premises may use open protocols such as: MQTT, AMQP, STOMP, Openwire, WSS  
→ 온프레미스에서 기존 애플리케이션을 실행하는 경우 개방형 프로토콜인 `MQTT`, `AMQP`, `STOMP`, `Openwire`, `WSS` 등을 사용하면 된다.

- When migrating to the cloud, instead of re-engineering the application to use SQS and SNS, we can use Amazon MQ  
→ 이 애플리케이션을 클라우드에 마이그레이션할 때 `SQS`, `SNS` 프로토콜 혹은 `API`를 사용하기 위해 애플리케이션을 다시 구축하고 싶지 않고 기존 프로토콜을 사용하고 싶을 수 있다. 이럴 때 사용하는 것이 `Amazon MQ`이다.

- Amazon MQ is a managed message broker service for  
→ `RabbitMQ`와 `ActiveMQ` 두 가지 기술을 위한 관리형 메세지 브로커 서비스이다. 

- Amazon MQ doesn’t “scale” as much as SQS / SNS  
→ 확장성이 크지는 않다.

- Amazon MQ runs on servers, can run in Multi-AZ with failover  
→ `Amazon MQ`는 서버에서 실행되므로 서버 문제가 생길 수 있다. 고가용성을 원한다면 장애 조치와 함께 다중 `AZ` 설정이 필요하다.

- Amazon MQ has both queue feature (~SQS) and topic features (~SNS)  
→ `SQS`처럼 보이는 대기열 기능과 `SNS`처럼 보이는 주제 기능을 제공한다.

### 2. Amazon MQ – High Availability
- 다중 `AZ`를 지원하는 `EFS`를 사용하면 가용성을 높일 수 있다.

![image](https://user-images.githubusercontent.com/97398071/235591636-686e27e7-9865-48b9-826c-cd00b3a88814.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---