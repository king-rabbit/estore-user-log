## 전자제품 이커머스 사이트 사용자 데이터 분석



### 들어가며

- 전자제품 이커머스 사이트의 사용자 행동 데이터를 분석한 프로젝트입니다.
- 퍼널 분석을 통해 문제점을 도출하고 개선 방안을 제시했습니다.
- 고객 분류로 고객군을 나눈 뒤 그룹별로 퍼널 전환율을 확인하고 추가적인 가설과 개선 방안을 검토합니다.



### 리포지토리 구성

- EDA.ipynb: 주문 건수, 세션 수 등 기초 지표 탐색

- funnel_analysis.ipynb : 고객 분류 및 퍼널 분석

- funnel.ipynb : 퍼널 분석

- churn.ipynb : 이탈률 분석

- estore-summary.pdf : 프로젝트 분석 보고서

- /plots: 그래프 등 이미지 파일

  

### 데이터

#### 1) 데이터 출처

 - [REES46 Marketing Platform](https://rees46.com/)의 [Open CDP](https://rees46.com/en/open-cdp) 프로젝트 데이터 'eCommerce events history in electronics store'를 사용했습니다.

	- [Kaggle](https://www.kaggle.com/mkechinov/ecommerce-events-history-in-electronics-store)에서도 다운받으실 수 있습니다.



#### 2) 데이터 형태

* 데이터는 다음과 같은 형태로 사용자가 접속해서 행동한 내역을 담고 있습니다.

* 사용자가 언제 접속해서 어떤 유형의 행동을 했는지, 어떤 제품을 조회하고, 장바구니에 넣고, 구매했는지에 대한 정보를 담고 있습니다.

   

<p align="center"><img src="/plots/dataframe_view.png" alt="dataframe" style="zoom:70%;" /></p>

###### 컬럼별 상세정보

* event_time: 사용자의 행동이 발생한 시간 (datetime 컬럼은 이 컬럼을 datetime 유형으로 변환한 것)

* event_type: 행동의 유형으로 'view'(페이지뷰), 'cart'(장바구니 추가), 'purchase'(구매) 중 하나임.

* product_id: 제품 고유 번호

* category_id: 카테고리 고유 번호

* category_code: 카테고리 코드. 대분류-중분류-소분류로 나뉘어져 있으며 '.'으로 구문됨.

* brand: 제품 브랜드

* price: 가격

* user_id: 사용자 아이디

* user_session: 세션 아이디

  
  
  

### EDA



###### 주요 지표

<div align="center">
  <div  align="center">
    월별 주문 건수 / 월별 세션 수
	</div>
 <img src="/plots/monthly_orders.png" alt="monthly_orders" style="zoom:70%;" width="40%"/> 
<img src="/plots/monthly_sessions.png" alt="monthly_sessions" style="zoom:70%;" width="40%"/>
</div>



<div align="center">
  <div  align="center">
    MAU / 건단가
	</div>
  <img src="/plots/monthly_active_users.png" alt="monthly_active_users" style="zoom:70%;" width="40%" /> 
  <img src="/plots/price_per_orders.png" alt="price_per_orders" style="zoom:70%;" width="40%" /> 
</div>

<div align="center">
  <div  align="center">
    반송률 / 객단가
	</div>
  <img src="/plots/bounce_rate.png" alt="bounce_rate" style="zoom:70%;" width="40%"/>
  <img src="/plots/price_per_user.png" alt="price_per_user" style="zoom:70%;" width="40%"/>
</div>




* 월별 세션 수는 2020년 10월 이후 증가하고 있지 않지만 MAU와 주문 건수는 장기적으로 증가하는 추세입니다.
* 건단가와 객단가가 모두 증가하고 있어 신규 고객 유치 시 매출이 증가할 것으로 예상됩니다.
* 첫 페이지에서 유저가 이탈하는 반송율 역시 증가하고 있어 랜딩 페이지 개선이 필요할 것으로 보입니다. 



###### 코호트 분석

<p align="center">
  <img src="/plots/cohort_analysis.png" alt="node.csv" style="zoom:70%;"  />
</p>

* M-1 리텐션이 최대 6%이며, 코호트간 비교했을 때 시간이 지나면서 점차 하락하는 추세입니다.
* 동일 코호트에서도 시간이 지날수록 리텐션 수치가 떨어지고 있어 **서비스의 흡입력이 높지 않은 것**으로 판단됩니다.





### 퍼널 분석

- 고객의 구매 여정을 분석하기 위해 퍼널 분석을 진행했습니다.

- 페이지뷰(view) -> 장바구니(cart) -> 구매(purchase) 순서의 퍼널별로 전환율이 얼마나 되는지 살펴봅니다.

  

<p align="center"><img src="/plots/conversion-funnel.png" alt="conversion-funnel" style="zoom:70%;" /></p>

###### 분석결과

- view -> cart 전환율: 8.45%
- cart -> purshase 전환율: 58.99%
- 장바구니에 상품을 담은 경우 구매로 이어지는 경우가 절반 이상입니다.
- 반면 페이지 조회 후 장바구니 이용으로 이어질 확률이 낮아 고객들이 원하는 상품이 없는 것은 아닌지 의심됩니다.



### 고객 분류

- 고객군별로 전환율의 양상이 다르게 나타나는지 확인하기 위해 먼저 고객 segmentation을 진행했습니다.
- 기본적을 RFM 프레임워크를 사용해 고객을 분류했습니다. 
  * Recency: 최근 3개월 간 구매 이력이 있는지
  * Frequency: 주문 건수는 얼마나 되는지
  * Monetary: 주문 금액은 얼마나 되는지
- 브랜드 제품을 얼마나 선호하느냐에 따라 고객의 특성이 다르게 나타날 것으로 보고, 브랜드 제품 구매 비율 변수를 추가했습니다.



#### 방법론: K-Means

<p align="center"><img src="/plots/inertia.png" alt="inertia" style="zoom:70%;" /></p>

* 적정 클러스터 수를 구하기 위해 클러스터 수에 따라 inertia를 계산합니다.
* 그래프의 기울기가 크게 꺾이는 지점이 4 => 적정 클러스터의 수를 4로 놓고 클러스터링 진행했습니다.



#### 고객 분류 결과

* K-means 방법으로 고객군을 분류한 결과 고객군별 특성을 다음과 같이 시각화하고 정리할 수 있었습니다.

  

<p float="left" align="center">
  <img src="/plots/cluster_recency.png" alt="cluster_recency" style="zoom:70%;" width="45%" /> 
  <img src="/plots/cluster_orders.png" alt="cluster_orders" style="zoom:70%;" width="45%"  /> 
  <img src="/plots/cluster_price.png" alt="cluster_price" style="zoom:70%;" width="45%"  />
   <img src="/plots/cluster_brand_pref.png" alt="cluster_brand_pref" style="zoom:70%;" width="45%"  />
</p>
 

##### 고객군별 특성 정리

- 0번 그룹
  - 최근 구매한 비율이 낮으며 평균 구매 건수가 1.5회 정도로 낮습니다.
  - 구매액도 적은 편으로 저렴한 제품을 1~2회 구매했고 구매 빈도가 적은 그룹입니다.
  - 브랜드 제품을 구매한 비율도 낮습니다.
  - 전반적으로 충성도가 낮은 그룹으로 볼 수 있으며, 전체 회원의 약 13%를 차지합니다.
- 1번 그룹
  - 구매 빈도가 4.3회로 비교적 높고 구매 건단가도 335불 정도로 높은 편입니다.
  - 브랜드를 구매한 비율이 90% 이상이며 최근 구매한 비율 역시 77.9%에 달합니다.
  - 비교적 꾸준히 제품을 구매할 의사가 있는 그룹으로 보입니다. 전체 회원의 2%를 차지합니다.
- 2번 그룹
  - 구매 빈도와 구매액이 모두 높고, 브랜드 제품 구매 비율이 95%에 달합니다.
  - 최근 구매한 비율도 80%인 VIP 그룹으로 충성도가 매우 높습니다.
  - 전체 회원의 0.48%에 해당합니다.
- 3번 그룹
  - 구매 빈도와 구매액 모두 비교적 낮은 편이나 0번 그룹보다는 양호합니다.
  - 최근 구매 비율과 브랜드 제품 구매 비율은 양호한 편입니다.
  - 전체 회원의 5.3%를 차지합니다.
- 4번 그룹
  - 구매 경험이 전혀 없는 그룹입니다.
  - 전체 회원의 78%에 달해 이들의 구매를 유도하는 것이 시급해 보입니다.






### 고객군별 퍼널 분석



<p align="center"><img src="/plots/conversion-funnel-by-customer-segmentation.png" alt="conversion-funnel" style="zoom:70%;" /></p>

##### 분석 결과

- 구매를 전혀 하지 않은 4번 그룹을 제외하면 view -> cart 단계의 전환율이 양호한 편입니다. 모두 50% 이상의 전환율을 보여주고 있습니다.
- 즉 view -> cart 단계 전환율을 제고하기 위해서는 4번 그룹의 활동을 활성화할 필요가 있습니다.
- 이들이 원하는 제품이 없어서 구매를 하지 않는 것인지, 아니면 그저 제품 정보를 찾기 위해 접속하는 것인지 확인해야 할 것으로 보입니다.
- 또한 4번 그룹은 cart -> purchase 단계의 전환율이 0입니다. 이들이 결제 과정을 진행하는 도중에 이탈하는 것인지, 아니면 결제 단계를 아예 진행하지 않은 것인지, 원인은 무엇인지 등을 확인할 필요가 있습니다.
- 4번 그룹을 제외한 모든 그룹에서 cart -> purchase 단계의 전환율은 100%입니다. 즉, 장바구니에 담은 이후에는 대부분 결제로 연결됩니다. 따라서 결제 과정에서 특별한 불편함은 없는 것으로 예상됩니다.
- 0번 그룹의 경우 view -> cart 비율보다 view -> purchase 단계의 비율이 더 높습니다. 장바구니에 담지 않고 바로 구매를 하는 고객들이 존재한다는 의미입니다. 0번 그룹은 구매액이 작아 비교적 손쉽게 구매를 진행하는 것으로 해석됩니다.
- 충성도가 낮은 것으로 분석된 0번 그룹의 전환율이 1,2,3번 그룹보다 높습니다. 0번 그룹은 구매 빈도 대비 구매 의사는 비교적 확실해 보이므로 업셀링이나 크로스셀링을 유도하면 좋을 것으로 예상됩니다.
