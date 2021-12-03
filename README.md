---
title: "보험 사기 예측"
author: "baekduyong"
date: "2021년 11월"
---

## 2016빅콘테스트 한화생명 데이터로 보험사기자 예측 프로젝트
* BGCON_CUST_DATA 
  * 고객의 특성을 나타내는 Data
  * SIU_CUST_YN 이라는 최종 보험사기 구분 Factor 포함
  *  고객의 성/연령/거주지/직업/배우자/소득 및 신용등급 정보 등 포함
  
* BGCON_CNTT_DATA
  * 고객들의 계약 속성을 나타내는 Data
  *  고객과 연관된 계약들의 상품종류 및 상태변화 및 보험료 수준 등
  *   고객테이블과 CUST_ID 값을 Key값으로 하여 Join 가능
  
* BGCON_CLAIM_DATA 
  * 고객들을 대상으로 한 지급 속성을 나타내는 Data
  * 언제 / 어떠한 사유로 / 얼마의 보험금이 지급 되었는지에 대한 정보를 포함함
  * CNTT_DATA와 POLY_NO값을 Key로 하여 JOIN 가능
 
* BGCON_FMLY_DATA
  *  고객간 가족여부를 알 수 있는 Data
  *  보험사기의 경우 다수가 연계하여 발생하는 경우가 많으므로 Network 분석 등으로 접근하는
     참가자를 위하여 해당 정보를 제공
     
* BGCON_FPINFO_DATA
  * 보험설계사 정보. 보험설계사의 재직기간 등을 알 수 있는 Data
  * 고객대비 보험에 대한 이해도가 높음. Network 분석을 위한 Data


## 01 Data Preprocessing
* 결측값은 각 컬럼 특성에 맞게 제거
* CUST_ID를 index로 사용하기 위해 BGCON_CUST_DATA를 기준으로 파생변수 생성
  
  
## 02 Data Join
* Join된 Data의 고객 수는 20606명, 보험사기자는 1806명으로 약 8.76%이다.
 
 
## 03 Train Data와 Test Data로 분리
* Train Data와 Test Data를 9대1로 분리함.
* 비식별화 컬럼은 삭제
* 고유 값이 많은 컬럼들을 확인한 뒤, 인코딩에 부담을 줄이는 선에서 적절히 삭제
* 피어슨 상관계수를 활용하여 변수간 상관관계를 분석하여 연관성이 큰 변수 삭제 
 
 
## 04 XGBoost를 활용한 feature importance 확인
* Normalization과 One-Hot Encoding을 진행
* Train Data를 XGBoost model에 적용하여 feature importance가 높은 것만 추출


## 05 K-Means를 이용한 clustering
* 보험사기자와 보험사기자가 아닌 고객들의 특성을 파악하기 위하여 군집분석 진행
* 보험사기자가 5%미만인 군집 10개와 15%를 초과하는 군집 6개를 추출
* 16개 군집들의 특성 분석

## 06 clustering된 Data들을 기준으로 Train Data 완성 
* 군집들의 인덱스를 이용해서 10009명의 최종 Train Data 완성
* Data의 label imbalance를 조정하기 위해 ADASYN를 사용해서 해소

## 07 보험사기자 예측을 위한 Classification Model생성
* hyperparameter tuning으로 최적의 parameter 적용
* RandomForest로 약 recall_score가 50.2%의 model 생성
* DecisionTree로 약 recall_score 64.3%의 model 생성

## 08 DecisionTree를 Graphviz통해 시각화
* 생성한 분류모델이 05번에서 분석한 군집들의 특성을 바탕으로 의사결정을 내린다는 것을 확인함

## License

### Code

### Text
