## Amazon Comprehend
### 1. Amazon Comprehend
- `Comprehend` = 일치

- For Natural Language Processing – NLP  
→ `Comprehend`는 이해하는 서비스이다. 자연어를 처리하는 `NLP` 서비스이다. 시험에서 `NLP`가 보이면 `Amazon Comprehend`이다.

- Fully managed and serverless service  
→ 완전 관리형 서버리스 서비스이다.

- Uses machine learning to find insights and relationships in text  
~~~
- 머신 러닝을 사용하여 텍스트에서 인사이트와 관계를 도출한다.

- Language of the text
→ 텍스트 언어를 이해할 수 있다.

- Extracts key phrases, places, people, brands, or events
→ 텍스트의 주요 문구, 장소 및 사람, 브랜드, 이벤트 등을 추출한다.

- Understands how positive or negative the text is
→ 분석 중인 텍스트가 긍정적인지 부정적인지 파악하는 감정 분석이 가능하다.

- Analyzes text using tokenization and parts of speech
→ 토큰화 및 품사를 사용하여 텍스트를 분석할 수 있다.

- Automatically organizes a collection of text files by topic
→ 텍스트 파일 모음을 주제에 따라 정리하고 주제를 식별할 수 있다.

- 대량의 데이터가 있으면 Comprehend가 그 의미를 이해하려고 시도한다. 이를 통해 텍스트 혹은 구조화되지 않은 데이터를 구조화할 수 있다.
~~~

- Sample use cases:
~~~
- analyze customer interactions (emails) to find what leads to a positive or negative experience
→ 고객 상호 작용을 분석할 때 사용된다. 많은 고객이 이메일을 보내올 경우 고객의 긍정적, 부정적 경험이 무엇인지에 대해 지원 서비스를 기반으로 전반적인 이해를 돕는다. 
이를 통해 비즈니스 인사이트를 얻을 수 있고 분석을 통해 비즈니스를 향상시킬 수 있다.

- Create and groups articles by topics that Comprehend will uncover
→ 식별하는 주제로 문서를 만들고 그룹화할 수도 있다. 많은 문서를 하나씩 읽지 않고 그룹화하고 싶은 경우 Comprehend가 그룹화에 필요한 주제를 식별해서 알려준다.
~~~

- 시험을 대비해서 알아아 할 내용은 자연어 처리 기능, NLP 정도이다.

### 2. Amazon Comprehend Medical
- Amazon Comprehend Medical detects and returns useful information in unstructured clinical text:
~~~
- 비정형 의료 텍스트에서 유용한 정보를 탐지해서 반환해주는 서비스이다. NLP, 자연어 처리를 사용해 텍스트를 탐지한다.

- Physician’s notes
→ 의사 소견서

- Discharge summaries
→ 퇴원 요약서

- Test results
→ 검사 결과서

- Case notes
→ 의료 사례 기록
~~~

- Uses NLP to detect Protected Health Information (PHI) – DetectPHI API  
→ 문서 속의 보호된 개인 건강 정보를 `DetachPHI API`로 탐지한다.

- Store your documents in Amazon S3, analyze real-time data with Kinesis Data Firehose, or use Amazon Transcribe to transcribe patient narratives into text that can be analyzed by Amazon Comprehend Medical.  
→ 문서를 `Amazon S3`에 저장하거나, `Kinesis Data Firehose`를 사용하여 실시간 데이터를 분석하거나, `Amazon Transcript`를 사용하여 환자의 내러티브를 `Amazon Comprehed Medical`이 분석할 수 있는 텍스트로 전사할 수 있다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---