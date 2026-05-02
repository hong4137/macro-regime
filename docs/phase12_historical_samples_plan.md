# Phase 12 — Historical Samples 확장 Plan

**생성**: 2026-05-02
**목적**: Fragility Index 검증을 위해 기존 28 sessions 외 **추가 18개+ historical sample** 을 LLM 분석 후 phase11_fusion_*.json 형식으로 저장
**근거 (사용자 요청)**:
> "역사적 큰 위기 뿐만 아니라 더 디테일하게 측정할만한 지점들을 조사해서 추가해보세요. 예를 들어, 코로나 이후 양적완화가 이뤄지면서 주가가 터닝 어라운드 했던 시기, 금리 인상기의 변화 등"

---

## 0. 검증 목표

Fragility Index 가 다음 4가지 시점을 **명확히 구분** 해야 한다:

| 시점 유형 | 예상 Fragility | Phase | 검증 포인트 |
|---|---|---|---|
| **양적완화 turning around** (QE turning) | 60→20 (낙폭) | active_crisis → recovery → calm | 회복 phase 가 잘 잡히는가? Latent 잔존이 점진적 감소? |
| **금리 인상 첫날** (rate hike) | 30→55 (점프) | calm → pre_crisis | Surface 강하지만 latent 점진 누적이 잡히는가? |
| **금리 인상 충격** (Powell pivot) | 70+ | pre_crisis → active_crisis | Sustainability 무너짐, decoupling 큰가? |
| **Geopolitical 충격** (러시아, 이란) | 60→80 | pre_crisis → active_crisis | Latent geopolitical_tail 가 dominant 한가? |

---

## 1. Major Crises 보강 (8개)

이미 일부 분석된 위기 외 **추가 측정해야 할 critical day**:

| # | 날짜 | 사건 | 예상 fragility | 분석 의도 |
|---|---|---|---|---|
| H1 | 2008-09-15 | Lehman 파산 D-day | 90+ active | 위기 정점 ground truth |
| H2 | 2008-10-10 | TARP 통과 직전 panic | 95+ active | 최악의 imminent |
| H3 | 2009-03-09 | S&P 666 bottom | 88 active→recovery | active_crisis recovery transition |
| H4 | 2000-04-14 | 닷컴 첫 큰 폭락일 | 65 pre→active | Bubble burst 첫 시그널 |
| H5 | 2002-10-09 | 닷컴 bottom | 85 active | 장기 active_crisis |
| H6 | 2010-05-06 | Flash Crash | 55 active | 단발성 acute_shock 검증 |
| H7 | 2011-08-05 | US 신용등급 강등 | 70 active | Sovereign risk Latent |
| H8 | 2018-02-05 | XIV 폭락 (vol shock) | 55 acute | acute_shock V-shape 검증 |

**ground truth 패턴**:
- 닷컴 bubble: 2000.04 (pre→active 전환점) vs 2002.10 (active 정점) **2개 sample** 필수
- 2008 위기: 2008.09 (Lehman) vs 2009.03 (bottom) **2개 sample** 으로 active 지속 검증

---

## 2. QE Turning Points (3개) — ★ 사용자 요청

가장 중요한 검증: **위기에서 정상으로 회복하는 transition**

| # | 날짜 | 사건 | 예상 fragility 추적 | 분석 의도 |
|---|---|---|---|---|
| Q1 | 2009-03-23 | QE1 발표 (Geithner Plan) | 80→55 | active_crisis → recovery transition |
| Q2 | 2012-09-13 | QE3 무제한 발표 | 45→25 | recovery → calm transition |
| Q3 | 2020-03-23 | Unlimited QE (코로나) | 88→60 (단 5d) | active_crisis → recovery 즉각 전환 |

**검증 핵심**:
- Phase detection 이 **연속적으로** 옮겨가는가? (active → recovery → calm)
- Surface 회복 (S&P 반등) vs Latent 잔존 (credit spread, USDJPY) — **Decoupling** 어떻게 변화?
- Sustainability score 증가 추세가 잡히는가?

---

## 3. Rate Hike Cycle Markers (5개) — ★ 사용자 요청

금리 인상기는 **Surface 강세 + Latent 누적** 의 전형 — 사용자 framing 의 핵심

| # | 날짜 | 사건 | 예상 fragility | 분석 의도 |
|---|---|---|---|---|
| R1 | 2015-12-16 | Fed 첫 인상 (제로금리 종료) | 25→35 | calm → mild pre-crisis transition |
| R2 | 2018-12-19 | Powell 75bp 후 시장 충격 | 55 | rate hike 충격 = pre-crisis 정점 |
| R3 | 2022-03-16 | 첫 25bp 인상 (인플레 대응) | 40→50 | pre-crisis 시작 |
| R4 | 2022-06-15 | 75bp 점프 인상 | 65 | pre→active transition |
| R5 | 2024-09-18 | Fed 첫 인하 (금리 cycle 종료) | 30→20 | active→recovery→calm |

**검증 핵심**:
- 금리 cycle 의 phase transition (calm → pre_crisis → active_crisis → recovery → calm) 이 모두 잡히는가?
- 2018-12-19: Powell pivot 직전 — Surface 70 + Latent 50 + Decoupling 큰가? (사용자 framing 검증)

---

## 4. Geopolitical Tail (2개) — ★ 사용자 요청

| # | 날짜 | 사건 | 예상 fragility | 분석 의도 |
|---|---|---|---|---|
| G1 | 2022-02-24 | 러시아 우크라이나 침공 | 60→78 | Latent geopolitical_tail dominant |
| G2 | 2024-04-13 | 이란-이스라엘 첫 직접 공습 | 50→65 | 사용자 framing 의 정확한 case |

**검증 핵심**:
- Latent vectors 중 **geopolitical_tail** 이 dominant 인가? (CFTC oil, USDJPY, 중동 risk)
- Tipping points 에 "Iran 군사 충돌 escalation" 이 자동 detection 되는가?

---

## 5. 데이터 수집 방식 (LLM 분석)

각 sample 은 다음 구조의 phase11_fusion_YYYYMMDD.json 으로 저장:

```json
{
  "as_of_date": "2009-03-23",
  "liq_input": {
    "cluster_layer": {"signals_text": "VIX 28, USDJPY 96, USEPU 250, ..."},
    "catalyst_layer": {"verdict": "..."}
  },
  "phase11_fusion": {
    "alert_level": "ELEVATED",
    "raw_alert": "RED",
    "bear_plus_crisis": 0.55,
    "regime_view": {"bull_continuation": 0.15, "sideways": 0.20, "bear_transition": 0.30, "crisis": 0.35},
    "cascade": {"scenario": "broad_recovery", "interpretation": "...", "action": "...", "confidence": "high"},
    "flow_regime": "leaning_bullish",
    "sentiment_regime": "neutral",
    "crisis_cluster_alert": false
  },
  "flow_result": {...},
  "sentiment_result": {...},
  "forecast_result": {...}
}
```

**LLM 분석 prompt template**:
> "다음 날짜의 매크로 환경을 분석합니다: {DATE}, {EVENT}.
> 1. VIX, USDJPY, S&P, USEPU, oil 의 당일 레벨
> 2. CFTC positioning, ETF flow, AAII sentiment
> 3. Fed/BoJ 정책 stance, geopolitical context
> 4. 4-Layer 분석: Stress/Flow/Sentiment/Forecast → Cascade scenario 분류
> Output: phase11_fusion_*.json 형식"

---

## 6. 실행 우선순위

### Tier A (즉시 — 3개)
1. **2020-03-23** (QE 무제한): active→recovery 검증
2. **2018-12-19** (Powell pivot): pre→active 검증
3. **2022-02-24** (우크라이나): geopolitical_tail 검증

### Tier B (1주 내 — 5개)
4. 2009-03-09 (S&P 666 bottom)
5. 2009-03-23 (QE1)
6. 2015-12-16 (Fed 첫 인상)
7. 2024-09-18 (Fed 첫 인하)
8. 2024-04-13 (이란 공습)

### Tier C (2주 내 — 10개)
나머지 H1, H2, H4-H7, Q2, R2-R4, G2

---

## 7. 검증 후 dashboard 통합

각 sample 분석 후:
1. `python fragility_index.py --date YYYY-MM-DD` 자동 실행
2. 결과를 `analyses/fragility_validation_history.md` 에 누적
3. dashboard v5 의 history table 에 fragility column 추가

---

## 8. 성공 기준

- 전체 46개 sample 중 **80%+ 가 예상 phase 와 일치**
- QE turning point 3개 모두 phase transition (active→recovery→calm) 명확
- Rate hike cycle 5개에서 **점진적 fragility 증가 추세** 관찰
- 사용자 framing **"표면 강세 + 잠재 위험 + 수급 cushion"** 시점 (R3 2022-03-16, 2026-04-29 등) 모두 pre_crisis 로 분류
