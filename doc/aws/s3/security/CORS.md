## CORS 
### 1. What is CORS?
• Cross-Origin Resource Sharing (CORS)  
→ `CORS`는 교차 오리진 리소스 공유이다.

• Origin = scheme (protocol) + host (domain) + port  
→ 오리진은 프로토콜과 호스트와 포트로 구성된다.
~~~
• example: https://www.example.com (implied port is 443 for HTTPS, 80 for HTTP)
~~~

• Web Browser based mechanism to allow requests to other origins while visiting the main origin  
→ `CORS`는 웹 브라우저 기반 보안 메커니즘으로 메인 오리진을 방문하는 동안 다른 오리진에 대한 요청을 허용하거나 거부한다.

• 프로토콜, 호스트, 포트가 동일할 때 오리진이 같다고 말한다.
~~~
• Same origin: http://example.com/app1 & http://example.com/app2
• Different origins: http://www.example.com & http://other.example.com
~~~

• The requests won’t be fulfilled unless the other origin allows for the requests, using CORS Headers (example: Access-Control-Allow-Origin)  
→ 웹 브라우저가 한 웹 사이트를 방문하는 동안 요청 체계의 일부러 다른 웹사이트에 요청을 보내야 할 때 다른 오리진이 `CORS` 헤더를 사용해서 요청을 허용하지 않는 한 해당 요청은 이행되지 않는다.
이를 `Access-Control-Allow-Origin Header`라고 한다.

• 발음은 `콜스`이다.

• `S3` 버킷에서 파일을 로드하는 웹 사이트가 있습니다. `Chrome` 브라우저에서 파일 `URL`을 직접 입력하면 정상적으로 가져오는데, 웹 사이트를 통해 같은 파일을 로드하려고 하면 동작하지 않습니다. 무엇이 문제입니까?  
→ `CORS`가 잘못되었다.

### 2. CORS Scenario
• 웹 브라우저가 오리진에 요청을 전송한다.

• 오리진에서의 응답에 다른 웹사이트로의 요청이 포함되어 있다. 이를 교차 오리진이라고 한다.

• 교차 오리진에 사전 요청을 보낸다. 이 사전 요청에는 해당 요청의 오리진이 `https://www.example.com`이라는 내용이 포함된다.

• 교차 오리진이 교차 오리진 리소스 공유를 허용하도록 구성되었다면 `CORS` 헤더를 담아 응답한다.
~~~
Access-Control-Allow-Origin: *
Access-Control-Allow-Method: GET, PUT, DELETE
~~~

• 웹 브라우저가 다른 서버의 파일을 검색하고 호출하도록 요청한다.

![image](https://user-images.githubusercontent.com/97398071/235286958-7e946430-435e-40ba-9f75-164cdb854bd1.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. Amazon S3 – CORS
• If a client makes a cross-origin request on our S3 bucket, we need to enable the correct CORS headers  
→ 클라이언트가 `S3` 버킷에서 교차 오리진 요청을 하면 정확한 `CORS` 헤더를 활성화해야 한다.

• It’s a popular exam question  

• You can allow for a specific origin or for * (all origins)  
→ 이 작업을 빠르게 수행하려면 특정 오리진을 허용하거나, `*`를 붙여 모든 오리진을 허용해야 한다.

![image](https://user-images.githubusercontent.com/97398071/235287358-1de994af-0202-45c7-b3ef-976069f5a146.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

• `permissions` - `Cross-origin resource sharing (CORS)` 옵션이 있다. 이는 `JSON` 형식으로 정의해야 한다.
~~~ json
[
    {
        "AllowedHeaders": [
            "Authorization"
        ],
        "AllowedMethods": [
            "GET"
        ],
        "AllowedOrigins": [
            "<url of first bucket with http://...without slash at the end>"
        ],
        "ExposeHeaders": [],
        "MaxAgeSeconds": 3000
    }
]
~~~

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
