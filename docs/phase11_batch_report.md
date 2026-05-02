# Phase 11 Batch Backtest Report

**생성**: 2026-05-02 21:21  
**Sessions discovered**: 29  
**Phase 11 fusion 완료**: 29  
**SPY scoring 완료**: 29  
**Total scored predictions**: 113

## 1. Overall Verdict

| Verdict | Count | % |
|---|---|---|
| **HIT** | 37 | 32.7% |
| **PARTIAL** | 38 | 33.6% |
| **FALSE_POSITIVE** | 35 | 31.0% |
| **MISS_CRISIS** | 3 | 2.7% |

## 2. By Alert Level

| Alert | HIT | PARTIAL | FP | MISS | Hit% | FP% |
|---|---|---|---|---|---|---|
| GREEN | 20 | 26 | 0 | 3 | 40.8% | 0.0% |
| YELLOW | 7 | 1 | 0 | 0 | 87.5% | 0.0% |
| ELEVATED | 9 | 9 | 18 | 0 | 25.0% | 50.0% |
| RED | 1 | 2 | 17 | 0 | 5.0% | 85.0% |

## 3. By Cascade Scenario

| Scenario | HIT | PARTIAL | FP | MISS | Hit% | FP% |
|---|---|---|---|---|---|---|
| `acute_shock` | 1 | 2 | 5 | 0 | 12.5% | 62.5% |
| `confirmed_crisis` | 0 | 0 | 8 | 0 | 0.0% | 100.0% |
| `genuine_calm` | 19 | 23 | 0 | 3 | 42.2% | 0.0% |
| `mixed` | 10 | 6 | 12 | 0 | 35.7% | 42.9% |
| `real_stress_brewing` | 6 | 4 | 10 | 0 | 30.0% | 50.0% |
| `smart_money_warning` | 1 | 3 | 0 | 0 | 25.0% | 0.0% |

## 4. By Episode

| Episode | HIT | PARTIAL | FP | MISS | Hit% | FP% |
|---|---|---|---|---|---|---|
| 2022 Bear | 2 | 3 | 7 | 0 | 16.7% | 58.3% |
| 2023-Q4 | 4 | 4 | 0 | 0 | 50.0% | 0.0% |
| 2024 Election | 0 | 1 | 3 | 0 | 0.0% | 75.0% |
| COVID 2020-Q1 | 1 | 2 | 6 | 3 | 8.3% | 50.0% |
| Calm 2017-2021 | 2 | 11 | 3 | 0 | 12.5% | 18.8% |
| Calm 2024-Q1 | 4 | 0 | 0 | 0 | 100.0% | 0.0% |
| Live 2026 | 8 | 5 | 0 | 0 | 61.5% | 0.0% |
| SVB 2023-Q1 | 3 | 5 | 12 | 0 | 15.0% | 60.0% |
| Stress 2025 | 3 | 1 | 0 | 0 | 75.0% | 0.0% |
| Yen Carry 2024-08 | 10 | 6 | 4 | 0 | 50.0% | 20.0% |

## 5. By Horizon

| Horizon | HIT | PARTIAL | FP | MISS | Hit% | FP% |
|---|---|---|---|---|---|---|
| T+1 | 9 | 14 | 6 | 0 | 31.0% | 20.7% |
| T+3 | 9 | 10 | 9 | 1 | 31.0% | 31.0% |
| T+5 | 10 | 7 | 10 | 1 | 35.7% | 35.7% |
| T+7 | 9 | 7 | 10 | 1 | 33.3% | 37.0% |

## 6. 핵심 발견

- **Overall HIT rate**: 32.7%
- **FALSE_POSITIVE rate**: 31.0% (사용자 통찰 #2 효과 측정)
- **MISS_CRISIS rate**: 2.7% (위기 놓침)
