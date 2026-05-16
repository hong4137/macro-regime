# Phase 12 Fragility Index — Batch History 시계열

**생성**: 2026-05-16 18:01  
**Sessions**: 60  
**Date range**: 2000-04-14 ~ 2026-05-15

## 1. Phase 분포 통계

| Phase | n | Avg fragility | Min | Max |
|---|---|---|---|---|
| **active_crisis** | 19 | 90.9 | 87.4 | 93.9 |
| **pre_crisis** | 26 | 63.5 | 32.9 | 82.6 |
| **recovery** | 7 | 36.9 | 18.6 | 52.6 |
| **calm** | 8 | 9.9 | 2.2 | 18.9 |

## 2. Phase 별 Top Latent Vectors (avg %)

### active_crisis

| Vector | Avg contribution |
|---|---|
| credit stress | 35.3% |
| geopolitical tail | 16.0% |
| policy uncertainty | 15.6% |
| catalyst density | 12.8% |
| em fx stress | 8.6% |

### pre_crisis

| Vector | Avg contribution |
|---|---|
| geopolitical tail | 21.0% |
| policy uncertainty | 20.6% |
| carry leverage | 16.4% |
| credit stress | 15.4% |
| em fx stress | 14.1% |

### recovery

| Vector | Avg contribution |
|---|---|
| credit stress | 21.8% |
| policy uncertainty | 17.2% |
| geopolitical tail | 15.9% |
| em fx stress | 13.6% |
| carry leverage | 12.0% |

### calm

| Vector | Avg contribution |
|---|---|
| credit stress | 56.4% |
| catalyst density | 10.0% |
| geopolitical tail | 9.9% |
| em fx stress | 8.6% |
| carry leverage | 5.7% |

## 3. Crisis 직전 14일 fragility trajectory

- Active crisis sessions: **19**
- 14일 이내 prior session 가 있는 case: **6**

### Per-crisis trajectory

**2020-03-23** (crisis fragility 92.6):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-7 | 2020-03-16 | 90.0 | active_crisis |

**2022-06-17** (crisis fragility 88.9):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-2 | 2022-06-15 | 93.3 | active_crisis |

**2023-03-13** (crisis fragility 88.9):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-4 | 2023-03-09 | 45.0 | pre_crisis |

**2023-03-15** (crisis fragility 91.2):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-2 | 2023-03-13 | 88.9 | active_crisis |
| D-6 | 2023-03-09 | 45.0 | pre_crisis |

**2024-08-02** (crisis fragility 90.4):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-2 | 2024-07-31 | 91.7 | active_crisis |

**2024-08-05** (crisis fragility 91.0):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-3 | 2024-08-02 | 90.4 | active_crisis |
| D-5 | 2024-07-31 | 91.7 | active_crisis |

## 4. Full timeline (모든 sessions)

| Date | Fragility | Level | Phase | Surface | Latent | Decoupling | Sustain. |
|---|---|---|---|---|---|---|---|
| 2000-04-14 | **88.9** | imminent | `active_crisis` | 5.1 | 26.1 | 1.3 | 30.0 |
| 2002-10-09 | **92.0** | imminent | `active_crisis` | 2.4 | 46.7 | 1.1 | 9.0 |
| 2008-09-15 | **93.2** | imminent | `active_crisis` | 2.4 | 54.4 | 1.3 | 9.0 |
| 2008-10-10 | **93.9** | imminent | `active_crisis` | 1.2 | 59.6 | 0.7 | 9.0 |
| 2009-03-09 | **93.5** | imminent | `active_crisis` | 1.2 | 56.5 | 0.7 | 9.0 |
| 2009-03-23 | **34.1** | mild | `recovery` | 43.6 | 42.6 | 18.6 | 76.7 |
| 2010-05-06 | **89.7** | imminent | `active_crisis` | 36.9 | 31.3 | 11.5 | 30.0 |
| 2011-08-05 | **91.1** | imminent | `active_crisis` | 5.1 | 40.9 | 2.1 | 30.0 |
| 2012-09-13 | **18.6** | calm | `recovery` | 66.6 | 23.2 | 15.5 | 100.0 |
| 2015-12-16 | **38.5** | mild | `pre_crisis` | 40.8 | 33.9 | 13.8 | 70.8 |
| 2017-09-15 | **2.2** | calm | `calm` | 57.4 | 4.5 | 2.6 | 75.4 |
| 2018-02-05 | **90.1** | imminent | `active_crisis` | 40.2 | 33.7 | 13.5 | 30.0 |
| 2018-12-19 | **59.9** | elevated | `pre_crisis` | 4.7 | 43.2 | 2.0 | 30.0 |
| 2019-04-15 | **6.7** | calm | `calm` | 61.0 | 13.4 | 8.2 | 65.7 |
| 2019-08-05 | **87.4** | imminent | `active_crisis` | 47.0 | 15.8 | 7.4 | 45.0 |
| 2020-02-19 | **10.1** | calm | `calm` | 48.6 | 20.1 | 9.8 | 71.5 |
| 2020-03-16 | **90.0** | imminent | `active_crisis` | 31.4 | 33.0 | 10.4 | 9.0 |
| 2020-03-23 | **92.6** | imminent | `active_crisis` | 0.7 | 50.6 | 0.4 | 22.3 |
| 2021-05-15 | **3.8** | calm | `calm` | 43.9 | 7.5 | 3.3 | 55.5 |
| 2022-02-24 | **45.0** | elevated | `pre_crisis` | 27.3 | 28.9 | 7.9 | 22.3 |
| 2022-03-16 | **52.1** | elevated | `pre_crisis` | 34.4 | 46.7 | 16.1 | 70.8 |
| 2022-06-15 | **93.3** | imminent | `active_crisis` | 5.1 | 55.3 | 2.8 | 30.0 |
| 2022-06-17 | **88.9** | imminent | `active_crisis` | 43.7 | 25.8 | 11.3 | 30.0 |
| 2022-10-13 | **52.4** | elevated | `pre_crisis` | 2.7 | 38.4 | 1.0 | 55.2 |
| 2023-03-09 | **45.0** | elevated | `pre_crisis` | 23.8 | 22.8 | 5.4 | 22.3 |
| 2023-03-13 | **88.9** | imminent | `active_crisis` | 13.2 | 25.9 | 3.4 | 9.0 |
| 2023-03-15 | **91.2** | imminent | `active_crisis` | 3.0 | 41.4 | 1.2 | 13.2 |
| 2023-03-24 | **45.0** | elevated | `pre_crisis` | 12.3 | 34.1 | 4.2 | 71.9 |
| 2023-04-06 | **12.1** | calm | `calm` | 36.5 | 24.1 | 8.8 | 62.3 |
| 2023-10-09 | **18.9** | calm | `calm` | 25.7 | 37.9 | 9.7 | 71.7 |
| 2023-12-13 | **13.7** | calm | `calm` | 66.1 | 27.4 | 18.1 | 84.2 |
| 2024-04-15 | **32.9** | mild | `pre_crisis` | 53.9 | 27.9 | 15.0 | 66.7 |
| 2024-07-31 | **91.7** | imminent | `active_crisis` | 36.1 | 44.6 | 16.1 | 46.5 |
| 2024-08-02 | **90.4** | imminent | `active_crisis` | 36.3 | 36.2 | 13.1 | 30.0 |
| 2024-08-05 | **91.0** | imminent | `active_crisis` | 40.7 | 40.3 | 16.4 | 9.0 |
| 2024-08-14 | **24.4** | mild | `recovery` | 32.5 | 30.5 | 9.9 | 77.9 |
| 2024-08-23 | **12.1** | calm | `calm` | 35.0 | 24.1 | 8.4 | 73.8 |
| 2024-09-18 | **31.8** | mild | `recovery` | 52.5 | 39.8 | 20.9 | 91.7 |
| 2024-10-01 | **59.9** | elevated | `pre_crisis` | 25.0 | 52.0 | 13.0 | 36.7 |
| 2024-12-18 | **90.1** | imminent | `active_crisis` | 43.9 | 34.2 | 15.0 | 38.8 |
| 2025-06-15 | **49.3** | elevated | `pre_crisis` | 47.3 | 42.6 | 20.1 | 64.3 |
| 2026-04-27 | **45.4** | elevated | `pre_crisis` | 61.2 | 37.7 | 23.1 | 83.9 |
| 2026-04-28 | **70.9** | critical | `pre_crisis` | 69.9 | 57.5 | 40.2 | 81.0 |
| 2026-04-29 | **75.5** | critical | `pre_crisis` | 64.0 | 62.2 | 39.8 | 82.0 |
| 2026-04-30 | **72.0** | critical | `pre_crisis` | 41.9 | 63.2 | 26.5 | 76.7 |
| 2026-05-01 | **68.0** | critical | `pre_crisis` | 70.2 | 55.1 | 38.7 | 84.3 |
| 2026-05-02 | **71.1** | critical | `pre_crisis` | 70.2 | 57.6 | 40.4 | 84.3 |
| 2026-05-03 | **71.1** | critical | `pre_crisis` | 70.2 | 57.6 | 40.4 | 84.3 |
| 2026-05-04 | **82.6** | imminent | `pre_crisis` | 47.2 | 71.4 | 33.7 | 80.0 |
| 2026-05-05 | **82.5** | imminent | `pre_crisis` | 46.8 | 71.4 | 33.4 | 91.7 |
| 2026-05-06 | **79.8** | critical | `pre_crisis` | 47.3 | 68.9 | 32.6 | 100.0 |
| 2026-05-07 | **80.4** | imminent | `pre_crisis` | 50.1 | 68.9 | 34.5 | 91.7 |
| 2026-05-08 | **79.3** | critical | `pre_crisis` | 53.3 | 67.3 | 35.9 | 100.0 |
| 2026-05-09 | **73.2** | critical | `pre_crisis` | 39.9 | 64.6 | 25.8 | 80.0 |
| 2026-05-10 | **70.1** | critical | `pre_crisis` | 39.7 | 61.9 | 24.6 | 80.0 |
| 2026-05-11 | **71.9** | critical | `pre_crisis` | 39.5 | 63.5 | 25.1 | 80.0 |
| 2026-05-12 | **77.0** | critical | `pre_crisis` | 49.5 | 66.1 | 32.7 | 91.7 |
| 2026-05-13 | **52.6** | elevated | `recovery` | 51.8 | 65.7 | 34.0 | 100.0 |
| 2026-05-14 | **47.6** | elevated | `recovery` | 50.1 | 59.5 | 29.8 | 100.0 |
| 2026-05-15 | **49.5** | elevated | `recovery` | 46.5 | 61.9 | 28.8 | 82.5 |

## 5. 핵심 발견 (Findings)

### 5.1 Phase 별 평균 fragility 단계적 분리

- **calm** 9.9 → **recovery** 36.9 → **pre_crisis** 63.5 → **active_crisis** 90.9

✅ **모노톤 증가 검증** — phase 별 fragility 가 정상 단계적 분리

### 5.2 Latent vector dominance

- Active crisis 의 top vector: **credit_stress** (35.3%)
- Pre-crisis 의 top vector: **geopolitical_tail** (21.0%)

