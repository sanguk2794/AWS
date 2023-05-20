## EC2 Instances Purchasing Options
### 1. EC2 Instances Purchasing Options
- On-Demand Instances – short workload, predictable pricing, pay by second
~~~
- 리눅스 또는 윈도우는 초 단위, 다른 모든 운영체제는 1시간 단위로 청구가 이루어진다. 따라서 비용 예측이 쉬우면서 비용이 가장 많이 든다. 
- 바로 지불할 금액은 없고 장기적인 약정도 필요없다. 
- 단기적이고 중단 없는 워크로드가 필요할 때나 애플리케이션의 거동을 예측할 수 없을 때 가장 추천되는 구매 옵션이다.
~~~

- Reserved Instance (1 & 3 years)
~~~
- 예약 인스턴스의 구매에는 특정한 인스턴스의 속성의 예약이 필요하다.
- 이 때 지정해야 하는 속성에는 인스턴스의 타입, 리전, Tenancy, 운영체제 등이 포함된다. 
- 예약 기간(1년, 3년), 선결제 여부를 선택해야 하며 선택에 따라 할인폭이 달라진다. 
- 사용량이 일정한 애플리케이션을 오래 사용할 것이 확실하다면 예약 인스턴스를 사용하는 것이 가장 효율적이다. 
- 사용하지 않은 표준 예약 인스턴스는 나중에 예약 인스턴스 마켓플레이스에서 판매할 수 있다.
~~~

- Convertible Reserved Instance
~~~
- 전환형 예약 인스턴스는 특별한 유형의 예약 인스턴스이다.
- 한 개 이상의 전환형 예약 인스턴스를 운영 체제, 테넌트를 비롯하여 구성이 다른 다른 인스턴스 패밀리의 전환형 예약 인스턴스와 교환할 수 있다.
- 교환 횟수에 제한이 없고 유연성이 더 크기 때문에 예약 인스턴스와 비교했을 때 할인율은 더 낮다.
- 표준 예약 인스턴스만 마켓플레이스에서 판매가 가능하다. 전환형 예약 인스턴스는 판매가 불가능하다.
~~~

- Savings Plans (1 & 3 years) – commitment to an amount of usage, long workload
~~~
- 장기간 사용으로 할인을 받을 수 있는 플랜이다. 
- RI와 같은 비용 절감 효과를 제공하지만 인스턴스 타입 변경시 별도 작업 없이 RI보다 유연하게 할인을 적용받을 수 있다.

- 예약 기간(1년, 3년)에 더해 컴퓨팅 사용량에 대한 약정이 필요하다. 사용량이 한도를 넘으면 절약 플랜은 온디맨드 가격으로 청구가 되므로 주의가 필요하다.
→ 예를 들어 한 시간에 $10의 컴퓨팅 사용량을 약정하면 컴퓨팅 사용량에 대해 $10까지 Savings Plans 가격이 청구되고 약정 이외의 사용량은 온디맨드 요금이 청구된다.
~~~

- Spot Instances – short workloads, cheap, can lose instances (less reliable)
~~~
- 온디맨드와 비교해 높은 폭의 할인을 받을 수 있는 플랜이다.
- 만일 스팟 가격이 사용자가 정의한 지불 용의 가격을 넘게 된다면 언제라도 그 인스턴스들이 손실될 수 있어서 신뢰성이 낮다.
- AWS에서 가장 비용 효율적인 인스턴스이다. 인스턴스가 고장에 대한 회복력이 있을 때 아주 유용하게 사용할 수 있다.
- 배치 작업, 데이터 분석, 실패해도 복원력이 있는 워크로드에 주로 사용된다.

- 지불 용의 최고가보다 스팟 인스턴스의 가격이 높아질 때를 대비하여 선택할 수 있는 옵션으로 stop과 terminate가 있다.
→ stop의 경우 스팟의 가격이 지불 용의 최고가보다 낮아지면 다시 재기동을 수행하며, terminate의 경우 인스턴스를 완전히 종료한다.
~~~

- Dedicated Hosts – book an entire physical server, control instance placement  
→ 전용 호스트는 물리적 서버 전체를 예약해서 인스턴스 배치를 제어할 수 있다.

- Dedicated Instances – no other customers will share your hardware  
→ 다른 고객이 하드웨어를 공유하지 않는다. 자신만의 하드웨어를 소유할 수 있다는게 장점이다.

- Capacity Reservations – reserve capacity in a specific AZ for any duration
~~~
- 용량 예약을 이용하면 원하는 기간 동안 특정한 AZ에 용량을 예약할 수 있고, 필요할 때마다 그 용량에 접근할 수 있다. 
- 기간 약정이 없어 언제라도 용량을 예약하고 취소할 수 있다. 청구 할인도 없는 용량을 예약하는 것이 유일한 목적인 플랜이다. 
- 용량 예약을 하며 청구 할인을 받기 위해서는 예약 인스턴스 또는 절약 플랜과 결합시켜야 한다.
~~~

![タイトルなし](https://user-images.githubusercontent.com/97398071/232197280-294d4f07-9b90-4253-93ca-c4eb1d2dfa67.png)

#### 1. Workload
- 워크로드는 고객 대면 애플리케이션이나 백엔드 프로세스 같이 비즈니스 가치를 창출하는 리소스 및 코드의 모음을 말한다.

#### 2. Tenancy
- `Tenancy`는 소프트웨어 인스턴스에 대해 공통이 되는 특정 접근 권한을 공유하는 사용자들을 말한다.

### 2. 전용 인스턴스와 전용 호스트의 차이
- 성능이나 보안상의 물리적 차이는 없으나 전용 호스팅의 경우 인스턴스가 물리적 서버에 배치되는 방법에 대한 추가적인 제어와 가시성을 제공하고 시간이 지나도 계속 같은 물리적 서버에 인스턴스를 배포할 수 있다. 이 부분이 전용 호스팅과 전용 인스턴스의 중요한 차이점이다.
- 즉, 전용 호스팅을 사용하면 기존 서버 한정 소프트웨어 라이선스를 사용하고 기업 규정 준수 및 규제 요구 사항을 해결할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/232194586-44a641d5-1781-4658-85fa-71fc944813ed.png)

출처 → [Amazon EC2 전용 호스트](https://aws.amazon.com/ko/ec2/dedicated-hosts/)

### 3. How to terminate Spot Instances?
- 스팟 인스턴스 생성시에는 원하는 인스턴스의 개수, 지불 의사가 있는 인스턴스 최고 가격, `AMI` 등 요구되는 사양, 그리고 요청의 유효 기간, 요청의 유형 설정이 필요하다.

- 그 중 요청의 유형에는 일회성 요청과 지속적 요청이 있다.
~~~
- 일회성 요청의 경우에는 스팟 요청이 이행되는 즉시 인스턴스가 시작되고 스팟 요청은 사라진다.

- 지속적 요청의 경우에는 설정한 스팟 요청의 Valid from부터 Valid until을 설정한다. 유효 기간 동안에는 스팟 인스턴스가 작동을 중지했을 경우 스팟 요청을 지속적으로 다시 전달한다. 
→ 요청이 검증되면 스팟 인스턴스가 재시작된다.
~~~

- 스팟 요청을 취소한다고 기존 실행중이던 인스턴스들이 종료되는 것은 아니다. 또, 스팟 인스턴스를 종료한다고 해서 스팟 요청이 사라지거나 하지 않는다.
- 그러므로 스팟 인스턴스를 영구히 종료하고 재실행되는 일이 없도록 하기 위해서는 스팟 요청을 취소한 다음 스팟 인스턴스를 종료할 필요가 있다.
- 순서를 반대로 수행할 경우 스팟 요청이 다시 작동해서 인스턴스를 재기동시킨다.

### 4. Spot Fleets
- 비용을 극한으로 절감하기 위한 방법이다.

- Spot Fleets = set of Spot Instances + (optional) On-Demand Instances  
→ 한 세트의 스팟 인스턴스에 온디맨드 인스턴스를 선택적으로 조합해 사용한다. `Fleet`은 집합이라는 의미이다.

- 스팟 플릿은 사용자의 요구 사항을 충족하는 스팟 용량 풀을 선택하고 플릿에 대한 목표 용량을 충족하는 스팟 인스턴스를 시작한다.
→ 기본적으로 스팟 집합은 플릿에서 스팟 인스턴스가 종료된 후 교체 인스턴스를 시작하여 목표 용량을 유지하도록 설정되어 있다.

- The Spot Fleet will try to meet the target capacity with price constraints  
→ 스팟 플릿은 사용자의 요구 사항을 충족하는 스팟 용량 풀을 선택하고 플릿에 대한 목표 용량을 충족하는 스팟 인스턴스를 선택해 실행시켜 준다. 기본적으로 스팟 집합은 플릿에서 스팟 인스턴스가 종료된 후 교체 인스턴스를 시작하여 목표 용량을 유지하도록 설정되어 있다.

- 만약 스팟 플릿이 정해진 예산 또는 원하는 용량에 달한 경우에는 인스턴스 실행을 멈춘다.

- Strategies to allocate Spot Instances:
~~~
- lowestPrice: from the pool with the lowest price (cost optimization, short workload)
→ 스팟 플릿이 가장 적은 비용을 가진 풀에서 인스턴스를 실행해 준다. 비용 최적화가 가능하며 아주 짧은 워크로드가 있을때 적합한 옵션이다.
스팟 플릿이 자동적으로 최저 가격의 스팟 인스턴스를 요청해주므로 추가적 비용 절감이 가능하다.

- diversified: distributed across all pools (great for availability, long workloads)
→ 정의한 기존의 모든 풀에 걸쳐 분산된다. 한 풀이 중단되더라도 다른 풀이 활성되어 있기에 긴 워크로드에 적합하고 가용성이 뛰어나다. 

- capacityOptimized: pool with the optimal capacity for the number of instances
→ 인스턴스 개수에 따라서 최적 용량으로 실행이 되고 적절한 풀을 찾아 주는 옵션이다. 
~~~

#### 1. 스팟 인스턴스와 스팟 플릿의 차이점
- 스팟 인스턴스는 단순히 사용하고자 하는 인스턴스의 유형과 가용 영역을 사용자가 지정한다.
- 스팟 플릿은 조건에 따라 인스턴스 유형과 가용 영역을 자동으로 선택해준다. 그러므로 스팟 플릿을 이용하면 인스턴스 유형이 다양하게 정의될 수 있다.

#### 2. Create Spot Request
- `EC2` → `Spot Request`

![image](https://user-images.githubusercontent.com/97398071/232196181-fd4bf1c1-55b3-4ff4-bd25-e774e518c606.png)

- `Pricing history`  
→ 현재 스팟 인스턴스의 요금 내역을 확인할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/232196239-705bf034-0f1a-4022-abd0-5d1dfb8e4d7b.png)

- `EC2` → `Spot Request` → `Request Spot Instance`  
→ 스팟 리퀘스트를 생성할 수 있다. 조건에 맞다면 스팟 인스턴스가 생성된다.

- 그냥 스팟 인스턴스를 생성하고자 한다면 `EC2` → `Instances` → `Launch an instance` → `Details`에서 스팟 인스턴스 요청을 체크하고 상세 설정하는 것만으로 충분하다.

![image](https://user-images.githubusercontent.com/97398071/232196821-a243c105-e43d-4292-9acf-27e3f9198158.png)

---
#### ▶ Reference
- [워크로드](https://docs.aws.amazon.com/ko_kr/wellarchitected/latest/userguide/workloads.html)
- [AWS 신규 할인 모델, Savings Plans는 무엇일까요?](https://www.bespinglobal.com/aws-discount-model-saving-plan/)
- [Amazon EC2 전용 호스트](https://aws.amazon.com/ko/ec2/dedicated-hosts/)
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---