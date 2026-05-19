# 시스템 운영 정책

**최종 업데이트**: 2026-05-19
**Status**: Phase 12.18 (Sample 확장 n=228, weights Phase 12.16 유지) + Manual Trigger 정책

---

## 0. ★ Manual Trigger 정책 (★ 2026-05-19 신규)

### 0.1 배경

- 자동 스케줄링 (Windows 작업 스케줄러 / cron) = ★ 사용자 컴퓨터가 켜져 있어야 작동
- 사용자 컴퓨터는 항상 켜져 있지 않음 → ★ 자동 스케줄링 의미 없음
- 따라서 batch 실행은 ★ "사용자 trigger 기반" 으로 운영

### 0.2 Trigger 방식

★ **사용자가 "오늘 [날짜] [시각] 입니다" 라고 알려주면**, Claude 는 ★ 그 시점 기준으로 ★ 누락된 batch 를 자동 진행한다.

**예시 trigger 표현** (★ 모두 동일하게 동작):
- "지금 5월 19일 오후 10시입니다"
- "오늘 5/20 새벽 1시"
- "지금 한국시각 화요일 밤"
- "한국시각 2026-05-20 06:00"
- "오늘은 5월 20일이야"

### 0.3 Trigger 시 Claude 가 자동 진행하는 작업

1. ★ **누락된 날짜 식별** — 마지막 batch 실행 시각 ~ 사용자 알린 시각 사이에 누락된 거래일
2. ★ **각 날짜별 batch 진행 (PowerShell command 작성 + 사용자에게 제공)**:
   - fetch_historical.py --date YYYY-MM-DD (시장 데이터 가져오기)
   - fetch_tier1.py --date YYYY-MM-DD (보조 지표 가져오기)
   - create_sample_from_snapshot.py --date YYYY-MM-DD (sample 생성)
   - regenerate_from_snapshots.py --dates YYYY-MM-DD (텍스트 표준화)
   - analyze_v3.py --date YYYY-MM-DD (캐스케이드 LLM 분류)
   - fragility_batch.py (잠재 위험도 일괄 재계산)
   - daily_dashboard.py (대시보드 재생성)
   - publish.ps1 (GitHub Pages 동기화)
3. ★ **휴장일 처리** (§1.2 정책 따라): 미국 시장 휴장일은 직전 거래일 inherit + `_weekend_inherited_from` 표시
4. ★ **잠재 위험도 / phase11_fusion / 캐스케이드 모두 갱신**
5. ★ **GitHub Pages publish 까지 완료**

### 0.4 Trigger 후 사용자가 확인할 것

- 새 잠재 위험도 값 (이전 inherit 값 → 진짜 값으로 변경)
- 새 캐스케이드 분류 (auto_pending → 진짜 시나리오)
- 대시보드의 표/카드 모두 갱신
- GitHub Pages (https://hong4137.github.io/macro-regime/) 반영

### 0.5 Trigger 없을 때

- ★ 대시보드는 ★ 직전 batch 데이터 그대로 유지
- 휴장일 inherit 로 빈 자리는 채움 (정책 §1.2)
- ★ 사용자가 trigger 안 하면 ★ 자동 갱신 없음 (★ 명시적 정책)

### 0.6 Trigger 의 실용 의미

- 사용자는 ★ 시장 마감 후 (한국시각 새벽 5시) ~ 다음 날 사이 ★ 편한 시점에 trigger
- 매일 trigger 안 해도 됨 — 며칠 누적해서 한 번에 trigger 가능
- 단, ★ 5/18 부터 pre-registered prediction 추적 시작했으므로 ★ 누락 일수가 많으면 신뢰도 평가에 영향
- 권장: ★ 1주일에 ★ 최소 2~3번 trigger (월/수/금 정도)

### 0.7 Claude 의 의무 (★ 명시)

- 사용자가 trigger 하면 ★ 즉시 누락 날짜 진단
- ★ 휴장일은 inherit (자동 fetch 시도 X)
- ★ 실제 시장 개장일 만 fetch + 분석
- ★ Trigger 없이 화면 갱신 요청 시 → ★ inherit 만 진행 (실제 batch 안 함)
- ★ 정책 명시: "Trigger 가 있어야 진짜 batch 실행" — Claude 가 자체 판단으로 시간을 추정하지 않음

### 0.7.1 ★ 장 개장 전 / 진행 중 처리 (★ 2026-05-20 신규, 정책 빈 곳 보완)

**배경**: 5/19 사용자 trigger (KST 22:16) 시점에 미국 시장 ★ 개장 14분 전. 제가 ★ 5/19 까지 ★ 5/18 inherit 으로 채워 넣음 → ★ "5/19 데이터 있는 것처럼" 표시. ★ 정직성 위반.

**원인**: 정책 §1.2 (휴장일 inherit) 의 빈 곳 — "장 개장 전" / "장 진행 중" 처리 정의 없음.

**★ 새 정책 (즉시 적용)**:

| 시장 상태 | 처리 | dashboard 표시 |
|----------|------|----------------|
| 휴장일 (토/일/공휴일) | 직전 거래일 inherit (§1.2) | inherit 표시 |
| **★ 개장 전 (해당 날짜 시장 안 열림)** | ★ **데이터 없음** (sample 생성 X) | ★ "—" 또는 "장 개장 전" 명시 |
| **★ 장 진행 중 (개장 ~ close 사이)** | ★ **데이터 없음** | ★ "장 진행 중" 명시 |
| 장 마감 후 (close 데이터 가용) | fetch + 분석 | 정상 표시 |

**★ Claude 의 trigger 시 의무**:
1. 사용자 trigger 시각 (KST) → ET 변환
2. ★ "오늘 날짜" 시장 상태 판단:
   - ET 09:30 이전 = 개장 전 → sample 생성 X
   - ET 09:30 ~ 16:00 = 장 진행 중 → sample 생성 X
   - ET 16:00 이후 (실제로 16:30 ~ fetch lag) = close 데이터 fetch 가능
3. ★ 개장 전 / 진행 중 날짜는 ★ "데이터 없음" 으로 표시 (★ inherit 으로 채우지 않음)

**★ 정직성 commitment 보강**:
- ★ "데이터 없음 = 정직" / "inherit = 정직" / "실제 fetch = 정직"
- ★ "장 진행 중 inherit = 정직성 위반" — ★ 데이터가 ★ "있는 것처럼" 보임

### 0.8 ★ Default 값 명시 의무 (★ 2026-05-20 신규)

**배경**: 5/16~5/19 dashboard 에서 ★ regime_view 가 default 값 (0.25/0.5/0.2/0.05) 으로 ★ 4일 동안 그대로 표시됨. 사용자가 "스트레스 4일째 같다" 라고 ★ 지적할 때까지 ★ Claude 가 발견 못함.

**원인**:
- `create_sample_from_snapshot.py` 가 LLM 분석 없을 시 ★ default regime_view 자동 삽입
- ★ 캐스케이드는 `auto_pending` 이라고 명시적 표시했지만 regime_view 는 ★ 정상 숫자처럼 보임
- Claude 가 inherit 진행 시 ★ regime_view default 여부 확인 안 함

**★ 새 정책 (즉시 적용)**:

1. ★ **Trigger 시 Claude 는 ★ regime_view 와 cascade ★ 둘 다 확인 의무**
   - default 값 발견 시 → ★ 즉시 사용자에게 보고 + 추정 또는 inherit 진행
   - "auto_pending" 같이 ★ 명시적 표시 없는 한 default 가능성 가정

2. ★ **시스템 코드 개선 (Phase 12.19)**:
   - `create_sample_from_snapshot.py` 의 default regime_view 에 `_regime_source: "default_no_llm"` 메타 추가
   - `daily_dashboard.py` 에서 `_regime_source == "default_no_llm"` 인 경우 ★ "(추정 미정)" 표시
   - `_regime_inherited_from` 또는 `_regime_source: "manual_claude_..."` 표시도 동일하게

3. ★ **사용자 trigger 시 chain 정정**:
   - fetch → create_sample → regenerate → ★ **regime_view default 확인** → 필요시 LLM/manual 분류 → cascade 분류 → fragility batch → dashboard → publish

**★ Default 값 silently 두지 않는 commitment**:
- ★ 모든 default 값은 dashboard 에 ★ 명시적 표시
- ★ Trigger 시 Claude 는 ★ 모든 default 값을 ★ 확인 + 갱신 의무
- ★ 사용자가 지적할 때까지 default 가 그대로 남는 일 ★ 재발 방지

---

## 1. Sample 작성 정책 (★ v3.1 갱신 — 캘린더 전체)

### 1.1 캘린더 전체 sample (★ 정책 변경)

**이전 (v2 까지)**: 거래일만 sample 작성  
**현재 (v3.1)**: **모든 캘린더 날짜 sample 작성** (토/일/공휴일 포함)

근거:
- 사용자 framing: "잠재 위험은 표면적으로 측정 불가" — 시장 휴장에도 macro/뉴스 변동 추적
- USEPU (FRED EPU index) 매일 publish (주말 포함)
- 누락 sample 갭이 trajectory 분석 정합성 약화
- 거래일 grid 만 사용하면 weekend macro shock 포착 불가

### 1.2 휴장일 처리 (★ 신규)

| 항목 | 처리 |
|---|---|
| **시장 데이터 (SPY/VIX/KRE 등)** | 직전 거래일 EOD reuse, `_market_data_origin` 명시 |
| **macro 데이터 (USEPU/FRED 등)** | 휴장일 raw 값 그대로 사용 (FRED 매일 publish) |
| **USEPU 보정** | weekend (토/일) → 5dMA 적용 (weekend bias 제거, 섹션 12 참조) |
| **flow/sentiment** | 직전 거래일 reuse + `_weekend_reused_from` 표시 |
| **메타 표시** | `market_session: "open" or "closed"`, `market_data_origin` 명시 |
| **phase11_fusion** | 휴장일 = 인접 거래일 결과 inherit + `_weekend_inherited_from` 표시 (analyze_v3 재계산 시 갱신) |

### 1.3 거래일 sample (정상)

- 미국 거래일 (월~금, 공휴일 제외): 정상 sample
- `market_session: "open"`
- 모든 layer 자동 계산

### 1.4 Catalyst calendar
- 캘린더 무관 daily refresh — imminent events 의 시간 거리는 매일 줄어듦

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

---

## 9. Data Fetch Timing 정책 (2026-05-15 추가)

### 9.1 발견 배경

5/15 fetch (한국 19:30 = 미국 EDT 06:30) 검증에서 발견:
- fetch_tier1.py 의 yfinance ETF 결과 `latest_date: 2026-05-15` 라벨
- 그러나 미국 정규장 (EDT 09:30-16:00) 미개장 시각
- 실제 데이터 검증: SPY 5/14 EOD = 748.17, snapshot 의 5/15 라벨 = 739.40 → **intraday partial 데이터**
- 즉 야후 finance 가 미국 EDT 일자 라벨로 fetch 시각 기준 데이터를 반환 — **labelling 신뢰 불가**

### 9.2 권고 fetch 시각

| 시간대 (KST) | 미국 ET 시간 | 미국장 상태 | 데이터 신뢰도 | 권고 |
|---|---|---|---|---|
| **06:00-09:00** | **17:00-20:00 (전일)** | **정규장 EOD 후** | **★★★ complete** | **권고** |
| 09:00-18:00 | 20:00-05:00 | 폐장 | ★★ stale (전일 EOD 그대로) | 가능 |
| 18:00-23:30 | 05:00-10:30 | 미개장 또는 장중 partial | ⚠️ unreliable | **금지** |
| 23:30-06:00 (익일) | 10:30-17:00 | 정규장 진행 | ⚠️ intraday partial | **금지** |

→ **권장**: 한국 시각 06:00-09:00 사이 daily Task Scheduler 실행
→ **금지**: 한국 시각 저녁 (미국 ET 새벽/아침) 수동 fetch

### 9.3 fetch 정확성 교차 검증 규칙

snapshot 의 `latest_date` 와 `generated_at_utc` 의 시차 검증:

```
KST_fetch_time = generated_at_utc.astimezone(KST)
ET_fetch_time = generated_at_utc.astimezone(US/Eastern)

# 미국 정규장 EOD = ET 16:00. 그 이후 fetch 만 EOD complete.
if ET_fetch_time < 16:00 (장중) or ET_fetch_time < ET market open:
    yfinance ETF 의 "오늘 라벨" 데이터는 의심
    → snapshot 의 raw history 에서 직전 거래일 (D-1) 값 사용
```

### 9.4 yfinance label 불일치 해결법

snapshot 작성/검증 시:
1. FRED 일별 데이터 (VIX, USEPU, UST 등) — 1일 lag 신뢰
2. yfinance ETF — `latest_date` 만 신뢰하지 말고 raw history 의 **연속성 검증**
   - `D-1` close 가 sample 의 직전일 EOD 와 일치하는지 확인
   - 불일치 시 사용자가 manual override
3. `_meta.event` 또는 `data_caveat` 에 fetch timing 명시

### 9.5 운영 정책 update

- **daily Task Scheduler 시각**: 06:00 KST (확인 완료)
- **수동 fetch 금지 시간**: 한국 시각 17:00-06:00 (미국장 미개장/장중)
- **데이터 검증**: snapshot 의 raw history 직접 확인 후 sample 작성

---

## 10. 검증 history (Phase 12.6 → 12.9.1)

| Version | 변경 | sessions | recovery n | 비고 |
|---|---|---|---|---|
| 12.6 | T2 snapshot 표준화 | 45 | 4 | LLM noise 부분 보정 |
| 12.7 | path 3f (lat>40 → pre_crisis) | 50 | 4 | 2023-10-09 false negative fix |
| 12.8 | path 3c 강화 (lat>30 → pre_crisis) | 52 | 4 | 2023-12-13 false positive fix |
| 12.8 | + 5/12, 5/13, 5/14 sample | 54 | 6 | 5/13-5/14 첫 연속 recovery 진입 |
| **12.9** | **캘린더 전체 sample (4/30, 5/2, 5/3, 5/10, 5/11 신규)** | **59** | 6 | weekend 정책 변경, 단순 5dMA 시도 |
| **12.9.1** | **USEPU 하이브리드 보정 (weekday=raw, weekend=5dMA)** | **60** | **7** | **2023-10-09 path 3f true positive 보존 + weekend bias 제거** |

### 10.1 View A 최종 검증 (4/27-5/15, 60 sessions batch, v3.1 하이브리드)

| 검증 항목 | 결과 |
|---|---|
| Surface 강세 실현 | SPY +3.88% (711.58→739.17, 16 거래일) |
| 잠재 위험 측정 정확 | fragility peak 82.9 (4/29, USEPU raw 578 weekday signal) |
| 수급 cushion 실효 | leaning_bullish 16 거래일 지속 |
| Catalyst 통과 → 자동 recovery | FOMC+CPI+I/O+Retail Sales **4 critical** 통과 → 5/13-15 recovery |
| Phase 모노톤 분리 | calm 8.6 < recovery 36.2 < pre 63.3 < active 91.0 (gap 4.9pt 안전) |
| Latent 자체 감소 | 71.4 peak → 57.1 (-14.3) |
| Sustainability 최강 | 100.0 (5/6, 5/8, 5/13 등) |
| **Weekend bias 제거** | 5/2 raw 1056 → 5dMA 379 (system artifact 처리) |
| **Weekday spike 보존** | 4/29 raw 578, 5/5 raw 540 imminent peak 유지 |
| **path 3f true positive** | 2023-10-09 raw 264 elevated 보존 — T+14d SPY -5% 잡힘 |

→ **View A 완전 정합**. 4/29 imminent (82.9) → 5/15 recovery (45.7) = **-37.2 fragility 감소 in 16 거래일**.

---

## 11. Cross-platform 운영 (2026-05-15 추가)

### 11.1 발견 배경

Cowork bash 샌드박스 (Linux) 에서 직접 스크립트 실행 시도 시 Windows 경로 (`D:\코워크`) 하드코드 에러 발생.

### 11.2 패치된 스크립트

다음 스크립트에 환경변수 + 자동 OS 감지 로직 추가:

- `D:\코워크\scripts\regenerate_from_snapshots.py`
- `D:\코워크\scripts\fragility_batch.py`
- `D:\코워크\scripts\fragility_index.py`

```python
_DEFAULT_WIN = Path(r"D:\코워크")
_DEFAULT_LIN = Path("/sessions/quirky-magical-bardeen/mnt/코워크")
_env_base = os.environ.get("PHASE12_BASE_DIR")
if _env_base:
    BASE_DIR = Path(_env_base)
elif _DEFAULT_WIN.exists():
    BASE_DIR = _DEFAULT_WIN
elif _DEFAULT_LIN.exists():
    BASE_DIR = _DEFAULT_LIN
```

### 11.3 미패치 (Windows 전용)

- `fetch_tier1.py`, `fetch_historical.py` — `.env` API key 위치 Windows 의존
- `daily_complete.ps1` 등 PowerShell 스크립트 — Windows 전용

### 11.4 사용

**Windows**: 기존대로 PowerShell 에서 직접 실행
**Linux bash (Cowork)**: 패치된 3개 스크립트는 자동 동작

---

## 12. USEPU 하이브리드 보정 정책 (2026-05-16 신규, Phase 12.9.1)

### 12.1 발견 배경

USEPU (FRED EPU Daily Index) 의 시계열 특성:

| 현상 | 원인 | 처리 필요 |
|---|---|---|
| **Weekend bias** (토/일 인위적 spike) | FRED EPU = 뉴스 article 비율 기반. 주말 뉴스 corpus 작음 → policy-related 비율 인위 부풀려짐 | ✓ 제거 (system artifact) |
| **Weekday spike** (평일 실제 정책 충격) | 진짜 정책 이벤트 (Trump tariff, FOMC leak, 지정학 등) | ✗ 보존 (true signal) |

**실제 사례 확인**:
- Weekend spike (artifact): 4/11 (토) raw 822, 4/25 (토) raw 564, **5/2 (토) raw 1056** (system max), 5/9 (토) raw 683
- Weekday spike (true signal): 4/29 (수) raw 578 → imminent peak 82.9, **2023-10-09 (월) raw 264 → T+14d SPY -5% 정확히 잡음**

### 12.2 하이브리드 알고리즘

```
target_date 의 요일 확인:
  - weekday (월~금): USEPU = raw (signal 보존)
  - weekend (토/일): USEPU = weekday-only 5-day MA (weekend bias 제거)

5dMA 계산 (weekend 전용):
  - 전 5개 weekday (Mon-Fri) 값의 평균
  - target 이전 + target 자체 (weekday 이면 포함)
  - target 이 weekend 이면 strictly before
```

### 12.3 구현 파일

- **regenerate_from_snapshots.py**: `compute_usepu_weekday_5dma()` helper + `build_standardized_signals()` 분기 처리
- **fragility_index.py**: regex 우선순위 — `USEPU 5dMA` > `USEPU spot` > `USEPU \d+`
  - weekend sample 텍스트: `"USEPU 5dMA 379 (raw spot 1056, weekend bias-adj n=5, ...)"` → 5dMA 매칭
  - weekday sample 텍스트: `"USEPU spot 578 (avg30d 352, 5dMA-ref 364 weekday raw-preserve n=5, ...)"` → spot 매칭 (raw)

### 12.4 정책 검증 (Phase 12.9 vs 12.9.1)

| Test case | v2 (raw 만) | v3 단순 5dMA | **v3.1 하이브리드** |
|---|---|---|---|
| **2023-10-09 path 3f** (월) | 49.5 elevated ✓ | 18.9 calm ❌ | **49.5 elevated ✓** |
| **2026-04-29 imminent** (수) | 82.9 imminent ✓ | 75.5 critical ❌ | **82.9 imminent ✓** |
| **2026-05-02 weekend** (토) | 71.1 (raw 1056 saturated) | 71.1 (5dMA 379) | **71.1 (5dMA 379) ✓** |
| **2026-05-09 weekend** (토) | 73.2 (raw 683 saturated) | 73.2 (5dMA 356) | **73.2 (5dMA 356) ✓** |
| 모노톤 분리 | OK | overlap 0.3pt ⚠️ | **gap 4.9pt 안전 ✓** |

→ **하이브리드는 양 trade-off 모두 해결**.

### 12.5 sample 메타 표시 규칙

신규 sample 작성 시 _meta 에 다음 명시:

```json
{
  "market_session": "open" or "closed",
  "market_data_origin": "YYYY-MM-DD",  // 휴장일이면 직전 거래일
  "usepu_raw": <number>,
  "usepu_corrected_5dma": <number>,
  "correction_method": "weekday_only_5d_ma (hybrid: weekend uses 5dMA)",
  "correction_window_dates": [...]  // 사용된 5 weekday
}
```

### 12.6 미해결 / 후속

- ⚠️ 17개 sample 보정 미적용 (snapshot 부재): 2000-04-14, 2002-10-09, 2008-09-15, 2008-10-10, 2009-03-09, 2009-03-23, 2010-05-06, 2011-08-05, 2012-09-13, 2015-12-16, 2018-02-05, 2018-12-19, 2022-03-16, 2022-06-15, 2024-09-18, 2024-10-01, 2026-04-30, 2026-05-02, 2026-05-03, 2026-05-10, 2026-05-11, 2026-05-14 — snapshot 확보 가능 시 재처리
- ⚠️ FRED USEPUINDXD publish lag — 당일 raw 미공개 시 직전일 latest 사용 (5dMA window 의 most recent 로)
- 후속: GEPUCURRENT (Global EPU) 도 동일 weekend bias 검토 필요

---

## 13. Pure Surface Panic 인식 — Path 3g (2026-05-16 신규, Phase 12.9.2)

### 13.1 발견 배경

COVID transition sample (2020-02-21, 02-24, 02-27, 03-09) 자동 생성 후 시스템이 모두 calm 분류 → 시스템 design 한계 발견:
- Surface 계산이 **LLM regime_view (bear_transition + crisis)** 에 의존
- VIX 39 panic, SPY -12% crash 직접 반영 안 됨
- Auto-created sample (LLM 분석 없음) → surface = 50 default 고정

### 13.2 두 가지 패치

**A. compute_surface_strength (fragility_index.py)**:
- forecast.^VIX.current 없을 때 → **coincident_layer.key_signals 텍스트에서 VIX 직접 추출 fallback**
- Backward compatible (forecast 있는 sample 결과 동일)

**B. detect_crisis_phase — Path 3g 신규**:
```python
# 3g) Pure Surface Panic
if surf < 20:
    return {"phase": "pre_crisis",
            "interpretation": "Pure surface panic — VIX/SPY 극단 stress, latent 미반영"}
```

**C. Pre_crisis fragility synthesis — Pure panic boost**:
```python
if surf < 20 and lat < 30:  # latent 누적 case 는 기존 path 들에서 처리
    base += (25 - surf) * 1.5  # surf 15 → +15, surf 5 → +30
```
→ Historical sample 들은 대부분 lat>30 이므로 영향 없음.

### 13.3 검증 결과 — COVID transition

| Sample | Surface | Latent | Phase | Fragility | 평가 |
|---|---|---|---|---|---|
| 2020-02-19 | 48.6 | 20.1 | calm | 10.1 | D-3 정상 (시그널 없음) |
| 2020-02-21 | 37.1 | 15.5 | calm | 7.8 | D-1 mild |
| **2020-02-24** | **15.0** | 16.3 | **pre_crisis** ✓ | **39.6** | Path 3g 발동 |
| **2020-02-27** | **15.0** | 18.6 | **pre_crisis** ✓ | **42.0** | panic 인식 |
| **2020-03-09** | **15.0** | 39.1 | **pre_crisis** ✓ | **48.6** | (lat>30 → boost 없음) |
| 2020-03-16 | 31.4 | 35.2 | active_crisis | 90.3 | 기존 유지 |

### 13.4 Historical 영향

기존 60 sessions 결과 모두 동일 — Path 3g 의 lat<30 조건이 보호:
- 2008-09-15 surf 2.4 lat 54.4 → lat>30 → boost 없음 (active_crisis path 우선)
- 2018-12-19 surf 4.7 lat 43.2 → lat>30 → boost 없음 (fragility 59.9 유지)
- 모든 active_crisis 19건 변화 없음

### 13.5 Auto Sample Creator (create_sample_from_snapshot.py)

LLM 분석 없이 historical sample 자동 생성:
- snapshot history 에서 모든 indicator 추출 (SPY/VIX/UST/FX/USEPU/SAHM 등 15+)
- USEPU 하이브리드 보정 자동 적용
- forecast 영역에 실제 시장 데이터 채움 (SPY 5d realized %, VIX current)
- regime_view = baseline default (LLM 분석 없으므로) — Surface 자동 계산이 이를 보완

```powershell
python create_sample_from_snapshot.py --date YYYY-MM-DD \
    --news "이벤트 설명" \
    --catalyst-importance 4-5
```

### 13.6 후속 검증 권고

추가 catalyst-driven pre-crisis sample 작성 후 Path 3g 추가 검증:
- 2007-12 (Bear Stearns 헤지펀드 붕괴)
- 2008-03-15 (Bear Stearns 인수)
- 2022-01-25 (Russia 침공 1개월 전)
- 2022-02-22 (Russia 침공 2일 전) → ✓ 2026-05-16 검증됨 (pre_crisis 39.5)

---

## 14. Volatility Surface + Cross-Asset 5 Vector (Phase 12.10/12.10.1, 2026-05-16 신규)

### 14.1 발견 배경 — Backtest 시스템 약점

Backtest report (60 sessions) + Path 3g 검증 (4 case) 결과 발견된 한계:
- SVB-type 위기 (2023-03-08): VIX 19 정상, surface > 20 → Path 3g 미발동 → calm 분류 ❌
- Carry unwind (2024-08-01 D-1): VIX 18, SPY +0.33% → 사전 신호 없음 ❌
- 시스템이 **VIX/SPY 시장 데이터 외 채권 vol, vol of vol, safe haven flow, oil 등 추가 차원 부재**

### 14.2 추가된 5 Indicator + Latent Vector

Indicator evaluation matrix (14 indicator 6 차원 평가) 결과 Top 5 채택:

| Indicator | Source | Vector | Weight | 별점 |
|---|---|---|---|---|
| **^MOVE** (ICE BofAML Treasury Vol) | yfinance | `bond_vol_stress` | 0.10 | ★★★★★ SVB-type leading |
| **^VVIX** (VIX of VIX) | yfinance | `vol_of_vol_spike` | 0.05 | ★★★★ VIX panic 임박 |
| **GLD** (Gold ETF) | yfinance | `safe_haven_demand` | 0.04 | ★★★★ Risk-off flow |
| **USO** (Oil ETF) | yfinance | `oil_shock` | 0.03 | ★★★★ Geopolitical proxy |
| **EMB** (EM Bond ETF) | yfinance | `em_credit_stress` | 0.02 | ★★★★ Global EM credit |

상세 분석: `D:\코워크\research\indicator_evaluation_matrix_v3.2.md`

### 14.3 Latent Vector Score 공식

```python
# MOVE — bond vol (정상 80, SVB 직전 140+)
move_score = max(0, min(1, (move - 80) / 100))

# VVIX — vol of vol (정상 80, panic 임박 110+)
vvix_score = max(0, min(1, (vvix - 80) / 60))

# GLD 5d% — safe haven (위기 시 +3% 이상)
gold_score = max(0, min(1, (gold_5d - 1) / 5))

# USO 5d% — oil shock (geopolitical, +10%+)
oil_score = max(0, min(1, (oil_5d - 3) / 15))

# EMB 5d% — EM credit stress (음수, < -3%)
emb_score = max(0, min(1, (-emb_5d - 1) / 5))
```

### 14.4 12 Vector 균형 + Weight 정규화 (Phase 12.10.1 ★)

**문제**: Historical sample (2017~2024) 에 새 5 indicator 데이터 부재 → 새 vector 0 값 → 전체 fragility 약화 (baseline shift 10-20pt).

**해결**: **Active vectors only** — leading_layer 텍스트에 indicator 패턴 있을 때만 weight 활성:

```python
vector_patterns = {
    "bond_vol_stress":   r"MOVE\s+\d",
    "vol_of_vol_spike":  r"VVIX\s+\d",
    "safe_haven_demand": r"GLD\s+\d.*5d",
    "oil_shock":         r"(USO|WTI)\s+\d.*5d",
    "em_credit_stress":  r"EMB\s+\d.*5d",
}
active_weights = {k: w for k, w in weights.items()
                  if k not in vector_patterns or re.search(vector_patterns[k], signals_text)}
normalized = {k: w / sum(active_weights.values()) for k, w in active_weights.items()}
```

**효과**:
- Historical sample: 7 vector 정규화 (sum = 1.0) → **이전 baseline 완전 보존**
- 새 sample: 12 vector 활성 (sum = 1.0) → **새 indicator 효과 발휘**
- Backward compatible

### 14.5 검증 결과 (67 sessions, 2026-05-16)

| 항목 | 결과 |
|---|---|
| 4/29 imminent | 82.9 → **84.1 imminent** ✓ 완전 복원 |
| Active crisis 19건 | 91.0 avg 유지 ✓ |
| COVID transition (2/24, 2/27, 3/09) | pre_crisis 유지 ✓ (Path 3g) |
| Russia 2/22 | pre_crisis 39.5 → 39.9 유지 ✓ |
| 모노톤 분리 | 8.2 < 34.4 < 61.0 < 91.0 ✓ |
| Calm-recovery gap | 4.9pt 안전 ✓ |

### 14.6 향후 검증 권고

새 5 indicator 가 sample 에 자동 추출되는지 (regenerate_from_snapshots.py 후 leading_layer 텍스트 확인):
```
- MOVE 79.87 (5d +X.X%) — ★ bond vol (SVB-type leading)
- VVIX 92.94 (5d +X.X%) — VIX panic leading
- GLD 417.29 5d +X.X% — safe haven flow
- USO 148.23 5d +X.X% — oil/geopolitical
- EMB 94.71 5d +X.X% — EM credit
```

향후 NVDA 5/19 등 fresh sample 에서 위 5 줄 모두 등장해야 시스템 정상.

### 14.7 미해결

- ⚠️ Historical sample (4/27~5/15) backfill: 새 5 indicator 의 120 day yfinance history 있으므로 가능 — regenerate 재실행 필요
- ⚠️ ^MOVE / ^VVIX yfinance 안정성 모니터링
- 후속: KBE, JNK, JGB 등 차순위 indicator 추가 검토


---

## Section 15 — 정직성 Commitment (2026-05-17 Phase A/B/C audit 후 도입)

### 15.1 사용자 commitment

> "솔직하고 자신을 속이지 않는 진짜 측정 도구가 되도록 합시다"

### 15.2 ★ 시스템 진짜 정의 — 'Stress Visualization Tool'

**Phase C1 결과 (2026-05-17)**:
- 현재 weights 의 historical T+30 SPY drawdown R² = **-7.55** (★ 음수)
- Hold-out R² = -0.41
- → 시스템 **forward-looking predictive 능력 거의 없음** 정량 입증

**시스템 capability**:
- ✓ 현재 시점 12 vector 다차원 stress 시각화
- ✓ 상대 ranking (A vs B)
- ✓ Trajectory 변화 (시간 경과)
- ✓ Cascade 시나리오 정성 해석
- ✗ T+30 SPY drawdown 정량 예측 (능력 없음)
- ✗ 위기 발생 binary classification (self-tautology)
- ✗ 외부 surprise (COVID-type, 본질적 한계)

### 15.3 ★ 사용자 commitment 정확한 정의 (2026-05-17 update)

**원문**: "로직이 틀렸는데, 그 로직을 맞는 것처럼 보이게 하려고 ★ 객관적 수치를 마음대로 변형시키지 말라"

#### ★ 3 카테고리 — 변경 자유도 정의

| 카테고리 | 정의 | 변경 자유도 |
|---------|------|------------|
| **A. 객관 시장 데이터** | SPY/NDX close, VIX, FRED (USEPU, USDKRW, USDJPY, Baa-Aaa), yfinance (MOVE, VVIX, GLD, USO, EMB), KRE, HYG | ★★★ **절대 변형 금지** |
| **B. LLM 분석가 판단** | regime_view (bull/sideways/bear/crisis 확률), cascade scenario, strength score, narrative tone | 결과-driven 조정 자제 (회색) |
| **C. 시스템 logic** | Vector weights, phase formulas, thresholds, cascade-density mapping, path logic (3f, 3g), normalization scheme | ★★★ **자유롭게 진화 권장** |

#### ❌ 절대 금지 — 객관 수치 변형 (Category A)

1. ❌ SPY close 사후 결과 맞추려 수정 (예: 5/13 742.31 → 730)
2. ❌ FRED USEPU 변형 (예: 5/13 raw 223 → 300)
3. ❌ VIX, USDKRW, MOVE 등 yfinance/FRED 데이터 변형
4. ❌ NDX 시트 데이터 사후 조정
5. ❌ 사후 SPY drawdown (T+30) 변형해서 시스템 정확도 인위적 향상

→ ★ 이건 "현실 왜곡". 사용자 commitment 의 가장 엄격한 line.

#### ⚠️ 회색 — LLM 판단값 결과-driven 조정 자제 (Category B)

이전 사례 (★ 인정):
- 2026-05-12~14 sample 의 regime_view 를 "trajectory 매끄럽게" motivation 으로 조정
- 5/13: bull 0.60 → 0.30, cascade broad_recovery → transient_caution
- 5/14: bull 0.65 → 0.30, 동일

해석:
- LLM 판단 자체의 internal inconsistency (bull 0.6 + leading_strength 0.3 모순) 수정 측면 ✓
- 다만 motivation 의 일부가 "trajectory matter" → 결과-driven 측면 ⚠

향후:
- LLM 판단값 변경 시 motivation 명시 + ledger 기록
- 모순 발견 시 ✓, "결과 매끄럽게" ✗

#### ✅ 권장 — 시스템 logic 진화 (Category C)

★ 모든 logic 변경 자유. Backfit 같은 logic 도 OK.

1. ✓ **새 indicator 추가** (KBE, JNK, RVX, JGB yield 등 새 정보원)
2. ✓ **Statistical refit** (regression, ML, Bayesian)
3. ✓ **Phase formula 데이터 기반 재추정** (ROC-optimal, statistical fit)
4. ✓ **Path logic 변경/추가** (Path 3g 같은 — sample 보고 추가도 OK)
5. ✓ **Vector weights 조정** (LOOCV, sample 기반 fit)
6. ✓ **Threshold tuning** (alert_min, cascade-density 등)
7. ✓ **Cascade scenario 정의 변경**
8. ✓ **Normalization scheme** (Active weight 정규화 같은)
9. ✓ **새 version label** (Phase 12.11, 12.12, ...)

★ Why 자유?
- Logic 은 우리가 만든 것 — 진화 가능
- 객관 데이터 (시장) 와 다름
- Logic 개선 시 ✓ 새 sample 부터 적용 (기존 prediction lock 유지)

#### Critical Distinction — 같은 결과 보고 한 행위라도

| 행위 | 카테고리 | 평가 |
|------|---------|------|
| "5/13 결과가 안 맞으니 5/13 SPY close 를 730 으로 수정" | A | ❌ ★ 절대 금지 |
| "5/13 결과가 안 맞으니 5/13 regime_view 의 bull 을 0.30 으로 수정" | B | ⚠️ 회색 (motivation 보고) |
| "5/13 결과가 안 맞으니 vector weights 를 새로 fit + 5/14 부터 적용" | C | ✅ 권장 |
| "Active 무조건 85+ 강제 formula 가 부정확 → ROC-optimal 로 재추정 + 5/18 부터 적용" | C | ✅ 권장 |
| "COVID 잡으려 Path 3g 추가 + Lock 된 COVID prediction 은 변경 안 함" | C | ✅ OK |

### 15.4 Version-Based Logic Evolution Policy (★ 5/18 부터 운영)

#### Rule 1: Prediction Lock (Strict)

- 각 prediction 의 fragility 값 ★ commit 후 변경 금지
- 그 시점 system version 으로 만든 결과 영구 보존
- 사후 평가는 그 prediction 의 정확도 평가 (lock 된 값으로)

#### Rule 2: Logic Evolution (Open + Documented)

★ Logic 변경 ✓ 허용 + 권장. 단, 모든 변경에:
- **Motivation** 명시 (왜 변경?)
- **변경 전/후 비교** (sample 영향 정량)
- **새 version label** (Phase 12.11, 12.12, ...)
- **Hold-out 사전 평가** 권장 (도입 전 성능 측정)
- **변경 ledger** 에 기록 (`logic_change_log.md`)

#### Rule 3: Sliding Window Evaluation

```
Logic v1 으로 만든 prediction A (5/18~5/30) → A 의 사후 결과 평가 (v1 기준)
Logic v2 로 만든 prediction B (6/01~6/30) → B 의 사후 결과 평가 (v2 기준)
★ 두 version 의 forward accuracy 비교 → 진화 가치 측정
```

#### Rule 4: Backfit 차단 (가장 critical)

- ❌ 새 logic 으로 과거 lock 된 prediction 재계산 후 "성능 향상" 주장
- ❌ Hold-out 결과 보고 그 sample 분류 변경
- ❌ "5/18 결과 안 맞으니 5/18 logic 수정" — 5/18 lock 후 5/19 logic 만 변경 가능

#### Protocol

- 매 거래일 sample 작성 → fragility 계산 → Hash commit
- Logic version + commit hash 기록 → 사후 재현 가능
- Ledger: `D:\코워크\research\preregistered_predictions.jsonl` (append-only)
- 변경 ledger: `D:\코워크\research\logic_change_log.md`

#### Evaluation

- T+30 거래일 시점에 사후 결과 비교
- TP / TN / FP / FN + correlation metrics
- 6개월 후 (2026-11) 진짜 forward-looking 정확도 측정
- ★ Version 별 정확도 분리 평가 → 진화 효과 측정

### 15.5 Dashboard / Messaging 원칙

**Dashboard 에 항상 표시**:
- "★ 시스템 진짜 capability — 정직한 한계 명시" 섹션
- R² 음수 결과 명시
- "Risk Predictor 아님 — Stress Visualization Tool" 명시

**금지 messaging**:
- "X% 정합" 같은 self-validation
- "fragility 80 = 위기 임박" 같은 정량 예측 주장
- "Active_crisis 분류 = drawdown 임박" 같은 추론

**허용 messaging**:
- 시스템이 측정한 vector 의 정성 narrative
- Trajectory shape 의 의미 (4/29 → 5/15 같은)
- 자의성/한계의 정직한 명시

### 15.6 미래 시스템 재설계 단계

**Phase C (statistical refit)** — 진정한 측정 도구로 진화:
- C1: Vector weights regression ✓ (2026-05-17 1차 시도)
- C2: Phase formula constants ROC fit (미실행)
- C3: Cascade density historical 추정 (미실행)

**Phase D (운영 정책)** — 정직성 보장:
- D1: Logic change log 의무화
- D2: Hold-out validation 매월 자동 재평가
- D3: Pre-registered prediction 누적 evaluation

**Phase E (정직 messaging)** — 사용자 commitment 의 외부 표현:
- Dashboard / Handoff / Reports 의 system 정의 일관 명시 ✓ (2026-05-17)
- 자기-검증 numbers 폐기 ✓

### 15.7 다음 step (1년 roadmap)

- **2026-05-18~22**: 첫 pre-registered predictions (5 entries)
- **2026-05-19 NVDA**: 결정적 prediction lock
- **2026-06-18**: 5/18 sample 의 첫 T+30 평가
- **2026-08-31**: ~80 pre-registered predictions 누적
- **2026-11-17**: 6개월 milestone — 진짜 정확도 측정
- **2027-05-17**: 1년 milestone — 시스템 capability 결정적 평가

### 15.8 운영 commitment 명문화 (2026-05-17)

이 문서 (operating_policy.md) Section 15 가 시스템 정직성의 binding commitment.
모든 후속 변경은 이 commitment 와 일관해야 함.
