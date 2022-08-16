# :bus: jeju_bus_project

# 📑 목차
* [1. 프로젝트 목표](#-1-프로젝트-목표)
* [2. 기획 배경](#-2-기획-배경)
* [3. 데이터 소개](#-3-데이터-소개)
* [4. 모델 선정 및 분석 기법](#-4-모델-선정-및-분석-기법)
* [5. 결론 및 한계, 보완 방법](#-5-결론-및-한계-보완-방법)
* [6. 참고 자료](#-6-참고-자료)

<br></br>


----------
## ❓ 1. 프로젝트 목표
이 프로젝트는 제주시에서 버스 승차인원을 예측하는 프로젝트입니다.</br>
목적은 제주도의 버스를 이용하는 이용자의 측면에서 불편사항을 파악하고 개선하는 데 도움이 되고자 함에 있습니다.</br>


<br></br>


----------
## ⚾ 2. 기획 배경
![image](https://user-images.githubusercontent.com/77037338/184810294-aded780b-45ed-4658-bfec-2390dcfa1856.png)
![image](https://user-images.githubusercontent.com/77037338/184810394-9415b30a-03a2-4849-8b2e-1f82ab8667ea.png)

2021년 기준 제주특별자치도 내 주민등록세대수가 증감률은 다르지만 매년 꾸준히 증가하고 있습니다.</br>
외국인, 외지인 방문객 수를 고려하면 전체 상주인구는 2019년 기준으로 일 평균 약 60만명 정도 됩니다.

![image](https://user-images.githubusercontent.com/77037338/184810559-5a3b3432-76da-45cb-8bf4-f4c19260dd0a.png)

이렇듯 제주도민과 외지인의 방문은 증가했으나, 2017년도 대중교통 개편이후에도 여전히 대중교통 이용자는 늘고 있지 않습니다.</br>
이에 따라 제주도청은 4차 대중교통 계획을 발표했으며 그 중 한 가지는 버스 수단 분담률 상승입니다.</br>
낮은 버스 수단 분담률의 이유는 이용자 부문에서는 버스 배차, 버스내 혼잡 등을 불편사항으로 지적했습니다. </br>
저희는 이용자의 측면에서 바라보고 불편사항을 개선하는데 도움이 되고자 이 프로젝트가 시작되었습니다. </br>
(4차 제주도 대중교통계획안에 뭐가 담겼나 - 제주일보, 효율성 떨어지는 제주 대중교통? 각종 불편사항도 산재)</br>



<br></br>


----------
## 🔧 3. 데이터 소개
### (1) 데이터 수집
![image](https://user-images.githubusercontent.com/77037338/184811301-09cca29b-5c3c-4d68-b800-6afb41d3d265.png)
- 모델을 학습할 데이터 갯수 : 415,423 개 (2019년 9월1일 - 30일)
- 예측에 이용할 데이터 갯수 : 228,170 개 (2019년 10월 1일 - 15일)

대중교통 관련 공공데이터를 살펴보던 중 실질적으로 이용자의 대중교통 사용과 관련된 데이터를 확보했고, 해당 데이터를 통해 뭘 알 수 있을까 생각해봤습니다.
승하차 인원수를 보고, 버스내 혼잡도를 개선해볼 수 있지 않을까? 하는 생각으로 이 데이터를 사용하기로 했습니다.</br>


### (2) 데이터 전처리 및 EDA
[1] 시간</br>
예측해야 하는 여섯시에서 여덟시 데이터는 두시간 차이를 가지고 있는 데이터이므로, 다른 승하차 시간을 두시간 단위별로 전처리를 해주었습니다.</br>
시간 외의 다른 고려사항들에 대해서 확인하고 전처리를 한 부분도 알아보겠습니다.</br>

[2] 날씨(비 여부)</br>
![image](https://user-images.githubusercontent.com/77037338/184814163-8eb8bd88-1708-4e6f-8c82-6789e9d08db6.png)
저희는 안 탄다면 왜 안탈까 언제 안타고 싶을까?에 초점이 맞춰졌습니다. </br>
저희조 의견으로는 비가 오면 대중교통을 이용하는게 싫어진다는 의견을 바탕으로 비가 오는 날과, 제주 지역 특성상 태풍의 영향을 받기 쉬우므로 추가로 함께 확인 해보았습니다.</br> 

[3] 평일, 주말, 공휴일</br>
![image](https://user-images.githubusercontent.com/77037338/184814558-e33aac4f-6c50-43df-b758-bd95ed77153b.png)
또 다른 요소로 주말이나 공휴일에는 버스를 타기보다 집에 있고 싶다는 의견을 바탕으로 라인 그래프를 확인해보았습니다. </br>
보시다시피 평일 대비 주말과 공휴일에서 낮은 승하차를 보여주고 있음을 확인했습니다. 따라서 평일, 주말, 공휴일로 나눠서 컬럼을 생성하였습니다.</br>

[4] 상관관계 분석 후 시간</br>
앞의 라인차트로 보셨을 때 파악하셨을 것 같은데요. 승하차별로 서로 관련이 있을거 같다고 판단하여 승하차별 상관관계를 봐보았습니다. </br>
보시면 승차는 승차끼리 하차는 하차끼리 서로 상관관계가 있는걸로 보입니다. </br>
저녁 6시에서 8시 사이의 승차 시간 즉, 예측하고자 하는 항목도 오전 시간대들과 상관관계가 있어보입니다.</br>

[5] 거리</br>
![image](https://user-images.githubusercontent.com/77037338/184818055-fc5b0ac1-e80b-4e53-b86a-7c801379b1d0.png)
![image](https://user-images.githubusercontent.com/77037338/184818826-0f44af98-07e7-4cf1-918b-314ec043dffb.png)
지리적으로 관계가 없을까 생각을 해봤습니다. 제주도를 사등분하여 각 측정소별로 정류소와의 거리를 계산한 결과들을 측정소 항목별로 입력해주었습니다. 
그리고 위도 경도 데이터를 통해 제주시와 서귀포시를 기준으로 버스정류소가 얼마나 분포되어 있는지를 확인해보기 위해 컬럼을 생성해줬습니다. </br>

<br></br>


----------
## 📉 4. 모델 선정 및 분석 기법
### (1) 모델 선정 기준

### (2) 사용한 분석기법



<br></br>


----------
## ❗ 5. 결론 및 한계, 보완 방법
### (1) 결론

### (2) 한계 및 보완 방법


<br></br>


----------
## 📌 6. 참고 자료
