## Amazon Rekognition
### 1. Amazon Rekognition
- `Rekognition` = 인식

- Find objects, people, text, scenes in images and videos using ML  
→ `Amazon Rekognition`는 기계 학습을 이용해 객체, 사람, 텍스트와 이미지와 비디오의 장면을 찾는 서비스이다.

- Facial analysis and facial search to do user verification, people counting  
→ 얼굴을 분석하고 비교하여 사용자 확인을 하며, 이미지 내의 인물 수를 셀 수 있다.

- Create a database of “familiar faces” or compare against celebrities  
→ 익숙한 얼굴을 저장해 자체 데이터베이스를 생성하거나 이미지 속 인물이 궁금할 때 유명인 얼굴의 데이터베이스와 비교할 수 있다.

- Use cases:
~~~
- Labeling
→ 비디오의 라벨링

- Content Moderation
→ 컨텐츠 조정

- Text Detection
→ 텍스트 탐지

- Face Detection and Analysis (gender, age range, emotions…)
→ 얼굴 탐지 및 분석 - 성별이나 연령 범위, 얼굴에 나타나는 표정

- Face Search and Verification
→ 얼굴 검색 및 확인

- Celebrity Recognition
→ 유명인 얼굴 인식

- Pathing (ex: for sports game analysis)
→ 이동 경로를 따라가므로 스포츠 경기 분석에도 사용된다. 
~~~

### 2. Amazon Rekognition – Content Moderation
- Detect content that is inappropriate, unwanted, or offensive (image and videos)  
→ 이미지가 비디오에서 부적절하거나 불쾌감을 주는 컨텐츠를 탐지하는 기능이다.

- Used in social media, broadcast media, advertising, and e-commerce situations to create a safer user experience  
→ `SNS`, 방송 매체, 광고 또는 전자 상거래 서비스를 운용할 때 안전한 사용자 경험을 마련할 수 있다.

- Set a Minimum Confidence Threshold for items that will be flagged  
→ `Amazon Rekognition`가 이미지를 분석한 후 플래그를 세우도록 항목의 최소 신뢰도 임계값을 설정해 사용한다. 임계값이 낮을수록 매칭되는 컨텐츠가 많아진다.

- Flag sensitive content for manual review in Amazon Augmented AI (A2I)  
→ 이미지에 플래그가 서 있는 대상에 대해 추가적으로 인적 검토가 필요할 경우에는 `Amazon Augmented AI, A2I`를 사용하면 된다. `Optional`이다.

- Help comply with regulations  
→ 애플리케이션에 컨텐츠를 게시하기 전에 컨텐츠를 탐지하여 규제를 준수하도록 도와준다.

![image](https://user-images.githubusercontent.com/97398071/236091281-640170c4-856a-4d4a-92c7-51b445471cb1.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---