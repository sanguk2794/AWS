## Transferring large amount of data into AWS
### 1. Transferring large amount of data into AWS
- Example: transfer 200 TB of data in the cloud. We have a 100 Mbps internet connection.  
→ 예를 들어 200TB의 데이터를 클라우드로 옮기고 싶다고 가정한다. 인터넷 연결 속도는 `100Mbps`이다.

- Over the internet / Site-to-Site VPN:
~~~
- 먼저 공용 인터넷을 사용하는 방법이 있다. 또 공용 인터넷을 사용해 사이트 간 VPN을 설치하는 방법도 있다.
- Immediate to setup
→ 설치가 빠르고 바로 연결이 가능하다.

- Will take 200(TB)*1000(GB)*1000(MB)*8(Mb)/100 Mbps = 16,000,000s = 185d
→ 엄청나게 오래 걸릴 수 있다. 
~~~

- Over direct connect 1Gbps:
~~~
- Long for the one-time setup (over a month)
→ 만약 Direct Connect를 통해 전송한다면 초기 설치에 시간이 오래 걸리는 것을 감안해야 한다.

- Will take 200(TB)*1000(GB)*8(Gb)/1 Gbps = 1,600,000s = 18.5d
→ 연결이 만들어진 후에는 공용 인터넷을 사용하는 것보다 훨씬 빠르다. 하지만 충분히 긴 시간이다.
~~~

- Over Snowball:
~~~
- Will take 2 to 3 snowballs in parallel
→ 2-3개 정도의 스노우볼이 필요하다.

- Takes about 1 week for the end-to-end transfer
→ 종단 간 전송에 약 일주일이 소요된다. 이는 배송 시간을 포함한 시간이다.

- Can be combined with DMS
→ DMS와 결합하여 나머지 데이터를 전송할 수 있다
~~~

- For on-going replication / transfers: Site-to-Site VPN or DX with DMS or DataSync  
→ `Snowball`은 일회성 전송이다. 지속적 복제에 대해서는 `Site-to-Site VPN` 등의 기술을 사용할 수 있다. 지속적 복제에는 전송해야 할 데이터 양이 크지 않기 때문이다.

- 지속적 복제에는 `Direct Connect`, `DMS`, `DataSync` 등의 서비스를 사용한다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---