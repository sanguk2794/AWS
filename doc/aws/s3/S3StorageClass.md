## S3 Storage Classes
### 1. S3 Storage Classes
- 여러 종류가 있다. 상황에 맞춰 선택하면 된다.
~~~
- Amazon S3 Standard - General Purpose
- Amazon S3 Standard-Infrequent Access (IA)
- Amazon S3 One Zone-Infrequent Access
- Amazon S3 Glacier Instant Retrieval
- Amazon S3 Glacier Flexible Retrieval
- Amazon S3 Glacier Deep Archive
- Amazon S3 Intelligent Tiering
~~~

- Can move between classes manually or using S3 Lifecycle configurations  
→ `Amazon S3`에서 객체를 생성할 때 클래스를 선택하거나 스토리지 클래스를 수동으로 수정할 수 있다. 혹은 `Amazon S3` 수명 주기 구성을 사용해 스토리지 클래스 간에 객체를 자동으로 이동시킬 수도 있다.

- 모든 수치를 기억할 필요는 없다. 각 클래스가 무엇인지만 이해하면 된다.

![image](https://user-images.githubusercontent.com/97398071/234881770-080bd3a1-5c62-4689-8ec8-eb85458f6c67.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 버킷의 스토리지 클래스 변경을 자동화할 수 있다. `S3` → `Bucket` → `Bucket` 선택 → `Management` → `Lifecycle rules`를 활용하는 것이다.

### 2. S3 Durability and Availability
- Durability:
~~~
- High durability (99.999999999%, 11 9’s) of objects across multiple AZ
→ 내구성은 Amazon S3로 인해 객체가 손실되는 횟수를 나타낸다. 그리고 Amazon S3는 매우 뛰어난 내구성을 제공한다. 

- If you store 10,000,000 objects with Amazon S3, you can on average expect to incur a loss of a single object once every 10,000 years
→ Amazon S3에 1000만개의 객체를 저장했을 때, 평균적으로 10000년에 한 번 객체 손실이 예상된다.

- Same for all storage classes
→ 모든 스토리지 클래스의 내구성은 동일하다.
~~~

- Availability:
~~~
- Measures how readily available a service is
→ 가용성은 서비스가 얼마나 용이하게 제공되는지를 나타낸다.

- Varies depending on storage class
→ 이는 스토리지 클래스별로 다르다.

- Example: S3 standard has 99.99% availability = not available 53 minutes a year
→ 예를 들어 S3 Standard의 가용성은 99.99%인데 1년에 약 53분 동안은 서비스를 사용할 수 없다는 의미이다.
그러므로 애플리케이션을 개발할 때 이 부분을 고려해야 한다.
~~~

### 3. S3 Standard – General Purpose
- 99.99% Availability

- Used for frequently accessed data  
→ 자주 액세스하는 데이터에 사용되는 기본적으로 사용되는 스토리지 유형이다.

- Low latency and high throughput  
→ 지연 시간이 짧고 처리량이 높다.

- Sustain 2 concurrent facility failures  
→ `AWS`에서 두 개의 기능 장애를 동시에 버틸 수 있다.

- Use Cases: Big Data analytics, mobile & gaming applications, content distribution…  
→ 사용 사례에는 빅데이터 분석, 모바일과 게임 애플리케이션, 콘텐츠 배포 등이 있다.

### 4. S3 Storage Classes – Infrequent Access
- For data that is less frequently accessed, but requires rapid access when needed  
→ 자주 액세스하지 않지만 필요한 경우 빠르게 액세스해야 할 경우에 사용하는 스토리지 유형이다.

- Lower cost than S3 Standard  
→ `S3 Standard`보다 비용이 적게 들지만 검색 비용이 발생한다.

- 객체를 `Standard-IA` 또는 `One Zone-IA`로 전환하려면 최소 30일 동안 오브젝트를 저장한 상태여야 한다.

- Amazon S3 Standard-Infrequent Access (S3 Standard-IA)
~~~
- 99.9% Availability
→ S3 Standard보다 약간 떨어진다.

- Use cases: Disaster Recovery, backups
→ 사용 사례에는 재해 복구와 백업이 있다.
~~~

- Amazon S3 One Zone-Infrequent Access (S3 One Zone-IA)  
→ `One Zone-IA`로도 불린다.
~~~
- 최소한 3개의 가용 영역에 데이터를 저장하는 다른 S3 스토리지 클래스와 달리, S3 One Zone-IA는 하나의 AZ에 데이터를 저장한다. 대신 S3 Standard-IA에 비해 비용이 20% 저렴하다.
- 최소 보관 기간은 30일이다.

- High durability (99.999999999%) in a single AZ; data lost when AZ is destroyed
→ 단일 AZ 내에서는 높은 내구성을 갖지만 AZ가 파괴된 경우 데이터를 잃게 된다.

- 99.5% Availability
→ 가용성은 더 낮은 수준인 99.5%이다.

- Use Cases: Storing secondary backup copies of on-premises data, or data you can recreate
→ 온프레미스 데이터를 2차 백업하거나 재생성 가능한 데이터를 저장하는 데 사용된다.
~~~

### 5. Amazon S3 Glacier Storage Classes
- 이름에서 알 수 있듯이 콜드 스토리지이다.

- Low-cost object storage meant for archiving / backup  
→ 아카이빙과 백업을 위한 저비용 객체 스토리지이다.

- Pricing: price for storage + object retrieval cost  
→ 비용에는 스토리지 비용과 검색 비용이 들어간다.

- Amazon S3 Glacier Instant Retrieval
~~~
- Millisecond retrieval, great for data accessed once a quarter
→ 밀리초 단위로 검색이 가능하다. 백업이지만 밀리초 이내에 액세스해야 하는 경우 적합하다. 예를 들어 분기에 한 번 액세스하는 데이터에 아주 적합하다.

- Minimum storage duration of 90 days
→ 최소 보관 기간이 90일이다. 이는 한 번 스토리지에 저장하면 최소 90일의 보관비용이 청구된다는 의미이다.
 ~~~

- Amazon S3 Glacier Flexible Retrieval (formerly Amazon S3 Glacier)
~~~
- Expedited (1 to 5 minutes), Standard (3 to 5 hours), Bulk (5 to 12 hours) – free
→ 세 가지 옵션이 있다. Expedited는 데이터를 1~5분 이내에 받을 수 있고 Standard는 3~5시간이 소요된다. Bulk는 무료지만 데이터를 돌려받는 데 5~12시간이 소요된다.

- Minimum storage duration of 90 days
→ 최소 보관 기간이 90일이다.
~~~

- Amazon S3 Glacier Deep Archive – for long term storage
~~~
- Standard (12 hours), Bulk (48 hours)
→ 장기 보관을 위한 아카이브이다. Standard는 12시간이, Bulk는 48시간이 걸린다.

- Minimum storage duration of 180 days
→ 오래 걸리긴 하지만 비용이 가장 저렴하다. 그리고 최소 보관 기간도 180일이다.
~~~

- `Glacier`에서 빠른 검색을 수행하기 위해서는 `Expedited retrievals, 신속 검색`을 선택하거나 프로비저닝된 용량을 구매해야 한다.
- 프로비저닝된 용량은 필요할 때 신속한 검색을 위한 검색 용량을 사용할 수 있도록 한다. 데이터 하위 집합에 대한 매우 안정적이고 예측 가능한 액세스가 필요하다면 프로비저닝된 검색 용량 또한 추천된다.

### 6. S3 Intelligent-Tiering
- 알아서 객체를 이동시켜주기 때문에 편하게 스토리지를 관리할 수 있다.

- Small monthly monitoring and auto-tiering fee  
→ 소액의 월별 모니터링 비용과 티어링 비용이 발생한다.

- Moves objects automatically between Access Tiers based on usage  
→ 사용 패턴에 따라 액세스된 티어 간에 객체를 이동시킨다.

- There are no retrieval charges in S3 Intelligent-Tiering  
→ `S3 Intelligent-Tiering`은 검색 비용이 없다.

- Frequent Access tier (automatic): default tier  
→ 자동이고 기본 티어이다.

- Infrequent Access tier (automatic): objects not accessed for 30 days  
→ 30일 동안 액세스하지 않은 객체의 전용 티어이다.

- Archive Instant Access tier (automatic): objects not accessed for 90 days  
→ 자동이지만 90일동안 액세스하지 않은 객체의 전용 티어이다.

- Archive Access tier (optional): configurable from 90 days to 700+ days  
→ 선택 사항이며 90일에서 700일 이상까지 구성할 수 있다.

- Deep Archive Access tier (optional): config. from 180 days to 700+ days  
→ 선택 사항이며 180일에서 700일 이상 액세스하지 않은 객체에 대해 구성할 수 있다.

### 7. Moving between Storage Classes
- You can transition objects between storage classes  
→ 스토리지 클래스 간에 객체를 이동시킬 수 있다. 자신보다 아래에 있는 모든 수준으로 이동이 가능하다.

![image](https://user-images.githubusercontent.com/97398071/235154399-94212fc6-c0e8-4d23-8e73-372b0bb6cc1b.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Moving objects can be automated using a Lifecycle Rules  
→ 객체는 수동으로 옮기거나 수명 주기 규칙을 이용해서 자동화할 수 있다.

### 8. Lifecycle Rules
- Transition Actions – configure objects to transition to another storage class  
→ 전환 작업은 다른 스토리지 클래스로 객체를 전환하도록 구성한다. 예를 들면 아래와 같다.
~~~
- Move objects to Standard IA class 60 days after creation
→ 생성 60일 이후에 Standard IA로 이동하도록 구성한다.

- Move to Glacier for archiving after 6 months
→ 6개월 후에 Glacier에 아카이빙되도록 구성한다.
~~~

- Expiration actions – configure objects to expire (delete) after some time  
→ 만료 작업을 설정하면 일정 시간이 지난 후 객체가 삭제 또는 만료되게 할 수 있다.
~~~
- Access log files can be set to delete after a 365 days
→ 365일 후 액세스 로그 파일을 삭제하도록 구성한다.

- Can be used to delete old versions of files (if versioning is enabled)
→ 버저닝을 활성화한 경우 이전 버전의 파일을 삭제하도록 구성한다.

- Can be used to delete incomplete Multi-Part uploads
→ 완료되지 않은 멀티파트 업로드를 삭제하도록 구성한다.
~~~

- Rules can be created for a certain prefix (example: s3://mybucket/mp3/*)  
→ 특정 접두사를 사용하여 전체 버킷 또는 버킷의 일부 경로에 규칙을 적용할 수 있다.

- Rules can be created for certain objects Tags (example: Department: Finance)  
→ 객체의 태그에 규칙을 적용할 수 있다.

- 추가적인 보호를 제공하기 위해 `MFA` 삭제를 활성화할 수 있다. `MFA` 삭제는 객체를 `Amazon S3` 버킷에서 영구적으로 삭제하기 위해 2차 인증을 요구한다.

### 9. Amazon S3 Analytics – Storage Class Analysis
- Help you decide when to transition objects to the right storage class  
→ 한 클래스에서 다른 클래스로 객체를 전환하는 데에 있어 최적의 기간은 `Amazon S3 Analytics`를 이용해서 결정하면 된다.

- Recommendations for Standard and Standard IA  
→ `Standard`와 `Standard IA`용의 권장 사항을 제공한다.

- Does NOT work for One-Zone IA or Glacier  
→ `One-Zone IA`나 `Glacier`은 해당하지 않는다.

- Report is updated daily  
→ `S3` 버킷에서 `S3 Analytics`를 실행하면 버킷에 대한 권장 사항과 통계를 `CSV` 파일로 제공한다. 이 보고서는 매일 업데이트된다.

- 24 to 48 hours to start seeing data analysis  
→ 해당 데이터의 분석 결과까지 24시간에서 48시간이 소요된다.

- Good first step to put together Lifecycle Rules (or improve them)!  
→ 이 `CSV` 보고서로 수명 주기 규칙을 알아보고 개선 방향을 생각해 볼 수 있다.

- `Management` → `Create lifecycle rule`에서 생성 가능하다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---