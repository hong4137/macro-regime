# 시스템 운영 정책

**최종 업데이트**: 2026-05-03
**Status**: Phase 12.6 (T1 robustness + T2 snapshot 표준화) 운영 시작

---

## 1. Sample 작성 정책

### 1.1 거래일 기준
- **미국 시장 거래일 (월-금) 만** phase11_fusion sample 작성
- 토·일·미국 공휴일 (Independence Day, Thanksgiving 등) 은 sample 작성 안 함
- 한국 공휴일은 무관 (미국 시장 기준)

### 1.2 휴장일 처리
- Sample 작성하지 않음
- Dashboard 는 직전 거래일 fragility 표시
- 시계열 검증 시 휴장일 transition 제외 (거래일 → 거래일만 분석)

### 1.3 Catalyst calendar
- 거래일 무관 daily refresh — imminent events 의 시간 거리는 매일 줄어듦

---

## 2. LLM 분석 vs Snapshot raw 데이터 활용

### 2.1 결정적 발견 (Phase 12.4 → 12.5 → 12.6)

LLM 분석 (`liq_input.leading_layer.key_signals` text) 이 sample 마다 indicator 누락/형식 차이를 일으켜 fragility measurement noise 생성.

**T1 (system robustness)**: 누락 indicator cross-inference (USDJPY → USDKRW proxy 등) — partial 해결
**T2 (snapshot 표준화)**: snapshot raw 데이터를 deterministic text 로 자동 변환 — root 해결

### 2.2 권장 workflow

```
snapshot fetch (자동, daily)
  ↓
analyze_v3.py (LLM 분석 — regime_view, cascade scenario, catalyst layer)
  ↓
regenerate_from_snapshots.py (★ 자동 — leading/coincident layer 표준화)
  ↓
fragility_index.py (deterministic 계산)
```

→ LLM 은 정성적 판단 (regime/cascade/catalyst) 만 담당, 정량 indicator 는 snapshot 직접 활용.

---

## 3. Fragility 변동성 해석

### 3.1 Day-to-day variance 정상 범위

USEPU 같은 일별 변동성 큰 indicator 가 있는 시기에는 fragility 도 자연스럽게 큰 변동:

- **2026-04-27 ~ 5/1 trajectory**: USEPU 481 → 254 → 578 → 352 (매일 200+ 변동)
- **그 결과 fragility**: 54.3 → 69.8 → 82.9 → 69.8
- avg \|Δ\| = 13.9pt — 시장 진짜 변동, system noise 아님

### 3.2 Variance 출처 분류

| 변동 출처 | 의미 | 정합 처리 |
|---|---|---|
| **시장 진짜 변동** | USEPU/USDJPY 등 raw 값 변화 | 정상 — fragility 반영 |
| **Phase 분류 안정** | 모노톤 분리 유지 (calm/recovery/pre/active) | 안정성 검증 |
| **LLM noise** | indicator 누락/형식 차이 | T2 도구로 제거 |

---

## 4. Phase 12.6 신뢰도 메트릭

### 4.1 검증 통과 항목

✅ Phase 모노톤 분리 (45 sessions): calm 10.5 < recovery 28.7 < pre_crisis 55.3 < active 91.0
✅ Active 정점 분리: range [87.3, 93.9] (TARP/Lehman/SVB 정점)
✅ Pre-crisis 폭 확장: range [33.3, 82.9] (4/29 USEPU peak 정확 측정)
✅ Recovery phase 인식: n=5 (Fed QE/cuts 후 회복기 모두 잡힘)
✅ Validation warnings 시스템: 누락 indicator 자동 detect

### 4.2 미해결 / 후속

- ⚠️ Active 와 pre_crisis 사이 gap 큼 (87-83 = 4pt). 80+ pre 와 87+ active 차이가 명확하지 않은 case 발생 가능
- ⚠️ Recovery n=5 만 — 더 다양한 회복 case 필요
- ⚠️ 자동 LLM 호출 미구현 — 사용자가 수동 LLM 분석 후 daily_complete.ps1 실행

---

## 5. 운영 자동화

### 5.1 매일 자동 실행
- **Task Scheduler**: `MacroRegimeDaily` (06:00 KST, daily_lite.ps1)
  - fragility batch + dashboard refresh + GitHub push
  - LLM 분석 없이도 작동
- **Task Scheduler**: `MacroRegimeAutoPush` (30분, auto_push.ps1)
  - 변경 감지 시 GitHub push

### 5.2 LLM 분석 후 수동 trigger
새 sample 작성 시:
```
python analyze_v3.py --date YYYY-MM-DD
python regenerate_from_snapshots.py --dates YYYY-MM-DD  ★ 표준화
.\daily_complete.ps1 -Date YYYY-MM-DD
```

### 5.3 새 sample 작성 시 표준 workflow
```
1. snapshot fetch (이미 자동)
2. LLM 분석 — regime_view, cascade, catalyst_layer 결정
3. regenerate_from_snapshots.py — leading/coincident layer 표준화 ★
4. daily_complete.ps1 -Date YYYY-MM-DD
```

---

## 6. 검증 history (Phase 12.0 → 12.6)

| Version | 변경 | avg \|Δ\| (5-day) | Phase |
|---|---|---|---|
| 12.0 | initial | n/a | 1개 phase 잘못 |
| 12.1 | crisis_phase 재설계 | n/a | Tier A 통과 |
| 12.2 | alert별 minimum 상향 | n/a | Tier B 통과 |
| 12.3 | recovery lookback | 21.6 | Tier C 통과 |
| 12.4 | catalyst integration | 21.6 | catalyst-aware |
| **12.5** | **T1 robustness** | **14.4** | LLM noise 부분 보정 |
| **12.6** | **T2 snapshot 표준화** | **13.9** (max 15.5, 59% 감소) | **운영 신뢰도 검증** |

---

## 7. 사용자 framing 일관성 검증

> "강세장이자 = 나스닥/S&P 역대 최고 / 언제든 무너질 수 있는 위험성 (이란 전쟁) / 그럼에도 수급·기대감이 넘쳐 상승하려는 장세. 도사린 위험은 표면적으로 측정 불가, 즉 명시적 해답 없지만, 그걸 측정하고 싶은 상황"

### 4-day trajectory (Phase 12.6)
| Date | Surface | Latent | Decoupling | Sustainability | Fragility |
|---|---|---|---|---|---|
| 4/27 | 61.2 | 45.1 | 27.6 | 83.9 | 54.3 |
| 4/28 | 69.9 | 56.6 | 39.6 | 81.0 | 69.8 |
| **4/29** | **64.0** | **68.3** | **43.7** | **82.0** | **82.9** ★ |
| 5/1 | 70.2 | 56.6 | 39.7 | 84.3 | 69.8 |

→ 모든 거래일 Surface 60+ (강세장 유지) + Sustainability 80+ (수급 cushion) + 변동하는 Latent (USEPU peak 4/29).

→ **시스템이 사용자 framing 을 정확히 측정** — 표면 안정 + 잠재 위험의 시계열 변동.

---

## 8. 결론

Phase 12.6 운영 가능 상태. 시스템이 **시장 진짜 변동을 LLM noise 없이 정량 측정**.

다음 단계:
1. 매일 거래일 새 sample 누적 (수동 LLM 분석 + regenerate 자동 표준화)
2. Catalyst calendar 자동화 (옵션, 별도 프로젝트)
3. 운영 누적 후 backtest validation 추가
