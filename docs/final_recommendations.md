# 7d 위기/기회 사전 감지 — 최종 권장 (Round 1 + Round 2 통합)

**생성**: 2026-04-30
**Phase**: 10 결정 직전
**Total Sources**: 6 research documents (~30,000 단어), 70+ 방법론, 7 historical episodes

---

## ★ Executive Summary — 솔직한 결론

### 🚨 Round 1 학술 추천 대부분 invalidated

1차 리서치의 Top 5 (BOCPD/VIX TS/Credit momentum/TimesFM/GPR) 가 7 historical episodes (SVB, COVID, 2022 Bear, Yen 2024, Volmageddon, 중국 2015, Brexit) 에서 거의 detection 못함.

### 💡 가장 중요한 Practitioner Insight

> **Top quant funds 는 7d regime prediction 자체를 포기하고, regime-aware alpha extraction 으로 pivot.**
>
> Bridgewater, AQR, Citadel 모두 HMM/VAR 같은 단순 baseline 사용. Foundation Model (TimesFM, Chronos) 은 금융 데이터에서 R² = -2.80% (즉 단순 평균보다 못함). LLM agents 는 news parsing 보조 역할.

### ✓ Round 2 가 검증한 진짜 강한 신호 5가지

1. **Granger Causality (Policy → Prices)** — 4/7 episodes
2. **Carry Trade Metrics** (rate differential + leverage) — 2/2 FX crises
3. **Leverage/Margin Surveillance** (CFTC futures margin) — 2/3 leverage events
4. **Currency Basis Divergence** (CNH-CNY) — 2/2 FX crises
5. **Polling + Betting Aggregation** — 1/1 political events

→ **모두 specialized data + crisis-type-specific**. 일반 macro indicator 가 아닌 **structural pre-crisis signals**.

---

## 1. Round 1 vs Round 2 Detailed Comparison

### 1.1 Round 1 Top 10 — Round 2 검증 결과

| # | Round 1 Method | Empirical Hit | Practitioner Use | 최종 평가 |
|---|---|---|---|---|
| 1 | BOCPD + NFCI | **0/7** | confirmation only | ❌ Reject |
| 2 | VIX Term Structure | 1/7 | confirmation only | ❌ Reject (단독) |
| 3 | Credit spread momentum | 1/7 | confirmation only | ❌ Reject (단독) |
| 4 | TimesFM/Chronos | 시뮬 어려움 | R² = -2.80% | ❌ Reject |
| 5 | GPR Index | 0/7 | 효과 없음 | ❌ Reject |
| 6 | Carry trade risk | **2/2 FX** | specialized 사용 | ✅ **Validated** |
| 7 | TradingAgents framework | 미검증 | 부분 사용 (news) | ⚠️ Partial |
| 8 | BDI + 주간 labor | 0/7 (lag) | minor | ❌ Reject |
| 9 | Granger causality | **4/7** | research→production | ✅ **Validated** |
| 10 | Long-context LLM + RAG | 미검증 | macro narrative | ⚠️ Partial |

### 1.2 Round 2 가 새로 발굴한 강한 신호

| # | Method | 검증 episodes | Round 1 에 없었던가? |
|---|---|---|---|
| 1 | **Leverage/Margin (CFTC futures)** | Volmageddon, Yen 2024 | ★ 새로 발굴 |
| 2 | **Currency Basis Divergence** (CNH-CNY) | 중국 2015, Yen 2024 | ★ 새로 발굴 |
| 3 | **Insider selling + deposit concentration** | SVB | ★ 새로 발굴 |
| 4 | **Polling + Betting odds** | Brexit | ★ 새로 발굴 |
| 5 | **Implied-realized vol skew** | Volmageddon | ★ 새로 발굴 |
| 6 | **Sentiment-to-Volatility Models** (Practitioner) | 5d vol 65-70% accuracy | ★ Round 1 미커버 |

→ **Round 1 의 광범위 카테고리 분류는 실제 episode 에서 작동하는 specialized signals 를 놓침**.

---

## 2. 검증된 신호의 공통 특징

### 2.1 Crisis-type-specific (보편적 단일 지표는 없다)

| Crisis Type | Best Signal | 7d Lead |
|---|---|---|
| 통화정책 전환 (2022 Bear) | Yield curve flattening velocity + Granger | 10-14d |
| Carry trade unwind (Yen 2024) | Rate differential + CFTC positioning + margin | 5-7d |
| Leverage cascade (Volmageddon) | Implied-realized vol skew + short-vol ETP flow | 3-5d |
| FX devaluation (중국 2015) | CNH-CNY basis spike + PBOC fixing band extreme | 7-10d |
| 정치 이벤트 (Brexit) | Polling + betting odds | 3d |
| 은행 위기 (SVB) | Insider selling + deposit concentration | <3d |
| Pandemic (COVID) | Epidemic doubling time | 7-14d |

→ **하나의 universal regime detector 는 존재하지 않음**. Crisis type 별 specialized signal 필요.

### 2.2 Price-based vs Flow-based

- **Price-based** (VIX, credit spread, NFCI) → 위기 직전에 false comfort 주기 일쑤
- **Flow-based** (CFTC positioning, deposit data, insider trades) → 진짜 leading
- **Microstructure** (basis divergence, vol skew) → 5-7d 에 가장 유효

→ **일반 FRED data 만으로는 부족**. CFTC, SEC EDGAR, 중앙은행 micro data 필요.

### 2.3 단일 vs Multi-signal Confirmation

- 단일 signal: false positive 많음
- Multi-signal confirmation (예: 3개 이상 동시 점등): 신뢰도 ↑
- **현재 시스템의 cascade signal 컨셉은 valid** — 다만 input signals 가 wrong

---

## 3. 새 Top 5 권장 (Phase 10 v2)

### 3.1 즉시 도입 (1-2주)

#### **R1. Carry Trade Risk Index** ★★★
- **데이터**: USDJPY, USDKRW (free) + 미일/한국 rate spread (FRED) + USDJPY 1M implied vol (Bloomberg or yfinance)
- **로직**: rate differential + 유효 carry yield + leverage proxy (CFTC speculator positioning)
- **검증**: Yen 2024 detect (2/2)
- **분량**: 1주

#### **R2. Currency Basis Divergence** ★★
- **데이터**: CNH (offshore) vs CNY (onshore) spot, USDKRW NDF vs onshore
- **로직**: basis > 50bp + 5d trend → FX 위기 leading
- **검증**: 중국 2015, Yen 2024
- **분량**: 3-5일
- **데이터 source**: Bloomberg, Refinitiv, free 일부 (HKEX)

#### **R3. CFTC Speculator Positioning + Margin** ★★
- **데이터**: CFTC COT (free, weekly), CME margin requirements (CME data)
- **로직**: speculator long/short imbalance + margin spike → leverage cascade leading
- **검증**: Volmageddon, Yen 2024
- **분량**: 1주

### 3.2 검토 후 도입 (2-4주)

#### **R4. Granger Causality Discovery System** ★★
- **데이터**: 30+ macro indicators (현재 시스템에 있음) + SPY/QQQ
- **로직**: rolling-window Granger causality test → 진짜 leading indicator 발굴 → fusion weight ↑
- **검증**: 4/7 episodes (Policy→Prices)
- **분량**: 2주
- **위험**: rolling-window 의 sample size, 다중 비교 보정

#### **R5. HMM 기반 Regime Persistence Tracker**
- **로직**: regime 평균 지속 기간 (5-20d) base rate → regime change probability 산출
- **Practitioner 검증**: Bridgewater, AQR 등 production workhorse
- **분량**: 1-2주

### 3.3 보류 / 폐기 (Round 1 추천 중)

- ❌ **BOCPD + NFCI** — 0/7 detection, 위기 후에만 spike
- ❌ **TimesFM/Chronos** — 금융 R² = -2.80%, 무용
- ❌ **GPR Index** — 0/7 detection
- ❌ **BDI + labor** — lag indicator
- ⚠️ **VIX TS / Credit spread** — confirmation only, 단독 사용 금지

---

## 4. Architecture 권장 — Phase 10 v2

### 4.1 새 architecture

```
[Existing 24h system]
   Liq Agent + Geo Agent → Fusion (M1-M7) → 24h alert
   (유지 — 검증됨)

[New 7d-specific layer]
   Crisis-type-specific detectors (병렬):
   ├── Carry Trade Risk      (USDJPY/USDKRW + rate diff + CFTC)
   ├── Currency Basis        (CNH-CNY, NDF basis)
   ├── Margin Cascade        (CFTC + CME margin spike)
   ├── Granger Discovery     (rolling causality)
   └── HMM Persistence       (regime base rate)
   ↓
   7d Alert (specialized, crisis-type별)

[Cascade integration]
   24h alert + 7d Alert + Crisis Type → Action
```

### 4.2 Cascade scenario 재정의

기존 cascade (regime_calm / broad_recovery / front_loaded_caution / 등 10개) 는 **crisis-type-agnostic**. 새 cascade 는 crisis-type-aware:

| Scenario | Trigger |
|---|---|
| `carry_unwind_imminent` | Carry trade risk High + Margin spike |
| `fx_crisis_imminent` | Currency basis 50bp+ + 5d trend |
| `leverage_cascade_imminent` | Margin spike + vol skew |
| `policy_pivot_imminent` | Granger Policy→Prices high + yield curve flatten |
| `regime_persist` | HMM tells current regime stable |
| `regime_transition_unknown` | Multiple weak signals, no specific pattern |

→ **Crisis type 인식이 prediction 보다 더 actionable**.

### 4.3 Top funds 의 진짜 game = "Regime-aware alpha"

만약 Phase 10 v2 도 어렵다면:
- **24h 시스템 + Crisis-type detector** 로 정확한 알림
- 그 외 시간은 **regime-specific portfolio** (e.g., bull regime SPY ↑, bear regime IEF ↑)
- "예측" 보다 "현재 regime + 그에 맞는 portfolio" 가 ROI 높음

---

## 5. 다음 결정 옵션

### Option A: Phase 10 v2 — 새 Top 5 도입 (점진적, 추천)
- 1-2주: Carry trade risk + Currency basis + CFTC margin
- 2-4주: Granger discovery + HMM persistence
- **위험**: 데이터 source 일부 paid, 작업량 증가
- **효과**: 진짜 7d 사전 신호 가능성 ↑

### Option B: Phase 10 minimal — Carry trade risk 만 (가장 빠름)
- 1주: USDJPY/USDKRW + rate diff + CFTC positioning 통합
- Yen 2024 같은 case detect 가능
- **효과**: 한 가지 specific crisis 만 cover, 나머지는 24h 시스템에 의존

### Option C: 방향 전환 — Regime-aware Alpha Extraction
- 7d prediction 포기
- 24h 시스템 유지 + regime별 portfolio strategy 자동화
- Practitioner consensus 와 일치
- **효과**: 더 robust + sustainable alpha
- **위험**: 큰 architecture 변경

### Option D: 현 시스템 최적화 + 검증 강화
- 24h 시스템 fine-tune
- T+5 채점 인프라 강화 (현재 sparse)
- 새 방법론 도입 보류
- **효과**: 안정적, 점진적
- **위험**: 7d horizon 약점 그대로

---

## 6. 솔직한 추천

### 단기 (Phase 10.1, 1-2주)
**Option B**: Carry trade risk index 만 추가. Yen 2024 detect, 8월 2024 같은 unwind 사전 신호. 무료 데이터, 1주 작업.

### 중기 (Phase 10.2, 4-8주)
**Option A**: Currency basis + CFTC margin 추가. 더 다양한 crisis type cover.

### 검토 후 (Phase 10.3, 8주+)
**Option C 부분 도입**: regime-aware portfolio (24h alert + position sizing 강화). 기존 hybrid_sizing.py 를 더 정교화. Top funds 의 game 따라가기.

### 보류
- TimesFM/Chronos foundation model (R² 검증 실패)
- BOCPD on NFCI (0/7 detection)
- 비싼 alternative data (satellite 등)

---

## 7. 학습한 교훈 (Phase 9 + 10 결합)

1. **5d/7d 단순 reweighting 안 통한다** (Phase 9.2.2 입증)
2. **학술 추천이 실제 episode 에서 안 맞을 수 있다** (Round 2 입증)
3. **Specialized signal > general indicator** for crisis detection
4. **Practitioner > Academic** for actionable insights
5. **Crisis type 인식 > Universal regime detector**
6. **Foundation Model 은 금융 underperform** — overhyped
7. **Top funds 는 prediction 보다 regime-aware alpha 에 집중**

---

## 참고: 6 research documents

1. `7d_methodology_comparison.md` — Round 1 종합 (이 문서가 이를 invalidate)
2. `group1_statistical_macro_industry.md` — Round 1 detailed
3. `group2_crossasset_sentiment_geopolitical.md` — Round 1 detailed
4. `group3_ai_ml_alternative.md` — Round 1 detailed
5. `round2_empirical_case_studies.md` — Round 2 episode validation ★
6. `round2_failure_modes_best_practices.md` — Round 2 함정 ★
7. `round2_practitioner_landscape.md` — Round 2 실무자 ★

이 문서가 종합 → Phase 10 진행 결정의 single source of truth.
