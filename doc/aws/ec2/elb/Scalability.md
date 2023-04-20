## Scalability & High Availability
### 1. Scalability & High Availability
- Scalability means that an application / system can handle greater loads by adapting.  
→ 확장성은 애플리케이션 시스템이 조정을 통해 더 많은 양을 처리할 수 있다는 의미이다.

- There are two kinds of scalability:  
→ 확장성에는 수직 확장성과 탄력성이라고 불리기도 하는 수평 확장성 두 개의 개념이 포함되어 있다.
~~~
- Vertical Scalability
- Horizontal Scalability (= elasticity)
~~~

- Scalability is linked but different to High Availability  
→ 확장성과 고가용성은 연관된 개념이지만 서로 다른 의미를 가지고 있다.

#### 1. Vertical Scalability
- Vertically scalability means increasing the size of the instance  
→ 수직 확장성은 인스턴스의 크기를 확장하는 것을 의미한다.
~~~
- For example, your application runs on a t2.micro
- Scaling that application vertically means running it on a t2.large
~~~

- Vertical scalability is very common for non distributed systems, such as a database.  
→ 수직 확장성은 데이터베이스와 같은 분산되지 않은 시스템에서 흔히 사용된다. `RDS`나 `ElastiCache` 등의 데이터베이스에서 쉽게 찾아볼 수 있다.

- RDS, ElastiCache are services that can scale vertically.  
→ 하지만 일반적으로 확장 정도에 한계가 있다. 하드웨어의 성능 제약이 존재하기 때문이다.

#### 2. Horizontal Scalability
- Horizontal Scalability means increasing the number of instances / systems for your application  
→ 수평 확장성은 애플리케이션에서 인스턴스나 시스템의 수를 늘리는 것을 의미한다.
 
- Horizontal scaling implies distributed systems.  
→ 수평 확장을 했다는 것은 분배 시스템이 존재한다는 것을 의미한다.

- This is very common for web applications / modern applications  
→ 웹이나 현대적 애플리케이션은 대부분 이 분배 시스템으로 이루어져 있다.

#### 3. High Availability 
- High availability means running your application / system in at least 2 data centers (== Availability Zones)
→ 고가용성은 애플리케이션 시스템을 적어도 둘 이상의 `AZ`나 데이터 센터에서 가동 중이라는 것을 의미한다.

- The goal of high availability is to survive a data center loss
→ 고가용성의 목적은 일부 데이터 센터의 손실이 발생하더라도 계속 작동이 가능하도록 하는 것에 있다.

#### 4. High Availability & Scalability For EC2
- Vertical Scaling: Increase instance size (= scale up / down)  
→ 수직 확장은 인스턴스의 크기를 늘리거나 줄이는 것을 말한다.
~~~
- From: t2.nano - 0.5G of RAM, 1 vCPU
- To: u-12tb1.metal – 12.3 TB of RAM, 448 vCPUs
~~~

- Horizontal Scaling: Increase number of instances (= scale out / in)  
→ 수평 확장은 인스턴스의 숫자를 늘리거나 줄이는 것을 말하며, 이 때에는 스케일링 그룹과 로드 밸런서가 함께 사용된다.
`AWS` 용어로 인스턴스 수를 늘리는 것을 `Scale Out`, 줄이는 것을 `Scale In`이라고 한다.
~~~
- Auto Scaling Group
- Load Balancer
~~~

- High Availability: Run instances for the same application across multi AZ  
→ 동일 애플리케이션의 동일 인스턴스를 다수의 `AZ`에 걸쳐 실행하는 것을 의미한다. 다중 `AZ`가 활성화된 다른 스케일링 그룹과 로드밸런서가 사용된다.
~~~
- Auto Scaling Group multi AZ
- Load Balancer multi AZ
~~~

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
