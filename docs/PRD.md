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

### ⏳ Phase 9.3 (결정 대기) — 방향 선택 필요
- 사용자 결정에 따라 다음 4 option 중 하나 진행

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
| 2026-04-30 | (대기) Option A/B/C/D 중 결정 | 사용자 선택 |

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
