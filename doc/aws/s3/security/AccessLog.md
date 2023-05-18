## Access Log 
### 1. S3 Access Logs
- For audit purpose, you may want to log all access to S3 buckets  
→ 감사 목적으로 `S3` 버킷에 대한 모든 액세스를 기록할 수 있다.

- Any request made to S3, from any account, authorized or denied, will be logged into another S3 bucket  
→ `S3`로 보낸 모든 요청은 승인 또는 거부 여부와 상관없이 다른 `S3` 버킷에 파일로 기록된다.

- That data can be analyzed using data analysis tools…  
→ 해당 데이터는 `Amazon Athena` 등 데이터 분석 도구를 사용해 분석할 수 있다.

- The target logging bucket must be in the same AWS region  
→ 대상 로깅 버킷은 같은 `AWS` 리전에 있어야 한다.

- The log format is at: https://docs.aws.amazon.com/AmazonS3/latest/dev/LogFormat.html  
→ 로그 형식은 정해져 있다. 이에 대해서는 [Amazon S3 서버 액세스 로그 형식](https://docs.aws.amazon.com/AmazonS3/latest/dev/LogFormat.html)에서 확인할 수 있다.

- `Amazon S3` → `Bucket` → 버킷 선택 → `Properties` → `Server access logging`에서 활성화 설정이 가능하다.

![image](https://user-images.githubusercontent.com/97398071/235289834-f1bbc074-7350-495f-b9b6-08dd8b11c6cd.png)

- 로그는 누가 액세스했는지, 어떤 버킷인지, 액세스 시간 등의 정보를 제공한다.

### 2. S3 Access Logs: Warning
- Do not set your logging bucket to be the monitored bucket  
→ 로깅 버킷을 모니터링하는 버킷과 동일하게 설정하면 안 된다.

- It will create a logging loop, and your bucket will grow exponentially  
→ 동일하게 설정하면 로깅 루프가 생성되고 무한 반복되어 버킷의 크기가 기하급수적으로 증가하게 된다.

![image](https://user-images.githubusercontent.com/97398071/235289716-747cdd72-337c-4f75-b3a3-3ec824bc27db.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
