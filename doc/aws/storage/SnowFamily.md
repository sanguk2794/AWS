## AWS Snow Family
### 1. AWS Snow Family
- Highly-secure, portable devices to collect and process data at the edge, and migrate data into and out of AWS  
→ `Snow Family`는 보안성이 뛰어난 휴대용 장치의 모음으로 아래의 두 가지 경우에 사용된다.
~~~
- Data migration: Snowcone, Snowball Edge, Snowmobile
→ AWS 안팎으로 데이터 마이그레이션할 때 사용된다. 

- Edge computing: Snowcone, Snowball Edge,
→ 엣지 컴퓨팅에 사용된다.
~~~

### 2. Data Migrations with AWS Snow Family
- 네트워크를 통해 많은 데이터를 전송하려면 아주 오랜 시간이 걸린다. 
~~~
- Limited connectivity

- Limited bandwidth
→ 제한되는 대역폭

- High network cost
→ 네트워크 전송으로 발생하는 높은 비용

- Shared bandwidth (can’t maximize the line)
→ 대역폭 공유

- Connection stability
→ 연결이 안정적이지 않을 경우 재시도가 필요할 수 있음
~~~

- AWS Snow Family: offline devices to perform data migrations If it takes more than a week to transfer over the network, use Snowball devices!  
→ `AWS Snow` 제품군은 위와 같은 문제를 해결하기 위해 사용되는 오프라인에서 데이터 마이그레이션을 실행하는 장치이다. 네트워크를 통한 전송에 일주일이 넘게 걸린다면 `Snowball` 장치를 활용하는 것을 고려하는 것이 좋다.

- `AWS`가 우편으로 물리적 장치를 보내주면 거기에 데이터를 저장해 다시 `AWS`로 배송한다. `AWS` 측에서 물리적 장치를 자체적인 인프라에 연결하여 `import`를 수행한다.
 
- `Amazon S3`로 직접 업로드하기 위해서는 클라이언트가 `Amazon S3`로 데이터를 전송해야 한다.

![image](https://user-images.githubusercontent.com/97398071/235310304-9bfbaa03-60c1-4bcb-ab1c-e23bd68f6e0e.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- `S3` 버킷 데이터를 저장한 `Snow` 제품군을 배송받는 `export` 또한 가능하다.

![image](https://user-images.githubusercontent.com/97398071/235312330-39af99be-fd05-4e6e-af44-cebeff3ddd69.png)

- 이 후에 다룰 데이터 마이그레이션의 세 가지 방법을 정리하면 다음과 같다.

![image](https://user-images.githubusercontent.com/97398071/235311139-d1d7cfe8-6ece-4411-b65c-62291aefa5d5.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

#### 1. Snowball Edge (for data transfers)
- Physical data transport solution: move TBs or PBs of data in or out of AWS  
→ `Snowball Edge`는 `TB` 혹은 `PB` 크기의 데이터를 `AWS` 안팎으로 전송할 수 있다.

- Alternative to moving data over the network (and paying network fees)  
→ 네트워크를 대신해서 데이터를 옮길 수 있다.

- Pay per data transfer job  
→ 데이터 전송 건수마다 비용이 청구된다.

- `Snowball Edge`에는 두 가지 옵션이 있다.
~~~
- Snowball Edge Storage Optimized - 80 TB of HDD capacity for block volume and S3 compatible object storage
→ 블록 볼륨으로 사용할 수 있도록 80TB 하드웨어 디스크 용량을 제공하거나 S3 호환 객체 스토리지를 제공한다. 데이터 마이그레이션에 사용되는 경우가 많다.

- Snowball Edge Compute Optimized - 42 TB of HDD capacity for block volume and S3 compatible object storage
→ Snowball Edge Compute Optimized는 42TB의 HDD 공간을 제공한다. 엣지 컴퓨팅에 사용되는 경우가 .
~~~

![image](https://user-images.githubusercontent.com/97398071/235312411-29b69e38-1d5c-46ae-8e70-49e3c60028a7.png)

- Use cases: large data cloud migrations, DC decommission, disaster recovery  
→ `Snowball Edge`는 데이터 센터 폐쇄 또는 데이터 백업을 통한 재해 복구를 위해 대량의 데이터를 클라우드에 마이그레이션하는 경우에 주로 사용된다. 

- 암호화를 지원한다.

- 수백 `TB`의 데이터를 `Amazon S3`로 이전한 후, `EC2` 인스턴스 플릿을 사용해 처리해야 합니다. 광대역은 `1Gbit/초`입니다. 여러분은 데이터를 더 빠르게 이전하고, 가능하면 전송 중에 데이터를 처리했으면 합니다. 어떤 방법을 추천할 수 있을까요?  
→ `Snowball Edge` 사용하기

- 여러분은 수백 `TB`의 데이터를 `AWS S3`로 최대한 빨리 이전시켜야 합니다. 여러분의 네트워크 대역폭을 사용해보려 했으나, 업로드 프로세스가 완료되기까지 약 3주가 소요됩니다. 이런 경우 어떤 접근법이 권장될까요?  
→ `Snowball Edge` 사용하기

#### 2. AWS Snowcone
- Small, portable computing, anywhere, rugged & secure, withstands harsh environments  
→ `Snowball Edge`보다 훨씬 작은 어디서나 컴퓨팅 가능한 작은 휴대용 장치이다. 견고하고 안전하다. 심지어 사막, 물 속 등 가혹한 환경을 버틸 수 있다.

- Light (4.5 pounds, 2.1 kg) - Device used for edge computing, storage, and data transfer  
→ 데이터 전송과 엣지 컴퓨팅에 사용된다.

- 8 TBs of usable storage  
→ `8TB`를 저장할 수 있다. `Snowball Edge Storage Optimized`와 비교하면 10배 적은 용량이다.

- Use Snowcone where Snowball does not fit (space-constrained environment)  
→ `Snowball Edge`의 사용이 불가능할 때 사용할 수 있다. 예를 들면 공간을 제약받는 환경이다. 심지어 드론 위에 설치할 수도 있다.

- Must provide your own battery / cables  
→ 배터리와 케이블은 직접 준비해야 한다.

- Can be sent back to AWS offline, or connect it to internet and use AWS DataSync to send data  
→ 오프라인으로 데이터를 전송할 수 있다. `AWS DataSync`를 사용하면 네트워크 상에서도 데이터를 전송할 수도 있다.

#### 3. AWS Snowmobile
- Transfer exabytes of data (1 EB = 1,000 PB = 1,000,000 TBs)  
→ 실제 트럭이다. `AWS Snowmobile`이 전송하는 데이터는 `EB, 엑사바이트`에 달한다.

- Each Snowmobile has 100 PB of capacity (use multiple in parallel)  
→ `AWS Snowmobile`의 용량은 `100PB`이다.

- High security: temperature controlled, GPS, 24/7 video surveillance  
→ 온도 조절, `GPS` 추적, 비디오 감시 기능이 탑재되어 있는 보안성이 뛰어난 트럭이다.

- Better than Snowball if you transfer more than 10 PB  
→ `10PB` 이상의 데이터를 전송할 생각이라면 `Snowball`보다 좋은 선택이 될 수 있다.

#### 4. Snow Family – Usage Process
- Request Snowball devices from the AWS console for delivery  
→ 콘솔에서 장치를 요청한다.

- Install the snowball client / AWS OpsHub on your servers  
→ `snowball` 클라이언트 또는 `AWS OpsHub`를 설치한다.

- Connect the snowball to your servers and copy files using the client  
→ `snowball`을 서버에 연결하고 클라이언트를 사용해서 파일을 복사한다.

- Ship back the device when you’re done (goes to the right AWS facility)  
→ 준비가 끝나고 장치를 `AWS`에 배송하면 올바른 `AWS` 시설로 바로 옮겨진다.

- Data will be loaded into an S3 bucket  
→ `S3` 버킷에 해당 데이터가 로드된다.

- Snowball is completely wiped  
→ 가장 높은 보안 조치에 따라 `Snowball`에 담았던 데이터는 전부 지워진다.

#### 5. 데이터 마이그레이션
데이터를 한 위치에서 다른 위치로, 한 형식에서 다른 형식으로 또는 한 애플리케이션에서 다른 애플리케이션으로 이동하는 프로세스이다.

### 3. Edge Computing
#### 1. What is Edge Computing?
- 엣지 컴퓨팅은 `Snow` 제품군의 두 번째 사용 사례이다. 로컬 컴퓨팅 및 스토리지 워크로드를 수행하기 위해 사용된다.

- Process data while it’s being created on an edge location  
→ 엣지 컴퓨팅은 사용자 또는 데이터 소스의 물리적인 위치 근처에서 컴퓨팅을 수행하는 것을 말한다. 실시간에 가까운 빠른 분석과 응답이 가능해진다.
~~~
- A truck on the road, a ship on the sea, a mining station underground...
→ 도로 위의 트럭이나 바다 위의 배, 지하의 광산 등 인터넷이 연결되어 있지 않거나 클라우드에서 멀리 있는 곳에서 자주 사용된다.
~~~

- These locations may have
~~~
- Limited / no internet access
→ 인터넷 액세스가 없는 곳

- Limited / no easy access to computing power
→ 컴퓨팅을 할 수 없는 곳
~~~

- We setup a Snowball Edge / Snowcone device to do edge computing  
→ 이런 장소에서 컴퓨팅이나 데이터 처리를 해야 할 경우에 필요한 것이 엣지 컴퓨팅이다. `Snowball Edge`나 `Snowcone`을 주문해서 엣지 컴퓨팅을 시작할 수 있다. 

- Use cases of Edge Computing:
~~~
- Preprocess data
→ 데이터의 전처리를 수행할 때

- Machine learning at the edge
→ 클라우드로 보내지 않고 엣지에서 머신 러닝을 수행할 때

- Transcoding media streams
→ 미디어 트랜스코딩을 수행할 때
~~~

- Eventually (if need be) we can ship back the device to AWS (for transferring data for example)  
→ 최종적으로 데이터를 `AWS`로 재전송해야 하는 경우 `Snowcone`이나 `Snowball Edge` 장치를 보내면 된다. 

#### 2. Snow Family – Edge Computing
- Snowcone (smaller)
~~~
- 2 CPUs, 4 GB of memory, wired or wireless access
→ 4GB 메모리와 Wifi를 가지고 있다.

- USB-C power using a cord or the optional battery
→ USB-C 전원을 사용할 수 있다.
~~~

- Snowball Edge – Compute Optimized
~~~
- 52 vCPUs, 208 GiB of RAM
→ Compute Optimized는 52개의 CPU와 200GB의 램으로 이루어진다. GPU를 선택할 수 있다.

- Optional GPU (useful for video processing or machine learning)
→ 영상 처리나 머신 러닝에 사용된다.

- 42 TB usable storage
→ 사용 가능한 스토리지의 크기는 42TB이다.
~~~

- Snowball Edge – Storage Optimized
~~~
- Up to 40 vCPUs, 80 GiB of RAM
→ 40개의 CPU와 80GB 램으로 이루어진다.

- Object storage clustering available
→ 객체 스토리지 클러스터링이 가능하다. 
~~~

- All: Can run EC2 Instances & AWS Lambda functions (using AWS IoT Greengrass)  
→ 모든 장치들은 내부 `EC2` 인스턴스를 기동시키거나 람다 함수를 실행할 수 있다. 람다 함수는 `AWS IoT Greengrass` 서비스를 통해서만 사용 가능하다.

- Long-term deployment options: 1 and 3 years discounted pricing  
→ 장기 배포 옵션을 선택할 수 있다. 장치를 장기간 빌리면 가격 할인을 받을 수 있다.

### 4. AWS OpsHub
- Historically, to use Snow Family devices, you needed a CLI (Command Line Interface tool)  
→ 예전에는 `Snow`와 같은 장치를 사용하는 방법이 굉장히 복잡했다.

- Today, you can use AWS OpsHub (a software you install on your computer / laptop) to manage your Snow Family Device  
→ `AWS`에서는 사용시의 복잡성을 줄이기 위해 `OpsHub`를 만들었다. `OpsHub`는 컴퓨터나 노트북에 설치하는 소프트웨어이다. 

- Unlocking and configuring single or clustered devices  
→ 단일 장치와 클러스터 장치를 잠금 해제하거나 설정할 수 있다. 

- Transferring files  
→ 파일 전송이 가능하다.

- Launching and managing instances running on Snow Family Devices  
→ `Snow` 장치에서 실행되는 `EC2` 인스턴스를 시작하거나 관리할 수 있다.

- Monitor device metrics (storage capacity, active instances on your device)  
→ 장치 지표 모니터링과 `AWS` 호환 서비스의 실행이 가능하다.

- Launch compatible AWS services on your devices (ex: Amazon EC2 instances, AWS DataSync, Network File System (NFS))  
→ `AWS` 호환 서비스에는 `EC2` 인스턴스, `DataSync`, `NFS` 등이 포함된다.

### 5. Solution Architecture: Snowball into Glacier
- Snowball cannot import to Glacier directly  
→ `Snowball`을 통해 `Glacier`에 직접 데이터를 임포트하는 것은 불가능하다.

- You must use Amazon S3 first, in combination with an S3 lifecycle policy  
→ `Glacier`로 데이터를 이동시키려면 `Amazon S3` 수명 주기 정책을 생성하여 `Amazon Glacier`로 객체를 전환하는 방법을 사용해야 한다. 중요하다.

![image](https://user-images.githubusercontent.com/97398071/235312665-c9f2a586-dcbe-4ef9-a261-0f9c049da6a9.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [데이터 마이그레이션이란?](https://www.netapp.com/ko/data-management/what-is-data-migration/)
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
