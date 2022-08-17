# :bus: 제주 버스 승차인원 예측
<img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=Python&logoColor=white"></br>

# 📑 목차
* [1. 프로젝트 목표](#-1-프로젝트-목표)
* [2. 기획 배경](#-2-기획-배경)
* [3. 데이터 소개](#-3-데이터-소개)
* [4. Pycaret을 이용한 모델 선정](#-4-Pycaret을-이용한-모델-선정)
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
![image](https://user-images.githubusercontent.com/77037338/184810294-aded780b-45ed-4658-bfec-2390dcfa1856.png)</br>
![image](https://user-images.githubusercontent.com/77037338/184810394-9415b30a-03a2-4849-8b2e-1f82ab8667ea.png)</br>

2021년 기준 제주특별자치도 내 주민등록세대수가 증감률은 다르지만 매년 꾸준히 증가하고 있습니다.</br>
외국인, 외지인 방문객 수를 고려하면 전체 상주인구는 2019년 기준으로 일 평균 약 60만명 정도 됩니다.

![image](https://user-images.githubusercontent.com/77037338/184810559-5a3b3432-76da-45cb-8bf4-f4c19260dd0a.png)</br>

이렇듯 제주도민과 외지인의 방문은 증가했으나, 2017년도 대중교통 개편이후에도 여전히 대중교통 이용자는 늘고 있지 않습니다.</br>
이에 따라 제주도청은 4차 대중교통 계획을 발표했으며 그 중 한 가지는 버스 수단 분담률 상승입니다.</br>
낮은 버스 수단 분담률의 이유는 이용자 부문에서는 버스 배차, 버스내 혼잡 등을 불편사항으로 지적되었습니다. </br>
저희는 이용자의 측면에서 바라보고 불편사항을 개선하는데 도움이 되고자 이 프로젝트가 시작되었습니다. </br>

<br></br>

----------
# 🔧 3. 데이터 소개
## (1) 데이터 수집
![image](https://user-images.githubusercontent.com/77037338/184811301-09cca29b-5c3c-4d68-b800-6afb41d3d265.png)</br>
- 모델을 학습할 데이터 갯수 : 415,423 개 (2019년 9월1일 - 30일)
- 예측에 이용할 데이터 갯수 : 228,170 개 (2019년 10월 1일 - 15일)

대중교통 관련 공공데이터를 살펴보던 중 실질적으로 이용자의 대중교통 사용과 관련된 데이터를 확보했고, 해당 데이터를 통해 뭘 알 수 있을까 생각해보았습니다.
승하차 인원수를 보고, 버스내 혼잡도를 개선해볼 수 있지 않을까? 하는 생각으로 이 데이터를 사용하기로 했습니다.</br>


## (2) 데이터 전처리 및 EDA
### [1] 시간</br>
예측해야 하는 여섯시에서 여덟시 데이터는 두시간 차이를 가지고 있는 데이터이므로, 다른 승하차 시간을 두시간 단위별로 전처리를 해주었습니다.</br>
시간 외의 다른 고려사항들에 대해서 확인하고 전처리를 한 부분도 알아보겠습니다.</br>

### [2] 날씨(비 여부)</br>
![image](https://user-images.githubusercontent.com/77037338/184814163-8eb8bd88-1708-4e6f-8c82-6789e9d08db6.png)</br>
추가할 항목은 버스를 안 탄다면 왜 안탈까? 언제 안타고 싶을까?에 초점이 맞춰졌습니다. </br>
팀 의견을 모았을 때, 비가 오면 대중교통을 이용하는게 싫어진다는 의견을 바탕으로 비가 오는 날과, 제주 지역 특성상 태풍의 영향을 받기 쉬우므로 추가로 함께 확인 해보았습니다.</br> 

### [3] 평일, 주말, 공휴일</br>
![image](https://user-images.githubusercontent.com/77037338/184814558-e33aac4f-6c50-43df-b758-bd95ed77153b.png)</br>
또 다른 요소로 주말이나 공휴일에는 버스를 타기보다 집에 있고 싶다는 의견을 바탕으로, 시간대를 나우어 날짜별로 승객수를 라인차트로 확인해보았습니다. </br>
보시다시피 평일 대비 주말과 공휴일에서 낮은 승하차를 보여주고 있음을 확인했습니다. 따라서 평일, 주말, 공휴일로 나눠서 컬럼을 생성하였습니다.</br>

### [4] 상관관계 분석 후 시간</br>
앞의 라인차트로 보셨을 때 파악하셨을 것 같은데요. 승하차별로 서로 관련이 있을거 같다고 판단하여 승하차별 상관관계를 봐보았습니다. </br>
보시면 승차는 승차끼리 하차는 하차끼리 서로 상관관계가 있는걸로 보입니다. </br>
저녁 6시에서 8시 사이의 승차 시간 즉, 예측하고자 하는 항목도 오전 시간대들과 상관관계가 있어보입니다.</br>

### [5] 거리</br>
![image](https://user-images.githubusercontent.com/77037338/184818055-fc5b0ac1-e80b-4e53-b86a-7c801379b1d0.png)</br>
![image](https://user-images.githubusercontent.com/77037338/184818826-0f44af98-07e7-4cf1-918b-314ec043dffb.png)</br>
지리적으로 관계가 없을까 생각을 해봤습니다. 제주도를 사등분하여 각 측정소별로 정류소와의 거리를 계산한 결과들을 측정소 항목별로 입력해주었습니다. 
그리고 위도 경도 데이터를 통해 제주시와 서귀포시를 기준으로 버스정류소가 얼마나 분포되어 있는지를 확인해보기 위해 컬럼을 생성해주었습니다. </br>

### [6] 사용된 Feature</br>
![image](https://user-images.githubusercontent.com/77037338/185042393-826a6bb0-940d-4732-bbcf-f42f4c647f7e.png)</br>
시간의 흐름에 따른 시간대별 승차 인원수의 패턴을 두시간 단위로 본 결과, 18~20ride의 값과 거의 유사한 패턴을 보이고 있습니다.</br>
![image](https://user-images.githubusercontent.com/77037338/185042064-ede4543b-da9a-43bd-9982-8ed28efe3248.png)</br>
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
![image](https://user-images.githubusercontent.com/77037338/184921205-18706e90-be63-4359-b6dd-5f7b6570f338.png)</br>

피쳐를 생성 후 오토머신러닝 라이브러리인 PyCaret을 이용하여 다양한 모델의 결과를 얻고, 이 중 RMSE 기준 Top3 모델을 뽑아 더 높은 성능을 가진 모델을 만들기 위해
앙상블 모델을 만들었고, 이 때 블랜딩하는 방식은 앙상블 기법 중 소프트 보팅을 사용하였습니다. 이를 통해 나온 피쳐별 최종 모델의 성능을 비교하였습니다.

## (3) 모델 성능 비교
### [1] Feature1 사용
![image](https://user-images.githubusercontent.com/77037338/185044551-f714640c-bf07-4d67-b0b4-ff4ae21deb5b.png)
![image](https://user-images.githubusercontent.com/77037338/185045250-da4a0821-5a74-4d16-a378-60e16ce3b89b.png)
![image](https://user-images.githubusercontent.com/77037338/185045315-adda1406-e798-4c04-b880-52663b147ba5.png)


### [2] Feature2 사용
![image](https://user-images.githubusercontent.com/77037338/185045429-9cbfb9cd-0816-4b09-a179-63ec6300a9dc.png)
![image](https://user-images.githubusercontent.com/77037338/185045474-3666fc06-74cf-4b6b-95ee-2326bf24354f.png)
![image](https://user-images.githubusercontent.com/77037338/185045533-e1deb583-faaa-412e-b71d-be0117fb9691.png)


<br></br>


----------
# ❗ 5. 결론 및 한계, 보완 방법
## (1) 결론
feature importance를 이용해 제안해볼 수 있다.
## (2) 한계 및 보완 방법
### [1] 한계
실제로 정답이라고 볼 수 있는 데이터가 없으므로 정답과 얼마나 차이가 나는지를 비교하기가 어려웠다.
### [2] 보완 방법
실제 데이터를 찾아 볼 수 있다면 좋았을 것입니다.

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
