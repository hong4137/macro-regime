# Multi-horizon Backtest Report (Phase 9.2.2)

**생성**: 2026-05-01 00:25  
**Sessions**: 28  
**Episodes**: 10

## 핵심 질문

**"24h 로직과 데이터가 5d/7d 에 유효한가?"** — 데이터로 답합니다.

---

## 1. Overall Hit Rate (horizon × period)

| Horizon | Period | HIT | PART | MISS | NoTgt | Hit% | Direction% |
|---|---|---|---|---|---|---|---|
| 24h | T+1 | 1 | 2 | 0 | 25 | 33.3% | **100.0%** |
| 24h | T+3 | 0 | 1 | 0 | 27 | 0.0% | **100.0%** |
| 24h | T+5 | 1 | 0 | 0 | 27 | 100.0% | **100.0%** |
| 5d | T+3 | 0 | 1 | 0 | 27 | 0.0% | **100.0%** |
| 5d | T+5 | 1 | 0 | 0 | 27 | 100.0% | **100.0%** |
| 5d | T+7 | 0 | 3 | 0 | 25 | 0.0% | **100.0%** |
| 7d | T+5 | 1 | 0 | 0 | 27 | 100.0% | **100.0%** |
| 7d | T+7 | 0 | 3 | 0 | 25 | 0.0% | **100.0%** |
| 7d | T+10 | 1 | 0 | 0 | 27 | 100.0% | **100.0%** |

## 2. Episode 별 Hit Rate (HIT+PARTIAL / total)

| Episode | Sessions | 24h | 5d | 7d |
|---|---|---|---|---|
| COVID 2020-Q1 | 3 | 100% (1) | 100% (1) | 100% (1) |
| Live 2026 | 3 | 100% (2) | — | — |
| SVB 2023-Q1 | 5 | — | 100% (1) | 100% (1) |
| Yen Carry 2024-08 | 5 | 100% (2) | 100% (3) | 100% (3) |

## 3. Horizon Correlation — 단순 smoothing 검증 ★

**5d/7d 가 24h 와 거의 같으면 단순 smoothing**.

| Pair | Same Alert | Total | Correlation |
|---|---|---|---|
| 24h vs 5d | 28 | 28 | **100.0%** |
| 24h vs 7d | 25 | 28 | **89.3%** |
| 5d vs 7d | 25 | 28 | **89.3%** |

**해석 가이드**:
- ≥90%: 단순 smoothing — Phase 9.3 LLM 분리 필수
- 70~90%: 약한 차별화 — fine-tune 가능
- <70%: 의미 있는 차별화 — config 최적화 진행

## 4. 격상 (Escalation) 분석

| Horizon | 격상 횟수 | False Positive (즉시 회복) | FP% |
|---|---|---|---|
| 24h | 5 | 2 | 40% |
| 5d | 5 | 2 | 40% |
| 7d | 5 | 2 | 40% |

→ **7d 의 FP% 가 24h 보다 낮으면** 7d 가 noise filter 역할 입증

## 5. 격상 시점 (lead time 비교)

```
24h : 2019-08-05, 2020-03-16, 2023-03-13, 2024-08-05, 2026-04-29
5d  : 2019-08-05, 2020-03-16, 2023-03-13, 2024-08-05, 2026-04-29
7d  : 2019-08-05, 2020-03-16, 2023-03-13, 2024-08-05, 2026-04-29
```

## 6. Cascade Scenario 분포

| Scenario | Count | % |
|---|---|---|
| `full_stress` | 15 | 53.6% |
| `regime_calm` | 5 | 17.9% |
| `broad_recovery` | 5 | 17.9% |
| `approaching_risk` | 1 | 3.6% |
| `front_loaded_caution` | 1 | 3.6% |
| `escalating_stress` | 1 | 3.6% |

## 결론

**평균 alert correlation**: 92.9%

⚠️ **5d/7d 가 24h 와 거의 같음 — 단순 smoothing 가능성 높음**.

→ **다음 액션: Phase 9.3 LLM prompt 분리 필수**
