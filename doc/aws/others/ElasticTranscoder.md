## Amazon Elastic Transcoder
### 1. Amazon Elastic Transcoder
- Elastic Transcoder is used to convert media files stored in S3 into media files in the formats required by consumer playback devices (phones etc..)  
→ `Elastic Transcoder`는 `Amazon S3`에 저장된 미디어 파일을 소비자 재생 장치(전화 등)에서 필요한 형식의 미디어 파일로 변환하는 데 사용된다.

- 파일 변환의 시나리오는 다음과 같다.
~~~
- 비디오 파일을 Amazon S3 버킷에 넣은 다음 Elastic Transcoder의 Transcoder 파이프라인을 통해 실행시킨다.
- 해당 비디오 파일이 다양한 비디오 파일로 변환되어 S3 출력 버킷으로 들어간다.
~~~

![image](https://github.com/sanguk2794/AWS/assets/97398071/30929bd0-a608-4413-ad98-12307dfeae45)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)


- Benefits:
~~~
- Easy to use
→ 사용하기 쉽다.

- Highly scalable – can handle large volumes of media files and large file sizes
→ 확장성이 높다. S3 버킷에 더 많은 파일이 있을수록 더 많은 처리 능력을 갖는다.

- Cost effective – duration-based pricing model
→ 미디어가 트랜스코딩된 시간만큼만 비용을 지불하면 되므로 비용 효율적이다.

- Fully managed & secure, pay for what you use
→ 완벽하게 관리되고 안전하다.
~~~

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---