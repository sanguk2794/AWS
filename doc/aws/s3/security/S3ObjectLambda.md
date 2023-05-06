## Access Points
### 1. S3 – Access Points
- 버킷 정책은 유저 그룹과 버킷의 수가 많을수록 그 설정이 복잡해진다. 이를 `Access Point`를 통해 단순화할 수 있다.  
→ 이는 `S3` 버킷 상위 수준에 하나의 계층을 만드는 것과 같다.

![image](https://user-images.githubusercontent.com/97398071/235292889-4c478ade-8645-4514-9da4-f62e0ae3b21e.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Each Access Point gets its own DNS and policy to limit who can access it  
→ 각 액세스 포인트마다 고유의 `DNS`와 정책을 설정할 수 있다.
~~~
- A specific IAM user / group
→ 사용자나 그룹의 액세스를 설정할 수 있다.

- One policy per Access Point => Easier to manage than complex bucket policies
→ 액세스 포인트별로 하나의 정책만 가지므로 복잡하고 고유한 버킷 정책보다 훨씬 다루기 쉽다.
~~~

### 2. S3 Object Lambda
- `Lambda`는 몇몇 코드를 손쉽게 실행할 수 있도록 도와준다.

- Use AWS Lambda Functions to change the object before it is retrieved by the caller application  
→ 버킷의 객체의 취득 가능 정보를 취득 이전에 수정하기 위해 사용한다.

- Only one S3 bucket is needed, on top of which we create S3 Access Point and S3 Object Lambda Access Points.  
→ 취득 가능 정보를 구분하기 위해 여러 개의 버킷을 생성할 필요가 없다. 액세스 포인트를 생성하고 람다 함수를 실행하도록 설정하는 것으로 충분하다.

- 예제의 `Analytics Application`은 `S3` 버킷으로부터 편집된 데이터를 얻게 되며, `Marketing Application`은 데이터베이스에서 호출한 정보를 포함한 강화된 객체를 얻게 될 것이다.

![image](https://user-images.githubusercontent.com/97398071/235293000-2c451fd6-e20e-4e4e-aef4-84ff0cafaa00.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Use Cases:
~~~
- Redacting personally identifiable information for analytics or non-production environments.
→ 분석이나 비프로덕션 환경에서 개인 식별 정보 등 PII 데이터를 삭제할 때

- Converting across data formats, such as converting XML to JSON.
→ XML 데이터를 JSON으로 변환할 때

- Resizing and watermarking images on the fly using caller-specific details, such as the user who requested the object.
→ 이외의 원하는 변환을 실행할 때, 예를 들면 이미지 크기를 바꾸거나 워터파크를 남기는 등의 처리를 수행할 수 있다.
~~~

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
