# Financial Factor Analysis

## Overview
S&P500 수익률 예측 가능성을 거시경제 변수와 XGBoost로 검증한 프로젝트.
약형 EMH(Efficient Market Hypothesis) 실증 분석.

## Research Question
> "어제의 거시경제 변수로 오늘의 S&P500 수익률 방향을 예측할 수 있는가?"

## Data
| 변수 | 티커 | 설명 |
|------|------|------|
| S&P500 | ^GSPC | 미국 대표 시장지수 |
| 금리 | ^TNX | 미국 10년물 국채 |
| VIX | ^VIX | 시장 공포지수 |
| 유가 | CL=F | WTI 원유 선물 |
| 달러 | DX-Y.NYB | 달러 인덱스 |

- 기간: 2000 ~ 2026
- 출처: Yahoo Finance (yfinance)

## Methodology
1. 일별 수익률 변환 (pct_change)
2. Lag 피처 생성 (t-1, t-2)
3. 시계열 분리 (train: 80%, test: 20%)
4. XGBoost Regressor 학습
5. 방향 정확도 + 통계적 유의성 검정

## Key Findings
| 지표 | 값 |
|------|-----|
| 테스트 R² | -0.07 |
| 방향 정확도 | 52.16% |
| p-value | 0.063 |

## Conclusion
- 방향 정확도 52%로 동전 던지기(50%) 대비 소폭 높음
- p-value 0.063으로 0.05 기준 통계적 유의성 없음
- **약형 EMH 기각 불가** — 어제 데이터로 오늘 수익률 예측 어려움
- 단, p-value 0.10 기준으로는 유의미 → 완전한 효율성도 단정 불가

## Note
데이터 누수 주의:
당일 변수 포함 시 방향 정확도 77% → lag만 사용 시 52%로 하락
실전 예측은 반드시 lag 피처만 사용해야 함

## Requirements
pandas, numpy, matplotlib, seaborn, yfinance, xgboost, scikit-learn, scipy

## Author
