## S3 Performance
### 1. S3 – Baseline Performance
- Amazon S3 automatically scales to high request rates, latency 100-200 ms  
→ 기본적으로 `S3`는 요청이 아주 많을 때 자동으로 확장된다. `S3`로부터 첫 번째 바이트를 수신하는 데 지연 시간도 100~200 밀리초 사이로 아주 짧다.

- Your application can achieve at least 3,500 PUT/COPY/POST/DELETE and 5,500 GET/HEAD requests per second per prefix in a bucket.  
→ `S3`는 버킷 내에서 접두사당 초당 3500개의 `PUT/COPY/POST/DELETE`, 초당 5500개의 `GET/HEAD` 요청을 지원한다. 굉장히 고성능이라는 뜻이다.

- There are no limits to the number of prefixes in a bucket.  
→ 버킷 내의 접두사 수에는 제한이 없다.

- Example (object path => prefix):  
→ `file`이라는 이름을 가진 네 개의 객체가 있다고 가정한다.
~~~
- bucket/folder1/sub1/file => /folder1/sub1/
→ bucket과 file 사이에 있는 것이 접두사가 된다. 이 접두사를 갖는 파일은 초당 3500개의 PUT 요청과 초당 5500개의 GET 요청을 처리한다.

- bucket/1/file => /1/
→ bucket과 file 사이에 있는 것이 접두사가 된다. 이 접두사를 갖는 파일은 초당 3500개의 PUT 요청과 초당 5500개의 GET 요청을 처리한다.
~~~

- If you spread reads across all four prefixes evenly, you can achieve 22,000 requests per second for GET and HEAD  
→ 여러 접두사에 읽기 요청을 균등하게 분산하면 초당 22000개의 `GET/HAED` 요청을 처리할 수 있다.

### 2. S3 Performance
#### 1. S3 Multi-Part upload
- recommended for files > 100MB, must use for files > 5GB
→ `100MB`가 넘는 파일은 멀티파일 업로드를 사용하는 것이 좋고 `5GB`가 넘는 파일은 반드시 사용해야 한다.

- Can help parallelize uploads (speed up transfers)
→ 멀티파트 업로드는 업로드를 병렬화하므로 전송 속도를 높여 대역폭을 최대화할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/235167938-7409a687-bb99-47a7-afe4-ffd4d8f0de08.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 2. S3 Transfer Acceleration
- `Amazon S3 Transfer Acceleration`를 사용하면 클라이언트와 `S3` 버킷 간 빠르고 쉽고 안전한 장거리 파일 전송이 가능하다.

- Increase transfer speed by transferring file to an AWS edge location which will forward the data to the S3 bucket in the target region  
→ 파일을 `AWS` 엣지 로케이션으로 전송한다. 엣지 로케이션에 도착한 데이터는 최적화된 네트워크 경로를 통해 `S3` 버킷으로 전달된다.

- Compatible with multi-part upload
→ 전송 가속화를 멀티파트 업로드와 함께 사용할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/235168434-edbdfc47-92a5-42c6-936d-950dc3ceac12.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- `S3`에 모든 데이터를 가장 빠른 방법으로 집계해야 한다면 `S3 Transfer Acceleration`를 사용해야 한다.

#### S3 Byte-Range Fetches
- `S3 바이트 범위 가져오기` 기능은 파일을 수신하고 파일을 읽는 가장 효율적인 방법 중 하나이다.

- Parallelize GETs by requesting specific byte ranges  
→ 파일에서 특정 바이트 범위를 가져와서 `GET` 요청을 병렬화한다.

- Better resilience in case of failures  
→ 특정 바이트 범위를 가져오는 데 실패한 경우에도 더 작은 바이트 범위에서 재시도하므로 실패에 대한 복원력이 높다.

- 첫 번째 사용 사례는 다운로드 속도를 높일 때이다.

![image](https://user-images.githubusercontent.com/97398071/235169224-e4fd999f-5112-42eb-bd0c-d5dd907ee26b.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 두 번째 사용 사례는 파일의 일부 검색이다.  
→ 예를 들어 `S3`에 있는 파일의 첫 50바이트가 헤더라는 것을 알고 파일에 대한 정보를 안다면 첫 50바이트를 이용해서 헤더에 대한 바이트 범위 요청을 보내면 된다.

![image](https://user-images.githubusercontent.com/97398071/235169644-4c90b338-06e9-4ad1-851b-640037970706.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 4. S3 Select & Glacier Select
- Retrieve less data using SQL by performing server-side filtering  
→ `S3`에서 파일을 검색할 때 검색한 다음에 필터링하면 너무 많은 데이터를 검색하게 된다.

- Can filter by rows & columns (simple SQL statements)  
→ `SQL`문에서 간단히 행과 열을 사용해 필터링할 수 있다.

- Less network transfer, less CPU cost client-side  
→ 네트워크 전송이 줄어들기 때문에 데이터 검색과 필터링에 드는 클라이언트 측의 `CPU` 비용도 줄어든다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---