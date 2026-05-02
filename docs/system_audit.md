# System Audit — Macro Regime Detection System

**시스템 버전**: v2.1 (Phase 9.2.2 + Phase 10 v3-2layer Week 1)
**감사 일자**: 2026-05-02
**감사 목적**: 사용자 질문 ("필연적 stress 인가?", "수급 capture 하나?") 으로 드러난 fundamental flaw 정리

---

## 1. 시스템 개요

### 1.1 Vision (당초 의도)
거시경제 regime 변화 (bull → sideways → bear → crisis) 를 **24h ~ 7d 시점에서 사전 감지** 하는 자동화 시스템. LLM 기반 multi-agent + 통계 fusion + 시각 dashboard.

### 1.2 운영 형태
- **Local**: Windows + Python + LLM via Cowork chat
- **Data**: 무료 (FRED + yfinance + EPU + Catalyst calendar)
- **Output**: GitHub Pages 자동 동기화 (https://hong4137.github.io/macro-regime/)
- **Cycle**: 일간 (미국 장 마감 후, 한국 새벽~오전)

### 1.3 현재 운영 통계
- **Historical sessions**: 28개 (2017-09 ~ 2026-04, 9 episodes)
- **Live cycles**: 4개 (4/27, 4/28, 4/29, 5/1)
- **GitHub commits**: 30+
- **Phase 진행**: 10 v3-2layer Week 1 (Carry trade detector 검증 완료)

---

## 2. Architecture (5 Layer)

### 2.1 Data Layer
```
D:\코워크\scripts\snapshots\
├── fetch_historical.py    # FRED Liquidity + Geo
├── fetch_tier1.py         # FRED Tier 1 leading + yfinance ETF + Catalyst
├── fetch_geo.py           # USEPU + GEPU + GDELT (rate-limited)
├── fetch_liquidity.py     # latest mode (다른 system)
├── fetch_sentiment.py     # CNN F&G (failed often) + UMCSENT
└── fetch_all.py           # 통합 (latest mode 만)

→ Output: D:\코워크\snapshots\
   ├── liquidity_hist_YYYYMMDD.json
   ├── geo_hist_YYYYMMDD.json
   └── tier1_hist_YYYYMMDD.json
```

**Data sources**:
- **FRED API** (free, 5만 시리즈)
- **yfinance** (free, ETF 가격)
- **CFTC COT** (free, weekly Friday)
- **USEPU/GEPU** (Baker-Bloom-Davis)
- **Catalyst Calendar** (수동 maintained, FOMC/BoJ/CPI/NFP)

### 2.2 LLM Agent Layer
```
D:\코워크\scripts\analyze_v2.py
├── LIQ_PROMPT_TEMPLATE
│   ├── Layer 1 (Leading): SAHM, SLOOS, BREAKEVEN, FX, KRE/SPY, HYG/IEF, SOFR-IORB
│   ├── Layer 2 (Coincident): VIX, YC, Baa-Aaa, NFCI, TGA, FED_BS, RRP, DXY
│   └── Layer 3 (Catalyst): Catalyst summary
└── GEO_PROMPT_TEMPLATE
    ├── Layer 1 (Catalyst): Calendar events
    └── Layer 2 (Coincident): USEPU spot + 5d MA + 10d MA, GEPU
```

**Output JSON schema**:
```json
{
  "agent_id": "liquidity_v2" | "geopolitics_v2",
  "regime_view": {bull, side, bear, crisis},
  "horizon_hours": 24,
  "confidence": 0.0~1.0,
  "key_signals": [...],
  "rationale_synthesis": "...",
  "next_trigger": "...",
  "uncertainty_flags": [...]
}
```

### 2.3 Fusion Layer (M1~M7)

**`fuse_v2()`** (legacy 24h) + **`fuse_v2_horizon()`** (multi-horizon Phase 9.1)

```python
HORIZON_CONFIGS = {
    "24h": {weight_balance: (0.5,0.5), catalyst_max: 1.15, m4_active: True, relax_pp: 0.05},
    "5d":  {weight_balance: (0.6,0.4), catalyst_max: 1.10, m4_active: True, relax_pp: 0.03},
    "7d":  {weight_balance: (0.7,0.3), catalyst_max: 1.00, m4_active: False, relax_pp: 0.01},
}
```

| Mechanism | 역할 |
|---|---|
| M1 | Confidence-weighted base (Liq×c_l + Geo×c_g) |
| M2 | Catalyst density modifier (×1.0~1.15) |
| M3 | Resonance boost (둘 다 ≥0.40 → +0.10) |
| M4 | Single-agent escalation (Geo crisis ≥0.30 + imminent → floor 0.20) |
| M5 | Crisis override (단일 agent crisis >0.35 + conf >0.6 → floor 0.30) |
| M6 | Renormalize to 1.0 |
| M7 | Asymmetric hysteresis (relax_pp 기반 stepdown) |

**Cascade analyzer** (`analyze_cascade`) — 10 시나리오:
- regime_calm / broad_recovery / transient_caution / front_loaded_caution
- short_shock / approaching_risk / regime_shift_imminent
- broad_caution / escalating_stress / full_stress / mixed

### 2.4 Output Layer

**Alert level**: GREEN / YELLOW / ELEVATED / RED

**History files**:
```
alert_history_live.jsonl              # legacy 24h
alert_history_24h_live.jsonl          # multi-horizon 24h
alert_history_5d_live.jsonl
alert_history_7d_live.jsonl
+ 각각 _backtest.jsonl
```

**Predictions + scoring** (`scoring.py`):
```
predictions.jsonl                # legacy
predictions_24h.jsonl            # T+1, T+3, T+5
predictions_5d.jsonl             # T+3, T+5, T+7
predictions_7d.jsonl             # T+5, T+7, T+10
```

**Position sizing** (3 mode + Hybrid):
- **Single** (vol-target 15%): risk-on/hedge/cash %
- **Multi-asset**: SPY/IEF/GLD/cash %
- **Hybrid**: BREAKEVEN 기반 자동 selection

**Dashboard v3**: 3-column (24h / 5d / 7d) + Cascade card + Ladder × 3 + 채점 표

**Alerts** (`alerts.py`): Windows toast + level 변경 detection

**Publish** (`publish.ps1`): GitHub Pages 자동 동기화 (PRD + dashboard + jsonl + fusion)

### 2.5 Detector Layer (Phase 10 v3-2layer)

```
D:\코워크\scripts\detectors\
└── carry_trade_detector.py
    └── 4 components:
        - rate_diff_compression (US-JP 5d Δ)
        - usdjpy_velocity (5d Δ)
        - cftc_position_extreme (CFTC speculator net)
        - leverage_proxy (1d Δ acceleration)
```

**Validation**: 4/4 케이스 정확 (2024-08-05 CRITICAL, 2024-07-31 ELEVATED, 2017-09 CALM, 2026-04-29 CALM)

---

## 3. Indicators 검증 상태

### 3.1 학술/empirical 검증된 진짜 leading
| Indicator | Lead | 검증 source |
|---|---|---|
| SAHM Rule | 침체 1-3M | 학술 (Sahm 2019) |
| SLOOS | 신용 긴축 1-3M | Fed 학술 |
| Baa-Aaa spread | flight-to-quality 1-3W | GFC 검증 |
| KRE/SPY ratio | sector stress 2-4W | SVB 2023 검증 |
| HYG/IEF ratio | credit stress 1-2W | 2020 COVID 검증 |
| USDJPY (carry) | unwind 5-7d | Round 2 (2/2 episodes) |
| NFCI | financial conditions | Fed 학술 |
| Yield Curve 10Y-2Y | 침체 12-18M | NY Fed |
| VIX | 변동성 (concurrent) | 광범위 검증 |

### 3.2 검증 안 된 / 의문스러운
| Indicator | 문제 | Round 2 결과 |
|---|---|---|
| **USEPU** | Round 2 가 0/7 episodes detection ★ | ❌ Reject |
| **GEPU** | 효과 없음 | ❌ |
| **Catalyst density** | 일률 stress 가정 (BoJ dovish 무시) | ⚠️ Weak |
| **USDKRW** | Korean specific, 글로벌 영향 작음 | ⚠️ |
| **TGA, FED_BS, RRP** | net liquidity 직접 시장 영향 검증 약함 | ⚠️ |
| **DXY** | 광범위 macro proxy, 시점 불분명 | ⚠️ |
| **Forward 5y5y** | very long horizon | ⚠️ 7d 부적합 |

→ **시스템 indicator 의 약 50% 가 검증 안 됨**.

### 3.3 수급 (Flow) 영역 — **거의 부재**
| 진짜 flow 영역 | 우리 시스템 |
|---|---|
| CFTC speculator positioning | ❌ (carry detector 만, 24h 시스템 X) |
| ETF flows (SPY/QQQ inflows) | ❌ |
| Options Gamma Exposure (GEX) | ❌ |
| 0DTE/1DTE flows | ❌ |
| Insider trading (Form 4) | ❌ |
| Margin debt (FINRA) | ❌ |
| AAII bullish/bearish | ❌ |
| NAAIM Exposure Index | ❌ |
| Fund flows (EPFR/Lipper) | ❌ |
| KRX 외국인 순매수 (한국) | ❌ |
| 신용잔고 (한국) | ❌ |

→ **8가지 진짜 flow 영역 중 0개 cover** (간접 proxy 4개만).

---

## 4. 작동 흐름 (Daily Routine)

```
1. fetch_historical.py + fetch_tier1.py
   → snapshots/liquidity_hist + geo_hist + tier1_hist

2. analyze_v2.py --mode prompt
   → analyses/liq_prompt + geo_prompt

3. (수동) LLM 분석
   → analyses/liq_output + geo_output JSON

4. daily_complete.ps1 (8 단계 자동):
   [1] analyze_v2 --mode fusion → fusion_*.json + history append
   [2] scoring --append + --score (T+1/T+3/T+5/T+7/T+10)
   [3] alerts --check --notify-toast (Windows toast)
   [4] daily_dashboard.py v3 → daily_dashboard.html
   [5] position_sizing.py → sizing_*.json
   [6] multi_asset_sizing.py → multi_asset_*.json
   [7] hybrid_sizing.py → hybrid_*.json (Single/Multi 자동)
   [8] publish.ps1 → GitHub Pages 동기화

5. (선택) carry_trade_detector.py → detectors/carry_trade_*.json
```

---

## 5. 장점 (검증된 부분)

### 5.1 자동화 + 운영 ✓
- **8 단계 daily_complete.ps1** 한 줄 실행 → 모든 산출물 + GitHub push
- **Live + backtest history 분리** (data leakage 방지)
- **Soft validation infrastructure** (predictions + 자동 채점)
- **Hot recovery** (file 누락/error 시 graceful)
- **PRD living document** + GitHub 자동 동기화

### 5.2 LLM Multi-agent ✓
- **2 agent 분리** (Liq + Geo) — domain expertise
- **3 layer prompt** (Leading + Coincident + Catalyst)
- **Confidence + uncertainty_flags** — meta-cognition
- **Blind mode** (AS_OF_DATE 이후 사건 차단)

### 5.3 Fusion mechanism ✓
- **7 mechanism (M1-M7)** — 정교한 결합 로직
- **Multi-horizon** (24h/5d/7d) — Phase 9.1 인프라
- **Asymmetric hysteresis** (v2.2) — false positive 통제
- **Catalyst-aware** (M2 modifier)

### 5.4 Position sizing ✓
- **3 mode** + Hybrid 자동 selection
- **Vol-target** (alert + VIX 기반)
- **BREAKEVEN-based regime** (인플레/deflation 인식)
- **자동 risk-on ↔ defensive 전환**

### 5.5 검증된 episodes ✓
- 8 episodes / 47 sessions backtest: ±0.4pp 정확도
- 5/5 random OOS hit (Phase 9.2.1)
- Live T+1 directional 100% (4/27, 4/28)
- Carry trade detector 4/4 정확
- Multi-horizon T+3 multi-horizon HIT (5/1 cycle)

### 5.6 Cascade scenario 분류 ✓
- 10 시나리오 (regime_calm ~ full_stress)
- in_recovery flag (회복기 detect)
- raw vs final 분리 (모델 view + 위험관리)

---

## 6. 단점 (Fundamental Flaws)

### 6.1 ★★ Flaw #1: Stress signal vs Market price 디커플링 (사용자 질문 1)

**증거**:
- 4/27 ELEVATED + SPY +1.17% 상승
- 4/28 YELLOW + SPY +0.5%
- 4/29 ELEVATED + SPY +0.5% (사상 최고)
- 5/1 broad_recovery (in_recovery)

**원인**:
1. **USEPU 에 과도 의존** — Round 2 검증에서 0/7 episodes detection
2. **Catalyst-driven stress 일률 가정** — BoJ dovish/expected 가능성 무시
3. **시스템 indicator 50% 가 검증 안 됨** (USEPU, GEPU, USDKRW 등)
4. **시장 가격 (SPY return) 을 보지 않음**

**impact**: 시스템이 "stress signal 누적" 알람만 발생, 진짜 시장 reaction 과 무관할 수 있음.

### 6.2 ★★ Flaw #2: 수급 (Flow) 거의 capture 안 함 (사용자 질문 2)

**누락된 flow 영역**:
- CFTC speculator positioning (전체)
- ETF inflows/outflows
- Options GEX, 0DTE flows
- Insider trading
- Margin debt
- AAII/NAAIM sentiment positioning
- Fund flows (EPFR/Lipper)
- KRX 외국인 / 신용잔고

**impact**:
- 단기 (1-2주) horizon 에서 가장 큰 결함
- 사용자 통찰 정확: "**욕망은 오래가지 못하지만 수급은 시장 분위기를 바꿀 수 있다**"
- 수급이 stress signal 을 압도하는 시기에 false positive 다발

### 6.3 ★ Flaw #3: Scoring 이 fusion-vs-fusion (circular validation)

**현재 채점**:
```python
predicted_alert (4/28) vs actual_alert (5/1)  # 둘 다 fusion 출력
→ HIT/PARTIAL/MISS
```

**진짜 검증 결여**:
- SPY return 과 무관
- HIT 라고 해도 시장 결과 안 맞을 수 있음
- 47 sessions ±0.4pp + 5/5 OOS 모두 fusion-vs-fusion

**impact**: 검증된 정확도가 사실상 internal consistency 만 측정.

### 6.4 ★ Flaw #4: Multi-horizon 5d/7d cosmetic copy

**Phase 9.2.2 입증**:
```
24h vs 5d: 100% same alert
24h vs 7d:  89.3% same alert
```

5d 가 24h 의 100% alias. 7d 는 약간만 다름. **진짜 multi-horizon view 아님**.

**원인**: 단일 LLM 호출 (24h reasoning) + Python fusion config 차별만으로 horizon 분리 안 됨.

**impact**: dashboard 의 3-column view 가 misleading 가능.

### 6.5 Minor Flaw #5: GDELT 부재
- fetch_geo.py 의 GDELT API 자주 429 (rate limit) / 접근 불가
- 5/1 fetch 시 gdelt_available=False
- 지정학 quantification 이 USEPU/GEPU 만으로 진행 — weak

### 6.6 Minor Flaw #6: alerts.py stale state
- 5/1 cycle 에서 "현재 alert: 🟠 ELEVATED (2026-04-27)" 표시
- 실제로는 5/1 GREEN/YELLOW
- alerts.py 가 multi-horizon history 인식 안함

### 6.7 Minor Flaw #7: CFTC parser 검증 미완료
- 5/1 carry detector 결과 +58,776 long
- column index 정확성 실제 CFTC FinFutWk.txt 와 cross-check 필요
- 4/29 hardcoded -85K 와 큰 차이 (-+143K swing)

### 6.8 Minor Flaw #8: Backtest data sparse
- 28 sessions, 9 episodes
- "no_target" 다수 (T+N target 시점 fusion 부재)
- 정확한 hit rate 측정 위해 episode 별 dense fusion 필요

### 6.9 Minor Flaw #9: Foundation Model R² 음수 (외부 검증)
- TimesFM, Chronos 금융 데이터에서 underperform (Round 2)
- Phase 10 v3-2layer 에서 도입 검토 필요

### 6.10 Minor Flaw #10: Operational LLM cost (production)
- 현재 Cowork chat (사용자 manual)
- 자동 LLM 호출 시 비용 (OpenRouter, Anthropic API)
- Phase 11 (자동화) 미진행

---

## 7. Fundamental 평가

### 7.1 시스템이 진짜 측정하는 것

```
입력: stress signals (검증 50%, 미검증 50%)
처리: LLM reasoning + 7 fusion mechanism + multi-horizon
출력: regime alert (GREEN/YELLOW/ELEVATED/RED)
검증: 자기 출력과 비교 (fusion-vs-fusion)
```

→ **시스템은 자체 stress signal 종합 정확도를 측정하고 있음**. 진짜 시장 결과 (SPY return, 사용자 portfolio P&L) 와 직결 안 됨.

### 7.2 시스템이 잘하는 것

1. **Stress signal 구조화** — 30+ macro indicator 를 일관된 framework 로 종합
2. **Catalyst-aware reasoning** — FOMC/BoJ 같은 큰 이벤트 인식
3. **Hysteresis 통제** — false positive 어느 정도 통제
4. **자동화 운영** — 무인에 가까운 daily routine
5. **Audit trail** — M1-M7 적용 내역 + key_signals 추적 가능
6. **Carry trade specific detection** (Phase 10 v3-2layer) — Round 2 검증된 영역
7. **Position sizing 자동** — 3 mode + Hybrid 결정

### 7.3 시스템이 못하는 것

1. **수급 (flow) 인식** — 거의 0% capture
2. **단기 (1-7d) 시장 가격 예측** — false positive 빈발 가능성
3. **시장 sentiment / positioning** — AAII/NAAIM/CFTC 부재
4. **Korean market 특화** — KRX 외국인/신용잔고 부재
5. **검증된 정확도** — 진짜 SPY return 기반 검증 부재
6. **Multi-horizon 차별화** — 5d/7d 가 24h cosmetic copy
7. **Mechanical crash detection** — Volmageddon-class (Round 3b 0/5 detector miss)
8. **Real-time data** — FRED 일부 lag (USDJPY 4/24 등)
9. **GDELT 활용** — rate limit 으로 부재
10. **자동 LLM 호출** — 사용자 수동 의존

### 7.4 솔직한 grade

| 영역 | Grade |
|---|---|
| Architecture 정교함 | A- |
| 자동화 운영 | A |
| Indicator 검증 | C |
| **Flow capture** | **F** |
| **시장 결과 검증** | **D** |
| Multi-horizon 차별화 | D+ |
| Carry detector | A |
| Cascade scenario | B+ |
| Position sizing | B |
| 종합 운영 가치 | C+ |

---

## 8. 결론 — 시스템의 정체

**현재 시스템은 'macro stress signal 누적 monitor' 입니다, 'market regime predictor' 가 아닙니다**.

이 차이가 사용자가 짚은 두 결함의 근본:
1. Stress signal 만 보고, 그게 시장에 반영될지는 검증 안 함
2. 수급 (반영을 결정하는 핵심) 을 안 봄

**둘 다 fundamental flaws**. patch 가 아닌 **architectural redesign** 이 필요한 영역.

다음 단계 결정은 보고서 2 (alternatives) 의 옵션 중에서 선택. 단순 patch 는 위 flaws 해결 못함.

---

## 부록: 모든 산출물 inventory

### 9.1 Scripts (D:\코워크\scripts\)
- analyze_v2.py
- scoring.py
- alerts.py
- daily_dashboard.py v3
- position_sizing.py / multi_asset_sizing.py / hybrid_sizing.py
- multi_horizon_backtest.py
- daily_complete.ps1 / publish.ps1 / daily_run.ps1
- snapshots/fetch_*.py (6개)
- detectors/carry_trade_detector.py
- test_horizon_fusion.py / audit_t4_overload.py

### 9.2 Data (D:\코워크\analyses\)
- liq_output_*.json, geo_output_*.json (28+ sessions)
- fusion_*.json (multi_horizon 포함)
- alert_history_{24h,5d,7d}_{live,backtest}.jsonl
- predictions_{24h,5d,7d}.jsonl + legacy
- detectors/carry_trade_*.json
- sizing/, multi_asset_sizing/, hybrid_sizing/

### 9.3 Documentation (D:\코워크\)
- PRD.md (living document)
- daily_dashboard.html
- master_final_report.html
- portfolio_backtest_dashboard.html
- backtest_multi_horizon_report.md

### 9.4 Research (D:\코워크\research\)
- 7d_methodology_comparison.md (Round 1 종합)
- group1/2/3_*.md (Round 1 detailed)
- round2_empirical_case_studies.md
- round2_failure_modes_best_practices.md
- round2_practitioner_landscape.md
- round3a_hybrid_technical.md (Phase 10 v3 feasibility)
- round3b_hybrid_validation.md (stress test)
- final_recommendations.md
- **system_audit.md** (이 문서)
- **market_prediction_alternatives.md** (보고서 2)

### 9.5 GitHub (https://github.com/hong4137/macro-regime)
- docs/index.html (live dashboard)
- docs/PRD.md
- docs/backtest_multi_horizon_report.md
- docs/data/* (jsonl + latest_fusion)
