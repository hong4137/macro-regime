# Phase 12 Fragility Index — Batch History 시계열

**생성**: 2026-05-03 00:33  
**Sessions**: 35  
**Date range**: 2009-03-09 ~ 2026-05-01

## 1. Phase 분포 통계

| Phase | n | Avg fragility | Min | Max |
|---|---|---|---|---|
| **active_crisis** | 11 | 89.1 | 87.4 | 94.0 |
| **pre_crisis** | 12 | 46.6 | 35.2 | 57.9 |
| **recovery** | 3 | 26.6 | 19.4 | 32.9 |
| **calm** | 9 | 7.5 | 2.2 | 15.4 |

## 2. Phase 별 Top Latent Vectors (avg %)

### active_crisis

| Vector | Avg contribution |
|---|---|
| credit stress | 30.4% |
| geopolitical tail | 14.9% |
| carry leverage | 10.7% |
| em fx stress | 7.5% |
| sector dispersion | 6.9% |

### pre_crisis

| Vector | Avg contribution |
|---|---|
| credit stress | 28.4% |
| geopolitical tail | 25.3% |
| carry leverage | 18.6% |
| em fx stress | 11.0% |
| sector dispersion | 10.9% |

### recovery

| Vector | Avg contribution |
|---|---|
| credit stress | 25.9% |
| em fx stress | 23.0% |
| carry leverage | 17.5% |
| policy uncertainty | 13.7% |
| sector dispersion | 10.4% |

### calm

| Vector | Avg contribution |
|---|---|
| credit stress | 27.6% |
| sector dispersion | 21.1% |
| carry leverage | 15.2% |
| geopolitical tail | 13.4% |
| policy uncertainty | 0.0% |

## 3. Crisis 직전 14일 fragility trajectory

- Active crisis sessions: **11**
- 14일 이내 prior session 가 있는 case: **5**

### Per-crisis trajectory

**2020-03-23** (crisis fragility 89.3):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-7 | 2020-03-16 | 88.3 | active_crisis |

**2023-03-13** (crisis fragility 89.2):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-4 | 2023-03-09 | 45.0 | pre_crisis |

**2023-03-15** (crisis fragility 89.0):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-2 | 2023-03-13 | 89.2 | active_crisis |
| D-6 | 2023-03-09 | 45.0 | pre_crisis |

**2024-08-02** (crisis fragility 88.5):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-2 | 2024-07-31 | 89.4 | active_crisis |

**2024-08-05** (crisis fragility 87.4):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-3 | 2024-08-02 | 88.5 | active_crisis |
| D-5 | 2024-07-31 | 89.4 | active_crisis |

## 4. Full timeline (모든 sessions)

| Date | Fragility | Level | Phase | Surface | Latent | Decoupling | Sustain. |
|---|---|---|---|---|---|---|---|
| 2009-03-09 | **94.0** | imminent | `active_crisis` | 1.2 | 60.2 | 0.7 | 9.0 |
| 2009-03-23 | **32.9** | mild | `recovery` | 43.6 | 41.1 | 17.9 | 76.7 |
| 2015-12-16 | **35.2** | mild | `pre_crisis` | 40.8 | 31.0 | 12.6 | 70.8 |
| 2017-09-15 | **2.2** | calm | `calm` | 57.4 | 4.5 | 2.6 | 75.4 |
| 2018-12-19 | **56.3** | elevated | `pre_crisis` | 4.7 | 39.7 | 1.9 | 30.0 |
| 2019-04-15 | **8.4** | calm | `calm` | 61.0 | 16.9 | 10.3 | 65.7 |
| 2019-08-05 | **87.7** | imminent | `active_crisis` | 47.0 | 18.0 | 8.5 | 45.0 |
| 2020-02-19 | **10.4** | calm | `calm` | 48.6 | 20.9 | 10.2 | 71.5 |
| 2020-03-16 | **88.3** | imminent | `active_crisis` | 31.4 | 22.0 | 6.9 | 9.0 |
| 2020-03-23 | **89.3** | imminent | `active_crisis` | 0.7 | 28.8 | 0.2 | 22.3 |
| 2021-05-15 | **3.8** | calm | `calm` | 43.9 | 7.5 | 3.3 | 55.5 |
| 2022-02-24 | **45.0** | elevated | `pre_crisis` | 27.3 | 19.5 | 5.3 | 22.3 |
| 2022-06-17 | **88.3** | imminent | `active_crisis` | 43.7 | 21.9 | 9.6 | 30.0 |
| 2022-10-13 | **50.7** | elevated | `pre_crisis` | 2.7 | 36.7 | 1.0 | 55.2 |
| 2023-03-09 | **45.0** | elevated | `pre_crisis` | 23.8 | 26.7 | 6.4 | 22.3 |
| 2023-03-13 | **89.2** | imminent | `active_crisis` | 13.2 | 28.2 | 3.7 | 9.0 |
| 2023-03-15 | **89.0** | imminent | `active_crisis` | 3.0 | 26.8 | 0.8 | 13.2 |
| 2023-03-24 | **45.0** | elevated | `pre_crisis` | 12.3 | 31.2 | 3.8 | 71.9 |
| 2023-04-06 | **4.0** | calm | `calm` | 36.5 | 8.1 | 3.0 | 62.3 |
| 2023-10-09 | **15.4** | calm | `calm` | 25.7 | 30.9 | 7.9 | 71.7 |
| 2023-12-13 | **3.0** | calm | `calm` | 66.1 | 6.1 | 4.0 | 84.2 |
| 2024-04-15 | **19.4** | calm | `recovery` | 53.9 | 24.3 | 13.1 | 66.7 |
| 2024-07-31 | **89.4** | imminent | `active_crisis` | 36.1 | 29.6 | 10.7 | 46.5 |
| 2024-08-02 | **88.5** | imminent | `active_crisis` | 36.3 | 23.1 | 8.4 | 30.0 |
| 2024-08-05 | **87.4** | imminent | `active_crisis` | 40.7 | 16.2 | 6.6 | 9.0 |
| 2024-08-14 | **10.6** | calm | `calm` | 32.5 | 21.1 | 6.9 | 77.9 |
| 2024-08-23 | **9.9** | calm | `calm` | 35.0 | 19.8 | 6.9 | 73.8 |
| 2024-09-18 | **27.6** | mild | `recovery` | 52.5 | 34.5 | 18.1 | 91.7 |
| 2024-10-01 | **55.3** | elevated | `pre_crisis` | 25.0 | 47.7 | 11.9 | 36.7 |
| 2024-12-18 | **88.7** | imminent | `active_crisis` | 43.9 | 24.5 | 10.8 | 38.8 |
| 2025-06-15 | **47.0** | elevated | `pre_crisis` | 47.3 | 40.6 | 19.2 | 64.3 |
| 2026-04-27 | **36.8** | mild | `pre_crisis` | 61.2 | 30.6 | 18.7 | 83.9 |
| 2026-04-28 | **44.5** | elevated | `pre_crisis` | 69.9 | 36.1 | 25.2 | 81.0 |
| 2026-04-29 | **57.9** | elevated | `pre_crisis` | 64.0 | 47.7 | 30.5 | 82.0 |
| 2026-05-01 | **40.1** | elevated | `pre_crisis` | 70.2 | 32.5 | 22.8 | 84.3 |

## 5. 핵심 발견 (Findings)

### 5.1 Phase 별 평균 fragility 단계적 분리

- **calm** 7.5 → **recovery** 26.6 → **pre_crisis** 46.6 → **active_crisis** 89.1

✅ **모노톤 증가 검증** — phase 별 fragility 가 정상 단계적 분리

### 5.2 Latent vector dominance

- Active crisis 의 top vector: **credit_stress** (30.4%)
- Pre-crisis 의 top vector: **credit_stress** (28.4%)

### 5.3 ★ Tier B 5 sample 검증 결과 (Phase 12.2)

| Date | Event | Fragility | Phase | 의의 |
|---|---|---|---|---|
| 2009-03-09 | S&P 666 cycle bottom | **94.0** | active_crisis | active 90+ 진입 (Phase 12.0 ceiling 깨짐) |
| 2009-03-23 | PPIP + QE1 expansion | 32.9 | recovery | Fed 개입 14일 효과 (94→33) |
| 2015-12-16 | Fed 첫 인상 (zero end) | 35.2 | pre_crisis | dovish hike — mild caution |
| 2024-09-18 | Fed 첫 인하 (jumbo 50bp) | 27.6 | recovery | Sustainability 91.7 (n=35 최고) |
| 2024-10-01 | Iran 미사일 직격 | 55.3 | pre_crisis | geopolitical_tail dominant |

### 5.4 Fed 개입의 정량 효과 (★ 시스템 핵심 가치)

**2009-03-09 → 2009-03-23 (14일 trajectory)**:
- Fragility: **94.0 → 32.9** (-65%)
- Latent: 60.2 → 41.1 (-32%) — credit stress 잔존하지만 감소
- Sustainability: **9.0 → 76.7** (+750%) — Fed 개입으로 cushion 회복
- Surface: 1.2 → 43.6 — 시장 반등 시작

→ **Fed 의 unlimited QE + PPIP + stress test framework 의 단일 14일 개입이 fragility 를 65% 감소시킴**

### 5.5 한계 및 추후 작업

- ✅ **Recovery phase 확장** — n=1 → n=3, avg 26.6, range 19.4-32.9
- ✅ **Active crisis 90+ 진입** — 2009-03-09 cycle bottom 94.0
- ✅ **Sustainability 동적 측정** — 2024-09-18 Fed cut 91.7 (peak), 2009-03-09 9.0 (bottom)
- ⚠️ **Tier C 미수행** — Lehman 2008-09-15, 닷컴 2000-04, Flash Crash 2010-05-06 등 10개 추가 필요

