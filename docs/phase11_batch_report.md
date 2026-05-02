# Phase 11 Batch Backtest Report

**생성**: 2026-05-02 16:42  
**Sessions discovered**: 29  
**Phase 11 fusion 완료**: 29  
**SPY scoring 완료**: 29  
**Total scored predictions**: 113

## 1. Overall Verdict

| Verdict | Count | % |
|---|---|---|
| **HIT** | 32 | 28.3% |
| **PARTIAL** | 42 | 37.2% |
| **FALSE_POSITIVE** | 36 | 31.9% |
| **MISS_CRISIS** | 3 | 2.7% |

## 2. By Alert Level

| Alert | HIT | PARTIAL | FP | MISS | Hit% | FP% |
|---|---|---|---|---|---|---|
| GREEN | 17 | 26 | 0 | 3 | 37.0% | 0.0% |
| YELLOW | 5 | 2 | 0 | 0 | 71.4% | 0.0% |
| ELEVATED | 9 | 12 | 19 | 0 | 22.5% | 47.5% |
| RED | 1 | 2 | 17 | 0 | 5.0% | 85.0% |

## 3. By Cascade Scenario

| Scenario | HIT | PARTIAL | FP | MISS | Hit% | FP% |
|---|---|---|---|---|---|---|
| `genuine_calm` | 16 | 23 | 0 | 3 | 38.1% | 0.0% |
| `mixed` | 7 | 8 | 13 | 0 | 25.0% | 46.4% |
| `narrative_scare` | 1 | 2 | 0 | 0 | 33.3% | 0.0% |
| `real_stress_brewing` | 7 | 6 | 23 | 0 | 19.4% | 63.9% |
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
| Live 2026 | 6 | 7 | 0 | 0 | 46.2% | 0.0% |
| SVB 2023-Q1 | 3 | 5 | 12 | 0 | 15.0% | 60.0% |
| Stress 2025 | 0 | 3 | 1 | 0 | 0.0% | 25.0% |
| Yen Carry 2024-08 | 10 | 6 | 4 | 0 | 50.0% | 20.0% |

## 5. By Horizon

| Horizon | HIT | PARTIAL | FP | MISS | Hit% | FP% |
|---|---|---|---|---|---|---|
| T+1 | 8 | 15 | 6 | 0 | 27.6% | 20.7% |
| T+3 | 7 | 12 | 9 | 1 | 24.1% | 31.0% |
| T+5 | 8 | 9 | 10 | 1 | 28.6% | 35.7% |
| T+7 | 9 | 6 | 11 | 1 | 33.3% | 40.7% |

## 6. 핵심 발견

- **Overall HIT rate**: 28.3%
- **FALSE_POSITIVE rate**: 31.9% (사용자 통찰 #2 효과 측정)
- **MISS_CRISIS rate**: 2.7% (위기 놓침)
- **narrative_scare 시나리오**: 3 건, HIT 33.3% — false positive 통제 효과
