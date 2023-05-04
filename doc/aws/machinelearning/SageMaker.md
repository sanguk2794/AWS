## Amazon SageMaker
### 1. Amazon SageMaker
• Fully managed service for developers / data scientists to build ML models  
→ 완전 관리형 서비스이며 머신 러닝 모델을 구축하는 개발자와 데이터 과학자를 위한 서비스이다. 조직의 실제 개발자와 데이터 과학자가 머신 러닝 모델을 만들고 구축하기 위해 사용한다. 그래서 훨씬 복잡하고 사용하기 어렵다.

• Typically, difficult to do all the processes in one place + provision servers  
→ 머신 러닝 모델을 구축하기 위한 과정에는 여러 단계가 있다. 이 과정은 서버를 프로비저닝해서 계산을 수행하여 모델을 생성해야 하므로 길고 복잡하다. 이 때 필요한게 `SageMaker`이다.

• Machine learning process (simplified): predicting your exam score
~~~
• 시험 성적을 예측하는 머신 러닝 모델을 구축하고 싶다.
• IT 경력, 데이터베이스 경력, 강의를 들은 횟수, 시험 점수 등 머신 러닝 모델 구축을 위해 학생들의 실제 시험 점수에 관한 모든 데이터를 수집해 해당 데이터를 라벨링한다.
• 이 데이터를 기반으로 머신 러닝 모델을 구축해야 한다. 러신 머닝 모델을 구축한 후에는 훈련 및 조정이 필요하다. 데이터와 출력이 더 잘 맞도록 점차 모델을 개선하는 것이다.
• SageMaker는 라벨링, 구축, 훈련 및 조정을 포함한 전 과정에 대해 도움을 준다. 
~~~

![image](https://user-images.githubusercontent.com/97398071/236099703-ac336226-58bd-464a-ac89-08851e16e504.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---