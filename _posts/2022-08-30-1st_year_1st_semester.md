---
layout: single
title: "1학년 1학기"
categories: my_life_story
tags: [career]
---

## 1학년 1학기 수강과목 [17학점]

|과목명|이수구분|학점|평점|
|:----------:|:----:|:--:|:--:|
|인공지능개론|전공|3|A+|
|기초프로그래밍|전공기초|3|A+|
|선형대수학|전공기초|3|A+|
|미적분학|전공기초|3|A+|
|글쓰기|교양|2|A+|
|Communication in English|교양|2|A+|
|CAU세미나|자유선택|1|P|


<br>

## During Summer Vacation

### python 정리하기
기초프로그래밍에서 배운 python 지식과 더불어 기본적인 built-in type/function 등을 정리하였다
- [Data Type](https://20226074.github.io/basic_programming/Data-Type/)
- [Class and Inheritance](https://20226074.github.io/basic_programming/Class-and-Inheritance/)
- [Value and Object](https://20226074.github.io/basic_programming/Value-and-Object/)
- [Higher-Order Function](https://20226074.github.io/basic_programming/Higher-Order-Function-and-Lambda-expression/)
- [Module, Package, and File I/O](https://20226074.github.io/basic_programming/Module,-Package,-and-File-I.O)
- [Generator and Coroutine](https://20226074.github.io/basic_programming/Generator-and-Coroutine/)
- [Reserved words and etc](https://20226074.github.io/basic_programming/Reserved-words-and-etc/)

### kaggle course 공부하기
<figure class="third">
  <img src="/assets/img/Intro_to_Programming.png" width=120 high=74>
  <img src="/assets/img/Python.png" width=120 high=74>
  <img src="/assets/img/Intro_to_Machine_Learning.png" width=120 high=74>
  <img src="/assets/img/Pandas.png" width=120 high=74>
  <img src="/assets/img/Intermediate_Machine_Learning.png" width=120 high=74>
  <img src="/assets/img/Data_Visualization.png" width=120 high=74>
  <img src="/assets/img/Feature_Engineering.png" width=120 high=74>
  <img src="/assets/img/Intro_to_SQL.png" width=120 high=74>
  <img src="/assets/img/Advanced_SQL.png" width=120 high=74>
  <img src="/assets/img/Intro_to_Deep_Learning.png" width=120 high=74>
  <img src="/assets/img/Computer_Vision.png" width=120 high=74>
  <img src="/assets/img/Time_Series.png" width=120 high=74>
  <img src="/assets/img/Data_Cleaning.png" width=120 high=74>
  <img src="/assets/img/Intro_to_AI_Ethics.png" width=120 high=74>
  <img src="/assets/img/Geospatial_Analysis.png" width=120 high=74>
  <img src="/assets/img/Machine_Learning_Explainability.png" width=120 high=74>
  <img src="/assets/img/Intro_to_Game_AI_and_Reinforcement_Learning.png" width=120 high=74>


### kaggle competition 의 진행 방식을 감안한 course 정리

Intro to Programming, Python 은 위의 'python 정리하기' 의 내용에 포함되어 있다 <br>
Intermediate Machine Learning 의 지식은 하나의 part 가 아니므로 각 content 마다 *기울임*으로 나타낸다 

- 모든 part 에 대하여 **AI Ethics** 를 염두하고 있어야 한다
  - ( Historical / Represetation / Measurement / Agreegation / Evaluation / Deployment ) bias 가 없는지 확인해야 한다
- 문제 파악 : **Machine Learning** / **Deep Learning** 으로 만들 것인지 **Reinforcement Learning** 으로 만들 것인지 결정한다
  - Reinforcement Learning 으로 만들기 위해서는 Rule 이 있어야 하므로, Machine Learning / Deep Learning 으로 만든다고 가정하자
- 모델 설정 : 목적에 따라 적절한 regressor / classifier 알고리즘을 찾는다  [ *Boosting* 도 생각할 수 있다 ]
- 데이터 처리 : **Pandas** 과 **Data_Visualization** 등을 사용한다  [ **SQL** 으로 데이터를 가져올 수 있다 ]
  - 데이터 전처리 ( **Data Clearning** )
    - Character Data 가 제대로 consistent 하게 encoding 되었는지, date data 의 type 이 datetime 인지 확인한다
    - Missing Value 를 처리하고, 모델에 따라 데이터를 Scaling / Normalization 한다
  - 데이터 마이닝 ( **Feature Engineering** )
    - Categorical Varaible 를 *One-hot Encoding / Ordinal Encoding* 또는 Target encoding 을 통해 numerical data 로 만든다
       - Target encoding 의 경우 *Data Leakage* 가 발생하지 않도록 주의해야 한다
    - Mutual Information 을 통해 변수간의 관계를 확인하고, 적절하게 새로운 Feature 를 만든다
    - Clustering 를 통해 복잡한 데이터를 label 로 변환한다
    - Principal Component Analysis (PCA) 를 통해 Feature 를 변형하여 만들어낸다
    - **Time Series** data 의 경우 Trend, Seasonality, Cycles 등을 파악한다
    - **Computer Vision** 에 사용될 data 의 경우 Augmentation 을 조정한다
    - geospatial data (지역 데이터) 의 경우 geopandas 등을 사용하여 **Geospatial Analysis** 한다
- 모델 학습 : *parameter tuning* 을 하면서 overfitting, underfitting 이 되지 않도록 학습시킨다
- 위의 과정을 수정하며 accuracy 를 높인다
  - Machine Learning 의 경우 Permutation Importance, SHAP Value 등 **Machine Learning Explainability** 를 통해 분석이 가능하다
  
#### kaggle course 에서 부족한 점 (추가로 배워야 할 점)
- Machine Learning 알고리즘 종류가 부족하고, ensemble 에 대한 설명이 부족하다
- Parameter tuning 에 대한 체계적인 search 기법이 없다

<br>
  
### 앞으로의 계획
- 대학에서 kaggle course 와 중복되는 개념을 배운다면 그 course 에 심화 공부용으로 권장되어 있는 competition 을 하며 공부할 계획이다
- 1학년이 끝나면 군대를 가서 데이터 분석 전문가 (ADP) 를 취득하고, 남는 시간동안 미적분학과 선형대수학을 복습할 계획이다
  - 수학과, 응용통계학과를 부전공으로 할 계획이다
