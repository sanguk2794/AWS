## Well Architected Framework
### 1. Well Architected Framework - General Guiding Principles
- 애플리케이션에 사용할 보안, 성능, 복원력 및 효율성이 뛰어난 인프라를 구축하는 클라우드 아키텍처를 위한 5가지 기반인 운영 우수성, 보안, 안정성, 성능 효율성 및 비용 최적화를 기반으로 하는 프레임워크이다.

- https://aws.amazon.com/architecture/well-architected

- 모범 사례의 주요 지침은 다음과 같다.
~~~
- Stop guessing your capacity needs
→ 필요 용량을 추측하지 말고 가능하면 오토 스케일링을 사용할 것

- Test systems at production scale
→ 프로덕션 규모에서 시스템을 테스트할 것, AWS를 사용하게 되면 대규모 테스트와 인프라를 빠르게 종료할 수 있다.

- Automate to make architectural experimentation easier
→ 자동화를 사용하여 아키텍처 실험을 쉽게 만들 것, CloudFormation 템플릿이 있다면 여러 환경에 쉽게 배포해서 실험할 수 있다.

- Allow for evolutionary architectures : Design based on changing requirements
→ 진화하는 아키텍처를 만들 것, 아키텍처는 시간에 따라 변경되므로 EC2 인스턴스 및 로드 밸런서로 시작하여 API Gateway와 Lambda 같은 서버리스 아키텍처로 발전해야 한다. 이를 위해 변화하는 요구 사항에 맞춘 설계를 해야 한다.

- Drive architectures using data
→ 데이터로 아키텍처를 구동할 것, 이 데이터는 매우 중요해서 옮겨가며 보관해야 한다.

- Improve through game days : Simulate applications for flash sale days
→ GameDay를 통해 연습하고 발전할 것. 시험과 개선을 반복해야 한다. 예를 들어 아키텍처에 많은 부담을 주는 애플리케이션을 반짝 세일 기간에 대입해 시뮬레이션해 보는 것이다.
~~~

### 2. Well Architected Framework - 6 Pillars
- `Well-Architected` 프레임워크에는 6개 원칙이 있다. 이름 정도만 알아두면 충분하다.

- Operational Excellence  
→ 운영 우수성

- Security  
→ 보안

- Reliability  
→ 안정성

- Performance Efficiency  
→ 성능 효율성

- Cost Optimization  
→ 비용 최적화

- Sustainability  
→ 지속 가능성

- They are not something to balance, or trade-offs, they’re a synergy  
→ 이 원칙들은 균형이나 절충할 사안이 아니다. 시너지가 있기 때문이다. 예를 들어 운영 우수성을 향상시키면 비용 최적화도 개선될 가능성이 높다.

### 3. AWS Well-Architected Tool
- Free tool to review your architectures against the 6 pillars Well-Architected Framework and adopt architectural best practices  
→ `AWS Well-Architected Tool`은 프레임워크 도구이며 위에서 정의한 6가지 원칙으로 아키텍처를 검토해 준다.

- How does it work?
~~~
- Select your workload and answer questions
→ 워크로드를 선택한다.

- Review your answers against the 6 pillars
→ 질문에 대답한다.

- Obtain advice: get videos and documentations, generate a report, see the results in a dashboard
→ 거기에 따른 조언을 확인한다. 영상, 문서, 보고서를 얻고 대시보드에서 결과를 볼 수 있다.
~~~

- Let’s have a look: https://console.aws.amazon.com/wellarchitected

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---