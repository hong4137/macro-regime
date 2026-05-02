# Macro Regime Detection System — PRD

**버전**: v0.9 (Phase 9.2.2 완료 시점)
**최종 업데이트**: 2026-04-30
**Status**: Live + Multi-horizon 검증 완료, 다음 phase 결정 대기

---

## 1. Vision

거시경제 regime 변화 (bull / bear / crisis) 를 단기 (24h) ~ 중장기 (7d) 시점에서 **사전 감지** 하는 자동화 시스템. LLM 기반 multi-agent 분석 + 통계 fusion + 시각 dashboard.

핵심 가치 명제:
- **24h alert** — 다음 거래일 risk 평가 (vol-target sizing)
- **5d/7d alert** — 추세/regime 전환 사전 감지 (선행 시그널)
- **Cascade signal** — 3 horizon 결합 해석 (시나리오 기반 액션)

---

## 2. Architecture

### 2.1 Data Layer
- **Source**: FRED + yfinance + EPU + Catalyst calendar
- **일간 fetch**: `liquidity_hist`, `geo_hist`, `tier1_hist` (D:\코워크\snapshots\)
- **Tier 1 leading**: SAHM, SLOOS, USDJPY, USDKRW, KRE/SPY, HYG/IEF, BREAKEVEN, SOFR-IORB
- **Tier 2 coincident**: VIX, YC 10Y-2Y, Baa-Aaa, NFCI, DXY 등
- **Geo**: USEPU (daily), GEPU (monthly), Catalyst calendar (FOMC/BoJ/CPI/NFP)

### 2.2 LLM Agent Layer (analyze_v2.py)
- **Liquidity Agent v2** — Tier 1 leading + Tier 2 coincident + Catalyst context
- **Geopolitics Agent v2** — USEPU + GEPU + Catalyst (5d MA spike noise filter)
- **Output**: `regime_view` (bull/side/bear/crisis 확률), `key_signals`, `confidence`
- **현재 한계**: 단일 LLM 호출, `horizon_hours: 24` 시각만 reasoning

### 2.3 Fusion Layer — M1~M7
| Mechanism | 역할 | Horizon-aware? |
|---|---|---|
| M1 | Confidence-weighted base | ✓ (weight_balance) |
| M2 | Catalyst density modifier | ✓ (24h ×1.15, 5d ×1.10, 7d off) |
| M3 | Sideways/Bear/Crisis resonance | ✗ |
| M4 | Single-agent escalation (Geo crisis + imminent) | ✓ (7d off) |
| M5 | Crisis override (단일 agent crisis > 0.35) | ✗ |
| M6 | Renormalize | ✗ |
| M7 | Asymmetric hysteresis (v2.2 fast recovery) | ✓ (relax_pp 차별) |

### 2.4 Multi-horizon (Phase 9.1)
- 24h / 5d / 7d 각각 fusion
- `HORIZON_CONFIGS`: 4가지 mechanism 차별화
- `analyze_cascade`: **10 시나리오** 분류
  - regime_calm, broad_recovery, transient_caution, short_shock
  - approaching_risk, regime_shift_imminent, broad_caution
  - front_loaded_caution, escalating_stress, full_stress, mixed
- ⚠️ **검증 결과**: 5d 는 24h 의 cosmetic copy (Section 4.2 참조)

### 2.5 Output Layer
- **Alert**: GREEN / YELLOW / ELEVATED / RED
- **Position Sizing** (3 mode):
  - Single — vol-target 15% based on alert + VIX
  - Multi-asset — SPY/IEF/GLD/cash 분배
  - Hybrid — BREAKEVEN 기반 자동 selection
- **Daily Dashboard v3** — 3-column horizon view + Cascade card + Ladder
- **GitHub Pages** — https://hong4137.github.io/macro-regime/

---

## 3. Phase 진행 상황

### ✅ Phase 0~8 (완료)
- 데이터 인프라 + 2-agent fusion (M1-M7)
- 8 episodes / 47 sessions backtest, ±0.4pp 정확도
- 5/5 random OOS hit rate (cherry-pick bias 해소)
- Position sizing 3-mode + Portfolio backtest (+4.80% / DD -7.49pp)
- GitHub Pages 자동 동기화 + Cascade Signal v3

### ✅ Phase 9.1 (완료) — Multi-horizon 인프라
- `HORIZON_CONFIGS` + `fuse_v2_horizon` + `apply_hysteresis_horizon`
- `analyze_cascade` (10 시나리오)
- horizon-aware history files (`alert_history_{24h,5d,7d}_live`)
- `extract_key_signals_by_horizon` (snapshot → dashboard 지표)
- `daily_dashboard.py v3` (3-column + Cascade card)
- `publish.ps1` multi-horizon 화이트리스트

### 🔍 Phase 9.2 (완료) — Multi-horizon 검증
- ✅ 9.2.1: `scoring.py` horizon-aware (T+1/T+3/T+5/T+7/T+10)
- ✅ 9.2.2: `multi_horizon_backtest.py` (28 sessions / 10 episodes)
- ⚠️ **결과**: 5d 는 24h 의 100% alias, 7d 는 89.3% 동일

### ⏸️ Phase 9.3 (보류) — 5d/7d 단순 reweighting 한계 인정
- Phase 10 에서 **새 방법론 도입** 으로 우회 (단순 LLM prompt 분리보다 더 근본적)

### 🔬 Phase 10 (계획) — 7d 위기/기회 사전 감지 새 방법론

**Phase 9 한계 인정 후 처음부터 광범위 리서치**. 8 카테고리 / 30+ 방법론 / 17,000 단어 종합 보고서:
- `D:\코워크\research\7d_methodology_comparison.md` (종합 비교)
- `D:\코워크\research\group1_statistical_macro_industry.md`
- `D:\코워크\research\group2_crossasset_sentiment_geopolitical.md`
- `D:\코워크\research\group3_ai_ml_alternative.md`

#### 핵심 발견
1. **OECD CLI / ISM / Sahm Rule 등 conventional leading indicator** 는 4-12개월 horizon 설계 → 7d 에 부적합
2. **5-7d 강한 신호 영역 3가지**:
   - 변화점 detection (BOCPD on NFCI)
   - 시장 microstructure (VIX TS, Credit spread momentum)
   - Foundation Model (TimesFM, Chronos-2)
3. **현재 LLM 시스템의 자연 진화** = TradingAgents framework
4. **무료 alternative data** (BDI + ADP + EIA) > 비싼 satellite/credit card
5. **Sentiment & Industry quant** 7d 에 약함 — 우선순위 ↓

#### Top 10 방법론 (우선순위)
| Rank | Method | Cost | Phase |
|---|---|---|---|
| 1 | BOCPD + Chicago Fed NFCI | $0 | 10.1 |
| 2 | VIX Term Structure (M1-M2) | $0 | 10.1 |
| 3 | Credit spread 3-day momentum | $0 | 10.1 |
| 4 | TimesFM + Chronos-2 ensemble | $0 (HF) | 10.2 |
| 5 | GPR Index (Caldara-Iacoviello) | $0 | 10.1 |
| 6 | Carry trade risk (USDJPY + CB div) | $0 | 10.1 |
| 7 | TradingAgents framework | LLM cost | 10.3 |
| 8 | BDI + 주간 labor (ADP/UI) | $0 | 10.5 |
| 9 | Granger causality discovery | $0 | 10.4 |
| 10 | Long-context LLM + RAG | LLM cost | 10.3 |

#### Roadmap
- **Phase 10.1** (1주) — BOCPD + VIX TS + Credit momentum 통합 (M8 new mechanism)
- **Phase 10.2** (2-4주) — TimesFM/Chronos foundation model PoC
- **Phase 10.3** (5-8주, 선택) — TradingAgents framework
- **Phase 10.4** (9-12주, 선택) — Granger causality discovery
- **Phase 10.5** (13주+, 선택) — Alternative data 통합

### 🔬 Phase 10 — 2차 리서치 (완료) ★ 충격적 결과

**1차 보완 리서치 3개 group**:
- `round2_empirical_case_studies.md` — 7 historical episodes 실증
- `round2_failure_modes_best_practices.md` — 함정 + 검증 방법
- `round2_practitioner_landscape.md` — 2024-2025 실무자 동향
- `final_recommendations.md` — 종합 결론 ★

#### 🚨 핵심 발견 — Round 1 추천 대부분 invalidated

| Round 1 Top 5 | Empirical | Practitioner | 평가 |
|---|---|---|---|
| BOCPD + NFCI | 0/7 detection | confirmation only | ❌ Reject |
| VIX Term Structure | 1/7 | confirmation only | ❌ Reject (단독) |
| Credit spread momentum | 1/7 | confirmation only | ❌ Reject (단독) |
| TimesFM/Chronos | 시뮬 어려움 | R² = -2.80% | ❌ Reject |
| GPR Index | 0/7 | 효과 없음 | ❌ Reject |

#### 💡 가장 중요한 Practitioner Insight

> **Top quant funds (Bridgewater, AQR, Citadel) 는 7d regime prediction 자체를 포기**.
> 대신 HMM/VAR baseline + **regime-aware alpha extraction** 에 집중.
> Foundation Model (TimesFM) 은 금융 데이터에서 R² 음수.
> Regime persistence 가 5-20 trading days 로 짧아짐.

#### ✓ Round 2 가 검증한 진짜 Top 5 (validated)

| # | Method | 검증 episodes | 데이터 |
|---|---|---|---|
| 1 | **Granger Causality (Policy → Prices)** | 4/7 | 무료 (FRED) |
| 2 | **Carry Trade Metrics** | 2/2 FX crises | 무료 (FRED + CFTC) |
| 3 | **Leverage/Margin Surveillance** (CFTC futures) | 2/3 leverage | 무료 (CFTC) |
| 4 | **Currency Basis Divergence** (CNH-CNY) | 2/2 FX | 일부 paid |
| 5 | **Polling + Betting Aggregation** | 1/1 정치 | 무료 |

#### 핵심 통찰

1. **Crisis-type-specific signals > Universal regime detector** — 보편적 단일 지표는 없음
2. **Flow-based > Price-based** — CFTC positioning, 예금 데이터, insider trades 가 진짜 leading
3. **Microstructure (basis, vol skew)** 가 5-7d 에 가장 유효
4. **Practitioner > Academic** 추천이 더 actionable
5. **현재 LLM-based 시스템 = HMM/VAR baseline 과 비슷** (검증됨)

### 🟢 Phase 10 v3-2layer (확정) — 2-Layer Hybrid Architecture ★

**Stress test (Round 3b) 결과 Layer 2 폐기**, 2-layer 단순화로 결정.

#### Layer 2 폐기 이유
- 5-7d directional accuracy **50% 가 empirical maximum** (동전던지기)
- Round 3a 의 55-65% claim 은 overstated (Round 3b 검증)
- Foundation model (Chronos-2) 도 calm regime 에서 directional 50% 미만
- Value 불분명한 layer 추가는 complexity ↑ + 사용자 confusion
- **정직한 architecture > 복잡한 architecture**

#### Architecture (확정)
```
[Layer 1: 24h System] (검증됨, 유지 + 고도화)
   Liq + Geo Agent → M1-M7 fusion → 24h alert
   현재 시스템 그대로 (T+1 directional 100% 검증)

[Layer 3: Crisis-type Siren] (신규)
   MVP (Week 1-3):
     ① USDJPY Carry Trade Risk
     ② CFTC Margin Spike Cascade
   Full ensemble (v3-2layer.1, Week 9+):
     ③ Currency Basis (CNH-CNY)
     ④ Policy Granger (Policy → Prices)
     ⑤ Euphoria/Squeeze (mechanical crash)
   Logic: MVP 2-of-2 consensus, Full 3-of-5 consensus

[Soft Invalidation Integration]
   Siren ON → 24h alert 옆에 "Crisis Watch" badge + crisis type
   Siren OFF → 정상 24h 표시
   Auto-hedge trigger: VIX call scale (사용자 manual override 가능)
   Hysteresis: 4h dwell-in / 8h recovery-out
```

#### Mandatory Fixes (Round 3b 의 6개 → 5개 적용, H1 은 Layer 2 폐기로 해결)
1. ~~H1: Calm mode target 재조정~~ → Layer 2 폐기로 해결 ✓
2. **H2: False positive target** → "2-3/month expected" 인정 (1/month 비현실적)
3. **Detector mapping 문서화** — 어떤 crisis type cover/miss 명시
4. **Automated hedging trigger** — warning + 자동 VIX call scale (manual override)
5. **Holdout test validation** — 2000-2019 train → 2020 COVID + 2024 Yen 검증
6. **Data quality 정량화** — CFTC 3-day lag, FRED revision schedule, graceful degradation

#### Roadmap (3주 MVP + 2주 hardening)

**Week 1: Detector ① USDJPY Carry Trade Risk**
- Data: USDJPY (FRED daily), 미일 rate spread (FRED), CFTC speculator positioning (CFTC weekly)
- Logic: rate diff threshold + leverage proxy + USDJPY 5d delta + CFTC speculator imbalance
- Output: Carry crisis probability (0-1) + lead time estimate

**Week 2: Detector ② CFTC Margin Spike Cascade**
- Data: CFTC futures margin (CME), VIX 1d Δ, dealer gamma exposure proxy (free CBOE)
- Logic: margin spike + vol skew + leverage 누적
- Output: Margin cascade probability (0-1)

**Week 3: Integration + Soft Invalidation + Auto-hedge**
- 2-of-2 consensus 트리거 (MVP)
- 24h dashboard 에 "Crisis Watch" badge 통합 (dashboard v4)
- Auto-hedge trigger (VIX call scale, 사용자 manual override)
- Hysteresis 구현 (4h/8h)

**Week 4-5 (Hardening)**:
- Holdout test (2020 COVID + 2024 Yen 시뮬)
- Data quality 정량화 + graceful degradation logic
- Detector mapping 문서 (어떤 crisis type 안 잡히는지)
- False alarm rate 모니터링 인프라

**Week 9+ (v3-2layer.1 Full Ensemble)**:
- Currency Basis 추가 (paid feed 결정 후)
- Policy Granger discovery system
- Euphoria/Squeeze detector
- 3-of-5 consensus 전환

#### Confidence Target
- MVP (Week 3): **65%**
- Hardening (Week 5): **70%**
- Production (Week 8+): **72%**

#### 새 Cascade Scenario (단순화)
기존 10개 cascade scenario 폐기 → 4개 단순화:
- `regime_calm` — Siren OFF + 24h GREEN/YELLOW
- `regime_elevated` — Siren OFF + 24h ELEVATED+
- `crisis_watch` — Siren ON, crisis type 표시 (carry/margin)
- `regime_crisis` — Siren ON + 24h ELEVATED+ (consensus)

#### 기각된 옵션들
- ❌ Phase 9.1 multi-horizon (5d/7d cosmetic copy)
- ❌ Phase 10 v2 직접 도입 (architecture depth 부족)
- ❌ Phase 10 v3 3-layer with Layer 2 (calm mode 가치 입증 안 됨)
- ❌ TimesFM/Chronos 단독 사용 (R² 음수)
- ❌ Foundation model in calm regime (Round 3b 검증 실패)

---

### ⏸️ Phase 10 v2 (보류) — 새 detector 직접 도입 (v3 에 통합)

---

### 🚀 Phase 11 (시작) — 완전 재설계

**사용자 질문이 발견한 두 fundamental flaw**:
1. "필연적 stress 인가?" — Indicator 50% 가 검증 안 됨 (USEPU 등)
2. "수급 capture 하나?" — 8가지 진짜 flow 영역 중 0개 cover

→ **System Audit 결론**: 현 시스템은 "macro stress signal 누적 monitor" 이지 "regime predictor 가 아님". Patch 로 해결 불가능, **완전 재설계** 필요.

#### 두 보고서 (재설계 기반)
- `D:\코워크\research\system_audit.md` — 현 시스템 진단 + 10 flaws
- `D:\코워크\research\market_prediction_alternatives.md` — 7 카테고리, 30+ 옵션

#### 사용자 결정 사항 (2026-05-02)
- **Architecture**: 완전 재설계 (Full redesign tier)
- **유료 데이터**: 배제 안 함, 단 **무료 vs 유료 ROI 검증 후 결정**
- **Korean market**: 배제 (글로벌 시장만)
- **Phase 10 v3-2layer 작업**: 기존 carry detector 만 Phase 11 에 통합 ✓, Week 2-5 보류

#### Phase 11 Roadmap (예정)

**Phase 11.0 (현재)** — Plan 정리 + GitHub 동기화

**Phase 11.1** — 무료 vs 유료 ROI 비교 리서치 (Round 4) ✅ 완료
- `D:\코워크\research\round4_free_vs_paid_roi.md` (7,500 단어)
- **결론**: 8 domain 중 7개 paid 가치 없음. SpotGamma ($1.2k) 만 MAYBE
- **권장 Tier**: Tier 1 → Tier 2 phased ($0 → $4k/year)

**Phase 11.2** — Architecture Spec 작성 ✅ 완료
- `D:\코워크\research\phase11_architecture_spec.md`
- 6 Layer architecture, 새 cascade 7 scenarios, Mandatory fixes 5개

**Phase 11.3** — Flow Layer 구축 (Week 1) ✅ 완료
- `D:\코워크\scripts\detectors\flow_detector.py`
- 6 sub-detectors (CFTC, ETF, AAII, NAAIM, PCR, NAAIM)
- Continuous score (extreme + mild signal 모두 capture)
- 4 케이스 검증: 4/29 leaning_bullish, 8/5 bearish_flow, 2017 neutral

**Phase 11.4** — analyze_v3.py 통합 ✅ 완료
- `D:\코워크\scripts\analyze_v3.py`
- Geo agent 폐기 (USEPU 등), Liq + Flow + 새 Cascade
- M8 Flow Modifier 양방향 작동 (4/29 dampened, 8/5 amplified)
- 7 cascade scenarios (genuine_calm/narrative_scare/smart_money_warning 등)

**Phase 11.5** — 28 sessions Batch Backtest + Non-linear Scoring v3 ✅ 완료
- `D:\코워크\scripts\batch_phase11_backtest.py`
- `D:\코워크\scripts\scoring_v2.py` (initial linear)
- `D:\코워크\scripts\scoring_v3.py` (★ non-linear 사용자 통찰)
- `D:\코워크\research\phase11_batch_report.md`

**v2 vs v3 비교 (113 scored, 29 sessions)**:
| Metric | v2 | **v3** | 변화 |
|---|---|---|---|
| HIT | 28.3% | **40.9%** | +12.6pp |
| FALSE_POSITIVE | 31.9% | **10.0%** | -21.9pp ★ |
| MISS_CRISIS | 2.7% | 2.7% | 그대로 |

**Alert Level 별 (v3)**:
- GREEN: 44% HIT, 0% FP
- YELLOW: 50% HIT, 0% FP
- ELEVATED: 51% HIT, 13% FP (v2: 22%/47% → 큰 개선)
- RED: 10% HIT, 30% FP (v2: 5%/85% → 큰 개선)

**Cascade 별 (v3)**:
- **smart_money_warning: 75% HIT** ★ — 사용자 통찰 #2 결정적 입증
- genuine_calm: 41% HIT, 0% FP
- real_stress_brewing: 29% HIT, 17% FP

**v3 Non-linear Scoring 핵심 (사용자 통찰)**:
- 평시 영역 좁고 strict (BDI 0-13 minimal 처럼)
- Severe 영역 wide 하게 (RED max_dd ≤ -5% 인정)
- FP 임계 강화 (ELEVATED +1% → +3%, RED +3% → +5%)
- Max drawdown 측정 (단순 SPY return 부족)

**여전히 남는 issue**:
- 시장 저점 V자 반등 미인식 (2020-03-23 RED → +17%)
- COVID-class 신호 detection 약함 (2020-02-19 GREEN MISS)
- narrative_scare sample 부족 (1 case)

**Phase 11.2** — Architecture 재설계 결정
- Dual-system (quant + LLM) vs Probabilistic continuous vs Crisis-type detection
- Round 2 의 "Top funds 가 regime-aware alpha 로 pivot" 인사이트 반영
- 새 architecture spec 문서화

**Phase 11.3** — SPY return-based scoring 구축
- 현재 fusion-vs-fusion 채점 폐기
- 진짜 시장 결과 (SPY return T+N) 기반 검증
- 28 historical sessions × SPY return 매핑 → 진짜 정확도 측정

**Phase 11.4** — Flow capture layer 구축
- CFTC speculator positioning (full 24h 시스템에)
- ETF flows (SPY/QQQ/sector)
- Options GEX
- AAII/NAAIM sentiment
- (선택) Insider trading, Margin debt

**Phase 11.5** — Validated indicator 만 사용
- USEPU, GEPU, USDKRW 등 폐기 또는 weight ↓
- 검증된 7가지 leading 만 (SAHM, SLOOS, Baa-Aaa, KRE/SPY, HYG/IEF, USDJPY, NFCI)
- + Flow indicators 추가

**Phase 11.6** — 새 Fusion + Cascade 설계
- Stress signal + Flow signal 결합
- "stress HIGH + flow BULLISH = false positive 가능" detection
- "stress LOW + flow BEARISH = 다가오는 위험 (사용자 질문 2 의 통찰)" 우선

**Phase 11.7** — 새 Dashboard + Validation
- 새 architecture 에 맞는 UI
- Holdout test (2000-2019 train, 2020+/2024 검증)

**Phase 11.8** — 점진 Migration
- 기존 시스템과 병렬 운영 (1-2개월)
- 새 시스템 검증 후 cutover

#### 보존 항목 (재설계에서도 유지)
- LLM Liq + Geo agent (validated stress signal 부분만)
- 7 fusion mechanism (M1-M7, 새 indicator 에 적용)
- Carry trade detector (Phase 10 v3-2layer Week 1, validated)
- Multi-horizon framework (24h/5d/7d 구조 유지, 단 진짜 차별화)
- Position sizing 3 mode + Hybrid
- GitHub Pages 자동 동기화
- Daily complete pipeline 자동화

#### 기각 항목
- USEPU/GEPU 의존 (Round 2 0/7 episodes)
- Catalyst-driven 일률 stress 가정 (BoJ dovish 무시)
- Fusion-vs-fusion scoring (circular validation)
- 5d/7d cosmetic copy (단순 reweighting)
- TimesFM/Chronos 단독 (R² 음수)
- Korean market 특화 (KRX, 신용잔고, USDKRW)

#### Option A: 새 Top 5 점진 도입 (1-8주)
- Phase 10.1: Carry trade risk + Currency basis + CFTC margin (1-2주)
- Phase 10.2: Granger discovery + HMM persistence (4-8주)

#### Option B: Carry trade risk 만 minimal (1주, 추천)
- Yen 2024 case detect 가능, 무료 데이터
- 효과 검증 후 다른 signal 추가 결정

#### Option C: 방향 전환 — Regime-aware Alpha Extraction
- 7d prediction 포기, 24h 유지
- regime별 portfolio 자동화 강화
- Practitioner consensus 일치

#### Option D: 24h 시스템 최적화만
- 현 시스템 fine-tune
- 새 방법론 도입 보류
- 안정적이나 7d 약점 그대로

#### 새 Architecture 컨셉 (Phase 10 v2)

```
[Existing 24h system] ✓ 검증됨, 유지
   Liq + Geo Agent → M1-M7 fusion → 24h alert

[New Crisis-type-specific detectors] (Phase 10 v2)
   ├── Carry Trade Risk (USDJPY/USDKRW + rate diff + CFTC)
   ├── Currency Basis Divergence (CNH-CNY)
   ├── Margin Cascade (CFTC + CME margin spike)
   ├── Granger Discovery (rolling causality)
   └── HMM Persistence (regime base rate)
   ↓
   Crisis-type-aware 7d Alert

[Cascade integration]
   24h alert + Crisis-type 7d Alert → Action
```

새 cascade scenarios (crisis-type-aware):
- `carry_unwind_imminent`, `fx_crisis_imminent`, `leverage_cascade_imminent`
- `policy_pivot_imminent`, `regime_persist`, `regime_transition_unknown`

---

## 4. ★ 핵심 검증 결과

### 4.1 24h System (검증됨, 신뢰도 A)
- 8 episodes backtest: ±0.4pp 정확도
- 5/5 random OOS: 100% directional hit
- Live T+1: HIT 1, PARTIAL 1, MISS 0 (50% strict, 100% directional)

### 4.2 Multi-horizon System (Phase 9.2.2 결과)
**Alert Level Correlation (28 sessions)**:
| Pair | Same Alert | Total | Correlation |
|---|---|---|---|
| 24h vs 5d | 28/28 | 28 | **100.0%** ★ |
| 24h vs 7d | 25/28 | 28 | 89.3% |
| 5d vs 7d | 25/28 | 28 | 89.3% |
| **평균** | | | **92.9%** |

**Escalation 패턴 (10 episodes)**:
| Horizon | Escalations | False Positives | FP% |
|---|---|---|---|
| 24h | 5 | 2 | 40% |
| 5d | 5 | 2 | 40% |
| 7d | 5 | 2 | 40% |

→ **24h/5d/7d 모두 같은 시점에 격상, 같은 시점에 회복**. 본질적으로 동일 신호.

**Cascade Scenario 분포 (28 sessions)**:
- full_stress 53.6%, regime_calm 17.9%, broad_recovery 17.9%
- approaching_risk / front_loaded_caution / escalating_stress 각 3.6%

### 4.3 결론 — 데이터 입증
- ⚠️ **5d 는 24h 의 cosmetic copy** (100% same alert)
- ⚠️ **7d 는 약간의 stress 완화만** (구조적 차별화 효과 없음)
- ⚠️ **LLM reasoning 분리 없이는 진짜 multi-horizon 불가능**

### 4.4 검증 자체의 한계
- 28 sessions 중 채점 가능 15개 (sparse)
- "no_target" 다수 — Episode 내 daily fusion 부재
- Sample size 작아 통계 신뢰도 제한적
- 향후 dense backtest data 필요

---

## 5. 결정 대기 사항 (Phase 9.3 방향)

| Option | 방향 | 작업량 | 효과 | 비용 |
|---|---|---|---|---|
| **A. 5d 폐기 + 7d 보조 유지** | 단순화 + 정직 | 0.5일 | dashboard 정직성 ↑ | 0 |
| **B. Config 극단 차별화** | weight 0.3/0.7 등 + threshold 분리 | 1일 | 약한 차별화 가능 | 0 |
| **C. Indicator 재매핑** | 5d=5d MA only, 7d=monthly | 2~3일 | 의미 있는 차별화 | 0 |
| **D. LLM prompt 분리 (Phase 9.3)** | 진짜 horizon reasoning | 2~3일 | 진짜 multi-horizon | LLM cost ×3 또는 1.2 |

**솔직한 추천**: A (즉시) → D (정석, 검토 후 결정)

---

## 6. Files Inventory

### 6.1 Scripts (D:\코워크\scripts\)
| 파일 | 역할 |
|---|---|
| `analyze_v2.py` | LLM prompt 생성 + fusion (24h legacy + multi-horizon) |
| `scoring.py` | predictions 기록 + horizon-aware 채점 |
| `alerts.py` | alert level 변경 detection + Windows toast |
| `daily_dashboard.py` v3 | multi-horizon dashboard 자동 생성 |
| `publish.ps1` | GitHub Pages 자동 push |
| `daily_complete.ps1` | 통합 파이프라인 (8단계) |
| `multi_horizon_backtest.py` | Phase 9.2.2 검증 |
| `position_sizing.py` / `multi_asset_sizing.py` / `hybrid_sizing.py` | Sizing 로직 |
| `snapshots/fetch_*.py` | 데이터 수집 |

### 6.2 Data (D:\코워크\analyses\)
- `liq_output_*.json`, `geo_output_*.json` (LLM 분석 결과 28+ sessions)
- `fusion_*.json` (multi-horizon fusion 결과)
- `alert_history_{24h,5d,7d}_live.jsonl`, `alert_history_live.jsonl` (legacy)
- `alert_history_{24h,5d,7d}_backtest.jsonl`
- `predictions_{24h,5d,7d}.jsonl`, `predictions.jsonl` (legacy)

### 6.3 Dashboards (D:\코워크\)
- `daily_dashboard.html` (live, file://)
- `master_final_report.html` (Phase 0~8 종합)
- `portfolio_backtest_dashboard.html` (Phase 8)
- `backtest_multi_horizon_report.md` (Phase 9.2.2 결과)
- **`PRD.md`** (이 문서)

### 6.4 GitHub Pages
- Repo: https://github.com/hong4137/macro-regime
- Live: https://hong4137.github.io/macro-regime/
- 자동 동기화: `daily_complete.ps1` [8/8] publish

---

## 7. Daily Operations Routine

### 매일 아침 (한국 시간, 미국 장 마감 후)
```powershell
# 1. 데이터 수집
cd D:\코워크\scripts\snapshots
python fetch_all.py --date YYYY-MM-DD

# 2. LLM prompt 생성
cd D:\코워크\scripts
python analyze_v2.py --date YYYY-MM-DD --mode prompt

# 3. LLM 분석 (Cowork 또는 외부 LLM)
# liq_output_YYYYMMDD.json + geo_output_YYYYMMDD.json 저장

# 4. 통합 파이프라인 (자동 8단계 + GitHub Pages push)
.\daily_complete.ps1 -Date YYYY-MM-DD
```

### 검증
- Local: `D:\코워크\daily_dashboard.html` (5분 자동 새로고침)
- Live: https://hong4137.github.io/macro-regime/ (1~2분 후 갱신)
- Hit rate: `python scoring.py --report`

### Schedule (선택)
- Windows Task Scheduler: 매일 오전 8:30 `daily_complete.ps1` 자동 실행
- (단, LLM 분석 단계는 수동)

---

## 8. Open Issues / Future Work

| Priority | Issue |
|---|---|
| 1 | Phase 9.3 방향 결정 (5d/7d cosmetic copy 대응) |
| 1 | Backtest data sparse — episode 별 daily fusion 추가 |
| 2 | Cascade scenario 한글화 (`broad_recovery` → "회복 진행" 등) |
| 2 | 4/28 prediction conservative bias — Liq agent 의 leading 신호 underweight 가능성 |
| 3 | Dashboard fine-tune (사용자 피드백 기반) |
| 3 | SPY return 기반 채점 (fusion vs fusion 이 아닌 진짜 시장 결과) |
| 4 | Phase 11: 자동 LLM 호출 (Cowork → OpenRouter API) — 완전 무인 운영 |
| 4 | Mobile UI 최적화 |

---

## 9. Decision Log

| 날짜 | 결정 | 이유 |
|---|---|---|
| 2026-04-29 | Multi-horizon Option A2 (LLM 1회 + Python fusion 3회) | Cost-efficient prototype |
| 2026-04-29 | GitHub Pages public repo | 무료, 외부 접근, daily push |
| 2026-04-30 | Phase 9.2.1 → 9.2.2 순서 진행 | 데이터 기반 검증 우선 |
| 2026-04-30 | escalating_stress cascade scenario 추가 (10번째) | 4/29 첫 multi-horizon ELEVATED 케이스 분류 부정확 |
| 2026-04-30 | **Phase 9.2.2 결과: 5d/7d 단순 smoothing 입증** | 평균 92.9% correlation |
| 2026-04-30 | Phase 9.3 보류 → Phase 10 새 방법론 리서치 결정 | 단순 LLM 분리보다 fundamental 한 접근 필요 |
| 2026-04-30 | Phase 10 1차 리서치 완료 (8 카테고리, 30+ 방법론) | Top 10 우선순위 + Roadmap 도출 |
| 2026-04-30 | Phase 10 2차 리서치 진행 — 실제 사례 / failure modes / 실무자 | 1차 보완 |
| 2026-04-30 | Round 2 결과: Round 1 학술 Top 5 invalidated | Empirical 0~1/7 detection |
| 2026-04-30 | **Phase 10 v3 사용자 제안: 3-layer hybrid architecture** | 24h + Calm + Siren + Soft invalidation |
| 2026-04-30 | Round 3a tech feasibility = VIABLE 80% confidence | MVP 4-6주, soft invalidation 권장 |
| 2026-04-30 | Round 3b stress test 진행 — 가설 5개 검증 + coverage gap + alternative | H1-H5 empirical evidence |
| 2026-04-30 | Round 3b 결과: confidence 80% → 58% 하향. Layer 2 (calm mode) 폐기 권고 | H1/H2/H4 invalidated, H3/Layer 1 confirmed |
| 2026-04-30 | **Phase 10 v3-2layer 확정 (Option C)** — 24h + Siren MVP, Layer 2 폐기 | Stress test 와 일치, 정직한 architecture |
| 2026-04-30 | Phase 10 v3-2layer Week 1-5 roadmap 확정 | USDJPY → CFTC margin → Integration → Hardening |
| 2026-05-02 | Phase 10 v3-2layer Week 1 완료 (Carry detector 4/4 validated) | 2024-08-05 CRITICAL, 평시 CALM 정확 |
| 2026-05-02 | 5/1 daily cycle: 첫 stress→recovery 전환 검증 (4/29 ELEVATED → 5/1 broad_recovery) | T+3 multi-horizon HIT |
| 2026-05-02 | **사용자 fundamental 질문**: stress signal 필연적 반영? + 수급 capture? | 시스템 진짜 정체 노출 |
| 2026-05-02 | System Audit + Alternatives 보고서 작성 | 5,000 + 9,847 단어 |
| 2026-05-02 | **Phase 11 시작 — 완전 재설계 결정** | 사용자 결정: Full redesign, 유료 ROI 검증, Korean 배제 |
| 2026-05-02 | Round 4 무료 vs 유료 ROI 완료 | 7개 paid 가치 없음, SpotGamma만 MAYBE |
| 2026-05-02 | Phase 11 Architecture Spec 작성 | 6 layer, 7 cascade scenarios |
| 2026-05-02 | Phase 11.3 Flow Layer 구축 (flow_detector.py) | 6 sub-detectors, 4 케이스 검증 |
| 2026-05-02 | Phase 11.4 analyze_v3.py 통합 | Liq + Flow + M8 + 새 Cascade |
| 2026-05-02 | Phase 11.5 28 sessions batch backtest | v2 등가 채점 결과 부정확 (FP 32%) |
| 2026-05-02 | **사용자 통찰: 채점이 등가 X, 심리 테스트 방식** | 평시 strict, severe wide |
| 2026-05-02 | Non-linear Scoring v3 도입 | HIT 41%, FP 10% (v2 대비 +13pp / -22pp) |
| 2026-05-02 | **smart_money_warning 75% HIT** | 사용자 통찰 #2 결정적 입증 ★ |

---

## 10. Live 운영 기록 (4/27 ~ 4/29)

| 날짜 | Legacy 24h | 24h | 5d | 7d | Cascade | T+1 채점 |
|---|---|---|---|---|---|---|
| 4/27 | YELLOW | YELLOW | YELLOW | GREEN | front_loaded_caution | (4/28: HIT) |
| 4/28 | GREEN | YELLOW | YELLOW | GREEN | broad_recovery | (4/29: PARTIAL) |
| 4/29 | ELEVATED | **ELEVATED** ★ | **ELEVATED** | **YELLOW** | escalating_stress | 대기 |

핵심 catalyst: BoJ 4/30 D-1, USEPU 4/27 481 → 4/28 578 sustained spike, USDKRW 1476 sustained EM stress.

---

**다음 업데이트**: Phase 9.3 방향 결정 후 또는 5/1 BoJ 결과 후 cycle.
