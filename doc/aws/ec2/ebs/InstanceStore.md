## EC2 Instance Store
### 1. EC2 Instance Store
- EBS volumes are network drives with good but “limited” performance  
→ `EC2` 인스턴스에 네트워크 드라이브를 연결하는 것은 그 성능이 제한된다. 이 제한된다는 것은 성능 자체가 떨어진다는 것은 아니다.

- If you need a high-performance hardware disk, use EC2 Instance Store  
→ 보다 높은 성능을 원한다면 이때는 `EC2` 인스턴스에 연결된 하드웨어 디스크의 성능이 향상되어야 한다. `EC2` 인스턴스는 가상 머신이지만 실제로는 하드웨어 서버에 연결되어 있기 때문이다. 이럴 때 사용하는 `EC2` 인스턴스를 `EC2` 인스턴스 스토어라고 부르며, 이는 해당하는 물리적 서버에 연결된 하드웨어 드라이브를 가리킨다.

- EC2 Instance Store lose their storage if they’re stopped (ephemeral)  
→ `EC2` 인스턴스 스토어를 중지 또는 종료하면 해당 스토리지 또한 손실되어 장기적으로 데이터를 보관할만한 장소가 될 수 없다.
이와 같은 이유로 인해 인스턴스 스토어는 임시 스토리지라고도 불린다. 장기 스토리지의 경우에는 `EBS`가 적합하다.

- Good for buffer / cache / scratch data / temporary content  
→ 버퍼나 캐시, 스크래치 데이터, 임시 컨텐츠 데이터를 보관할 때 사용할 수 있다.

- Backups and Replication are your responsibility  
→ 그러므로 `EC2` 인스턴스 스토어를 사용할 때에는 필요에 따라 데이터를 백업해 두거나 복제해 두어야 한다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---