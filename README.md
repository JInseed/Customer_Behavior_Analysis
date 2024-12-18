# 고객 행동 분석을 통한 수익 증대 전략

<br>

## 진행기간
> **2024/04/08~04/17**
<br>


## 사용 데이터
> **공급망 데이터**
>
> https://www.kaggle.com/datasets/shashwatwork/dataco-smart-supply-chain-for-big-data-analysis/data
<br>

## 개발 환경
> **Python**
<br>

### 역할
> **EDA, Data Preprocessing, RFM 분석(+CLTV), 보고서 작성**
<br>

## 분석 주제
> **고객 행동 분석을 통한 수익 증대 전략: 공급망 최적화를 통해 기업의 운영 효율성과 수익성을 향상**
<br>


## 문제 정의 및 KPI 설정

- **문제 정의**
    - `수익성 중심의 접근`
        - 판매량이 높다고 반드시 높은 수익성을 의미하지 않음
        - 낮은 마진 또는 높은 비용이 수익 기여도를 저하시킬 수 있음
        - 판매량 대비 높은 수익을 창출하는 요인을 식별하여 수익 극대화 전략 수립
    - `고객 만족도의 중요성`
        - 고객 만족도는 재구매율 및 장기적인 고객 관계 유지에 핵심적인 요소
        - 만족도가 낮은 제품군의 개선 필요
        - 주문 취소율 및 재구매율 분석을 통해 문제 해결
    - `지역별 매출 및 카테고리 분석`
        - 지역별 문화적 특성과 계절성을 고려한 수요 높은 상품 분석
        - 구체적인 마케팅 전략 및 세부 프로모션을 통해 수익 창출 극대화 가능
    - `배송 효율성의 중요성`
        - 배송 속도 및 정확성은 고객 만족도와 충성도에 큰 영향
        - 배송 인프라를 통한 경쟁력 강화
    - `RFM 분석과 CLTV 계산을 통한 고객 세분화 및 가치 관리`
        - RFM 분석: 고객의 최근 구매 활동, 구매 빈도, 구매 총액을 기준으로 세분화
        - CLTV 모델: 고객의 미래 가치를 예측하여 최적의 고객 관리 및 유치 전략 수립
        - 시계열 분석을 활용하여 고객 행동을 지속적으로 모니터링하고 관계를 심화
- **KPI 설정**
    - `수익 최적화 KPI`
        - 목표: 판매량 대비 수익성이 높은 요인과 구간을 식별
        - 전략: 분석 결과를 기반으로 수익 극대화 방안 수립
    - `고객 만족도 KPI`
        - 측정 지표: 주문 취소율 (낮을수록 좋음), 재구매율 (높을수록 좋음)
        - 목표: 낮은 취소율과 높은 재구매율을 달성하여 장기적인 수익성 강화
    - `배송 효율성 KPI`
        - 측정 지표: 배송 속도, 배송 효율성 (지역 및 방법별 분석)
        - 목표:
            - 배송 인프라 투자 우선순위 결정
            - 최적의 배송 네트워크 구축 전략 제안
    - `RFM 분석 KPI`
        - 측정 요소:
            - 최근 구매 일자(Recency), 구매 빈도(Frequency), 구매 금액(Monetary)
        - 목표:
            - 고객 군집별 특성 파악
            - 맞춤형 마케팅 전략 개발
            - 고가치 고객 유지 및 가치 증대 전략 수립
    - `고객 평생 가치 (CLTV) KPI`
        - 측정 지표: 고객의 평생 가치
        - 목표:
            - 고객의 미래 가치를 예측
            - 장기적인 고객 관계 관리 및 유치 전략 최적화
            - 시계열 분석을 통한 구매 패턴 및 변화 모니터링

<br>

## 분석 요약(담당 부분)

1. Data Preprocessing
    1. 중복  열 삭제
    2. 특정 시간 이전 데이터로 cutoff
2. Simple EDA
3. RFM 분석
    1. K-means 를 통한 고객 세분화
    2. RFM 고객 군집 해석 및 EDA 
    3. 고객 행동 시계열 분석 및 CLTV 분석

- **전체 분석 요약**
    - 할인율 및 취소율 분석
    - 재구매율 분석
    - 지역별  판매 분석
    - 사기 의심 정황 분석
    - RFM 분석
<br>



## 분석 과정

### *Data Preprocessing& SimpleEDA*

1. 필요치 않은 열 및 중복 열 삭제
2. 특정 시간 이전의 데이터로 cutoff

- RFM 테이블, 즉 고객별 최근 구매 날짜, 구매 빈도, 구매 총액을 구한 후 테이블을 살펴본 결과 아래쪽에 해당하는 Customer Id의 구매빈도가 전부 1이어서 확인을 진행
    - 확인 결과 Customer Id 가 12435부터 전부 1인 것을 확인했고, Id를 할당할 때 날짜 순으로 했을 가능성을 내포한다고 판단
    - 연도별 구매횟수 등 자세하게 확인한 결과 아래에 해당하는 사실을 발견 → Customer Id가 날짜순으로 할당되었음을 확인
        - **Customer Id 1~12435 : 2015~2017년 사이 다양한 구매 분포, 2018년은 한번도 구매 이력 없음**
        - **Customer Id 18635 ~ 20757 : 2018년에 단 한번의 구매만 진행**
        - **Customer Id 12436 ~ 18634 : 2017년에 단 한번의 구매만 진행**
- 이는 2017년부터 기존 고객의 Id를 재할당 했을 가능성 혹은 데이터 오류로 판단
    - Customer Id 가 12436을 기준으로 자세히 확인
    - 해당 고객의 구매 날짜인 2017-10-02 12:46:00 를 기준으로 데이터를 분리 후 해당 기점 이전 고객이 이후 고객에 포함되었는지 확인하여 Id를 재할당 했는지에 대한 가설을 확인
    - 결과적으로 재할당하지 않음을 확인했으며 이는 해당 시간대 이 후 기록된 모든 고객은 이전 고객과 동일한 경우는 없는 것으로 판단
    - **이에 해당 기점 이후의 데이터는 이전 고객의 거래 기록과 연관 지을 수 없으므로 제거 후 분석을 진행**

```
1) Number of columns are :  37
2) Number of rows are :  172199
3) Total number of data-points : 6371363
4) Count of Numerical Features : 19
5) Count of Categorical Features : 18
```

```
Percentage of Total Missing Values is  0.0 %
Missing Value Estimation :
```

- 결측 없음

<br>


<table width="100%">
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/e4f429fe-6f45-4e8c-829b-fb0940600cf4" width="90%">
    </td>
  </tr>
</table>

- 필요치 않은 열 및 중복열 처리 완료
- 어느 정도 상관계수가 높은 값을 가지는 열이 존재하나 중복열은 아닌 것으로 판단되었다
- 이를 설명하는 열 계산식과 사용된 열을 아래에 기술
    - Sales = Product Price * Order Item Quantity
    - Order Item Discount = Sales * Order Item Discount Rate
    - Sales per customer = Sales - Order Item Discount
    - Order Profit Per Order = Sales per customer * Order Item Profit Ratio

<br>

### *고객 세분화 및 가치 분석(RFM&CLTV)*

- **RFM 분석을 실시하여 고객을 여러 세그먼트로 분류. 각 세그먼트의 LTV를 계산하여 가치 있는 고객 그룹을 식별**
- **이를 통해 고객 그룹을 우선순위별로 분류하고, 고가치 고객 그룹에 맞춤형 마케팅 및 서비스를 제공**

- **지표 정의**
    - Recency의 값은 오늘 날짜(데이터 상 2017년 10월 2일)로부터 가장 최근에 배송된 날짜와의 차이
        - 실제로 제품이 고객에게 배송된 시점이 구매 완료의 가장 명확한 지표라고 판단하여 Recency를 산출할 때 주문 날짜가 아닌 배송 날짜를 활용
    - Frequency는 총 구매 횟수
    - MonetaryValue는 총 지출 금액
- RFM table을 생성하기 전 간단한 전처리로 취소된 배송 건수에 대한 행은 RFM 값을 산출하는 데 왜곡을 줄 수 있으므로 삭제하고 진행

| Customer Id | Recency | Frequency | MonetaryValue |
| --- | --- | --- | --- |
| 1 | 671 | 1 | 472.450012 |
| 2 | 17 | 4 | 1618.660042 |
| 3 | 114 | 5 | 3189.200037 |
| 4 | 262 | 4 | 1480.709993 |
| 5 | 340 | 3 | 1101.919998 |
| ... | ... | ... | ... |
| 12431 | 88 | 13 | 6614.170039 |
| 12432 | 153 | 9 | 4622.410011 |
| 12433 | 58 | 3 | 1293.489998 |
| 12434 | 107 | 6 | 2883.950024 |
| 12435 | 400 | 1 | 931.710007 |

<br>


**`K-means 를 통한 고객 세분화`**

- **고객을 Recency(최근 구매 날짜), Frequency(구매 빈도), Monetary(구매 총액) 값에 따라 군집화하고, 이를 통해 고객의 현재 가치와 행동
경향을 분석**

- `군집 생성하기 전 Elbow Plot과 Silhouette Score를 산출하여 최적의 군집 수 탐색`

<table width="100%">
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/9c22d9d7-09ce-4282-b36e-dfc58a347445" width="90%">
    </td>
  </tr>
</table>

- Silhouette Score 또한 고려하여(표는 생략) 최적의 군집 수를 5로 지정하여 군집을 생성

<br>

<table width="100%">
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/846abe20-e362-4b71-9afe-6bba56131647" width="90%">
    </td>
  </tr>
</table>

- 군집이 성공적으로 생성된 것을 확인
- 밀도가 높은 데이터 특성상 random state를 고정시키지 않으면 군집 번호가 변동될 가능성이 있어, 이를 고정하여 일관된 결과를 얻을 수 있도록 조치
- 추가적으로 DBSCAN 등의 밀도 기반 군집화 모델을 시도하였으나, 데이터의 높은 밀도로 인해 군집화 과정이 어려웠음

<br>

**`RFM 고객 군집 해석 및 EDA`**

- `Recency, Frequency, MonetaryValue 분포`

<table width="100%">
  <tr>
    <td align="left" width="50%">
      <img src="https://github.com/user-attachments/assets/92bfa469-ce58-435b-96a9-075a71660879" width="95%">
    </td>
    <td align="right" width="50%">
      <img src="https://github.com/user-attachments/assets/5fbe343c-beac-43eb-9b3e-c44510b29309" width="95%">
    </td>
  </tr>
</table>

<table width="100%">
  <tr>
    <td align="left">
      <img src="https://github.com/user-attachments/assets/8ddc6348-4d42-4fbf-82e5-c17b3a53b32c" width="100%">
    </td>
  </tr>
</table>

- 꽤 많은 사람이 최근까지 구매이력이 있는 것으로 파악
- 절반 이상의 고객이 4번 이상 주문 경험이 있음

<br>

- `군집 해석`

<table width="100%">
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/d594b032-3264-4970-b84d-05bbd84d9a2f" width="90%">
    </td>
  </tr>
</table>

- **0: 충성 고객(Loyal Customers )**
    - 최근에 구매한 것은 물론 자주 구매하고 상당한 금액을 지출
- **1: 이탈 위험 고객(At Risk Customers)**
    - 최근 구매한 지 오래되었고, 그 빈도도 낮아 잠재적으로 이탈할 위험이 있는 고객으로 분류
- **2: 불만족 고객(Dissatisfied Customers)**
    - 구매한지 굉장히 오래되었으며 구매 빈도와 지출 금액 또한 매우 낮음
    - 불만족한 고객일 확률이 높으며 이미 이탈했거나 이탈할 가능성이 높은 군집으로 해석
- **3: VIP 고객(VIP Customers)**
    - 최근에 구매가 이루어졌으며, 구매 빈도와 지출 금액이 가장 높음
    - 이들은 VIP 고객 혹은 단골 고객으로 해석
- **4: 이탈 위험 충성 고객(Loyal Customers (At Risk))**
    - 과거에는 충성스러운 구매 패턴을 보였을 수도 있으나 최근 구매가 없어 잠재적으로 이탈 위험이 있는 고객으로 해석

<br>



- `군집별 고객 수`

<table width="100%">
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/56b5eadb-dabd-4bbb-bac7-9483a5ef38a2" width="90%">
    </td>
  </tr>
</table>


- 충성 고객이 전체 고객 중 가장 큰 비율을 차지 하고 있어, 회사의 효율적인 운영을 반영
    - 그러나 이탈 위험 고객과 충성 고객(이탈위험) 역시 상당수 존재하여 구매 빈도를 증가시키기 위한 전략이 요구
- 회사가 현재 운영하는 마케팅 전략을 재검토하고, 특히 이러한 위험군에 속하는 고객들의 유지 및 활성화를 위한 구체적인 방안을 마련해야 함
<br>


- `군집별 Recency 분포`

<table width="100%">
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/7db7d6fe-5f96-4b40-8656-7305f046ce73" width="90%">
    </td>
  </tr>
</table>

- 일부 VIP 고객들이 높은 Recency 값을 가지고 있는 것을 확인할 수 있음
    - 이는 VIP 고객이라 할지라도 이탈 위험을 갖고 있음을 나타냄
- 불만족 고객군에서는 최근 구매한 사람조차도 마지막 구매 이후 거의 1년이 지난 사례가 관찰
    - 이들이 이미 이탈했을 가능성이 높다는 점을 시사
    - 고객 유지 전략을 재정비할 필요가 있음
- **특히 고객 충성도가 높지 않은 군에 대한 추가적인 마케팅 및 관리 전략이 요구**
<br>


- `군집별 Frequency 분포`

<table width="100%">
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/c03f85d0-98e6-4899-94e7-952a629747d7" width="90%">
    </td>
  </tr>
</table>

- VIP 고객 중 일부는 구매 횟수가 특히 높음
    - VVIP 고객으로 분류할 수 있으며, 쿠폰이나 이벤트와 같은 특별한 마케팅 활동을 통해 이들을 장기적으로 유지하는 것이 중요
- 이탈 위험의 고객군은 수가 많음에도 불구하고 구매 빈도가 낮은 편
    - 이러한 고객군을 다시 활성화시키고 구매를 유도하기 위한 전략이 필요
    - 타깃 마케팅과 개인화된 접근이 중요

<br>

- `군집별 MenetaryValue 분포, 군집 별 실제 총 이익 분포`

<table width="100%">
  <tr>
    <td align="left" width="50%">
      <img src="https://github.com/user-attachments/assets/833a397b-5315-4d4b-98e0-452d7c93d6b2" width="95%">
    </td>
    <td align="right" width="50%">
      <img src="https://github.com/user-attachments/assets/f22c6804-629c-4467-9194-5a197cebe6cb" width="95%">
    </td>
  </tr>
</table>

- VIP 고객은 전체 고객 수가 적음에도 불구하고 총 이익에 상당한 기여를 하고 있음이 명확하게 보임
    - 이들의 지속적인 유지는 회사에 있어 필수적인 전략
- 충성 고객(이탈 위험)의 수는 이탈 위험 고객과 비교할 때 상대적으로 적지만, 실제로 기여하는 이익은 비슷한 수준임을 확인
    - 이탈 위험이 있는 충성 고객을 효과적으로 관리하고 유지하기 위한 전략이 필요
    - 이들에 대한 집중적인 관리와 맞춤형 마케팅 전략을 개발하는 것이 중요
<br>

**`RFM 군집화를 활용한 고객 행동 시계열 분석 및 CLTV`**

- `월별 군집 신규 유입 분포`

<table width="100%">
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/2b159d0e-df54-4228-960d-ad1b6cec066f" width="90%">
    </td>
  </tr>
</table>

- 월별로 각 군집에 신규로 유입되는 고객 수의 분포
- VIP 고객 군집에는 신규 유입이 거의 없는데, 이는 대부분의 VIP 고객이 처음부터 단골이었을 가능성을 시사
- 충성 고객은 2016년 초에 유입이 많았지만, 2016년 말 이후로는 유입이 줄어들어 이 군집을 타깃으로 한 마케팅 전략이 필요
- 이탈 위험 고객은 지속적으로 높은 신규 유입률을 보여주며, 이들은 일시적으로 활동하는 고객일 가능성이 높다
    - 이러한 고객군이 지속적으로 존재하는 경우, 이들이 매출에 미치는 영향을 고려할 필요
<br>


- `월별 실제 활동하는 고객 분포`

<table width="100%">
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/93cc53cc-2be7-4023-bb5b-a773dbac9e5e" width="90%">
    </td>
  </tr>
</table>


- 2016년 중반부터 불만족 고객과 이탈 위험이 있는 충성 고객의 활동이 현저히 줄어들며, 이들이 대부분 이탈했을 가능성이 높다고 볼 수 있음
- 그러나 2017년부터 충성 고객의 활동이 급격히 증가하고, 이탈 위험 고객의 활동도 상승세를 보이기 시작
    - 이는 2016년에 활동이 감소한 이후, 2017년에 효과적인 마케팅 전략이 시행되었을 가능성을 시사
    - 이러한 변화는 마케팅의 성공적인 적용이 고객 활동에 어떻게 긍정적인 영향을 미칠 수 있는지를 보여주는 사례로 해석
<br>


- `월별 이탈 고객 분포`

<table width="100%">
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/f71709fa-054b-4ce2-adcb-83b53867d40a" width="90%">
    </td>
  </tr>
</table>

- 불만족 고객의 이탈은 예상되었지만, 이탈 위험이 있는 충성 고객이 2016년 중반부터 크게 이탈하기 시작했다는 점은 주목할 필요
- 2017년부터는 충성 고객과 VIP 고객까지도 이탈 추세를 보이기 시작
- 충성 고객과 VIP 고객 군집에 신규 유입은 없었지만, 실제 활동하는 고객 수에는 큰 변동이 없음을 볼 수 있음
    - 이는 휴면 상태였던 고객들의 활동이 증가했음을 시사하며, 이는 2017년에 진행된 특별 이벤트나 프로모션의 영향일 가능성이 높음
    - 이러한 변화는 회사의 마케팅 전략이 고객 활동에 어떻게 긍정적으로 작용할 수 있는지를 보여주는 중요한 지표
<br>

- `월별 주문 빈도, 고객 당 주문 빈도`

<table width="100%">
  <tr>
    <td align="left" width="50%">
      <img src="https://github.com/user-attachments/assets/856001ea-daa6-4f00-bdd0-9827b5fec32e" width="95%">
    </td>
    <td align="right" width="50%">
      <img src="https://github.com/user-attachments/assets/87fed4a9-046b-4393-bbbc-4c40cffb50d5" width="95%">
    </td>
  </tr>
</table>

- 충성 고객과 VIP 고객의 높은 주문 빈도는 긍정적인 지표로 평가
- 특히, 2017년부터 이탈 위험 고객의 주문 빈도가 증가한 것 역시 주목할 만하며, 이는 해당 연도에 특별 이벤트가 있었을 가능성을 시사
- VIP 고객과 충성 고객의 주문 수가 꾸준히 높은 것으로 나타남
- 이탈 위험이 있는 충성 고객 역시 주문 빈도가 높으나, 전체 고객 수에 비해 이미 상당수가 이탈한 것으로 보임
<br>

- `월별 실제 수익 분포, 고객 당 실제 수익 분포`

<table width="100%">
  <tr>
    <td align="left" width="50%">
      <img src="https://github.com/user-attachments/assets/1930f79d-20eb-46a9-aa98-e637dfa88032" width="95%">
    </td>
    <td align="right" width="50%">
      <img src="https://github.com/user-attachments/assets/e2ce29b4-f22d-4626-9016-73545c397b2e" width="95%">
    </td>
  </tr>
</table>

- 2017년도부터 VIP 고객, 충성고객, 이탈위험 고객 모두 고객 당 수익이 증가하고 있는 것을 볼 수 있음
    - 이 또한 2017년에 무언가 이벤트가 있었음을 시사

<br>
<br>

`CLTV`

- **고객이 평생 우리 기업에 어느 정도의 가치를 가져올 수 있을지를 나타내는 지표. 쉽게 말해, 고객 한 명에게 기대할 수 있는 매출과 수익**
- **고객 행동 패턴을 분석하여 군집별로 가치 평가**
- **CLTV = 고객 가치 * 평균 고객 수명**
    - 고객 가치 (=평균 구매 가치 * 평균 구매 빈도)
    - 평균 구매 가치 (=특정 기간 내 총 수익 / 특정 기간 내 총 구매 횟수)
    - 평균 구매 빈도 (=특정 기간 내 구매 횟수 / 특정 기간 내 총 고객 수)
    - 평균 고객 수명 (=고객이 지속적으로 구매한 기간 / 총 고객
- 평균 고객 수명 값은 1/평균 구매 간격으로 계산하였으며 군집별로 따로 구하여 진행
<br>


- `분기별 CLTV 분포`

- <table width="100%">
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/86bd8ba8-6d8b-4576-8c5e-1643e3ae99de" width="90%">
    </td>
  </tr>
</table>

- VIP 고객은 지속적으로 높은 값을 유지하며, 2017년부터 VIP, 충성 고객, 이탈 위험 고객 모두 상승세를 보이고 있음
    - 이 그룹에 프로모션과 이벤트를 집중하면 수익 창출에 크게 기여할 것으로 예상
- 불만족 고객과 이탈 위험 충성 고객은 이미 이탈한 상태이므로 이들을 대상으로 한 이벤트는 진행하지 않는 것이 좋음
    - 완전히 새로운 신규 고객을 유치하기 위한 마케팅 전략을 개발하는 방법을 추천

<br>

## 시사점 및 보완할 점

- **RFM 분석**
    - 일반적으로 분위수로 나누어서 고객을 세분화 하나 군집화 알고리즘을 통해 진행
    - 고객 군집별 행동과 수익 기여도의 차이를 명확히 식별
    - 특히 이탈 위험이 높은 충성 고객 군집에서 이탈율을 조기에 감소시킬 수 있었다면 매출 증대에 크게 기여할 수 있었을 것
        - 하지만, 이 군집의 고객들이 이미 이탈한 상태였으므로, 불필요한 투자를 줄일 수 있었다는 점에서 분석이 유용
    - 이탈 위험이 있는 고객층이 지속적으로 유입되고 있으며, 일시적으로 상품을 구매해보는 ‘찍먹’ 고객층이 유지되고 있음을 확인
        - 각 군집에 맞춘 맞춤형 전략을 수립하여 고객 만족도를 높이고, 이탈율을 감소시키며, 신규 고객 유입을 촉진할 수 있는 방안을 제안
- **고객 행동 시계열 분석(CLTV)**
    - 2016년에 대규모 이탈과 2017년에 특별 프로모션 또는 이벤트가 있었던 것으로 추정되나, 이에 대한 자세한 데이터가 부족하여 확정 짓기 어려웠던 것이 아쉬움
        - 관련 데이터를 추가로 수집할 경우, 더욱 효과적인 마케팅 전략을 수립하는 데 도움이 될 것
    - RFM 값으로 고객을 세분화하고 있지만, 성별이나 나이와 같은 추가적인 지표를 통합한다면 더욱 구체적이고 효과적인 맞춤 전략을 개발할 수 있을 것
- **종합**
    - 문제 정의 및 KPI를 확실하게 설정함으로써 목표 달성의 방향성을 명확히 함
    - 전처리
        - 매우 꼼꼼한 전처리를 통해 데이터 정합성 확인



