# house-prices prediction(Ames)
*Ames 도시, Iowa 주 주택가격예상 데이터

종속변수 y= 80-75 (pure house price)

독립변수 1~79

삭제-3,15,16,19,23,24,26,34,36,37,39,41,43,44,45,54,58,61,74

3 삭제 (4,12 와 연관)
15,16.삭제(1 대체)
19 삭제(20 대체)
23,24 삭제(one-hot 하기에 너무 많고 27,28도 대체가능)
26 삭제 (25 대체)
34,36,37. 삭제(38 대체)
39 삭제(40 대체)
41 삭제(대조군 적다)
43,44,45 삭제 (46 대체)
54 삭제(방통합으로 대체)
58 삭제(차고 대체변수 많고 중요치않다)
61 삭제(62 대체)
74 삭제(75대체)



--------------------------------------

*주거 스타일-
1.MSSubClass :주거유형 one-hot
2.MSZoning : 주거구역 one-hot



*입지-
 5 순서형
 12 one-hot
 13,14 one-hot

*땅 조건-

 4.LotArea 
 7.LotShape  순서형 
 8.LandContour: one-hot 
 10.LotConfig : one-hot
 11.LandSlope  순서형



*유틸- 전부 통합
 9.utility 순서형
 40.난방 QC 순서형
 42.전기 : 순서형 (믹스 1개,NA 1개->가장많은것으로 대체)
 


*집상태-
 17,18 순서형-통합 (OverallQual,OverallCond)
 20.YearRemodAdd 
 26.MasVnrArea
 27,28 순서형-통합 (ExterQual,ExterCond)
 29.기초 : one-hot
 46.GrLivArea 
 55.기능: 순서형
 56*57 통합 (벽난로 수, FireplaceQu)
  
 
*지하실-
 30,31,32,33,35 순서형-통합 (35 가중치 낮게해서 합산)
 38


*방 (화장실,침실,부엌)-
 47+49 + (48+50)/2 +51 + 52*53 -통합
 

*차고-
 59. 순서형( 날짜가 최신순 즉 큰순일수록 가치있기 때문)
 60,63,64 순서형 통합
 62
 65


*베란다-
 66,67,68,69,70 -통합
 71*72- 통합
 73 순서형


*시간-
 76,77 -년도 분기별 통합 (시간과 가격 분포 보자)



-----------------------------------

1)변수 정리: 순서형,카테고리형 정리,na 정리,삭제 컬럼,컬럼 통합
  a.연속형 자료 변화하기
 1.MSSubClass: 카테고리
 17.OverallQual, 18.OverallCond: 순서형-통합
 76.MoSold, 77.YrSold: 분기로 바꾸고 연분기 합친 새컬럼 만든후 카테고리형(최신순으로 가격이 높다고 볼수없다)

 47+49 + (48+50)/2 +51 + 52*53 -통합 (모든 방 개수)
 66,67,68,69,70 -통합(베란다 면적)



  b.질적 자료 변화하기 (순서형)
 5.Street: 
 7.LotShape: 
 9.Utilities: 
 11.LandSlope:
 27.ExterQual, 28.ExterCond: 통합
 
 30.BsmtQual:
 31.BsmtCond,
 32.BsmtExposure,
 33.BsmtFinType1,
 35.BsmtFinType2: 30,31,32,33,35 순서형-통합 (35 가중치 낮게해서 합산)

 40.HeatingQC:
 42.Electrical:
 53.KitchenQual:
 55.Functional:
 57.FireplaceQu: (56*57통합)

 60.GarageFinish,
 63.GarageQual,
 64.GarageCond,
 65.PavedDrive: 통합

 72.PoolQC: (71*72통합)
 73.Fence:



  c.나머지 질적자료 모두 카테고리형- one-hot(dummy) OR 따로 encording 하기


2)모든변수들 정규화하기


3)모델 
 k-fold, 앙상블, RF, pcv, DNN, 다중회귀 

모델전에 뱐수가 잘 설정되어 통계적 유의미 한지 분석

회귀분석에서 변수들의 기본적 전제조건
 1.독립변수와 종속변수 선형적관계
 2.오차항의 동분산,정규분포 확인- 정규 Q-Q 도표로 확인
 3.오차항의 독립성(잔차끼리 상관관계가 있음 안된다)- durbin-watson값으로 확인(2근처)
 4.독립변수들 끼리 상관관계 분석- 다중공선성 체크(VIF 10이상, 독립변수끼리 상관이 90%이상이나 높을경우, 공차한계 0.1이하 다중공선성이다),


4)예측
