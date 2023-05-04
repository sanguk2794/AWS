## AWS Machine Learning - Summary
### 1. AWS Machine Learning - Summary
- 이 내용은 모두 시험에 나온다.
- Rekognition: face detection, labeling, celebrity recognition  
→ `Rekognition`는 얼굴 탐지 및 라벨링, 유명인 인식을 수행할 수 있다. `Content Moderation`를 사용하면 이미지가 비디오에서 부적절하거나 불쾌감을 주는 컨텐츠를 탐지할 수 있다.

- Transcribe: audio to text (ex: subtitles)  
→ 오디오로부터 자막을 얻을 수 있다. 이 때, 개인 식별 정보, `PII`를 제거할 수 있다.

- Polly: text to audio  
→ 텍스트로 오디오를 얻을 수 있다. 약자를 정의할 수 있는 `Pronunciation lexicons`와 숨소리, 속삭임, 강조 단어 등을 추가할 수 있는 `음성 합성 마크업 언어, Speech Synthesis Markup Language, SSML` 기능을 제공한다.

- Translate: translations  
→ 번역에 사용된다.

- Lex: build conversational bots – chatbots  
→ 챗 봇과 같은 대화형 봇을 구축한다.

- Connect: cloud contact center  
→ `Lex`를 `Connect`와 묶으면 클라우드 고객 센터를 만들 수 있다.

- Comprehend: natural language processing  
→ 자연어를 처리하는 `NLP` 서비스이다. 텍스트 언어를 이해할 수 있다. `Comprehend Medical`를 통해 의료데이터를 분석할 수 있다.

- SageMaker: machine learning for every developer and data scientist  
→ 개발자와 데이터 과학자를 위한 완전한 기능의 머신 러닝 서비스이다.

- Forecast: build highly accurate forecasts  
→ 정확한 예측이 가능하다.

- Kendra: ML-powered search engine  
→ 머신 러닝 기반의 문서 검색 엔진이다. 

- Personalize: real-time personalized recommendations  
→ 고객을 위한 실시간 맞춤형 추천을 제공한다.

- Textract: detect text and data in documents  
→ 텍스트와 데이터를 탐지하고 다양한 문서에서 이를 추출한다.

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 한 회사가 음성을 텍스트로 변환해 주고, 고객의 의도를 인식할 수 있는 챗봇을 구현하려 합니다. 이런 경우, 어떤 서비스를 이용해야 할까요?  
→ `Lex`

- 다음 완전 관리형 서비스 중, 높은 정확성의 예측을 제공하는 서비스는?  
→ `Forecast`

- 사진과 영상에서 물체, 사람, 텍스트, 혹은 장면을 찾으려 합니다. 이 경우 어떤 `AWS` 서비스를 사용해야 할까요?  
→ `Rekognition`

- 어느 스타트업 기업이 맞춤형 사용자 경험을 제공하고자 합니다. 이 경우 어떤 `AWS` 서비스를 사용해야 할까요?  
→ `Personalize`

- 어느 연구 팀이 자연어 처리(NLP)를 통해 논문을 주제 별로 묶으려 합니다. 이 경우 어떤 서비스를 사용해야 할까요?  
→ `Comprehend`

- 어느 기업이 자연스럽고 정확한 단어를 사용해 문서를 다양한 언어로 번역하려 합니다. 이 경우 어떤 서비스를 사용해야 할까요?  
→ `Translate`

- 한 개발자가 머신 러닝 모델을 빠르게 구축, 훈련 및 배포를 하려 합니다. 이 경우 어떤 서비스를 사용해야 할까요?  
→ `SageMaker`

- 다음 AWS 서비스 중 음성-텍스트 변환을 용이하게 해주는 서비스는 무엇인가요?  
→ `Transcribe`

- 다음 서비스 중 머신 러닝에 의해 구동되는 문서 검색 서비스는 무엇인가요?  
→ `Kendra`

- 한 회사에서 전 세계 고객들이 이용하는 이미지 및 동영상 공유 플랫폼을 운영하고 있습니다. 이 플랫폼은 `AWS`에서 운영되고 있으며 `S3` 버킷을 사용해 이미지와 동영상을 호스팅하고, 클라우드 프론트를 `CDN`으로 사용해 전 세계 고객들에게 지연 시간이 거의 없이 콘텐츠를 제공하고 있습니다. 지난 몇 달 동안 부적절한 콘텐츠가 보이기 시작했다는 불만을 표한 고객이 많았고, 지난주에는 불만 사항 접수가 늘어나기 시작했습니다. 플랫폼에 게시하기 전에 이미지와 동영상을 직원이 수동으로 승인하는 방식에는 아주 많은 시간과 비용이 소요됩니다. 플랫폼의 요구 사항은 부적절하고 불쾌한 이미지와 동영상을 자동으로 감지하고 수동으로 검토할 항목을 표시할 수 있도록 최소 신뢰 임계값 설정 기능을 제공하는 솔루션을 찾는 것입니다. 이런 요구 사항을 충족할 수 있는 `AWS` 서비스는 무엇입니까?  
→ `Rekognition`

- 전화 통화로 의사와 진료 예약을 할 수 있는 서비스를 제공하는 온라인 의료 회사가 `AWS`를 사용해 인프라를 호스팅하 고 있습니다. 해당 서비스는 `Amazon Connect`와 `Amazon Lex`를 사용하여 전화 수신 및 워크플로 생성, 진료 예약, 진료비 결제를 처리합니다. 회사 방침에 따라 모든 통화는 검토를 위해 녹음되어야 합니다. 하지만 통화를 저장하기 전에 통화 내용에서 개인 식별 가능 정보(PII)를 제거해야 한다는 요구 사항이 있습니다. 통화에서 `PII`를 삭제하는 데 도움이 되는 서비스는 무엇입니까?  
→ `Transcribe`

- `Amazon Polly`를 사용하면 텍스트를 음성으로 변환할 수 있습니다. 이 서비스에는 두 가지 중요한 기능이 있습니다. 첫 번째 기능은 단어의 발음을 사용자 정의할 수 있는 ………………입니다(예: "Amazon EC2"의 발음을 "Amazon Elastic Compute Cloud"로 지정). 두 번째는 기능은 숨소리, 속삭임 등을 포함한 단어를 강조할 수 있는 ………………..입니다.  
→ `Pronunciation lexicons`, `Speech Synthesis Markup Language, SSML`

- 한 의료 회사가 의사 소견서, 임상 시험 보고서, 방사선 보고서와 같은 비정형 의료 문서에서 정보를 탐지 및 추출하여 분석하는 솔루션을 구현하고 있습니다. 해당 문서들은 S3 버킷에 업로드되고 저장됩니다. 회사 규정에 따르면, 솔루션은 `HIPAA` 자격을 갖출 수 있도록 개인 건강 정보(PHI)를 식별하여 환자의 개인 정보를 보호할 수 있게 설계 및 구현되어야 합니다. 이런 경우 어떤 `AWS` 서비스를 사용해야 합니까?  
→ `Comprehend Medical`

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---