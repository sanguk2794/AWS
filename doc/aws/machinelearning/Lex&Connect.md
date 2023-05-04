## Amazon Lex & Connect
### 1. Amazon Lex & Connect
• Amazon Lex: (same technology that powers Alexa)
~~~
• Amazon Lex 기술은 Amazon의 Alexa 장치를 구현하는 기술이다. Alexa는 자동 음성 인식을 지원한다.

• Automatic Speech Recognition (ASR) to convert speech to text
→ 음성을 인식하는 ASR이다. 말을 텍스트로 바꿔준다.

• Natural Language Understanding to recognize the intent of text, callers
→ Lex는 자연어 이해를 통해 말의 의도를 파악하고 문장을 이해할 수 있다.

• Helps build chatbots, call center bots
→ Amazon Lex 기술은 챗봇 구축이나 콜 센터 봇 구축에 도움을 준다.
~~~

• Amazon Connect:
~~~
• Amazon Connect라는 가상 고객 센터가 있다고 가정한다.

• Receive calls, create contact flows, cloud-based virtual contact center
→ 이는 전화를 받고 고객 응대의 흐름을 생성하는 클라우드 기반의 서비스이다.

• Can integrate with other CRM systems or AWS
→ 다른 고객 관리 시스템인 CRM 및 AWS 서비스와 통합할 수 있다.

• No upfront payments, 80% cheaper than traditional contact center solutions
→ 기존 고객 센터 방식에 비해 초기 비용이 없고 비용이 약 80% 저렴하다.
~~~

• 스마트 고객 센터를 구축하는 시나리오는 다음과 같다.
~~~
• 일정을 예약하기 위해 전화를 건다. 지정한 번호로 Amazon Connect와의 통화가 이루어진다.
• Amazon Connect로 전화가 연결되면, Lex는 모든 정보를 스트리밍하여 통화 목적을 이해한다.
• 그런다음 Lex가 올바른 람다 함수를 호출한다. 이 람다 함수는 똑똑해서 전화를 건 사람이 내일 오후 3시에 Tom과 미팅을 원함을 이해하고 CRM으로 이동하여 코드를 작성해서 미팅 일정을 잡는다.
~~~

![image](https://user-images.githubusercontent.com/97398071/236096446-d85c571f-10e1-4b11-ae98-3720e9ccd127.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

• 정리하면 `Lex`는 `ASR`이고 `Connect`는 고객 센터이다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---