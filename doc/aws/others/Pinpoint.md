## Amazon Pinpoint
### 1. Amazon Pinpoint
- Scalable 2-way (outbound/inbound) marketing communications service  
→ `Amazon Pinpoint`는 확장 가능한 양방향 인바운드 및 아웃바운드 마케팅 커뮤니케이션 서비스이다.

- Supports email, SMS, push, voice, and in-app messaging  
→ 이메일, SMS, 푸시, 음성 및 앱 내 메시징을 지원한다. 주요 사용 사례 중 하나는 `SMS`이다.

- Ability to segment and personalize messages with the right content to customers  
→ 고객에게 적합한 콘텐츠로 메시지를 세분화하고 개인화하여 전송할 수 있다.

- Possibility to receive replies  
→ 답장을 받을 수 있다. 

- Scales to billions of messages per day  
→ 하루에 수십억 개의 메시지를 보낼 수 있다.

- Use cases: run campaigns by sending marketing, bulk, transactional SMS messages  
→ `Pinpoint`의 사용 사례는 마케팅 이메일을 대량으로 보내거나 트랜잭션 `SMS` 메시지를 전송하여 캠페인을 실행하는 것이다.

- 누군가 응답하거나 하면 `TEXT_SUCCESS`, `TEXT_DELIVERED`, `REPLIED`와 같은 모든 이벤트가 `Amazon SNS`, `Kinesis Data Firehose`, `CloudWatch Logs` 등으로 전달된다. 

- Versus Amazon SNS or Amazon SES
~~~
- In SNS & SES you managed each message's audience, content, and delivery schedule
→ SNS 또는 SES에서는 사용자가 각 메시지의 대상, 내용, 전달 일정을 관리해야 한다. 

- In Amazon Pinpoint, you create message templates, delivery schedules, highly-targeted segments, and full campaigns
→ Amazon Pinpoint에서는 직접 메시지 템플릿, 전달 일정 대상 세그먼트, 전체 캠페인 등을 만들지 않고 Pinpoint 서비스로 관리할 수 있다.
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/07e0b288-fab0-4b0b-a975-30d113a2c928)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---