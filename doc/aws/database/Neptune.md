## Amazon Neptune
### 1. Amazon Neptune
- Fully managed graph database  
→ `Neptune`은 완전 관리형 그래프 데이터베이스이다. 시험에서 그래프 데이터베이스와 관련된 것이 나온다면 `Neptune`만 생각하면 된다.

- A popular graph dataset would be a social network
~~~
- 그래프 데이터 셋의 대표적인 예는 소셜 네트워크이다. 아래 모든 것들이 연결되어 그래프를 만든다.

- Users have friends 
→ 친구가 있다.

- Posts have comments 
→ 게시물에는 댓글이 있다.

- Comments have likes from users 
→ 댓글에는 좋아요가 있다.

- Users share and like posts… 
→ 사용자는 좋아요 게시물을 공유한다.
~~~

![image](https://user-images.githubusercontent.com/97398071/235919471-2f166bba-de24-4f20-9636-5028703b39bb.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- Highly available across 3 AZ, with up to 15 read replicas  
→ `Neptune`은 3개의 `AZ`에 걸쳐 있으며 최대 15개의 읽기 전용 복제본을 둘 수 있다.

- Build and run applications working with highly connected datasets – optimized for these complex and hard queries  
→ `Neptune`은 소셜 네트워크처럼 고도로 연결된 데이터셋을 사용하는 애플리케이션을 구축하고 실행하는데 사용된다. `Neptune`은 그래프 데이터 셋에서 복잡하고 어려운 쿼리를 실행하기에 최적화되어 있다.

- Can store up to billions of relations and query the graph with milliseconds latency  
→ 데이터베이스에 최대 수십억 개의 관계를 저장할 수 있고, 그래프를 쿼리할 때의 지연시간은 밀리초이다.

- Highly available with replications across multiple AZs   
→ 여러 가용 영역에 걸친 애플리케이션에서도 매우 가용성이 높다.

- Great for knowledge graphs (Wikipedia), fraud detection, recommendation engines, social networking  
→ 지식 그래프를 저장하는데도 뛰어난 성능을 보여준다. 예를 들어 모든 위키피디아 기사들은 서로 연결되어 있으므로 위키피디아 데이터베이스는 지식 그래프이다. 이외에도 사기 탐지, 추천 엔진, 소셜 네트워킹도 그렇다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---