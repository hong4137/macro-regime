# Phase 11 — New Architecture Specification

**Version**: 1.0
**작성일**: 2026-05-02
**기반**: System Audit + Market Prediction Alternatives + Round 4 Free vs Paid ROI
**Tier**: Tier 1 (무료 stack) → Tier 2 (선택 $4k/year) phased
**제외**: Korean market, Bloomberg Terminal-class paid data

---

## 1. Design Principles

### 1.1 사용자 fundamental insights (이 architecture 의 출발점)

> **"필연적 stress 인가?"** — Indicator 50% 가 검증 안 됨 (USEPU 등) → 검증된 것만 사용
>
> **"수급 capture 하나?"** — 8가지 flow 영역 중 0개 cover → Flow Layer 추가
>
> **"누더기 보완 안 됨"** — Patch 거부, 완전 재설계

### 1.2 Architecture 핵심 원칙

1. **Validated indicators only** — 학술 검증 + Round 2 episode 검증 통과한 것만
2. **Flow as core** — stress signal 과 동등 weight 의 새 layer
3. **Market-truth scoring** — SPY return 기반 검증, fusion-vs-fusion 폐기
4. **Crisis-type aware** — universal regime detector 포기, type-specific
5. **Tier 1 무료 우선** — 8주 검증 후 Tier 2 ($4k) 결정
6. **점진 migration** — 기존 시스템 병렬 운영, 검증 후 cutover

---

## 2. Layer Architecture (6 Layers)

### Layer 0: 기각된 부분 (제거)

| 기각 항목 | 이유 |
|---|---|
| **USEPU spot/5d MA/30d** | Round 2: 0/7 episodes detection |
| **GEPU** | Round 2: 효과 없음 |
| **USDKRW EM stress** | Korean specific, 글로벌 영향 작음 |
| **Catalyst-driven 일률 stress 가정** | BoJ dovish 무시, 4/27-29 false positive 원인 |
| **Catalyst density M2 modifier ×1.15** | Catalyst 자체 = 위험 으로 잘못 가정 |
| **Geopolitics Agent v2 전체** | 위 indicator 만 사용, 실효성 부족 |
| **Fusion-vs-fusion scoring** | Circular validation, 진짜 시장 결과 무관 |
| **5d/7d cosmetic copy** | Phase 9.2.2: 100%/89.3% correlation 입증 |
| **Korean market 특화** | 사용자 결정 (배제) |
| **TimesFM 단독 사용** | 금융 R² = -2.80% (Round 2) |

### Layer 1: Validated Stress (유지 + 정리)

**기존 Liq Agent 유지, 단 검증된 indicator 만**:

#### Tier 1 Leading (학술 + 실증 검증)
| Indicator | Lead | Source |
|---|---|---|
| SAHM Rule | 침체 1-3M | FRED SAHMREALTIME |
| SLOOS Lending | 신용 긴축 1-3M | FRED DRTSCILM |
| Baa-Aaa spread | flight 1-3W | FRED DBAA - DAAA |
| KRE/SPY ratio | sector 2-4W | yfinance |
| HYG/IEF ratio | credit 1-2W | yfinance |
| USDJPY | carry 5-7d | FRED DEXJPUS |
| NFCI | conditions | FRED NFCI |

#### Tier 2 Coincident
| Indicator | 역할 | Source |
|---|---|---|
| VIX | volatility | FRED VIXCLS |
| YC 10Y-2Y | recession leading | FRED T10Y2Y |
| BREAKEVEN_10Y | inflation expectation | FRED T10YIE |
| UST_10Y / UST_2Y | yields | FRED |

→ Liq prompt 정리: 위 11개만 input. **USEPU/GEPU/USDKRW 등 제거**.

### Layer 2: ★ Flow Layer (NEW)

**가장 큰 추가 — 사용자 질문 #2 직접 대응**.

#### 2.1 CFTC Speculator Positioning
- **Source**: CFTC FinFutWk.txt (free, weekly Friday)
- **Coverage**:
  - ES futures (S&P 500) — equity speculator
  - NQ futures (Nasdaq) — tech speculator
  - VIX futures — vol speculator
  - JPY futures — already in carry detector
- **Lag**: 3 trading days (Tuesday data, Friday published)
- **Logic**:
  - Speculator extreme positioning (>1.5σ from 1y mean) = reversal risk
  - Net long/short trend (5d Δ) = momentum
  - Combined CFTC stress score 0-1

#### 2.2 ETF Flows
- **Source**: yfinance (SPY/QQQ/IWM/sector ETFs daily AUM)
- **Logic**:
  - AUM Δ vs price Δ → infer net flow (rough proxy)
  - Sector rotation (XLF/XLE/XLK rolling correlation)
  - Bond ETF flows (TLT/HYG/LQD)
- **Output**: Flow regime (risk-on / neutral / risk-off)

#### 2.3 Options Microstructure
- **Source**: CBOE option chain (free), put/call ratio
- **GEX self-compute**:
  - SPX option open interest × delta
  - Daily dealer gamma exposure estimate
  - Positive GEX = stable, negative GEX = cascade risk
- **Tier 2 옵션** (8주 검증 후): SpotGamma Essential ($1.2k/year)

#### 2.4 Sentiment Positioning (contrarian)
- **AAII bullish/bearish** (weekly, free RSS)
- **NAAIM Exposure Index** (weekly, free)
- **Investor's Intelligence** (weekly, free)
- **Logic**: Extreme bullish (>60%) = reversal risk, extreme bearish = bottom signal

#### 2.5 Fund Flows
- **ICI weekly Mutual Fund flows** (free)
- **Lipper basic** (free aggregate)
- **TIC monthly** (international flows, free)

#### 2.6 Margin Debt
- **FINRA monthly margin debt** (free)
- **Reg T total** (free)
- **Logic**: Margin spike + decline = leverage cascade leading

→ Flow Layer = 6 sub-detectors → **Flow regime classifier** (M8 mechanism)

### Layer 3: ★ News/Sentiment Layer (NEW)

#### 3.1 FinBERT News Sentiment
- **Source**: FinBERT (HuggingFace, free, 97.5% accuracy)
- **Input**: Reuters / Bloomberg RSS / WSJ RSS / FT RSS
- **Output**: Daily sentiment score per asset class
- **Update**: Hourly aggregation, daily snapshot

#### 3.2 Macro Narrative
- **Source**: FOMC minutes, BoJ statements, ECB releases
- **Input**: NLP on official communications
- **Hawkish/Dovish detection**

#### 3.3 Crisis Clustering Detection
- **Logic**: News topic clustering (LDA or sentence embeddings)
- **Crisis topic suddenly clusters** = early warning
- **Volume of crisis-related articles** as signal

→ Sentiment Layer = News sentiment + Macro narrative + Crisis clustering → **Sentiment regime**

### Layer 4: ★ Numerical Forecasting (NEW)

#### 4.1 Chronos-2 Foundation Model
- **Source**: Amazon Chronos-2 (HuggingFace, free)
- **Inputs**: SPY, VIX, UST 10Y, USDJPY (4 univariate or multivariate)
- **Output**: 7d ahead probabilistic forecast (quantiles)
- **Use**: Cross-validation with LLM regime view

#### 4.2 (선택) TimesFM ensemble
- 다른 foundation model 비교
- Ensemble averaging

→ Numerical Layer = direct 7d ahead 예측, LLM 의 reasoning 보완

### Layer 5: New Fusion + Cascade

#### 5.1 새 Mechanism (M8-M10 추가, M1-M7 유지)

```
Existing M1-M7 (validated indicators 만):
   M1: Confidence-weighted base
   M2: ~~Catalyst density modifier~~ (제거 — false positive 원인)
   M3: Sideways/Bear/Crisis resonance
   M4: Single-agent escalation
   M5: Crisis override
   M6: Renormalize
   M7: Asymmetric hysteresis

New M8-M10:
   M8: Flow Regime Modifier
       Flow bearish → stress signal weight ↑
       Flow bullish → stress signal weight ↓ (false positive 통제)

   M9: Sentiment Overlay
       FinBERT sentiment 와 stress 일관성 체크
       Crisis clustering 점등 시 alert ↑

   M10: Numerical Confirmation
       Chronos-2 forecast 와 LLM regime view 일치 시 confidence ↑
       불일치 시 uncertainty flag
```

#### 5.2 새 Cascade Scenarios (사용자 통찰 직접 반영)

| Scenario | 조건 | 의미 | Action |
|---|---|---|---|
| `genuine_calm` | stress LOW + flow BULLISH + sentiment NEUTRAL+ | 진짜 평시 | risk-on 유지 |
| `narrative_scare` ★ | **stress HIGH + flow BULLISH** + sentiment NEUTRAL | 시장이 stress 무시 (4/27-29 같은 케이스) | **alert 약화**, 관망 |
| `smart_money_warning` ★ | **stress LOW + flow BEARISH** + insider selling | 사용자 통찰: "수급이 먼저 빠지기 시작" | **선제 헷지 권장** |
| `real_stress_brewing` | stress HIGH + flow BEARISH + sentiment NEGATIVE | 진짜 위기 형성 중 | full risk-off |
| `mechanical_cascade` | flow severe BEARISH (margin spike + GEX negative) | Volmageddon-class | 즉시 hedge |
| `regime_calm_recovery` | stress receding + flow normalizing | 회복 진행 | 점진 risk-on |
| `mixed` | 기타 | 불분명 | horizon 별 판단 |

→ **`narrative_scare`** 가 4/27-29 false positive 같은 case 를 명시적 분류. **`smart_money_warning`** 이 사용자 질문 #2 의 핵심 가치.

### Layer 6: ★ SPY Return-based Scoring (NEW)

#### 6.1 새 채점 logic

```python
def score_with_spy(prediction, as_of_date, horizon_days):
    spy_start = get_spy_close(as_of_date)
    spy_end = get_spy_close(as_of_date + business_days(horizon_days))
    spy_return_pct = (spy_end - spy_start) / spy_start * 100

    pred = prediction["alert_level"]
    cascade = prediction.get("cascade_scenario")

    # Base scoring matrix
    if pred == "GREEN" and spy_return_pct > 0.5:
        base = "HIT"
    elif pred == "YELLOW" and -1 < spy_return_pct < 1:
        base = "HIT"
    elif pred == "ELEVATED" and spy_return_pct < -1:
        base = "HIT"
    elif pred == "RED" and spy_return_pct < -3:
        base = "HIT"
    elif pred in ["ELEVATED", "RED"] and spy_return_pct > 1:
        base = "FALSE_POSITIVE"  # ★ 핵심 metric
    elif pred == "GREEN" and spy_return_pct < -3:
        base = "MISS_CRISIS"
    else:
        base = "PARTIAL"

    # Cascade-aware adjustment
    if cascade == "narrative_scare" and base == "FALSE_POSITIVE":
        base = "PARTIAL"  # narrative_scare 가 false positive 인 건 OK (자체 인식)

    return {"verdict": base, "spy_return": spy_return_pct, ...}
```

#### 6.2 핵심 metric (새)

| Metric | 의미 |
|---|---|
| **HIT rate** | regime 정확 |
| **FALSE_POSITIVE rate** ★ | stress 외쳤는데 시장 강세 (현 시스템 약점) |
| **MISS_CRISIS rate** | 위기 놓침 |
| **Brier score** | probabilistic calibration |
| **Sharpe vs benchmark** | trading 가치 |

#### 6.3 28 historical sessions 재채점

기존 sessions × SPY return 매핑 → 진짜 정확도 측정. **Phase 11.3 의 첫 검증**.

---

## 3. Implementation Roadmap (10주)

### Week 1-2: Flow Layer 구축
- CFTC parser (모든 futures, 정확성 검증)
- ETF flow estimator (yfinance 기반)
- AAII/NAAIM scraping
- ICI / TIC parser
- CBOE put/call ratio + GEX self-compute
- **Deliverable**: `flow_detector.py` 단일 모듈, daily 산출

### Week 3: Sentiment Layer
- FinBERT 셋업 (HuggingFace download + GPU/CPU)
- News RSS aggregator (Reuters, Bloomberg, WSJ, FT)
- Daily sentiment 산출
- Crisis clustering (sentence embeddings)
- **Deliverable**: `sentiment_detector.py`

### Week 4: Numerical Forecasting Layer
- Chronos-2 셋업 (HuggingFace)
- 4 univariate forecasts (SPY, VIX, UST, USDJPY)
- Backtest on 2020-2024
- **Deliverable**: `forecast_layer.py`

### Week 5: New Fusion + Cascade
- M8-M10 mechanism 구현
- 새 cascade scenarios (7 종)
- SPY return-based scoring
- **Deliverable**: `analyze_v3.py` (analyze_v2 의 후속)

### Week 6: Architecture Integration
- Layer 1-6 연결
- 새 daily pipeline (`daily_complete_v2.ps1`)
- 새 dashboard (3-column → 6-layer 표시)
- **Deliverable**: 통합 운영 가능

### Week 7-8: Tier 1 검증
- 28 historical sessions 재채점 (SPY return-based)
- 2024-2025 backtest (가능한 dense)
- **Critical metrics**:
  - FALSE_POSITIVE rate (target <20%)
  - HIT rate (target >65%)
  - MISS_CRISIS rate (target <10%)
  - Brier score
- **Deliverable**: `phase11_validation_report.md`

### Week 9-10: Decision Point
- Tier 1 결과 평가
- Tier 2 (SpotGamma $1.2k) 도입 여부 결정
- 기존 시스템과 병렬 운영 결정 (1-2개월?)
- Production cutover plan

---

## 4. 보존 항목 (재설계에서도 유지)

### 4.1 Code 보존
| 모듈 | 역할 | Phase 11 변경 |
|---|---|---|
| `analyze_v2.py` | LLM Liq agent + M1-M7 fusion | 새 `analyze_v3.py` 로 fork. Geo 제거, Flow/Sentiment/Forecast layer 추가 |
| `scoring.py` | T+1/T+3/T+5 채점 | SPY return 기반 재구현 |
| `daily_dashboard.py` | v3 dashboard | 6-layer view 로 확장 |
| `position_sizing.py` 등 | 3 mode + Hybrid | 그대로 (input 만 새 alert) |
| `publish.ps1` | GitHub 동기화 | 그대로 + 새 파일 화이트리스트 |
| `daily_complete.ps1` | 8단계 pipeline | `daily_complete_v2.ps1` 로 확장 |
| `detectors/carry_trade_detector.py` | Phase 10 v3-2layer | Flow Layer 의 sub-component 로 통합 |

### 4.2 Data 보존
- `analyses/liq_output_*.json` (28 sessions, 검증된 부분만 사용)
- `snapshots/` 인프라
- `alert_history_*_live.jsonl` (참고용)
- GitHub Pages 인프라

---

## 5. 새 Architecture 시각화

```
┌─────────────────────────────────────────────────────────────┐
│  Phase 11 New Architecture (Tier 1 무료 stack)              │
└─────────────────────────────────────────────────────────────┘

┌──── Layer 1: Validated Stress ────┐    ┌──── Layer 2: Flow ─────┐
│  LLM Liq Agent (validated only)   │    │  CFTC Positioning      │
│  - SAHM, SLOOS, Baa-Aaa           │    │  ETF Flows             │
│  - KRE/SPY, HYG/IEF, USDJPY       │    │  Options GEX           │
│  - NFCI, VIX, YC, BREAKEVEN       │    │  AAII/NAAIM            │
│  Stress Probability               │    │  Fund Flows / Margin   │
└──────┬────────────────────────────┘    │  Flow Regime Score     │
       │                                  └──────┬─────────────────┘
       │                                         │
┌──── Layer 3: Sentiment ───────────┐    ┌──── Layer 4: Forecast ─┐
│  FinBERT News Sentiment           │    │  Chronos-2 (HF)         │
│  Macro Narrative (FOMC NLP)       │    │  - SPY 7d forecast      │
│  Crisis Clustering                │    │  - VIX 7d forecast      │
│  Sentiment Score                  │    │  - UST/USDJPY forecast  │
└──────┬────────────────────────────┘    └──────┬─────────────────┘
       │                                         │
       └─────────────┬───────────────────────────┘
                     │
       ┌──── Layer 5: Fusion + Cascade ────┐
       │  M1-M7 (existing, validated)       │
       │  M8: Flow Regime Modifier ★        │
       │  M9: Sentiment Overlay ★           │
       │  M10: Numerical Confirmation ★     │
       │                                    │
       │  New Cascade Scenarios:            │
       │  - genuine_calm                    │
       │  - narrative_scare ★               │
       │  - smart_money_warning ★           │
       │  - real_stress_brewing             │
       │  - mechanical_cascade              │
       │  - regime_calm_recovery            │
       │  - mixed                           │
       └──────────────┬─────────────────────┘
                      │
       ┌──── Layer 6: SPY Return Scoring ──┐
       │  HIT / FALSE_POSITIVE / MISS_CRISIS│
       │  Brier score / Sharpe              │
       └──────────────┬─────────────────────┘
                      │
              ┌───────┴───────┐
              │ Position      │
              │ Sizing (3+    │
              │ Hybrid)       │
              │ Dashboard v4  │
              └───────────────┘
```

---

## 6. 예상 효과 (Round 4 기반)

| Metric | 현 시스템 (추정) | Phase 11 Tier 1 | Phase 11 Tier 2 |
|---|---|---|---|
| Regime accuracy | 50-60% (fusion-vs-fusion) | **70-75%** (SPY return) | **80-85%** |
| FALSE_POSITIVE rate | 50%+ (4/27-29 같은 케이스) | <20% | <15% |
| MISS_CRISIS rate | 미측정 | <15% | <10% |
| Cost | $0 (engineering ~$10k) | $0 (engineering ~$10k) | $4k/year (+ engineering) |
| 7d horizon utility | low (cosmetic copy) | high (specialized layers) | very high |

---

## 7. Open Questions (구현 중 결정)

1. **Geo Agent 완전 폐기 vs 축소** — Catalyst calendar 만 유지?
2. **Multi-horizon (24h/5d/7d) 유지 vs 단순화** — 새 architecture 에서 의미?
3. **Cascade scenarios 7개로 충분?** — 추가 패턴 필요?
4. **Tier 2 SpotGamma 8주 후 결정** — 그 시점에 어떤 metric 으로 판단?
5. **Korean market 완전 배제 vs 일부 무료 (KRX 외국인) 유지** — 사용자 결정 (배제) 존중

---

## 8. 다음 단계

**Phase 11.3 — Week 1: Flow Layer 구축 시작**

가장 critical 한 module. CFTC parser 부터 (Phase 10 v3-2layer 의 carry detector 에서 부분 검증됨).

- `flow_detector.py` 작성
- CFTC FinFutWk.txt parser (모든 futures: ES, NQ, VIX, JPY, EUR, etc.)
- yfinance ETF flows
- AAII/NAAIM weekly scraping
- 첫 통합 테스트

**또는** 사용자가 architecture 검토 후 변경 원하면 spec update.

이 spec 으로 진행 결정 부탁드립니다.
