## Amazon Polly
### 1. Amazon Polly
- Turn text into lifelike speech using deep learning  
→ `Polly`는 `Transcribe`와 반대이다. 딥 러닝 기술을 사용해 텍스트를 음성으로 변환한다.

- Allowing you to create applications that talk  
→ 음성 작동 애플리케이션을 만들 수 있다.

### 2. Amazon Polly – Lexicon & SSML
- `Amazon Polly`는 `Lexicon, 어휘 목록`과 `SSML`을 활용한다. 발음 어휘 목록을 사용해 사용자 지정 발음을 생성할 수 있다.

- Customize the pronunciation of words with Pronunciation lexicons
~~~
- Stylized words: St3ph4ne => “Stephane”
→ 스타일링한 단어 입력에 대한 지정 발음을 설정할 수 있다.

- Acronyms: AWS => “Amazon Web Services”
→ 약자로 된 단어는 그대로 읽는 대신 풀어서 읽도록 설정할 수 있다.
~~~

- Upload the lexicons and use them in the SynthesizeSpeech operation  
→ 어휘 목록을 업로드해서 `SynthesizeSpeech` 작업에 사용하면 된다.

- Generate speech from plain text or from documents marked up with Speech Synthesis Markup Language (SSML) – enables more customization
~~~
- Speech Synthesis Markup Language (SSML) 는 음성 합성 마크업 언어라는 의미이다. 이를 통해 보다 다양한 사용자 지정 음성을 만들 수 있다.

- emphasizing specific words or phrases
→ 특정 단어나 구절을 강조할 수 있다.

- using phonetic pronunciation
→ 음성학적 발음을 구현할 수 있다.

- including breathing sounds, whispering
→ 숨소리를 넣거나 속삭이듯 말할 수 있다.

- using the Newscaster speaking style
→ 뉴스 진행자 스타일로 말할 수 있다.
~~~


---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---