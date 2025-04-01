> 📘 **자세한 내용은 _"최종- 분석 결과 보고서 - 제6회 빅데이터 경진대회"_ 를 참고해주세요.**  
> 🏆 **본 프로젝트는 _제6회 빅데이터 경진대회_에서 우수상을 수상한 작품입니다.**

---

# 대구시 전기차 충전소 태양광 발전 타당성 분석

## Ⅰ. 분석 개요

### ▢ 분석 목적
- **발전량 예측**  
  대구시 전기차 충전소별 일사량 및 기후 데이터를 기반으로 태양광 패널 발전량을 예측하여, 효율적인 발전 전략 수립에 기여.
- **충전 패턴과의 상관관계 분석**  
  예측된 발전량과 전기차 충전량 사이의 상관관계를 분석하여 에너지 소비 최소화 방안 도출.
- **태양광 발전 타당성 평가**  
  발전량과 충전량을 비교 분석하여 충전소에 태양광 패널 설치의 타당성 평가.

### ▢ 배경 및 필요성
- 대구시의 태양광 사업은 초기 투자 문제로 지연되고 있음.
- 전기차 충전소는 공공기관 직영 비중이 높아 설치 및 관리가 용이.
- 2023년 태양광 모듈 가격 하락 → 설치비용 절감 기대.
- 태양광 에너지는 지속 가능한 친환경 에너지원으로 주목받음.
- 대구공공시설관리공단 발전소 평균 효율: **1.203 MWh/kW**
- 설치비용 예측 (QH 테크 기준): 70억~130억 원 (10kW당 988만~1833만 원)
- 2022년 충전량: 약 **8.5 GWh**
- 필요 용량 추산: 약 **7.1 MW**

### ▢ 분석 요약

- **활용 데이터**
  - 한국동서발전(제주) 기후·발전 현황
  - 한국에너지기술연구원 태양에너지 자원 정보
  - 기상청 종관기상관측(ASOS)
  - 대구시 전기차 충전소 구축 현황

- **분석 도구**  
  `Python`, `Scikit-learn`, `Excel`

- **분석 기법**
  - 지도 API 활용 좌표 변환 → 일사량 API 수집
  - Heatmap 시각화, StandardScaler 정규화
  - XGBoost + GridSearchCV로 발전량 예측

- **분석 결과 요약**
  - 여름철 과잉 발전 대비 에너지 판매 전략 필요
  - 겨울철 수요 대비 ESS 도입 필요
  - 위탁운영 충전소의 데이터는 일부 기간 이상치로 분석에서 제외됨
  - 상관분석 결과: 충전량과 발전량 **상관관계 없음**

- **활용 방안**
  - 계절별 전력 수급 계획 수립
  - 공공 인프라를 활용한 태양광 확대 가능
  - 에너지 자립 및 전력비 절감 가능

- **독창성**
  - 단순 발전량 예측을 넘어, **전기차 충전 인프라 확충**과 연결한 정책적 모델 제시

---

## Ⅱ. 분석 방법

### ▢ 활용 데이터

| 데이터명 | 대상기간 | 사용 변수 | 출처 |
|---------|---------|----------|------|
| 제주 기상·발전 데이터 | 2020~2022 | 기온, 강수량, 일사량 등 | 공공데이터포털 |
| 태양에너지 시공간 자원정보 | 2022 | 위도, 경도, 일사량 | 공공데이터포털 |
| 종관기상관측(ASOS) | 2022 | 기온, 일사량 등 | 기상청 |
| 대구시 충전소 현황 | 2022 | 충전소명, 충전량 등 | 빅데이터활용센터 |
| Google Maps | X | 좌표 변환 | Google API |

---

### ▢ 분석 절차 요약

1. **기후 및 발전량 데이터 전처리**
   - 시간 단위 기준
   - 결측값 제거
   - Z-score 이상치 제거
   - `StandardScaler`로 정규화
   - XGBoost → 결정계수: **92%**
   - GridSearchCV → 하이퍼파라미터 최적화

2. **좌표 수집 및 검증**
   - Google Maps API → 정확도 낮음 → 네이버 로드뷰 병행
   - 실측 좌표와 최대 1km 오차 → 수동 보정

3. **일사량 데이터 수집**
   - 한국에너지기술연구원 API 사용
   - SSL/TLS 설정, JSON 파싱
   - 자동화 알고리즘 설계 (예외처리 포함)

4. **기후 데이터 통합**
   - 대구 지역 특성상 지역 간 기후 차이 무시
   - 단위 통일 및 결측치 제거

5. **충전량 데이터 전처리**
   - 쉼표, 단위 문자 제거
   - NULL 값 → 0으로 대체
   - 월별, 시간별 집계 및 이상치 제거

---

## Ⅲ. 분석 결과

- **충전 특성**
  - **시간대별**: 12시 ~ 19시에 충전량 높음
  - **월별**: **겨울철 충전량 증가** → ESS 필요

- **SHAP 분석 결과**
  - 일사량, 이전시간 온도, 전운량 순으로 발전량에 영향

- **패널 용량 추천**
  - 공단직영 충전소: **56kW ~ 78kW**
  - 위탁운영 충전소: **약 56.96kW**

- **상관분석**
  - 충전량 vs 발전량: **상관관계 없음**
  - 충전량: 비교적 일정, 겨울철 증가
  - 발전량: 여름에 증가, 낮에 급증

- **설치 환경 분석**
  - 대부분 충전소는 주차장에 위치 → 설치 용이
  - 일부 충전는은 **그늘 영향** 고려 필요

---

## Ⅳ. 활용 방안

- **적용 부문**
  - 대구시 공공 전기차 충전소 에너지 자립 전략 수립

- **정책 활용**
  - 태양광 발전량과 충전 패턴 기반 운영 전략 수립
  - ESS 도입 통한 계절 변동 대응
  - 보조금 정책 연계 통한 초기 설치비 완화

---

## Ⅴ. 기타 참고 자료

- [국내 도심 산단태양광 프로젝트 착공](https://www.industrynews.co.kr/news/articleView.html?idxno=48551)
- [대구시 3조 태양광 사업, 난항](https://www.newsmin.co.kr/news/105322/)
- [태양광 기술 격차와 탠덤 셀](https://industrynews.co.kr/news/articleView.html?idxno=50219)
- [예측 알고리즘 GitHub](https://github.com/cih468/solarPower)
- [대구공공시설관리공단 - 태양광발전 정보](https://www.dgeic.or.kr/dgeic_doc.html?cid=286)

---

## 📌 결론


---

> **결론**  
공단직영 상위 10개 충전소에는 56kW ~ 78kW 수준의 태양광 패널 설치가 적절합니다. 계절에 따른 변동성을 고려하여 ESS와 같은 보완 장치를 함께 도입해야 하며, 대부분 주차장에 위치해 설치에 대한 부지 문제는 크지 않을 것으로 판단됩니다.


