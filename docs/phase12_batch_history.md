# Phase 12 Fragility Index — Batch History 시계열

**생성**: 2026-05-16 23:39  
**Sessions**: 67  
**Date range**: 2000-04-14 ~ 2026-05-15

## 1. Phase 분포 통계

| Phase | n | Avg fragility | Min | Max |
|---|---|---|---|---|
| **active_crisis** | 19 | 89.6 | 87.7 | 92.0 |
| **pre_crisis** | 30 | 50.4 | 25.0 | 76.1 |
| **recovery** | 8 | 26.2 | 13.7 | 38.2 |
| **calm** | 10 | 7.4 | 2.2 | 16.4 |

## 2. Phase 별 Top Latent Vectors (avg %)

### active_crisis

| Vector | Avg contribution |
|---|---|
| credit stress | 32.4% |
| policy uncertainty | 18.3% |
| geopolitical tail | 16.9% |
| catalyst density | 10.0% |
| em fx stress | 8.5% |

### pre_crisis

| Vector | Avg contribution |
|---|---|
| geopolitical tail | 21.3% |
| policy uncertainty | 21.1% |
| credit stress | 17.6% |
| carry leverage | 13.1% |
| em fx stress | 12.5% |

### recovery

| Vector | Avg contribution |
|---|---|
| credit stress | 21.8% |
| policy uncertainty | 15.7% |
| geopolitical tail | 15.1% |
| em fx stress | 14.9% |
| carry leverage | 13.2% |

### calm

| Vector | Avg contribution |
|---|---|
| credit stress | 53.1% |
| em fx stress | 8.8% |
| catalyst density | 8.7% |
| geopolitical tail | 8.4% |
| policy uncertainty | 5.5% |

## 3. Crisis 직전 14일 fragility trajectory

- Active crisis sessions: **19**
- 14일 이내 prior session 가 있는 case: **7**

### Per-crisis trajectory

**2020-03-16** (crisis fragility 89.0):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-7 | 2020-03-09 | 39.0 | pre_crisis |

**2020-03-23** (crisis fragility 90.8):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-7 | 2020-03-16 | 89.0 | active_crisis |
| D-14 | 2020-03-09 | 39.0 | pre_crisis |

**2022-06-17** (crisis fragility 87.7):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-2 | 2022-06-15 | 91.3 | active_crisis |

**2023-03-13** (crisis fragility 88.3):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-4 | 2023-03-09 | 45.0 | pre_crisis |
| D-5 | 2023-03-08 | 6.7 | calm |

**2023-03-15** (crisis fragility 89.5):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-2 | 2023-03-13 | 88.3 | active_crisis |
| D-6 | 2023-03-09 | 45.0 | pre_crisis |
| D-7 | 2023-03-08 | 6.7 | calm |

**2024-08-02** (crisis fragility 89.0):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-1 | 2024-08-01 | 17.7 | recovery |
| D-2 | 2024-07-31 | 89.9 | active_crisis |

**2024-08-05** (crisis fragility 89.7):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-3 | 2024-08-02 | 89.0 | active_crisis |
| D-4 | 2024-08-01 | 17.7 | recovery |
| D-5 | 2024-07-31 | 89.9 | active_crisis |

## 4. Full timeline (모든 sessions)

| Date | Fragility | Level | Phase | Surface | Latent | Decoupling | Sustain. |
|---|---|---|---|---|---|---|---|
| 2000-04-14 | **88.0** | imminent | `active_crisis` | 5.1 | 19.8 | 1.0 | 30.0 |
| 2002-10-09 | **90.3** | imminent | `active_crisis` | 2.4 | 35.4 | 0.8 | 9.0 |
| 2008-09-15 | **91.2** | imminent | `active_crisis` | 2.4 | 41.2 | 1.0 | 9.0 |
| 2008-10-10 | **92.0** | imminent | `active_crisis` | 1.2 | 46.4 | 0.6 | 9.0 |
| 2009-03-09 | **91.5** | imminent | `active_crisis` | 1.2 | 43.6 | 0.5 | 9.0 |
| 2009-03-23 | **25.8** | mild | `recovery` | 43.6 | 32.3 | 14.1 | 76.7 |
| 2010-05-06 | **88.6** | imminent | `active_crisis` | 36.9 | 24.0 | 8.9 | 30.0 |
| 2011-08-05 | **89.7** | imminent | `active_crisis` | 5.1 | 31.3 | 1.6 | 30.0 |
| 2012-09-13 | **13.7** | calm | `recovery` | 66.6 | 17.1 | 11.4 | 100.0 |
| 2015-12-16 | **29.3** | mild | `pre_crisis` | 40.8 | 25.8 | 10.5 | 70.8 |
| 2017-09-15 | **2.2** | calm | `calm` | 57.4 | 4.5 | 2.6 | 75.4 |
| 2018-02-05 | **88.9** | imminent | `active_crisis` | 40.2 | 25.9 | 10.4 | 30.0 |
| 2018-12-19 | **50.1** | elevated | `pre_crisis` | 4.7 | 33.6 | 1.6 | 30.0 |
| 2019-04-15 | **4.8** | calm | `calm` | 61.0 | 9.7 | 5.9 | 65.7 |
| 2019-08-05 | **87.7** | imminent | `active_crisis` | 47.0 | 18.0 | 8.5 | 45.0 |
| 2020-02-19 | **7.2** | calm | `calm` | 48.6 | 14.5 | 7.0 | 71.5 |
| 2020-02-21 | **5.7** | calm | `calm` | 37.1 | 11.4 | 4.2 | 65.0 |
| 2020-02-24 | **35.0** | mild | `pre_crisis` | 15.0 | 11.9 | 1.8 | 65.0 |
| 2020-02-27 | **36.8** | mild | `pre_crisis` | 15.0 | 13.6 | 2.0 | 65.0 |
| 2020-03-09 | **39.0** | mild | `pre_crisis` | 15.0 | 30.0 | 4.5 | 65.0 |
| 2020-03-16 | **89.0** | imminent | `active_crisis` | 31.4 | 26.5 | 8.3 | 9.0 |
| 2020-03-23 | **90.8** | imminent | `active_crisis` | 0.7 | 38.7 | 0.3 | 22.3 |
| 2021-05-15 | **3.8** | calm | `calm` | 43.9 | 7.5 | 3.3 | 55.5 |
| 2022-02-22 | **47.2** | elevated | `pre_crisis` | 15.0 | 23.5 | 3.5 | 65.0 |
| 2022-02-24 | **45.0** | elevated | `pre_crisis` | 27.3 | 22.0 | 6.0 | 22.3 |
| 2022-03-16 | **39.9** | mild | `pre_crisis` | 34.4 | 35.8 | 12.3 | 70.8 |
| 2022-06-15 | **91.3** | imminent | `active_crisis` | 5.1 | 42.1 | 2.1 | 30.0 |
| 2022-06-17 | **87.7** | imminent | `active_crisis` | 43.7 | 18.2 | 8.0 | 30.0 |
| 2022-10-13 | **76.1** | critical | `pre_crisis` | 2.7 | 28.7 | 0.8 | 55.2 |
| 2023-03-08 | **6.7** | calm | `calm` | 44.6 | 13.4 | 6.0 | 65.0 |
| 2023-03-09 | **45.0** | elevated | `pre_crisis` | 23.8 | 16.3 | 3.9 | 22.3 |
| 2023-03-13 | **88.3** | imminent | `active_crisis` | 13.2 | 22.0 | 2.9 | 9.0 |
| 2023-03-15 | **89.5** | imminent | `active_crisis` | 3.0 | 30.2 | 0.9 | 13.2 |
| 2023-03-24 | **54.7** | elevated | `pre_crisis` | 12.3 | 25.7 | 3.2 | 71.9 |
| 2023-04-06 | **8.4** | calm | `calm` | 36.5 | 16.9 | 6.2 | 62.3 |
| 2023-10-09 | **16.4** | calm | `calm` | 25.7 | 32.7 | 8.4 | 71.7 |
| 2023-12-13 | **9.9** | calm | `calm` | 66.1 | 19.9 | 13.2 | 84.2 |
| 2024-04-15 | **25.0** | mild | `pre_crisis` | 53.9 | 21.2 | 11.4 | 66.7 |
| 2024-07-31 | **89.9** | imminent | `active_crisis` | 36.1 | 32.5 | 11.7 | 46.5 |
| 2024-08-01 | **17.7** | calm | `recovery` | 43.4 | 22.1 | 9.6 | 65.0 |
| 2024-08-02 | **89.0** | imminent | `active_crisis` | 36.3 | 26.4 | 9.6 | 30.0 |
| 2024-08-05 | **89.7** | imminent | `active_crisis` | 40.7 | 31.3 | 12.7 | 9.0 |
| 2024-08-14 | **18.6** | calm | `recovery` | 32.5 | 23.2 | 7.5 | 77.9 |
| 2024-08-23 | **8.9** | calm | `calm` | 35.0 | 17.9 | 6.3 | 73.8 |
| 2024-09-18 | **23.9** | mild | `recovery` | 52.5 | 29.9 | 15.7 | 91.7 |
| 2024-10-01 | **46.2** | elevated | `pre_crisis` | 25.0 | 39.3 | 9.8 | 36.7 |
| 2024-12-18 | **89.2** | imminent | `active_crisis` | 43.9 | 28.1 | 12.3 | 38.8 |
| 2025-06-15 | **38.4** | mild | `pre_crisis` | 47.3 | 33.2 | 15.7 | 64.3 |
| 2026-04-27 | **42.3** | elevated | `pre_crisis` | 61.2 | 35.1 | 21.5 | 83.9 |
| 2026-04-28 | **52.5** | elevated | `pre_crisis` | 69.9 | 42.6 | 29.8 | 81.0 |
| 2026-04-29 | **63.8** | critical | `pre_crisis` | 64.0 | 52.6 | 33.7 | 82.0 |
| 2026-04-30 | **54.7** | elevated | `pre_crisis` | 41.9 | 48.0 | 20.1 | 76.7 |
| 2026-05-01 | **53.8** | elevated | `pre_crisis` | 70.2 | 43.6 | 30.6 | 84.3 |
| 2026-05-02 | **54.7** | elevated | `pre_crisis` | 70.2 | 44.3 | 31.1 | 84.3 |
| 2026-05-03 | **54.7** | elevated | `pre_crisis` | 70.2 | 44.3 | 31.1 | 84.3 |
| 2026-05-04 | **63.5** | critical | `pre_crisis` | 47.2 | 54.9 | 25.9 | 80.0 |
| 2026-05-05 | **63.5** | critical | `pre_crisis` | 46.8 | 54.9 | 25.7 | 91.7 |
| 2026-05-06 | **58.8** | elevated | `pre_crisis` | 47.3 | 50.8 | 24.0 | 100.0 |
| 2026-05-07 | **59.3** | elevated | `pre_crisis` | 50.1 | 50.8 | 25.5 | 91.7 |
| 2026-05-08 | **58.2** | elevated | `pre_crisis` | 53.3 | 49.4 | 26.3 | 100.0 |
| 2026-05-09 | **56.3** | elevated | `pre_crisis` | 39.9 | 49.7 | 19.8 | 80.0 |
| 2026-05-10 | **54.4** | elevated | `pre_crisis` | 39.7 | 48.0 | 19.1 | 80.0 |
| 2026-05-11 | **55.5** | elevated | `pre_crisis` | 39.5 | 49.0 | 19.4 | 80.0 |
| 2026-05-12 | **59.4** | elevated | `pre_crisis` | 49.5 | 51.0 | 25.2 | 91.7 |
| 2026-05-13 | **38.2** | mild | `recovery` | 51.8 | 47.8 | 24.8 | 100.0 |
| 2026-05-14 | **36.2** | mild | `recovery` | 50.1 | 45.2 | 22.6 | 100.0 |
| 2026-05-15 | **35.3** | mild | `recovery` | 46.5 | 44.1 | 20.5 | 82.5 |

## 5. 핵심 발견 (Findings)

### 5.1 Phase 별 평균 fragility 단계적 분리

- **calm** 7.4 → **recovery** 26.2 → **pre_crisis** 50.4 → **active_crisis** 89.6

✅ **모노톤 증가 검증** — phase 별 fragility 가 정상 단계적 분리

### 5.2 Latent vector dominance

- Active crisis 의 top vector: **credit_stress** (32.4%)
- Pre-crisis 의 top vector: **geopolitical_tail** (21.3%)

