# Phase 12 Fragility Index — Batch History 시계열

**생성**: 2026-05-17 15:48  
**Sessions**: 72  
**Date range**: 2000-04-14 ~ 2026-05-15

## 1. Phase 분포 통계

| Phase | n | Avg fragility | Min | Max |
|---|---|---|---|---|
| **active_crisis** | 24 | 89.5 | 87.4 | 91.6 |
| **pre_crisis** | 29 | 46.1 | 20.0 | 76.2 |
| **recovery** | 6 | 23.5 | 14.2 | 36.0 |
| **calm** | 13 | 7.5 | 2.2 | 13.7 |

## 2. Phase 별 Top Latent Vectors (avg %)

### active_crisis

| Vector | Avg contribution |
|---|---|
| sector dispersion | 17.3% |
| credit stress | 13.6% |
| catalyst density | 8.8% |
| carry leverage | 7.8% |
| bond vol stress | 6.2% |

### pre_crisis

| Vector | Avg contribution |
|---|---|
| carry leverage | 29.8% |
| sector dispersion | 24.1% |
| catalyst density | 7.7% |
| credit stress | 7.2% |
| em credit stress | 1.5% |

### recovery

| Vector | Avg contribution |
|---|---|
| sector dispersion | 28.2% |
| carry leverage | 20.1% |
| credit stress | 10.8% |
| catalyst density | 7.8% |
| em credit stress | 0.7% |

### calm

| Vector | Avg contribution |
|---|---|
| carry leverage | 25.2% |
| credit stress | 21.3% |
| catalyst density | 10.0% |
| sector dispersion | 5.0% |
| em credit stress | 1.6% |

## 3. Crisis 직전 14일 fragility trajectory

- Active crisis sessions: **24**
- 14일 이내 prior session 가 있는 case: **11**

### Per-crisis trajectory

**2008-09-15** (crisis fragility 90.5):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-3 | 2008-09-12 | 89.0 | active_crisis |

**2008-09-29** (crisis fragility 91.2):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-14 | 2008-09-15 | 90.5 | active_crisis |

**2008-10-10** (crisis fragility 89.8):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-11 | 2008-09-29 | 91.2 | active_crisis |

**2018-12-24** (crisis fragility 88.7):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-5 | 2018-12-19 | 76.2 | pre_crisis |

**2020-03-16** (crisis fragility 88.6):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-7 | 2020-03-09 | 39.8 | pre_crisis |

**2020-03-23** (crisis fragility 89.3):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-7 | 2020-03-16 | 88.6 | active_crisis |
| D-14 | 2020-03-09 | 39.8 | pre_crisis |

**2022-06-17** (crisis fragility 88.3):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-2 | 2022-06-15 | 91.6 | active_crisis |

**2023-03-13** (crisis fragility 89.2):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-4 | 2023-03-09 | 45.0 | pre_crisis |
| D-5 | 2023-03-08 | 4.1 | calm |

**2023-03-15** (crisis fragility 89.2):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-2 | 2023-03-13 | 89.2 | active_crisis |
| D-6 | 2023-03-09 | 45.0 | pre_crisis |
| D-7 | 2023-03-08 | 4.1 | calm |

**2024-08-02** (crisis fragility 89.5):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-1 | 2024-08-01 | 36.0 | recovery |
| D-2 | 2024-07-31 | 90.7 | active_crisis |

**2024-08-05** (crisis fragility 91.1):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-3 | 2024-08-02 | 89.5 | active_crisis |
| D-4 | 2024-08-01 | 36.0 | recovery |
| D-5 | 2024-07-31 | 90.7 | active_crisis |

## 4. Full timeline (모든 sessions)

| Date | Fragility | Level | Phase | Surface | Latent | Decoupling | Sustain. |
|---|---|---|---|---|---|---|---|
| 2000-04-14 | **87.4** | imminent | `active_crisis` | 5.1 | 15.7 | 0.8 | 30.0 |
| 2002-10-09 | **90.1** | imminent | `active_crisis` | 2.4 | 34.0 | 0.8 | 9.0 |
| 2008-09-12 | **89.0** | imminent | `active_crisis` | 2.1 | 26.9 | 0.6 | 9.0 |
| 2008-09-15 | **90.5** | imminent | `active_crisis` | 2.4 | 36.7 | 0.9 | 9.0 |
| 2008-09-29 | **91.2** | imminent | `active_crisis` | 0.9 | 41.1 | 0.4 | 9.0 |
| 2008-10-10 | **89.8** | imminent | `active_crisis` | 1.2 | 31.7 | 0.4 | 9.0 |
| 2009-03-09 | **89.8** | imminent | `active_crisis` | 1.2 | 31.7 | 0.4 | 9.0 |
| 2009-03-23 | **26.1** | mild | `recovery` | 43.6 | 32.6 | 14.2 | 76.7 |
| 2010-05-06 | **89.3** | imminent | `active_crisis` | 36.9 | 28.7 | 10.6 | 30.0 |
| 2011-08-05 | **89.6** | imminent | `active_crisis` | 5.1 | 30.8 | 1.6 | 30.0 |
| 2012-09-13 | **14.2** | calm | `recovery` | 66.6 | 17.8 | 11.9 | 100.0 |
| 2015-08-24 | **89.0** | imminent | `active_crisis` | 1.8 | 27.0 | 0.5 | 9.0 |
| 2015-12-16 | **33.9** | mild | `pre_crisis` | 40.8 | 29.8 | 12.2 | 70.8 |
| 2017-09-15 | **2.2** | calm | `calm` | 57.4 | 4.5 | 2.6 | 75.4 |
| 2018-02-05 | **89.3** | imminent | `active_crisis` | 40.2 | 28.4 | 11.4 | 30.0 |
| 2018-12-19 | **76.2** | critical | `pre_crisis` | 4.7 | 29.3 | 1.4 | 30.0 |
| 2018-12-24 | **88.7** | imminent | `active_crisis` | 43.1 | 24.6 | 10.6 | 9.0 |
| 2019-04-15 | **3.8** | calm | `calm` | 61.0 | 7.5 | 4.6 | 65.7 |
| 2019-08-05 | **87.7** | imminent | `active_crisis` | 47.0 | 18.0 | 8.5 | 45.0 |
| 2020-02-19 | **4.1** | calm | `calm` | 48.6 | 8.2 | 4.0 | 71.5 |
| 2020-02-21 | **13.7** | calm | `calm` | 37.1 | 27.3 | 10.1 | 65.0 |
| 2020-02-24 | **52.1** | elevated | `pre_crisis` | 15.0 | 28.2 | 4.2 | 65.0 |
| 2020-02-27 | **53.1** | elevated | `pre_crisis` | 15.0 | 29.1 | 4.4 | 65.0 |
| 2020-03-09 | **39.8** | mild | `pre_crisis` | 15.0 | 30.8 | 4.6 | 65.0 |
| 2020-03-16 | **88.6** | imminent | `active_crisis` | 31.4 | 24.3 | 7.6 | 9.0 |
| 2020-03-23 | **89.3** | imminent | `active_crisis` | 0.7 | 28.5 | 0.2 | 22.3 |
| 2021-05-15 | **3.8** | calm | `calm` | 43.9 | 7.5 | 3.3 | 55.5 |
| 2022-02-22 | **51.4** | elevated | `pre_crisis` | 15.0 | 27.5 | 4.1 | 65.0 |
| 2022-02-24 | **45.0** | elevated | `pre_crisis` | 27.3 | 19.5 | 5.3 | 22.3 |
| 2022-03-16 | **21.2** | mild | `pre_crisis` | 34.4 | 19.0 | 6.5 | 70.8 |
| 2022-06-15 | **91.6** | imminent | `active_crisis` | 5.1 | 44.2 | 2.3 | 30.0 |
| 2022-06-17 | **88.3** | imminent | `active_crisis` | 43.7 | 21.9 | 9.6 | 30.0 |
| 2022-09-26 | **90.9** | imminent | `active_crisis` | 2.7 | 39.3 | 1.1 | 9.0 |
| 2022-10-13 | **68.7** | critical | `pre_crisis` | 2.7 | 21.4 | 0.6 | 55.2 |
| 2023-03-08 | **4.1** | calm | `calm` | 44.6 | 8.2 | 3.7 | 65.0 |
| 2023-03-09 | **45.0** | elevated | `pre_crisis` | 23.8 | 20.1 | 4.8 | 22.3 |
| 2023-03-13 | **89.2** | imminent | `active_crisis` | 13.2 | 28.2 | 3.7 | 9.0 |
| 2023-03-15 | **89.2** | imminent | `active_crisis` | 3.0 | 28.2 | 0.8 | 13.2 |
| 2023-03-24 | **48.2** | elevated | `pre_crisis` | 12.3 | 19.5 | 2.4 | 71.9 |
| 2023-04-06 | **4.7** | calm | `calm` | 36.5 | 9.4 | 3.4 | 62.3 |
| 2023-10-09 | **12.2** | calm | `calm` | 25.7 | 24.5 | 6.3 | 71.7 |
| 2023-12-13 | **8.9** | calm | `calm` | 66.1 | 17.8 | 11.8 | 84.2 |
| 2024-04-15 | **31.5** | mild | `pre_crisis` | 53.9 | 26.7 | 14.4 | 66.7 |
| 2024-07-31 | **90.7** | imminent | `active_crisis` | 36.1 | 38.0 | 13.7 | 46.5 |
| 2024-08-01 | **36.0** | mild | `recovery` | 43.4 | 45.0 | 19.5 | 65.0 |
| 2024-08-02 | **89.5** | imminent | `active_crisis` | 36.3 | 30.3 | 11.0 | 30.0 |
| 2024-08-05 | **91.1** | imminent | `active_crisis` | 40.7 | 40.9 | 16.6 | 9.0 |
| 2024-08-14 | **17.7** | calm | `recovery` | 32.5 | 22.1 | 7.2 | 77.9 |
| 2024-08-23 | **6.6** | calm | `calm` | 35.0 | 13.2 | 4.6 | 73.8 |
| 2024-09-18 | **18.0** | calm | `recovery` | 52.5 | 22.5 | 11.8 | 91.7 |
| 2024-10-01 | **46.6** | elevated | `pre_crisis` | 25.0 | 39.7 | 9.9 | 36.7 |
| 2024-12-18 | **89.3** | imminent | `active_crisis` | 43.9 | 28.8 | 12.6 | 38.8 |
| 2025-06-15 | **20.0** | mild | `pre_crisis` | 47.3 | 16.5 | 7.8 | 64.3 |
| 2026-04-27 | **41.9** | elevated | `pre_crisis` | 61.2 | 34.8 | 21.3 | 83.9 |
| 2026-04-28 | **56.7** | elevated | `pre_crisis` | 69.9 | 46.0 | 32.2 | 81.0 |
| 2026-04-29 | **53.4** | elevated | `pre_crisis` | 64.0 | 44.0 | 28.2 | 82.0 |
| 2026-04-30 | **53.9** | elevated | `pre_crisis` | 41.9 | 47.3 | 19.8 | 76.7 |
| 2026-05-01 | **47.5** | elevated | `pre_crisis` | 70.2 | 38.5 | 27.0 | 84.3 |
| 2026-05-02 | **10.4** | calm | `calm` | 70.2 | 20.8 | 14.6 | 84.3 |
| 2026-05-03 | **10.4** | calm | `calm` | 70.2 | 20.8 | 14.6 | 84.3 |
| 2026-05-04 | **45.4** | elevated | `pre_crisis` | 47.2 | 39.2 | 18.5 | 80.0 |
| 2026-05-05 | **45.3** | elevated | `pre_crisis` | 46.8 | 39.2 | 18.3 | 91.7 |
| 2026-05-06 | **45.3** | elevated | `pre_crisis` | 47.3 | 39.1 | 18.5 | 100.0 |
| 2026-05-07 | **45.6** | elevated | `pre_crisis` | 50.1 | 39.1 | 19.6 | 91.7 |
| 2026-05-08 | **45.5** | elevated | `pre_crisis` | 53.3 | 38.6 | 20.6 | 100.0 |
| 2026-05-09 | **45.0** | elevated | `pre_crisis` | 39.9 | 26.2 | 10.5 | 80.0 |
| 2026-05-10 | **45.0** | elevated | `pre_crisis` | 39.7 | 24.8 | 9.8 | 80.0 |
| 2026-05-11 | **12.8** | calm | `calm` | 39.5 | 25.6 | 10.1 | 80.0 |
| 2026-05-12 | **44.7** | elevated | `pre_crisis` | 45.1 | 38.9 | 17.5 | 91.7 |
| 2026-05-13 | **44.7** | elevated | `pre_crisis` | 44.6 | 38.9 | 17.3 | 100.0 |
| 2026-05-14 | **44.4** | elevated | `pre_crisis` | 42.1 | 38.9 | 16.4 | 100.0 |
| 2026-05-15 | **29.2** | mild | `recovery` | 46.5 | 36.5 | 17.0 | 82.5 |

## 5. 핵심 발견 (Findings)

### 5.1 Phase 별 평균 fragility 단계적 분리

- **calm** 7.5 → **recovery** 23.5 → **pre_crisis** 46.1 → **active_crisis** 89.5

✅ **모노톤 증가 검증** — phase 별 fragility 가 정상 단계적 분리

### 5.2 Latent vector dominance

- Active crisis 의 top vector: **sector_dispersion** (17.3%)
- Pre-crisis 의 top vector: **carry_leverage** (29.8%)

