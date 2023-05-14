## Caching Strategies
### 1. Caching Strategies
- `CloudFront`
~~~
- CloudFront는 엣지에서 캐싱을 한다. 사용자와 최대한 가까이에서 캐싱을 한다는 뜻이다. CloudFront에서 캐싱을 허용하면 캐시를 히트하는 모든 사용자들이 즉시 응답을 빠르게 얻게 된다. 
- 하지만 엣지에 있기 때문에 백엔드에서 변화가 일어났을 때에 대한 대응이 필요하다. 캐시가 최신인지 확인하려면 TTL을 사용할 수 있다.
~~~

- `API Gateway`
~~~
- API Gateway 또한 캐싱이 가능하다. API Gateway는 리전 서비스라서 API Gateway에서 캐시를 사용할 경우 캐시도 리전 스코프가 된다.
- 클라이언트와 API Gateway 사이에 네트워크 라인이 형성되어 캐시가 히트된다. 
~~~

- `Database Cache`
~~~
- Redis, Memcached, DAX 등을 자주 발생하는 쿼리나 복잡한 쿼리를 공유 캐시에 저장할 수 있다.
- 데이터베이스에 가해지는 압력을 줄이고 읽기 용량을 늘릴 수 있다.
~~~

- `Amazon S3`에는 캐싱 기능이 없다. 알아두는 것이 좋다.

![image](https://github.com/sanguk2794/AWS/assets/97398071/bcad4230-2fb8-43dc-a382-bbb1bda4f609)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 뒤로 갈수록 비용과 지연 시간이 늘어난다. 

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---