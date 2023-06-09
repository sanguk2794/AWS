## AMI
### 1. AMI
- AMI = Amazon Machine Image  
→ `AMI`는 `EC2`의 인스턴스를 통해 만든 이미지를 총칭한다.

- AMI are a customization of an EC2 instance
~~~
- You add your own software, configuration, operating system, monitoring…
→ AMI에 원하는 소프트웨어 설정 파일을 추가하거나 별도의 운영 체제를 설치하거나 모니터링 툴을 추가할 수 있다.

- Faster boot / configuration time because all your software is pre-packaged
→ 부팅 및 설정에 걸리는 시간을 줄일 수 있다. EC2 인스턴스에 설치하고자 하는 모든 소프트웨어를 AMI에 패키징해 둘 수 있기 때문이다.
~~~

- AMI are built for a specific region (and can be copied across regions)  
→ `AMI`를 특정 지역에 구축한 다음 다른 지역으로 복사해서 `AWS`의 글로벌 인프라를 활용할 수도 있다.

- You can launch EC2 instances from:
~~~
- A Public AMI: AWS provided
→ 전체 공개 AMI는 AWS가 기본적으로 제공해 준다. 그 중 아마존 리눅스 AMI는 매우 인기있는 AMI 중 하나이다.

- Your own AMI: you make and maintain them yourself
→ AMI를 자체적으로 생성할 수 있다. 직접 만들면 유지나 관리도 직접 해야한다.

- An AWS Marketplace AMI: an AMI someone else made (and potentially sells)
→ 마켓플레이스 AMI를 통해 EC2 인스턴스를 실행할 수도 있다. 이건 다른 사람이 구축한 이미지를 쓰는 것이다. 보통 구매해야 한다.
→ 개인 또는 기업에서 자체적으로 AMI를 구성해 자신들이 만든 소프트웨어를 넣고 구성까지 마친 다음 마켓플레이스에서 판매하는 것이다.
~~~

### 2. AMI Process (from an EC2 instance)
- Start an EC2 instance and customize it  
→ `EC2` 인스턴스를 원하는대로 설정한다.

- Stop the instance (for data integrity)  
→ 인스턴스를 중지해 데이터 무결성을 확보한다.

- Build an AMI – this will also create EBS snapshots  
→ 중지한 인스턴스를 바탕으로 `AMI`를 구축한다. 이 과정에서 `EBS` 스냅샷이 생성된다.

- Launch instances from other AMIs  
→ 구축한 `AMI`로 인스턴스를 실행할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/232325616-e8ec78d0-a6b2-4cf2-86e7-fa383a77e55c.png)

- `EC2` → `Instances` → `Actions` → `Image and templates` → `Create image`를 선택하면 정지된 인스턴스로부터 `AMI`를 생성할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/232325843-59020359-c45f-4986-ae66-d908a3fac026.png)

- 구축한 `AMI`로 인스턴스를 실행할 수 있다.

![image](https://user-images.githubusercontent.com/97398071/232326104-7afa9506-f07f-440f-8cda-2b74868c78f5.png)

### 3. AMI vs. EBS Snapshot
- 기본적으로 `EBS`를 백업한다는 점에서 `AMI` 와 `Snapshot`은 동일하다.
- `AMI` 는 `EC2` 인스턴스에 연결되어 있는 모든 `EBS Volume`과 `OS`를 동시에 백업한다. 하지만, 스냅샷은 사용자가 선택한 `EBS Volume`의 데이터만을 백업한다.
- `AMI`는 바로 `EC2` 인스턴스를 생성할 수 있다. 하지만, 스냅샷은 해당 스냅샷을 이용해 `AMI`를 생성하는 단계를 거쳐야한다.
- `OS` 와 별개로 데이터만 백업하고 싶은 경우에 스냅샷을 사용하면 된다.

---
#### ▶ Reference
- [[AWS] 📚 AMI / Snapshot 개념 & 백업 사용법 💯 정리](https://inpa.tistory.com/entry/AWS-📚-AMI-Snapshot-개념-백업-사용법-💯-정리)
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---