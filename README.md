# Project
### 실무 다이캐스팅 공장 전체 IoT 데이터를 활용한 데이터 분석 및 불량 예측 모델 개발

# Intro 
### 📚 Course
산업 인공지능(INE4105-22148) </br>
### 🗓️ Date 
Project term : 2023.05.17 ~ 2023.06.13 </br>
Presentation Date : 2023.06.14 </br>
### :man: Professor 
  한양대학교 ERICA, 산업경영공학과 오요셉 교수님 
### 👥 Team member 
  * 산업경영공학과 김윤성
  * 산업경영공학과 김규현
  * 산업경영공학과 이준범
  * 경제학부 이승재

## 1.  Introduction
1)	프로젝트 소개
(주)다이캐스팅은 다이캐스팅 공정에 초점을 두고 ICT 활용을 통해 공정 데이터를 수집하고 있지만, 데이터 활용을 위한 체계적인 방법론과 시스템이 부족한 상황이다. 특히, 주조 공정에만 초점을 두고 있어 다른 주요 공정인 가공 및 품질 검사에 대한 데이터 수집을 위한 인프라와 시스템이 필요하다. 따라서 이번 프로젝트에서는 다이캐스팅 공정에서 수집된 데이터를 처리하고 분석하여 활용할 계획이다. 주요 목표는 주조 공정 변수와 불량 데이터를 기반으로 한 불량 판정 및 원인 진단 분석을 수행하는 것이다. 다이캐스팅에서 발생하는 불량은 주로 주조 공정에서 발생하기 때문에, 통계적/분석적 접근을 지원하는 시스템과 체계적인 방법론이 필요하다. 또한, 현재 다이캐스팅은 주조 공정이 완료된 후 제품별로 LOT을 부여하고 있어 공정 변수에 따른 불량 상관관계를 파악하기 위한 효과적인 수단이 부족한 상태다. 따라서 주어진 데이터를 기반으로 주조 공정의 불량률을 예측하고 분석하는 것이 이번 프로젝트의 목표이다.

2)	프로젝트 진행 과정
-	프레임워크
 
파이썬 환경에서의 데이터 임포트 문제를 해결하기 위해 파일 저장 및 다시 불러오는 과정을 거친 후, 불량률 예측을 위한 데이터 전처리와 모델링 작업에 대한 결과를 설명한다. 이를 위해 모델링에 필요한 변수들을 적절하게 전처리하여 각 호기별 불량률에 대한 데이터셋을 생성하였다. 그 후 추가적인 전처리 작업을 수행한 뒤, DNN(Dynamic Neural Network)과 랜덤 포레스트(Random Forest) 모델을 사용하여 모델링을 수행하고 성능평가를 진행하였다.

3)	데이터 탐색: 데이터의 특성을 파악하기 위한 분석
-	불량률 예측을 위한 데이터 분석
 
생산진행현황2 파일을 기반으로 전체 데이터를 분석하여 주조공정의 불량률을 파악하였다. 그러나 공정시간이 6시간을 넘어가고 해당 공정에서 발생한 불량률이 합쳐지는 문제로 인해 시간 변동에 따른 불량을 정확히 예측하거나 각 변수들과 불량률 사이의 상관관계를 확인하는 것이 어려움을 판단하였다. 그러나 각 호기에서 하루에 한 가지의 물품이 생산된다는 사실을 확인하였다.
-	불일치 데이터 처리를 위한 공정 시간 파악
 

공정은 8시30분부터 시작한 것으로 추정되지만, 파라미터 데이터는 8시23분부터 기록되어 있으며 이런 경우와 같이 시간이 일치하지 않는 경우가 존재한다.

 

공정은 14시에 완료되는 것으로 나와 있지만, 파라미터 기록은 11시 54분에 중단되어 데이터가 부족하거나 생산 진행 시간을 정확히 추정하기 어렵다고 판단된다.

실제 데이터에서 주조 파라미터의 공정 시작 시간과 공정 실제 시작 시간이 정확하게 일치하지 않는 것으로 판단되어, 시간에 맞춰 공정이 진행되었다고 가정하기 어렵다고 판단하였다. 이에 따라 하루 단위로 추정하여 분석하는 것을 가정하였다.

따라서, 공정 변수와 불량률 사이의 상관관계를 파악하는 것이 어렵다고 판단되었지만, 대신에 각 날짜별 파일들을 평균, 분산, 이상치 등을 계산하여 날짜별 불량률을 분석하고자 한다. 분산을 사용하는 이유는 변동의 정도를 파악하기 위함, 이상치를 사용하는 이유는 불량률에 영향을 주는 이상한 값들을 확인하기 위함이다.

따라서, 1호기부터 8호기까지의 다이캐스팅 제조 공정에서 발생하는 제품의 불량률을 예측하기 위한 데이터 분석 및 모델링 결과를 제시하고, 이를 위해 다양한 주조 환경 데이터와 공정 변수들을 활용하여 불량률을 예측하는 모델 개발을 목표로 하였다.

4)	데이터 처리 및 모델링 과정
-	Feature Engineering 프레임워크
 

(1)	에어압력, 인입 냉각수 온도, 주조동온습도와 같은 주조 환경 데이터에 대해 시간에 따른 변수 값의 평균, 분산, 이상치 수와 같은 파생 변수를 생성하였다. 또한, 호기별로 제품을 구분하기 위해 데이터셋을 분류하고, 보온로 온도와 주조 파라미터에 대한 파생 변수를 생성하였다. 불량률은 다이캐스팅 공정에서 투입량과 불량품 수를 나눈 비율로 계산되었다.
(2)	각 호기별로 생성된 주조 환경 파생 변수 데이터, 보온로 온도 파생 변수 데이터, 주조 파라미터 파생 변수 데이터, 그리고 불량률 데이터를 날짜를 기준으로 병합하여 5개 호기에 대한 불량률 데이터셋을 생성했다. 이후, 데이터셋을 전처리하여 결측치를 평균값으로 대체하고, 수치형 변수 이외의 변수들을 삭제하며, 변수들의 범위를 맞추기 위해 스케일링을 수행하였다.
(3)	1호기부터 8호기까지의 데이터셋을 개별적으로 학습과 예측을 수행하기 위해 train set과 test set을 8:2의 비율로 분리하였다. DNN 모델과 랜덤 포레스트 모델을 사용하여 각 호기별 불량률을 예측하였으며, 랜덤 포레스트 모델을 통해 변수의 중요도를 확인하여 불량률 예측에 중요한 영향을 미치는 변수를 식별하였다.
(4)	각 호기별로 학습된 모델의 예측 결과를 분석하고, 모델의 성능을 평가하였다. 또한, 랜덤 포레스트 모델과 DNN 모델의 성능을 비교 분석하여 최적의 모델을 선택하였다.



## 2.	Results 

## 3.	How to run 

## 4. References


# 한계점
- 전체적인 공정에서 센서의 위치를 알 수 없었음
- 주조 공정에서 쇳물을 넣었을때 틀 위치마다의 온도를 알아야 데이터 평가를 할때 더 좋음
- 온도 센서가 아닌 열화상 카메라 데이터로 처리하는 것이 더 좋을 것임
