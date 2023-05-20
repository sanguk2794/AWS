## Edge Function
### 1. Customization At The Edge
- 보통 함수와 애플리케이션은 특정 리전에서 배포된다. 하지만, `CloudFront`를 사용하면 전 세계에 분포되어 있는 엣지를 통해 콘텐츠를 배포할 수 있다.

- Many modern applications execute some form of the logic at the edge  
→ 많은 모던 애플리케이션은 애플리케이션에 도달하기 전 엣지에서 로직을 실행할 수 있도록 요구한다. 이 때 사용되는 것이 엣지 함수이다.

- Edge Function:
~~~
- A code that you write and attach to CloudFront distributions
→ CloudFront 배포판에 작성하여 첨부하는 코드이다.

- Runs close to your users to minimize latency
→ 이 함수는 로직을 사용자 근처에서 실행하여 지연시간을 최소화하는 것이 목적이다.
~~~

- CloudFront provides two types: CloudFront Functions & Lambda@Edge  
→ `CloudFront`에는 `CloudFront` 함수와 `Lambda@Edge` 함수, 두 종류의 함수가 있다.

- You don’t have to manage any servers, deployed globally  
→ 엣지 함수를 사용하면 서버를 관리할 필요가 없다. 전역적으로 배포되기 때문이다.

- Use case: customize the CDN content  
→ 사용 사례에는 `CloudFront`의 `CDN` 컨텐츠를 커스터마이즈하는 경우를 들 수 있다.

- Pay only for what you use, Fully serverless  
→ 사용한 만큼의 비용을 지불하며 완전한 서버리스이다.

#### 1. CloudFront Functions & Lambda@Edge Use Cases
- Website Security and Privacy  
→ 웹사이트 보안과 프라이버시에 활용 가능하다.

- Dynamic Web Application at the Edge  
→ 엣지에서의 동적 웹 애플리케이션 로드에 활용 가능하다.

- Search Engine Optimization (SEO)  
→ 검색 엔진 최적화에 활용 가능하다.

- Intelligently Route Across Origins and Data Centers  
→ 오리진 및 데이터 센터 간의 지능형 경로에 활용 가능하다.

- Bot Mitigation at the Edge  
→ 엣지에서의 봇 완화에 활용 가능하다.

- Real-time Image Transformation  
→ 엣지에서의 실시간 이미지 변환에 활용 가능하다.

- A/B Testing  
→ `A/B` 테스트에 활용 가능하다.

- User Authentication and Authorization  
→ 사용자 인증 및 권한 부여에 활용 가능하다.

- User Prioritization  
→ 사용자 우선순위 지정에 활용 가능하다.

- User Tracking and Analytics  
→ 사용자 추적 및 분석에 활용 가능하다.

###### 1. A/B 테스트
- 분할 테스트 또는 버킷 테스트라고도 하는 `A/B` 테스트는 두 가지 콘텐츠를 비교하여 방문자/뷰어가 더 높은 관심을 보이는 버전을 확인한다.
- 주요 측정지표를 기반으로 가장 성공적인 버전을 측정하기 위해 변형 버전과 컨트롤 버전을 비교, 검증한다.

#### 2. CloudFront Functions
- `CloudFront` 함수의 원리와 활용 방법은 다음과 같다. 요청을 하는 유저를 여기에서는 뷰어로 지칭한다.

![image](https://user-images.githubusercontent.com/97398071/235751032-39ff6415-d67b-4d98-920f-8d03acaef218.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Lightweight functions written in JavaScript  
→ `CloudFront` 함수는 `Javascript`로 작성된 경량 함수이다. 이 함수는 뷰어 요청과 응답을 수정한다.

- For high-scale, latency-sensitive CDN customizations  
→ 확장성이 높고 지연 시간에 민감한 `CDN` 사용자 지정에 사용된다.

- Sub-ms startup times, millions of requests/second  
→ 시작 시간은 1 밀리초 미만이며 초당 백만 개의 요청을 처리한다.

- Used to change Viewer requests and responses:  
→ 앞서 말한대로 뷰어 요청과 응답을 수정할 때만 사용된다.
~~~
- Viewer Request: after CloudFront receives a request from a viewer
→ CloudFront가 뷰어로부터 요청을 받으면 뷰어 요청을 수정할 수 있다.

- Viewer Response: before CloudFront forwards the response to the viewer
→ CloudFront가 뷰어에게 응답을 보내기 전에 응답을 수정할 수 있다.
~~~

- Native feature of CloudFront (manage code entirely within CloudFront)  
→ `CloudFront`의 네이티브 기능으로 모든 코드가 `CloudFront`에서 직접 관리된다.

#### 3. Lambda@Edge
- `CloudFront Functions`보다 좀 더 많은 기능을 한다.

- Lambda functions written in NodeJS or Python  
→ 이 함수는 `Node.js`나 `Python`으로 작성된다.

- Scales to 1000s of requests/second  
→ 초당 수천 개의 요청을 처리할 수 있다.

- Used to change CloudFront requests and responses:  
→ 모든 `CloudFront` 요청 및 응답을 변경할 수 있다.
~~~
- Viewer Request – after CloudFront receives a request from a viewer
→ CloudFront가 뷰어로부터 요청을 받으면 뷰어 요청을 수정할 수 있다.

- Viewer Response – before CloudFront forwards the response to the viewer
→ CloudFront가 뷰어에게 응답을 보내기 전에 응답을 수정할 수 있다.

- Origin Request – before CloudFront forwards the request to the origin
→ CloudFront가 오리진에 요청을 전송하기 전에 수정할 수 있다.

- Origin Response – after CloudFront receives the response from the origin
→ CloudFront가 오리진에게서 받은 응답을 수정할 수 있다.
~~~

- Author your functions in one AWS Region (us-east-1), then CloudFront replicates to its locations  
→ `Lambda@Edge` 함수는 `us-east-1` 리전에만 작성할 수 있다. `CloudFront`의 배포를 관리하는 리전과 같은 리전이다. 함수를 작성하면 `CloudFront`가 모든 로케이션에 해당 함수를 복제한다.

#### 4. CloudFront Functions vs. Lambda@Edge
- 가장 눈에 띄는 차이는 런타임 지원 언어이다. `CloudFront Functions`는 `Javascript`지만 `Lambda@Edge`는 `Node.js`나 `Python`를 지원한다.
- `CloudFront Functions`의 확장성은 매우 높다. 수백만 개의 요청을 처리한다. 반면에 `Lambda@Edge`는 수천 개에 불과하다.
- 트리거 발생 위치도 다르다. `CloudFront Functions`는 뷰어에게만 영향을 미치는 반면 `Lambda@Edge`는 뷰어와 오리진 모두에게 영향을 미칠 수 있다.
- `CloudFront Functions`의 최대 실행 시간은 1밀리초 미만이다. 아주 빠르고 간단하다. 하지만, `Lambda@Edge`의 실행에는 5~10초가 소요된다.

#### 5. CloudFront Functions vs. Lambda@Edge - Use Cases
- CloudFront Functions  
→ 모든 작업이 1밀리초 이내에 이루어진다.
~~~
- Cache key normalization : Transform request attributes (headers, cookies, query strings, URL) to create an optimal Cache Key
→ CloudFront Functions는 캐시 키를 정규화한다. 요청 속성을 변환하여 최적의 캐시 키를 생성한다.

- Header manipulation : Insert/modify/delete HTTP headers in the request or response
→ 요청이나 응답에 HTTP 헤더를 삽입, 수정, 삭제하도록 헤더를 조작할 수 있다.

- URL rewrites or redirects
→ URL을 다시 쓰거나 리디렉션할 수 있다.

- Request authentication & authorization : Create and validate user-generated tokens (e.g., JWT) to allow/deny requests
→ 요청을 허용 또는 거부하기 위해 JWT을 생성하거나 검증하는 요청 인증 및 권한 부여에도 사용된다.
~~~

- Lambda@Edge
~~~
- Longer execution time (several ms)
→ 실행 시간이 10초가 걸릴 수도 있다.

- Adjustable CPU or memory
→ CPU와 메모리가 증가하므로 여러 라이브러리를 로드할 수 있다.

- Your code depends on a 3rd libraries (e.g., AWS SDK to access other AWS services)
→ 타사 라이브러리에 코드를 의존시킬 수도 있다. 가령 SDK에서 다른 AWS 서비스에 액세스할 수 있도록 할 수 있다.

- Network access to use external services for processing
→ 네트워크 액세스를 통해 외부 서비스에서 데이터를 처리할 수 있어 대규모 데이터 통합을 수행할 수 있다.

- File system access or access to the body of HTTP requests
→ 파일 시스템이나 HTTP 요청 본문에도 바로 액세스할 수 있다.
~~~

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---