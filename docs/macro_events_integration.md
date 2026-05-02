# 매크로 이벤트 — 현재 프로젝트 통합 방안 탐구

**대상 시스템**: Phase 12 Latent Fragility Detection
**작성일**: 2026-05-03
**목적**: 매크로 이벤트 캘린더 가치를 현재 시스템에 어떻게 더할지 설계

---

## 0. 출발점 — 현재 시스템에서 catalyst 가 어떻게 사용되나

### 0.1 현재 구조

`phase11_fusion_*.json` 의 `liq_input.catalyst_layer`:
```json
{
  "catalyst_layer": {
    "verdict": "Density 0.333 — Russia 침공 today (importance 5 surprise)",
    "key_catalysts": [
      "2022-02-24 러시아 우크라이나 침공 (today, importance 5 surprise)"
    ]
  }
}
```

### 0.2 현재 활용

**Fragility Index 의 latent vector 6개 중 단 1개**:
```python
# fragility_index.py — compute_latent_vectors()
geo_keywords = ["imminent", "Iran", "war", "tariff", "escalation",
                "Russia", "Ukraine", "침공", "전쟁", ...]
geo_hits = sum(1 for k in geo_keywords if k in catalyst_text)
if geo_hits >= 3: geo_score = 0.85
elif geo_hits >= 2: geo_score = 0.65
elif geo_hits >= 1: geo_score = 0.45
```

**즉 catalyst 는 단순 문자열 keyword matching 으로만 활용 중**. 정량적인 importance/density/timing 정보는 사실상 미활용.

### 0.3 미활용된 정보

1. **Importance 등급** (1-5) — verdict 에 textual 로만 존재, 점수화 안됨
2. **Density** (다음 7일 catalyst 개수) — verdict 에 한 번 언급되지만 fragility 산식 미반영
3. **Imminent_high** flag — true/false 만 있고 활용 미미
4. **Timing** (T+0/T+1/T+5/T+14) — 시간 거리별 가중 없음
5. **Category** (정책 vs 거시 vs 지정학 vs 실적) — 모두 동일 취급
6. **Surprise factor** — 예상된 vs 깜짝 이벤트 구분 없음

→ **catalyst layer 의 정보 손실률이 매우 높음**. 통합 가능성이 큼.

---

## 1. 5가지 통합 path

각 path 는 **독립적으로 적용 가능** + 누적 효과.

### Path P1 — Catalyst Density Score (단순)

**가설**: 다음 7일 안에 importance ≥ 4 catalyst 가 많을수록 시장 변동성 ↑ → fragility ↑.

**구현**:
```python
def compute_catalyst_density(as_of_date: str) -> dict:
    """다음 7일/14일 catalyst 가중 점수."""
    upcoming = fetch_catalyst_calendar(as_of_date, days_ahead=14)

    # Time decay weight: T+1 = 1.0, T+7 = 0.5, T+14 = 0.25
    score_7d = sum(
        e["importance"] * max(0.5, 1.0 - days_away/14)
        for e in upcoming
        if (days_away := days_between(e["date"], as_of_date)) <= 7
    )

    # density per day
    n_high_7d = len([e for e in upcoming if e["importance"] >= 4 and days_between(e["date"], as_of_date) <= 7])

    return {
        "score": min(score_7d / 30, 1.0),  # 0-1
        "n_high_events_7d": n_high_7d,
        "imminent_24h_high": any(e["importance"] >= 4 and days_between(e["date"], as_of_date) <= 1 for e in upcoming),
    }
```

**Fragility 통합**:
```python
# compute_latent_vectors() 에 vector 7 추가
vectors["catalyst_density"] = catalyst["score"]
weights["catalyst_density"] = 0.10  # 기존 6개 vector 비중 약간 축소
```

**작업량**: 1일 (catalyst_calendar.py + density score + fragility vector 추가)
**효과**: FOMC week, NFP day, 빅테크 실적 같은 high-density 시점이 자동으로 fragility 가산
**ROI**: ★★★★ (매우 직접적 가치)

---

### Path P2 — Imminent_High Flag → Surface Strength 조정

**가설**: 다음 24h 안에 importance 5 이벤트 (FOMC, NFP, CPI, NVDA earnings 같은) 가 있으면 시장은 **결과 대기 모드**. 이 시점의 표면 강세는 환상에 가까움 — 이벤트 결과에 따라 V자/N자 반등이 흔함.

**구현**:
```python
def adjust_surface_for_imminent(surface: dict, catalyst: dict) -> dict:
    """다음 24h 안에 high-importance 이벤트 있으면 surface confidence 감점."""
    if catalyst.get("imminent_24h_high"):
        # 표면 강도를 30% discount
        adjusted = surface["score"] * 0.7
        return {
            **surface,
            "score": adjusted,
            "imminent_discount": True,
            "note": f"이벤트 대기 surface — actual impact undetermined",
        }
    return surface
```

**효과**: FOMC day 같은 시점이 "표면 강세이지만 위험" 으로 자동 인식
**ROI**: ★★★ (특정 시점만 영향, 평시엔 무영향)

---

### Path P3 — Category-Specific Fragility 가산

**가설**: 카테고리별로 fragility 에 다른 영향. 예:
- **정책 이벤트** (FOMC, BoJ): policy_uncertainty vector 직접 가산
- **거시 지표** (NFP, CPI): credit/em_fx vector 가산
- **빅테크 실적**: sector_dispersion vector 가산
- **지정학 뉴스**: geopolitical_tail vector 직접 가산
- **Tech 컨퍼런스** (GTC, WWDC): sector_dispersion + sentiment 가산

**구현**:
```python
CATEGORY_VECTOR_MAP = {
    "fed_fomc":       {"policy_uncertainty": 0.3},
    "boj_meeting":    {"carry_leverage": 0.4},
    "nfp":            {"credit_stress": 0.2},
    "cpi":            {"policy_uncertainty": 0.25},
    "earnings_mag7":  {"sector_dispersion": 0.3},
    "tech_conf":      {"sector_dispersion": 0.2},
    "geopolitical":   {"geopolitical_tail": 0.5},
    "trade_war":      {"em_fx_stress": 0.3, "geopolitical_tail": 0.3},
}

def category_specific_boost(catalyst_events: list) -> dict[str, float]:
    """카테고리별 vector boost 합산."""
    boosts = defaultdict(float)
    for ev in catalyst_events:
        if days_until(ev["date"]) <= 3:  # 3일 내 이벤트만
            mapping = CATEGORY_VECTOR_MAP.get(ev["category"], {})
            for vec, w in mapping.items():
                boosts[vec] += w * (ev["importance"] / 5)
    return dict(boosts)
```

**효과**: catalyst 의 정성적 정보가 정량 vector 에 reflected
**ROI**: ★★★★ (가장 정교한 통합)

---

### Path P4 — Pre-Event Positioning Helper

**가설**: 시스템이 catalyst 를 안다면 **pre-event hedge 권장** 가능. 예:
- D-1 FOMC: surface 가 강해도 "이벤트 hedge 5%" 권장
- D-1 NVDA earnings: tech beta hedge
- 휴일 (Triple Witching D-day): vol hedge

**구현 (cascade_sizing.py 보강)**:
```python
def pre_event_adjustment(sizing: dict, catalyst: dict, as_of_date: str) -> dict:
    """Imminent_24h 시 sizing 보정."""
    upcoming_24h = [e for e in catalyst.get("events", [])
                    if days_until(e["date"], as_of_date) <= 1]

    if not upcoming_24h:
        return sizing

    high_imp = [e for e in upcoming_24h if e["importance"] >= 4]
    if not high_imp:
        return sizing

    # Risk -10%, hedge +5%, cash +5% (24h 보정)
    return {
        "risk":   max(0, sizing["risk"] - 0.10),
        "hedge":  sizing["hedge"] + 0.05,
        "cash":   sizing["cash"] + 0.05,
        "pre_event_note": f"D-1: {high_imp[0]['title']} (24h hedge)",
    }
```

**효과**: position_sizing 이 catalyst-aware 가 됨
**ROI**: ★★★ (실무 trade 에 직접 도움)

---

### Path P5 — Post-Event Regime Shift Detection

**가설**: 큰 이벤트 후 시스템이 **regime shift 자동 감지**. 예:
- FOMC 75bp 점프 후 → cascade 자동 escalating_stress 격상
- NVDA 큰 surprise 후 → surface_strength 재평가
- 지정학 escalation 후 → geopolitical_tail 자동 0.85+

**구현**:
```python
def post_event_regime_check(as_of_date: str, lookback_hours: int = 48) -> dict:
    """직전 48h 내 발생한 high-impact 이벤트 + 시장 reaction 분석."""
    target = datetime.strptime(as_of_date, "%Y-%m-%d")
    recent = [e for e in past_catalysts
              if 0 < hours_since(e["actual_time"], target) <= lookback_hours
              and e.get("importance", 0) >= 4]

    if not recent:
        return {"regime_shift": False}

    # 시장 reaction 측정
    spy_change_2d = fetch_spy_return(as_of_date, days=2)
    vix_change_2d = fetch_vix_change(as_of_date, days=2)

    if abs(spy_change_2d) > 3 or vix_change_2d > 5:
        return {
            "regime_shift": True,
            "trigger_event": recent[0]["title"],
            "spy_2d": spy_change_2d,
            "vix_2d": vix_change_2d,
            "recommendation": "Cascade 재평가 + 새 sample LLM 분석 권장",
        }
```

**효과**: catalyst 결과 → 시스템 자동 재평가
**ROI**: ★★★★ (데이터 기반 system response)

---

## 2. 통합 priority matrix

| Path | 작업량 | Fragility 가치 | Trade-actionable | 권장 priority |
|---|---|---|---|---|
| **P1** Density score | 1 일 | ★★★★ | indirect | **1순위** |
| **P3** Category boost | 3 일 | ★★★★ | indirect | **2순위** |
| **P5** Regime shift | 5 일 | ★★★★ | direct | **3순위** |
| **P4** Pre-event sizing | 3 일 | ★★ | direct | 4순위 |
| **P2** Imminent surface | 1 일 | ★★ | indirect | 5순위 |

**총 작업량**: 13 일 (모두 적용 시)
**최소 ROI**: P1 + P3 만 = 4일에 fragility 정확도 큰 향상

---

## 3. P1 + P3 통합 — 구체적 구현 plan (4일)

### Day 1 — catalyst_calendar.py (D:\코워크\scripts)

`research/macro_events_catalog.md` 의 Section 5 코드 그대로 가져옴 + 한 가지 추가:

```python
def fetch_calendar_for_date(as_of_date: str) -> list[dict]:
    """as_of_date 기준 다음 30일 events."""
    cache_path = Path(r"D:\코워크\catalysts\upcoming.json")
    if cache_path.exists() and cache_age_hours(cache_path) < 24:
        return json.load(open(cache_path))
    return fetch_all(days_ahead=30)
```

### Day 2 — fragility_index.py 보강

```python
# compute_latent_vectors() 에 vector 7 추가
def compute_catalyst_density_vector(as_of_date: str) -> tuple[float, list]:
    """다음 7일 catalyst density (0-1) + events."""
    events = fetch_calendar_for_date(as_of_date)
    upcoming_7d = [e for e in events if 0 <= days_until(e["date"], as_of_date) <= 7]

    if not upcoming_7d:
        return 0.0, []

    score = sum(
        e["importance"] / 5 * max(0.4, 1 - days_until(e["date"], as_of_date)/10)
        for e in upcoming_7d
    ) / 5  # normalize
    return min(score, 1.0), upcoming_7d


# compute_latent_vectors() 안에 추가
catalyst_score, catalyst_events = compute_catalyst_density_vector(as_of_date)
vectors["catalyst_density"] = catalyst_score
weights = {
    "policy_uncertainty": 0.18,
    "carry_leverage":     0.18,
    "em_fx_stress":       0.13,
    "sector_dispersion":  0.10,
    "credit_stress":      0.18,
    "geopolitical_tail":  0.15,
    "catalyst_density":   0.08,  # ★ 신규
}
```

### Day 3 — Category boost (P3)

```python
# fragility_index.py — compute_latent_vectors() 끝에 추가
def apply_category_boost(vectors, weights, catalyst_events, as_of_date):
    boosts = defaultdict(float)
    for ev in catalyst_events:
        days = days_until(ev["date"], as_of_date)
        if days > 3: continue
        imp_factor = ev["importance"] / 5
        time_factor = max(0.3, 1 - days/3)
        for vec, w in CATEGORY_VECTOR_MAP.get(ev["category"], {}).items():
            boosts[vec] += w * imp_factor * time_factor
    # Apply boosts (max 0.5 increase to any vector)
    for vec, b in boosts.items():
        if vec in vectors:
            vectors[vec] = min(1.0, vectors[vec] + min(b, 0.5))
    return vectors
```

### Day 4 — Dashboard 통합

`daily_dashboard.py` 의 fragility card 에 다음 추가:

```html
<div class="upcoming-events">
  <div class="ev-label">📅 다음 7일 주요 catalysts</div>
  <ul>
    <li>D-1 (5/12): Fed FOMC + Powell ★★★★★</li>
    <li>D-2 (5/13): CPI - April ★★★★★</li>
    <li>D-3 (5/14): Google I/O ★★★★★</li>
    <li>D-7 (5/19): NVDA Earnings ★★★★★</li>
  </ul>
</div>
```

또 situation_summary 에 추가:
```python
if catalyst.get("imminent_24h_high"):
    story += f" 다음 24h 안에 {catalyst['imminent_event']} 예정 — 결과 대기 변동성 가능."
```

---

## 4. 통합 후 예상 효과 (5/2 sample 시뮬레이션)

**현재 (catalyst 통합 전)**:
- Fragility 73.8 critical pre_crisis
- Top vectors: carry 24% / geo 20% / em_fx 19%

**P1 + P3 통합 후 (예상)**:
- 5/2 시점 다음 7일 catalysts:
  - 5/5 (D+3) NFP — imp 5 → catalyst_density +0.4
  - 5/8 (D+6) BoE — imp 4 → +0.2
  - 5/12 (D+10) FOMC — imp 5 (out of 7d window, 0)
  - 5/13 (D+11) CPI — imp 5 (out of 7d window, 0)

→ catalyst_density vector ≈ 0.10 (낮음, 현재 직접 imminent 가 5/5 NFP 만)
→ Fragility 73.8 → ~75 (마이너 가산)

**더 극적인 case** (FOMC week, 5/11-15):
- 5/11 D-1 FOMC + CPI 동시 → catalyst_density 0.85
- + policy_uncertainty boost +0.3
- → Fragility 73.8 → ~82 critical (현재 ELEVATED 단계 → CRITICAL 단계 자동 격상)

→ **시스템이 catalyst-rich 시점에 더 보수적 자세** 자동 채택.

---

## 5. 시스템 정합성 — 기존 logic 영향

### 5.1 Phase 11 cascade scenario 와의 관계

Cascade scenario (8개) 는 변경 없음. catalyst 는 fragility (Phase 12) 만 보강.

### 5.2 Cascade-aware Position Sizing 영향

P4 (Pre-event sizing) 적용 시 sizing 미세 조정. 적용 안 하면 변경 없음.

### 5.3 Backtesting compatibility

Historical sample 들은 catalyst_layer 가 이미 있음 (verdict + key_catalysts). 새 catalyst_calendar 는 future-only fetch 하므로 historical 검증에는 영향 없음.

→ **현재 phase11_fusion JSON 의 catalyst_layer 를 그대로 활용** 하는 path 가 가장 안전.

```python
# 변형: catalyst_calendar.py 호출 대신 phase11_fusion 의 catalyst_layer 그대로 사용
def compute_catalyst_density_vector(as_of_date: str) -> tuple[float, list]:
    """Existing catalyst_layer 에서 추출."""
    fusion = load_phase11(as_of_date)
    cat_layer = fusion.get("liq_input", {}).get("catalyst_layer", {})
    catalysts_text = cat_layer.get("verdict", "") + " ".join(cat_layer.get("key_catalysts", []))

    # Density: T+0 ~ T+7 mentions 카운트
    import re
    today_count = catalysts_text.count("today")
    imminent_count = len(re.findall(r"T-?\d|T\+\d", catalysts_text))

    # Importance: "importance 5" 빈도
    imp5_count = catalysts_text.count("importance 5")
    imp4_count = catalysts_text.count("importance 4")

    score = min(1.0, (imp5_count * 0.3 + imp4_count * 0.15 + today_count * 0.2) / 2)
    return score, []
```

→ 기존 historical 46개 sample 도 자동 적용 가능 (LLM 분석 시 이미 catalyst_layer 작성됨).

---

## 6. 권장 실행 순서

### Phase 1 (즉시, 1일)
- P1 (Catalyst density) 만 적용
- 기존 catalyst_layer 텍스트에서 importance 추출 (Section 5.3 의 변형)
- 46 sample 재계산 → Phase 12.4 batch validation

### Phase 2 (다음 주, 2-3일)
- P3 (Category boost) 적용
- Catalyst category 분류 logic 추가
- Phase 12.5 batch validation

### Phase 3 (선택, 별도 프로젝트로)
- catalyst_calendar.py 별도 프로젝트로 fork
- Future events 자동 fetch + dashboard 별도 구축
- 본 시스템과 separate

---

## 7. 핵심 메시지

| 통합 path | 한 줄 가치 |
|---|---|
| **P1 Density** | "다음 주 FOMC + CPI 동시 시점이 자동으로 더 위험하게 감지됨" |
| **P3 Category** | "지정학 뉴스가 자동으로 geo_tail vector 가산 — 수동 LLM 분석 불필요" |
| **P5 Regime shift** | "FOMC 후 시장이 -3% 떨어지면 자동으로 새 sample LLM 분석 trigger" |
| **P4 Pre-event** | "FOMC D-1 자동으로 hedge 5% 추가 권장" |
| **P2 Imminent surface** | "FOMC day 의 강세는 환상 — surface 30% discount" |

→ 가장 큰 leverage 는 **P1 + P3** 4일 작업. 이걸 적용하면 시스템이 **catalyst-aware** 가 되고, LLM 수동 분석 부담이 크게 줄어듦.

---

## 8. 다음 결정 포인트

| 결정 | 옵션 |
|---|---|
| **이걸 지금 적용할까?** | A: 즉시 P1 (1일) / B: 별도 프로젝트 분리 / C: 운영 누적 후 결정 |
| **catalyst_calendar 어디에?** | A: 본 시스템 통합 / B: 별도 프로젝트 fork |
| **Fragility weight 재조정?** | A: 신규 vector 추가 (현재 6 → 7) / B: 기존 vector 보강 |
| **LLM 자동 호출도?** | A: catalyst-driven LLM trigger / B: 수동 유지 |

---

## 9. 결론

**현재 시스템은 catalyst 의 정량 정보를 80% 손실 중**. P1 + P3 (4일 작업) 만으로도:

1. ✅ FOMC week, NFP day 등 high-density 시점 자동 가산
2. ✅ 지정학 뉴스 → geo_tail vector 자동 boost
3. ✅ Tech 컨퍼런스 → sector_dispersion 가산
4. ✅ Pre-event 자세 자동 보수화

→ **수동 LLM 분석 빈도 감소** + **fragility 정확도 향상** 동시 달성.

별도 프로젝트의 calendar dashboard (`macro_events_catalog.md`) 와 본 시스템 통합 (`macro_events_integration.md`) 은 **독립적이므로 둘 다 진행 가능**.
