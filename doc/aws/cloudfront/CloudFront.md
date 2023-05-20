## Amazon CloudFront
### 1. Amazon CloudFront
- `CloudFront`는 글로벌 서비스이다.

- Content Delivery Network (CDN)  
→ `CloudFront`는 `CDN`, 컨텐츠 전송 네트워크이다. 시험에서 `CDN`을 본다면 `CloudFront`를 떠올리면 된다.

- `Amazon CloudFront`는 `.html`, `.css`, `.js` 및 이미지 파일과 같은 정적 및 동적 웹 콘텐츠를 엣지 로케이션에 캐싱하여 사용자에게 더 빨리 배포하도록 지원하는 웹 서비스이다.
~~~
- 콘텐츠가 이미 지연 시간이 가장 낮은 엣지 로케이션에 있는 경우 CloudFront가 콘텐츠를 즉시 제공한다.
- 콘텐츠가 엣지 로케이션에 없는 경우 CloudFront는 콘텐츠의 최종 버전에 대한 소스로 지정된 오리진(Amazon S3 버킷, MediaPackage 채널, HTTP 서버(예: 웹 서버) 등)에서 콘텐츠를 검색한다.
~~~

- Improves users experience  
→ 컨텐츠가 캐싱되므로 전세계의 사용자들이 낮은 레이턴시로 접근할 수 있어서 사용자 경험을 향상시킬 수 있다.

- 216 Point of Presence globally (edge locations)  
→ 현재 `CloudFront`는 전 세계 총 216개의 엣지 로케이션을 통해 구성된다. 그리고 `AWS`는 지속적으로 엣지 로케이션을 추가하고 있다.

- DDoS protection (because worldwide), integration with Shield, AWS Web Application Firewall  
→ 컨텐츠가 전체적으로 분산되어 있으므로 `DDoS, Distributed Denial of Service` 공격으로부터 상대적으로 자유롭다. 

![image](https://user-images.githubusercontent.com/97398071/235298917-17467254-78ab-4f83-b421-d31a5eb2c764.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 2. CloudFront – Origins
- 클라우드 프론트에서 원본이 될 수 있는 대상에는 `S3 bucket`, `Custom Origin (HTTP)` 등이 있다.

- S3 bucket 
~~~
- For distributing files and caching them at the edge 
→ S3 버킷의 경우 CloudFront를 통해 파일을 분산하고 캐싱할 수 있다.

- Enhanced security with CloudFront Origin Access Control (OAC) 
→ 이 경우 버킷에는 CloudFront만 접근 가능할 것이 보장되며, 이를 통해 오리진을 보호한다. 이를 가능하게 하는 것은 Origin Access Control이다.

- OAC is replacing Origin Access Identity (OAI) 
→ OAC는 기존의 OAI, Origin Access Identity를 대체한다.

- CloudFront can be used as an ingress (to upload files to S3) 
→ CloudFront를 통해 버킷에 데이터를 전송하는 것도 가능하다. 이를 Ingress라 한다.
~~~

- `OAI, Origin Access Identity`는 `CloudFront`가 `S3`에 저장된 `Private` 객체에 액세스 할 수 있도록 하는 특별한 식별자이다.

- Custom Origin (HTTP) 
~~~
- Application Load Balancer 
- EC2 instance 
- S3 website (must first enable the bucket as a static S3 website) 
→ 단, 이 경우 버킷을 활성화해서 정적 웹사이트로 설정해야 한다.

- Any HTTP backend you want
→ 이외의 다른 백엔드도 설정 가능하다.
~~~

- 클라우드가 엣지 로케이션에 `HTTP` 요청을 보내면 엣지는 요청받은 컨텐츠가 캐싱되어 있는지 확인한다.
- 캐싱되어 있지 않을 경우에는 원본으로 가서 요청 결과를 가져오면서 이를 로컬 캐시에 저장한다.
- 다른 클라이언트가 같은 컨텐츠를 같은 엣지에서 요청하면 캐싱되어 있는 값을 응답에 사용한다.

![image](https://user-images.githubusercontent.com/97398071/235299150-36373d7e-600a-4a07-8928-c10361848b23.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 1. CloudFront – S3 as an Origin
- `S3`를 원본으로 사용한다고 가정할 때, `LA` 엣지에 접근하는 클라이언트에게는 `LA` 엣지에서 직접 컨텐츠를 제공한다. 그리고 이 `LA` 엣지는 내부 `AWS`망을 통해 `S3` 버킷의 원본을 받아온다.
- 버킷 자체는 `OAC`로 보호받으며, `S3` 버킷 정책에 의해서만 수정할 수 있다.
- 이는 다른 엣지 로케이션을 통해 요청을 수행하는 클라이언트에게도 동일하게 적용된다.
- `CloudFront`와 엣지 로케이션을 통해 특정 리전에 속한 `S3` 버킷을 전 세계의 엣지 로케이션으로 분산시킬 수 있다.

![image](https://user-images.githubusercontent.com/97398071/235299450-8f5c4e92-776e-497e-897b-2a76f5590375.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- `CloudFront` → `distribution` → `Origin access` → `Create control setting`에서 `OAC`를 생성할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/235300341-5498241b-ccc1-41ec-a956-3b52a3c7dbe8.png)

- `S3` 정책이 `CloudFront`로부터의 접근을 허용하도록 수정해야한다.
~~~ json
{
    "Version": "2008-10-17",
    "Id": "PolicyForCloudFrontPrivateContent",
    "Statement": [
        {
            "Sid": "AllowCloudFrontServicePrincipal",
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudfront.amazonaws.com"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::your bucket/*",
            "Condition": {
                "StringEquals": {
                  "AWS:SourceArn": "arn:aws:cloudfront::209938065432:distribution/E1ELXA3PWVPY26"
                }
            }
        }
    ]
  }
~~~

#### 2. CloudFront vs S3 Cross Region Replication
- 목적이 다르다. `CloudFront`는 전세계에 걸친 `CDN`이라면 `S3` 교차 리전 복제는 다른 리전으로의 버킷 복제이다.

- CloudFront:
~~~
- Global Edge network
→ CloudFront는 전세계의 엣지 로케이션을 이용한다.

- Files are cached for a TTL (maybe a day)
→ 아마 하루동안 캐싱된다.
 
- Great for static content that must be available everywhere
→ 전세계를 대상으로 한 정적 컨텐츠를 사용하고자 할 때 용이하다.
~~~

- S3 Cross Region Replication:
~~~
- Must be setup for each region you want replication to happen
→ 각 리전에 설정함으로써 복제가 가능하다. 즉 전세계를 대상으로 하지 않는다.

- Files are updated in near real-time
→ 파일은 거의 실시간으로 갱신된다. 즉, 캐싱이 되지 않는다.

- Read only
→ 읽기 전용으로만 설정 가능하다. 

- Great for dynamic content that needs to be available at low-latency in few regions
→ 일부 리전을 대상으로 동적 컨텐츠를 낮은 지연 시간 내 제공하고자 할 때 유용하다. 
~~~

#### 3. CloudFront – ALB or EC2 as an origin
- `CloudFront`가 원본으로 지정 가능한 대상에는 `EC2` 인스턴스나 `ALB` 등, `Custom Origin (HTTP)` 또한 포함된다.

- `EC2` 인스턴스를 직접 지정
~~~
- 사용자들은 CloudFront를 통해 요청을 수행한다. 사용자들은 CloudFront의 엣지 로케이션에 요청을 보내고, 엣지는 EC2 인스턴스에 요청을 보낼 것이다.
- 이 때 CloudFront에는 VPC가 없기 때문에 EC2 인스턴스는 무조건 public으로 설정되어야 하며, 엣지 로케이션의 모든 공용 IP가 EC2에 접근할 수 있도록 보안 그룹을 변경해야 한다.
~~~

- `ALB`를 지정
~~~
- 사용자들은 CloudFront를 통해 요청을 수행한다. 사용자들은 CloudFront의 엣지 로케이션에 요청을 보내고, 엣지는 EC2 인스턴스에 요청을 보낼 것이다.
- 이 때 CloudFront에는 VPC가 없기 때문에 ALB는 무조건 public으로 설정되어야 하나 EC2 인스턴스들은 프라이빗 설정을 사용해도 문제가 없다.
- 엣지 로케이션의 모든 공용 IP가 ALB에 접근할 수 있도록 보안 그룹을 변경해야 한다.
~~~

![image](https://user-images.githubusercontent.com/97398071/235300685-e4617ca5-1d91-44a4-9fcf-e40c7f525dc7.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. CloudFront Geo Restriction
- You can restrict who can access your distribution  
→ 지리적 제한 기능을 이용하면 사용자들의 지역에 따라 객체의 배포를 제한할 수 있다.
~~~
- Allowlist: Allow your users to access your content only if they're in one of the countries on a list of approved countries.
→ 접근 가능한 국가 목록을 설정할 수 있다.

- Blocklist: Prevent your users from accessing your content if they're in one of the countries on a list of banned countries.
→ 접근 불가능한 국가 목록을 설정할 수 있다.
~~~

- The “country” is determined using a 3rd party Geo-IP database  
→ 여기에서 국가는 서드파티 `Geo-IP database` 정보를 기반으로 하며, 해당 서드파티는 `IP`가 어떤 국가에 해당하는지 반환해 준다.

- Use case: Copyright Laws to control access to content  
→ 컨텐츠 저작권법으로 인한 제한 등이 대표적인 사용 예제이다.

![image](https://user-images.githubusercontent.com/97398071/235301285-35482471-fb27-403e-9887-be851b9114b7.png)

### 4. CloudFront - Pricing
- CloudFront Edge locations are all around the world  
→ `CloudFront`의 엣지 로케이션은 전 세계에 고루 분포되어 있다.

- The cost of data out per edge location varies  
→ 엣지 로케이션마다 데이터 전송 비용이 다르다. 요금 정보는 아래와 같다.

![image](https://user-images.githubusercontent.com/97398071/235301381-b82bb435-672b-4a0d-a7b7-7412da085144.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 1. CloudFront – Price Classes
- You can reduce the number of edge locations for cost reduction  
→ 비용 절감을 위해 엣지 로케이션의 수를 제한할 수 있다. 

- Three price classes:  
→ 세 가지 가격 등급이 있다.
~~~
- Price Class All: all regions – best performance
→ 모든 리전을 사용하며 최상의 성능을 제공한다.

- Price Class 200: most regions, but excludes the most expensive regions
→ 대부분의 리전을 사용할 수 있지만 가장 비싼 리전은 제외된다.

- Price Class 100: only the least expensive regions
→ 가장 저렴한 리전만 사용할 수 있다. 북미와 유럽의 엣지 로케이션만 제공한다. 
~~~

![image](https://user-images.githubusercontent.com/97398071/235301606-6f2c1c41-cc94-42e3-a41e-2a189819c5a2.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 5. CloudFront – Cache Invalidations
- In case you update the back-end origin, CloudFront doesn’t know about it and will only get the refreshed content after the TTL has expired  
→ 엣지 로케이션은 우리가 백엔드 오리진을 업데이트했을 때 업데이트 사항을 알 수 없다. 캐시의 `TTL`가 만료되고 나서야 업데이트된 컨텐츠를 받게 되는 것이다.

- However, you can force an entire or partial cache refresh (thus bypassing the TTL) by performing a CloudFront Invalidation  
→ `CloudFront`의 캐시 무효화를 사용하면 `CloudFront`의 전체 또는 일부 캐시를 강제로 새로고침해서 캐시에 있는 `TTL`을 모두 제거할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/235301894-8844fd62-8cd0-48f7-9d17-c46c61428b89.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- You can invalidate all files (*) or a special path (/images/*)  
→ 이 때에는 특정 파일 경로를 전달해야 한다. `*`를 전달하면 모든 경로의 캐시가 무효화되며, `images/*`를 전달하면 특졍 경로인 `image` 버킷 아래의 캐시만 무효화된다.

![image](https://user-images.githubusercontent.com/97398071/235302136-3ab18b84-33c8-4fa3-affc-2342759f0332.png)

### 6. CloudFront - Using signed cookies
- 현재의 `URL`을 변경하지 않으려는 경우나 여러 제한된 파일에 대한 액세스 권한을 제공하려는 경우, `CloudFront` 서명된 쿠키를 사용하여 콘텐츠 액세스를 제어할 수 있다. 
- `URL`을 변경하지 않고 유료 구독자에게만 여러 비공개 미디어 파일에 대한 액세스를 제공해야 할 때 주로 사용된다.

---
#### ▶ Reference
- [Amazon CloudFront 오리진 액세스 제어(OAC)로 S3 오리진 보호하기](https://aws.amazon.com/ko/blogs/korea/amazon-cloudfront-introduces-origin-access-control-oac/)
- [서명된 쿠키 사용](https://docs.aws.amazon.com/ko_kr/AmazonCloudFront/latest/DeveloperGuide/private-content-signed-cookies.html)
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
