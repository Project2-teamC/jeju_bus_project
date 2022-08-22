![image](https://user-images.githubusercontent.com/104780664/185581215-bf28776d-4729-4e76-ac6a-127b8e2cf6f2.png)</br>

# 📑 목차
* [1. 프로젝트 목표](#-1-프로젝트-목표)
* [2. 기획 배경](#-2-기획-배경)
* [3. 데이터 소개](#-3-데이터-소개)
* [4. PyCaret을 이용한 모델 선정](#-4-PyCaret을-이용한-모델-선정)
* [5. 결론 및 한계, 보완 방법](#-5-결론-및-한계-보완-방법)
* [6. 참고 자료](#-6-참고-자료)

<br></br>


----------
# ❓ 1. 프로젝트 목표
이 프로젝트는 제주시에서 버스 승차인원을 예측하는 프로젝트입니다.</br>
목적은 제주도의 버스를 이용하는 이용자의 측면에서 불편사항을 파악하고 개선하는 데 도움이 되고자 함에 있습니다.</br>

<br></br>

----------
# ⚾ 2. 기획 배경
<p align="center"><img width="700" alt="pairplot" src="https://user-images.githubusercontent.com/104780664/185564321-b5e5bd26-bddd-46ab-ad95-218077e7a140.png"></p>
<p align="center"><img width="700" alt="pairplot" src="https://user-images.githubusercontent.com/104780664/185564020-4368ab55-f41b-4b59-b3c1-202ab2cd6e23.png"></p>

2021년 기준 제주특별자치도 내 주민등록세대수가 증감률은 다르지만 매년 꾸준히 증가하고 있습니다.</br>
외국인, 외지인 방문객 수를 고려하면 전체 상주인구는 2019년 기준으로 일 평균 약 60만명 정도 됩니다.

<p align="center"><img width="700" alt="pairplot" src="https://user-images.githubusercontent.com/104780664/185564056-5a649a32-20ef-4029-9259-b63736089c54.png"></p>

이렇듯 제주도민과 외지인의 방문은 증가했으나,</br>
2017년도 대중교통 개편이후에도 여전히 대중교통 이용자는 늘고 있지 않습니다.</br>
이에 따라 제주도청은 4차 대중교통 계획을 발표했으며 그 중 한 가지는 버스 수단 분담률 상승입니다.</br>
낮은 버스 수단 분담률의 이유는 이용자 부문에서는 버스 배차, 버스내 혼잡 등을 불편사항으로 지적되었습니다. </br>
저희는 이용자의 측면에서 바라보고 불편사항을 개선하는데 도움이 되고자 이 프로젝트가 시작되었습니다. </br>

<br></br>

----------
# 🔧 3. 데이터 소개
## (1) 데이터 수집
<p align="center"><img width="700" alt="pairplot" src="https://user-images.githubusercontent.com/104780664/185576700-9440a8c9-9910-4ea9-80b2-65a6f473cbf8.png"></p>
- 모델을 학습할 데이터 갯수 : 415,423 개 (2019년 9월1일 - 30일)</br>
- 예측에 이용할 데이터 갯수 : 228,170 개 (2019년 10월 1일 - 15일)</br>

대중교통 관련 공공데이터를 살펴보던 중 실질적으로 이용자의 대중교통 사용과 관련된 데이터를 확보했고,</br>
해당 데이터를 통해 뭘 알 수 있을까 생각했습니다.</br>
승하차 인원수를 보고, 버스내 혼잡도를 개선해볼 수 있지 않을까? 하는 생각으로 이 데이터를 사용하기로 했습니다.</br>


## (2) 데이터 전처리 및 EDA
### [1] 시간</br>
예측해야 하는 여섯시에서 여덟시 데이터는 두시간 차이를 가지고 있는 데이터이므로,</br>
다른 승하차 시간을 두시간 단위별로 전처리를 해주었습니다.</br>
시간 외의 다른 고려사항들에 대해서 확인하고 전처리를 한 부분도 알아보겠습니다.</br>

### [2] 날씨(비/ 여부)</br>
![image](https://user-images.githubusercontent.com/104780664/185568937-c97227eb-6b30-4745-a2ed-52c2ce391234.png)</br>
추가할 항목은 버스를 안 탄다면 왜 안탈까? 언제 안타고 싶을까?에 초점이 맞춰졌습니다. </br>
팀 의견을 모았을 때, 비가 오면 대중교통을 이용하는게 싫어진다는 의견을 바탕으로 비가 오는 날과,</br>
제주 지역 특성상 태풍의 영향을 받기 쉬우므로 추가로 함께 확인 해보았습니다.</br> 

### [3] 평일, 주말, 공휴일</br>
![image](https://user-images.githubusercontent.com/104780664/185569248-83fe0e34-b99a-4197-bcfb-b4c044ec41b0.png)</br>
또 다른 요소로 주말이나 공휴일에는 버스를 타기보다 집에 있고 싶다는 의견을 바탕으로,</br>
시간대를 나누어 날짜별로 승객수를 라인차트로 확인했습니다. </br>
보시다시피 평일 대비 주말과 공휴일에서 낮은 승하차를 보여주고 있음을 확인했습니다.</br>
따라서 평일, 주말, 공휴일로 나눠서 컬럼을 생성하였습니다.</br>

### [4] 상관관계 분석 후 시간</br>
![image](https://user-images.githubusercontent.com/104780664/185572793-de4cba5e-9979-498a-8e74-1432b3f71cc8.png)</br>
앞의 라인차트를 통해 승하차별로 서로 관련이 있을거 같다고 판단하여 승하차별 상관관계를 확인했습니다.</br>
보시면 승차는 승차끼리 하차는 하차끼리 서로 상관관계가 있는걸로 보입니다. </br>
저녁 6시에서 8시 사이의 승차 시간 즉, 예측하고자 하는 항목도 오전 시간대들과 상관관계가 있어보입니다.</br>

### [5] 지리적 특성</br>
![image](https://user-images.githubusercontent.com/104780664/185569066-3161b221-c3af-4263-b0f4-d2c2980afe70.png)</br>
지리적으로 관계가 없을까 생각을 해봤습니다.</br>
제주도를 사등분하여 각 측정소별로 정류소와의 거리를 계산한 결과들을 측정소 항목별로 입력해주었습니다. 
그리고 위도 경도 데이터를 통해 제주시와 서귀포시를 기준으로 버스정류소가 얼마나 분포되어 있는지를 확인해보기 위해 컬럼을 생성해주었습니다. </br>

### [6] 사용된 Feature</br>
![image](https://user-images.githubusercontent.com/104780664/185569605-f52a0405-0955-43e2-8a59-03d99e80fa4a.png)</br>
시간의 흐름에 따른 시간대별 승차 인원수의 패턴을 두시간 단위로 본 결과,</br>
18~20ride의 값과 거의 유사한 패턴을 보이고 있습니다.</br>
<p align="center"><img width="700" alt="pairplot" src="https://user-images.githubusercontent.com/104780664/185570091-3b2d34ef-e2f4-46ca-a08c-3a225451ed48.png"></p>
따라서 시간대별 승차인원수를 변수로 넣은 Feature1과 그렇지 않은 Feature2로 나누어 모델 결과를 비교하였습니다.</br>

<br></br>


----------
# 📉 4. PyCaret을 이용한 모델 선정
## (1) 오토 머신러닝 [PyCaret]
PyCaret은 파이썬의 오픈 소스, 로우 코드 머신러닝 라이브러리로서 머신 러닝 워크플로우를 자동화합니다. 실험 주기의 속도를 기하급수적으로 높이고 생산성을 높이는 end-to-end 머신러닝 및 모델 관리 도구입니다.</br>
다른 오픈 소스 머신 러닝 라이브러리와 비교했을 때, PyCaret은 수백 줄의 코드를 몇 줄로만 대체하는 데 사용할 수 있는 대체 로우 코드 라이브러리입니다. </br>
PyCaret은 본질적으로 skickit-learn, XGBoost, LightGBM, CatBoost, spaCy, Optuna, Hyperopt, Ray 등과 같은 여러 머신러닝 라이브러리와 프레임워크에 대한 파이썬 래퍼(wrapper함수) 입니다.</br>

***모델 사용코드***</br>
```python
from pycaret.regression import *
```
```python
# 여러개의 머신러닝 모델을 한 번에 비교하는 코드
best_models = compare_models(n_select=3, sort='RMSE')
# n_select는 사용할 파라미터의 갯수
```
```python
# 상위 3개의 모델을 블랜딩해서 앙상블을 만들 수 있는 코드
blend_models = blend_models(estimator_list = [모델1, 모델2, 모델3])
```
```python
# 만든 모델을 훈련시키는 코드
final_model = finalize_model(blend_models)
```
```python
# 훈련된 모델로 값(Label)을 예측하는 코드
prediction = predict_model(final_model, data = test_x)
```
```python
# 결과 값을 plot으로 보기 위한 코드
plot_model(만든 모델, plot = '플롯 정보')
```
<br></br>
## (2) 사용한 모델 선정
![image](https://user-images.githubusercontent.com/104780664/185572597-9ad99462-4dbd-4c73-b35d-2270765c07e8.png)</br>
피쳐를 생성 후 오토머신러닝 라이브러리인 PyCaret을 이용하여 다양한 모델의 결과를 얻고, 이 중 RMSE 기준 Top3 모델을 뽑아 더 높은 성능을 가진 모델을 만들기 위해
앙상블 모델을 만들었고, 이 때 블랜딩하는 방식은 앙상블 기법 중 소프트 보팅을 사용하였습니다. 이를 통해 나온 피쳐별 최종 모델의 성능을 비교하였습니다.</br>

## (3) 모델 성능 비교
### [1] Feature1 사용
<p align="center"><img width="700" alt="pairplot" src="https://user-images.githubusercontent.com/104780664/185575374-f7cc9301-dd4f-479b-a6e0-43a0236cc8f9.png"></p>
RMSE를 기준으로 top3 모델(Catboost, RandomForestRegressor, LightGBM)이 선정되어 앙상블 모델을 만들었습니다.</br>

![image](https://user-images.githubusercontent.com/104780664/185574685-d4c3960f-a1ac-4ce5-a802-4f9f4af67cd5.png)</br>
![image](https://user-images.githubusercontent.com/104780664/185574720-8f90ce97-aad1-45e4-97ce-c70726842f8b.png)</br>

### [2] Feature2 사용
<p align="center"><img width="700" alt="pairplot" src="https://user-images.githubusercontent.com/104780664/185575929-c77be496-dd1d-4d2c-9b32-cf111752b071.png"></p>
RMSE를 기준으로 top3 모델(RandomForestRegressor, Catboost, ExtraTrees)이 선정되어 앙상블 모델을 만들었습니다.</br>

![image](https://user-images.githubusercontent.com/104780664/185579505-5744afbf-7131-4492-bb29-a92d732f4268.png)</br>
![image](https://user-images.githubusercontent.com/104780664/185579554-89b0e56a-0cda-4af0-9311-f0eac391da50.png)</br>

<br></br>
Feature1과 Feature2의 top3 모델들과 앙상블 모델의 잔차와 예측 에러를 비교한 결과, </br>
모델의 잔차에서 베스트 모델은 Train과 Test의 정확도가 높고, 그 둘의 정확도 차이가 적은 앙상블 모델입니다.</br>
예측 에러에서 베스트 모델 또한 실제와 예측에 따른 에러 그래프를 통해 기울기가 1에 더 가깝고, 정확도가 높고, 이상적인 회귀선에 데이터 분산 정도가 작은 앙상블 모델이었습니다.</br>
따라서 Feature1과 Feature2에서 각각의 앙상블 모델이 최종 모델로 선정되었습니다.</br>

### [3] Feature1과 Feature2에 사용된 최종 모델 비교
이 둘의 모델들을 비교해보면, 다음과 같습니다.</br>
<p align="center"><img width="700" alt="pairplot" src="https://user-images.githubusercontent.com/104780664/185579381-7b597ecb-6e55-408c-afed-453e08887cd9.png"></p>
<p align="center"><img width="700" alt="pairplot" src="https://user-images.githubusercontent.com/104780664/185579196-ef283416-cf54-468e-8400-d58a3620da21.png"></p>

앞서 본 결과(잔차, 예측 에러)에서 100명 이상일 경우, 예측 오류가 더 많이 나는 것을 확인 할 수 있습니다. 이는 데이터에 100 이상일 때 이상치가 존재한다고 판단해볼 수 있습니다.</br>
따라서 이상치에 민감한 MSE 값보다 MAE와 MSE의 중간 민감도를 가진 RMSE와 R2(결정계수)를 기준으로 두 모델을 비교하였습니다.</br>
결과적으로 Feature2와 그에 따른 앙상블 모델이 더 좋은 모델이라고 평가할 수 있었습니다.</br>

<br></br>


----------
# ❗ 5. 결론 및 한계, 보완 방법
## (1) 결론
Feature별로 중요도를 판단하기 위해 각 모델별로 중요도 그래프를 그려보았습니다.</br>
![image](https://user-images.githubusercontent.com/104780664/185579702-8d0315f1-489b-4e3a-a1cd-155dbcddb1e5.png)</br>
이때, bus_route_id(노선정보)가 18~20시에 승차한 인원수에 가장 높은 중요도를 보여주고 있습니다. 이는 버스 노선에 따라 승객수에 영향을 줄 수 있으므로 노선별의 개선이 필요할 것이라고 보여집니다.</br>
![image](https://user-images.githubusercontent.com/77037338/185057596-09e01cef-1739-4300-ab24-8b017d312fa7.png)</br>
위 예측값(Label)에서 보면 특정 노선의 특정 정류장에서 사람들이 많이 타는곳과 거의 타지 않아 (-)값이 나오는 곳들에서는 특히 개선이 필요해보입니다.</br>

## (2) 한계 및 보완 방법
### [1] 한계
- 실제로 정답이라고 볼 수 있는 데이터가 없으므로 실제값과 예측값이 얼마나 차이가 나는지를 비교하기가 어려웠습니다.</br>
- 현재 데이터는 각 버스 정류장 별로 승차한 인원수는 알고 있지만 버스 노선별로 정류장 순서는 알기 어려워 이러한 데이터가 있었을 경우 더 정확한 버스 혼잡도를 예측할 수 있지 않았을 까 싶습니다.</br>
예) 301번 버스(bus route_id)가 a->b->c 버스 정류장(statation code) 을 순서로 이동할 경우 a+b에서 승차한 인원수를 알았으면 a+b+c의 값을 알 수 있음.</br>

### [2] 보완 방법
- 실제 데이터를 찾아 볼 수 있다면 좋았을 것입니다.</br>
- 정류소 위치별로 지도화해서 노선을 연결해볼 수 있으면, 버스 혼잡도를 좀 더 정확하게 예측할 수 있을거라 판단됩니다.</br>
- 모델 같은 경우엔 하이퍼파라미터 튜닝을 했으면 더 좋은 정확도를 보여줄 수 있을 것입니다.</br>

<br></br>


----------
# 📌 6. 참고 자료
* 퇴근시간 버스승차인원 예측 경진대회 - DACON (<a href="https://dacon.io/competitions/official/229255/data" target="_blank">링크</a>)
* 주민등록 인구현황 행정구역(시군구)별 주민등록세대수의 5년치 통계|통계청
* 제주특별자치도 방문자수 추이|한국관광데이터랩
* 제주 버스-노선 2배로 늘었지만 교통 분담률 '10년째 제자리'|제주소리 (<a href="http://www.jejusori.net/news/articleView.html?idxno=405628" target="_blank">링크</a>)
* 제주도, 버스 준공영제 문제점 정밀진단…개선 속도 낸다|뉴스로 (<a href="https://www.newsro.kr/%EC%A0%9C%EC%A3%BC%EB%8F%84-%EB%B2%84%EC%8A%A4-%EC%A4%80%EA%B3%B5%EC%98%81%EC%A0%9C-%EB%AC%B8%EC%A0%9C%EC%A0%90-%EC%A0%95%EB%B0%80%EC%A7%84%EB%8B%A8%EA%B0%9C%EC%84%A0-%EC%86%8D%EB%8F%84-%EB%82%B8%EB%8B%A4/" target="_blank">링크</a>)
* 4차 제주도 대중교통계획안에 뭐가 담겼나|제주일보 (<a href="https://www.jejunews.com/news/articleView.html?idxno=2194390" target="_blank">링크</a>)
* 효율성 떨어지는 제주 대중교통? 각종 불편사항도 산재|미디어제주 (<a href="http://www.mediajeju.com/news/articleView.html?idxno=339254" target="_blank">링크</a>)
* 케이웨더(kweather.co.kr)>기상청>과거날씨 (<a href="https://www.kweather.co.kr/kma/kma_past.html" target="_blank">링크</a>)
* 과거태풍 > 태풍 > 기상플러스 (<a href="https://www.weather.go.kr/plus/typ/typ_history.jsp" target="_blank">링크</a>)
* PyCaret (<a href="https://pycaret.readthedocs.io/en/latest/index.html" target="_blank">링크</a>)
* PyCaret offical blog (<a href="https://pycaret.gitbook.io/docs/learn-pycaret/official-blog" target="_blank">링크</a>)
