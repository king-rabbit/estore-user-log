## 전자제품 이커머스 사이트 사용자 데이터 분석



### 들어가며

- 전자제품 이커머스 사이트의 사용자 행동 데이터를 분석한 프로젝트입니다.
- 퍼널 분석을 통해 문제점을 도출하고 개선 방안을 제시했습니다.
- 고객 분류로 고객군을 나눈 뒤 그룹별로 퍼널 전환율을 확인하고 추가적인 가설과 개선 방안을 검토합니다.



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

<p float="left" align="center">
  <img src="/plots/monthly_orders.png" alt="monthly_orders" style="zoom:70%;" width="40%"/> 
  <img src="/plots/monthly_sessions.png" alt="monthly_sessions" style="zoom:70%;"  width="40%"/>
</p>

<p float="left" align="center">
  <img src="/plots/monthly_active_users.png" alt="monthly_active_users" style="zoom:70%;" width="40%" /> 
  <img src="/plots/price_per_orders.png" alt="price_per_orders" style="zoom:70%;" width="40%" /> 
</p>

<p float="left" align="left">
  <img src="/plots/bounce_rate.png" alt="bounce_rate.csv" style="zoom:70%;"
       width="40%"/>
</p>





###### 코호트 분석

<p align="center">
  <img src="/plots/cohort_analysis.png" alt="node.csv" style="zoom:70%;"  />
</p>







### 퍼널 분석

- 고객의 구매 여정을 분석하기 위해 퍼널 분석을 진행했습니다.

- 페이지뷰(view) -> 장바구니(cart) -> 구매(purchase) 순서의 퍼널별로 전환율이 얼마나 되는지 살펴봅니다.

  

<p align="center"><img src="/plots/conversion-funnel.png" alt="conversion-funnel" style="zoom:70%;" /></p>

	###### 분석결과





### 고객 분류

#### 방법론: K-Means



<p align="center"><img src="/plots/inertia.png" alt="inertia" style="zoom:70%;" /></p>



<p float="left" align="center">
  <img src="/plots/cluster_recency.png" alt="cluster_recency" style="zoom:70%;" width="45%" /> 
  <img src="/plots/cluster_orders.png" alt="cluster_orders" style="zoom:70%;" width="45%"  /> 
  <img src="/plots/cluster_price.png" alt="cluster_price" style="zoom:70%;" width="45%"  />
   <img src="/plots/cluster_brand_pref.png" alt="cluster_brand_pref" style="zoom:70%;" width="45%"  />
</p>







### 고객군별 퍼널 분석



<p align="center"><img src="/plots/conversion-funnel-by-customer-segmentation.png" alt="conversion-funnel" style="zoom:70%;" /></p>
