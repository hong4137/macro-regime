# Phase 12 Fragility Index — Batch History 시계열

**생성**: 2026-05-17 00:20  
**Sessions**: 67  
**Date range**: 2000-04-14 ~ 2026-05-15

## 1. Phase 분포 통계

| Phase | n | Avg fragility | Min | Max |
|---|---|---|---|---|
| **active_crisis** | 19 | 91.0 | 87.7 | 94.2 |
| **pre_crisis** | 31 | 61.0 | 32.9 | 84.1 |
| **recovery** | 8 | 34.4 | 18.0 | 50.3 |
| **calm** | 9 | 8.2 | 2.2 | 13.1 |

## 2. Phase 별 Top Latent Vectors (avg %)

### active_crisis

| Vector | Avg contribution |
|---|---|
| credit stress | 25.1% |
| policy uncertainty | 13.9% |
| geopolitical tail | 12.8% |
| catalyst density | 7.8% |
| em fx stress | 6.5% |

### pre_crisis

| Vector | Avg contribution |
|---|---|
| policy uncertainty | 16.0% |
| geopolitical tail | 16.0% |
| credit stress | 13.6% |
| carry leverage | 10.0% |
| em fx stress | 9.5% |

### recovery

| Vector | Avg contribution |
|---|---|
| credit stress | 16.6% |
| policy uncertainty | 11.9% |
| em fx stress | 11.4% |
| geopolitical tail | 11.4% |
| carry leverage | 10.1% |

### calm

| Vector | Avg contribution |
|---|---|
| credit stress | 45.3% |
| catalyst density | 6.7% |
| em fx stress | 6.3% |
| geopolitical tail | 5.7% |
| policy uncertainty | 3.3% |

## 3. Crisis 직전 14일 fragility trajectory

- Active crisis sessions: **19**
- 14일 이내 prior session 가 있는 case: **7**

### Per-crisis trajectory

**2020-03-16** (crisis fragility 90.2):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-7 | 2020-03-09 | 49.0 | pre_crisis |

**2020-03-23** (crisis fragility 92.6):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-7 | 2020-03-16 | 90.2 | active_crisis |
| D-14 | 2020-03-09 | 49.0 | pre_crisis |

**2022-06-17** (crisis fragility 88.6):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-2 | 2022-06-15 | 93.3 | active_crisis |

**2023-03-13** (crisis fragility 89.3):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-4 | 2023-03-09 | 45.0 | pre_crisis |
| D-5 | 2023-03-08 | 8.8 | calm |

**2023-03-15** (crisis fragility 91.0):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-2 | 2023-03-13 | 89.3 | active_crisis |
| D-6 | 2023-03-09 | 45.0 | pre_crisis |
| D-7 | 2023-03-08 | 8.8 | calm |

**2024-08-02** (crisis fragility 90.2):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-1 | 2024-08-01 | 23.2 | recovery |
| D-2 | 2024-07-31 | 91.4 | active_crisis |

**2024-08-05** (crisis fragility 91.2):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-3 | 2024-08-02 | 90.2 | active_crisis |
| D-4 | 2024-08-01 | 23.2 | recovery |
| D-5 | 2024-07-31 | 91.4 | active_crisis |

## 4. Full timeline (모든 sessions)

| Date | Fragility | Level | Phase | Surface | Latent | Decoupling | Sustain. |
|---|---|---|---|---|---|---|---|
| 2000-04-14 | **88.9** | imminent | `active_crisis` | 5.1 | 26.1 | 1.3 | 30.0 |
| 2002-10-09 | **92.0** | imminent | `active_crisis` | 2.4 | 46.6 | 1.1 | 9.0 |
| 2008-09-15 | **93.1** | imminent | `active_crisis` | 2.4 | 54.2 | 1.3 | 9.0 |
| 2008-10-10 | **94.2** | imminent | `active_crisis` | 1.2 | 61.0 | 0.7 | 9.0 |
| 2009-03-09 | **93.6** | imminent | `active_crisis` | 1.2 | 57.3 | 0.7 | 9.0 |
| 2009-03-23 | **34.1** | mild | `recovery` | 43.6 | 42.6 | 18.6 | 76.7 |
| 2010-05-06 | **89.7** | imminent | `active_crisis` | 36.9 | 31.6 | 11.7 | 30.0 |
| 2011-08-05 | **91.2** | imminent | `active_crisis` | 5.1 | 41.2 | 2.1 | 30.0 |
| 2012-09-13 | **18.0** | calm | `recovery` | 66.6 | 22.5 | 15.0 | 100.0 |
| 2015-12-16 | **38.6** | mild | `pre_crisis` | 40.8 | 34.0 | 13.9 | 70.8 |
| 2017-09-15 | **2.2** | calm | `calm` | 57.4 | 4.5 | 2.6 | 75.4 |
| 2018-02-05 | **90.1** | imminent | `active_crisis` | 40.2 | 34.0 | 13.7 | 30.0 |
| 2018-12-19 | **60.9** | critical | `pre_crisis` | 4.7 | 44.2 | 2.1 | 30.0 |
| 2019-04-15 | **6.3** | calm | `calm` | 61.0 | 12.7 | 7.7 | 65.7 |
| 2019-08-05 | **87.7** | imminent | `active_crisis` | 47.0 | 18.0 | 8.5 | 45.0 |
| 2020-02-19 | **9.6** | calm | `calm` | 48.6 | 19.1 | 9.3 | 71.5 |
| 2020-02-21 | **7.5** | calm | `calm` | 37.1 | 15.0 | 5.6 | 65.0 |
| 2020-02-24 | **39.0** | mild | `pre_crisis` | 15.0 | 15.7 | 2.4 | 65.0 |
| 2020-02-27 | **41.3** | elevated | `pre_crisis` | 15.0 | 17.9 | 2.7 | 65.0 |
| 2020-03-09 | **49.0** | elevated | `pre_crisis` | 15.0 | 39.5 | 5.9 | 65.0 |
| 2020-03-16 | **90.2** | imminent | `active_crisis` | 31.4 | 34.8 | 10.9 | 9.0 |
| 2020-03-23 | **92.6** | imminent | `active_crisis` | 0.7 | 50.9 | 0.4 | 22.3 |
| 2021-05-15 | **3.8** | calm | `calm` | 43.9 | 7.5 | 3.3 | 55.5 |
| 2022-02-22 | **39.9** | mild | `pre_crisis` | 15.0 | 30.9 | 4.6 | 65.0 |
| 2022-02-24 | **45.0** | elevated | `pre_crisis` | 27.3 | 29.0 | 7.9 | 22.3 |
| 2022-03-16 | **52.5** | elevated | `pre_crisis` | 34.4 | 47.1 | 16.2 | 70.8 |
| 2022-06-15 | **93.3** | imminent | `active_crisis` | 5.1 | 55.3 | 2.8 | 30.0 |
| 2022-06-17 | **88.6** | imminent | `active_crisis` | 43.7 | 23.9 | 10.4 | 30.0 |
| 2022-10-13 | **51.7** | elevated | `pre_crisis` | 2.7 | 37.7 | 1.0 | 55.2 |
| 2023-03-08 | **8.8** | calm | `calm` | 44.6 | 17.7 | 7.9 | 65.0 |
| 2023-03-09 | **45.0** | elevated | `pre_crisis` | 23.8 | 21.4 | 5.1 | 22.3 |
| 2023-03-13 | **89.3** | imminent | `active_crisis` | 13.2 | 28.9 | 3.8 | 9.0 |
| 2023-03-15 | **91.0** | imminent | `active_crisis` | 3.0 | 39.7 | 1.2 | 13.2 |
| 2023-03-24 | **45.0** | elevated | `pre_crisis` | 12.3 | 33.9 | 4.2 | 71.9 |
| 2023-04-06 | **11.1** | calm | `calm` | 36.5 | 22.2 | 8.1 | 62.3 |
| 2023-10-09 | **48.9** | elevated | `pre_crisis` | 25.7 | 43.0 | 11.1 | 71.7 |
| 2023-12-13 | **13.1** | calm | `calm` | 66.1 | 26.1 | 17.3 | 84.2 |
| 2024-04-15 | **32.9** | mild | `pre_crisis` | 53.9 | 27.9 | 15.0 | 66.7 |
| 2024-07-31 | **91.4** | imminent | `active_crisis` | 36.1 | 42.8 | 15.5 | 46.5 |
| 2024-08-01 | **23.2** | mild | `recovery` | 43.4 | 29.0 | 12.6 | 65.0 |
| 2024-08-02 | **90.2** | imminent | `active_crisis` | 36.3 | 34.7 | 12.6 | 30.0 |
| 2024-08-05 | **91.2** | imminent | `active_crisis` | 40.7 | 41.2 | 16.8 | 9.0 |
| 2024-08-14 | **24.4** | mild | `recovery` | 32.5 | 30.5 | 9.9 | 77.9 |
| 2024-08-23 | **11.8** | calm | `calm` | 35.0 | 23.5 | 8.2 | 73.8 |
| 2024-09-18 | **31.4** | mild | `recovery` | 52.5 | 39.3 | 20.6 | 91.7 |
| 2024-10-01 | **59.6** | elevated | `pre_crisis` | 25.0 | 51.7 | 12.9 | 36.7 |
| 2024-12-18 | **90.5** | imminent | `active_crisis` | 43.9 | 37.0 | 16.2 | 38.8 |
| 2025-06-15 | **50.6** | elevated | `pre_crisis` | 47.3 | 43.7 | 20.7 | 64.3 |
| 2026-04-27 | **55.5** | elevated | `pre_crisis` | 61.2 | 46.1 | 28.2 | 83.9 |
| 2026-04-28 | **69.2** | critical | `pre_crisis` | 69.9 | 56.1 | 39.2 | 81.0 |
| 2026-04-29 | **84.1** | imminent | `pre_crisis` | 64.0 | 69.3 | 44.4 | 82.0 |
| 2026-04-30 | **71.9** | critical | `pre_crisis` | 41.9 | 63.1 | 26.4 | 76.7 |
| 2026-05-01 | **70.8** | critical | `pre_crisis` | 70.2 | 57.4 | 40.3 | 84.3 |
| 2026-05-02 | **71.9** | critical | `pre_crisis` | 70.2 | 58.3 | 40.9 | 84.3 |
| 2026-05-03 | **71.9** | critical | `pre_crisis` | 70.2 | 58.3 | 40.9 | 84.3 |
| 2026-05-04 | **83.7** | imminent | `pre_crisis` | 47.2 | 72.3 | 34.1 | 80.0 |
| 2026-05-05 | **83.6** | imminent | `pre_crisis` | 46.8 | 72.3 | 33.8 | 91.7 |
| 2026-05-06 | **77.3** | critical | `pre_crisis` | 47.3 | 66.8 | 31.6 | 100.0 |
| 2026-05-07 | **78.0** | critical | `pre_crisis` | 50.1 | 66.8 | 33.5 | 91.7 |
| 2026-05-08 | **76.5** | critical | `pre_crisis` | 53.3 | 65.0 | 34.6 | 100.0 |
| 2026-05-09 | **74.1** | critical | `pre_crisis` | 39.9 | 65.4 | 26.1 | 80.0 |
| 2026-05-10 | **71.6** | critical | `pre_crisis` | 39.7 | 63.2 | 25.1 | 80.0 |
| 2026-05-11 | **73.0** | critical | `pre_crisis` | 39.5 | 64.5 | 25.5 | 80.0 |
| 2026-05-12 | **78.3** | critical | `pre_crisis` | 49.5 | 67.2 | 33.3 | 91.7 |
| 2026-05-13 | **50.3** | elevated | `recovery` | 51.8 | 62.9 | 32.6 | 100.0 |
| 2026-05-14 | **47.5** | elevated | `recovery` | 50.1 | 59.4 | 29.8 | 100.0 |
| 2026-05-15 | **46.4** | elevated | `recovery` | 46.5 | 58.0 | 27.0 | 82.5 |

## 5. 핵심 발견 (Findings)

### 5.1 Phase 별 평균 fragility 단계적 분리

- **calm** 8.2 → **recovery** 34.4 → **pre_crisis** 61.0 → **active_crisis** 91.0

✅ **모노톤 증가 검증** — phase 별 fragility 가 정상 단계적 분리

### 5.2 Latent vector dominance

- Active crisis 의 top vector: **credit_stress** (25.1%)
- Pre-crisis 의 top vector: **policy_uncertainty** (16.0%)

