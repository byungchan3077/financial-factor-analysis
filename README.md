# Financial Factor Analysis

## Overview
S&P500 수익률 예측 가능성을 거시경제 변수와 XGBoost로 검증하고,
Fama-French 3 Factor 프리미엄의 시장 반영 과정을 분석한 프로젝트.

## Research Questions
> 1. "어제의 거시경제 변수로 오늘의 S&P500 수익률 방향을 예측할 수 있는가?" (약형 EMH)
> 2. "Fama-French 논문 발표 후 SMB/HML 프리미엄이 실제로 감소했는가?" (시장 효율성)

## Data

### EMH 검증
| 변수 | 티커 | 설명 |
|------|------|------|
| S&P500 | ^GSPC | 미국 대표 시장지수 |
| 금리 | ^TNX | 미국 10년물 국채 |
| VIX | ^VIX | 시장 공포지수 |
| 유가 | CL=F | WTI 원유 선물 |
| 달러 | DX-Y.NYB | 달러 인덱스 |

- 기간: 2000 ~ 2026
- 출처: Yahoo Finance (yfinance)

### Fama-French 분석
| 변수 | 설명 |
|------|------|
| SMB | 소형주 - 대형주 수익률 |
| HML | 가치주 - 성장주 수익률 |
| Mkt-RF | 시장 초과수익률 |

- 기간: 1963 ~ 2026
- 출처: Kenneth French Data Library

## Methodology

### Part 1 - 약형 EMH 검증
1. 일별 수익률 변환 (pct_change)
2. Lag 피처 생성 (t-1, t-2)
3. 시계열 분리 (train 80%, test 20%)
4. XGBoost Regressor 학습
5. 방향 정확도 + 이항검정 (p-value)

### Part 2 - Fama-French 프리미엄 분석
1. FF3 데이터 월단위 수집 (1963~)
2. 논문 발표 기준 (1992) 전/후 분리
3. SMB, HML 월평균 수익률 비교
4. 12개월 이동평균으로 추세 시각화

## Key Findings

### Part 1 - EMH 검증
| 지표 | 값 |
|------|-----|
| 테스트 R² | -0.07 |
| 방향 정확도 | 52.16% |
| p-value | 0.063 |

### Part 2 - 팩터 프리미엄 변화
| 팩터 | 발표 전 월평균 | 발표 후 월평균 | 감소율 |
|------|--------------|--------------|--------|
| SMB | 0.246% | 0.056% | 77% ↓ |
| HML | 0.399% | 0.227% | 43% ↓ |

## Conclusion

### Part 1 - EMH 검증
- 방향 정확도 52%는 통계적으로 유의미하지 않음 (p=0.063)
- 설령 유의미해도 수수료/슬리피지 감안 시 실용적 의미 없음
- 공개된 거시경제 정보는 이미 가격에 반영됨 → **약형 EMH 지지**

### Part 2 - Fama-French 프리미엄
- SMB 77%, HML 43% 감소 → 논문 발표 후 시장이 프리미엄 반영
- "정보 공개 → 기대수익률 재조정 → 가격 상승 → 프리미엄 소멸" 메커니즘 확인
- HML 프리미엄 잔존 이유: 가치주 분석에 드는 정보 취득 비용이 프리미엄을 상쇄
  → 분석 비용 자체가 진입장벽으로 작용

### 종합
- 정보 취득에 비용이 드는 한 프리미엄이 완전히 사라지면 아무도 분석하지 않음
- 결국 시장은 완전한 효율성과 비효율성 사이 균형점으로 수렴
- **정보가 공개될수록 효율적으로 수렴하지만, 정보 비용이 존재하는 한 완전한 EMH는 성립하지 않는다**

## Note
**데이터 누수 주의:**
당일 변수 포함 시 방향 정확도 77% → lag만 사용 시 52%로 하락
실전 예측은 반드시 lag 피처만 사용해야 함

## Requirements
pandas, numpy, matplotlib, seaborn, yfinance, pandas-datareader, xgboost, scikit-learn, scipy
