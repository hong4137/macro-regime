# Phase 12 Fragility Index — Batch History 시계열

**생성**: 2026-05-18 01:08  
**Sessions**: 127  
**Date range**: 2000-04-14 ~ 2026-05-16

## 1. Phase 분포 통계

| Phase | n | Avg fragility | Min | Max |
|---|---|---|---|---|
| **active_crisis** | 24 | 89.5 | 87.4 | 91.6 |
| **pre_crisis** | 53 | 44.4 | 20.0 | 76.2 |
| **recovery** | 6 | 23.5 | 14.2 | 36.0 |
| **calm** | 44 | 10.7 | 2.2 | 20.0 |

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
| sector dispersion | 37.9% |
| carry leverage | 21.9% |
| credit stress | 8.6% |
| bond vol stress | 5.6% |
| catalyst density | 5.2% |

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
| sector dispersion | 43.7% |
| carry leverage | 17.7% |
| credit stress | 13.8% |
| bond vol stress | 7.6% |
| catalyst density | 4.6% |

## 3. Crisis 직전 14일 fragility trajectory

- Active crisis sessions: **24**
- 14일 이내 prior session 가 있는 case: **14**

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

**2018-02-05** (crisis fragility 89.3):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-5 | 2018-01-31 | 7.7 | calm |

**2018-12-24** (crisis fragility 88.7):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-3 | 2018-12-21 | 41.9 | pre_crisis |
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

**2022-09-26** (crisis fragility 90.9):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-3 | 2022-09-23 | 51.5 | pre_crisis |
| D-13 | 2022-09-13 | 17.1 | calm |

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

**2024-12-18** (crisis fragility 89.3):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-14 | 2024-12-04 | 13.9 | calm |

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
| 2017-12-15 | **7.8** | calm | `calm` | 67.9 | 15.5 | 10.5 | 65.0 |
| 2017-12-29 | **7.8** | calm | `calm` | 54.5 | 15.5 | 8.4 | 65.0 |
| 2018-01-31 | **7.7** | calm | `calm` | 48.0 | 15.3 | 7.3 | 65.0 |
| 2018-02-05 | **89.3** | imminent | `active_crisis` | 40.2 | 28.4 | 11.4 | 30.0 |
| 2018-03-23 | **38.9** | mild | `pre_crisis` | 15.3 | 16.2 | 2.5 | 65.0 |
| 2018-06-29 | **8.4** | calm | `calm` | 39.0 | 16.9 | 6.6 | 65.0 |
| 2018-09-28 | **8.4** | calm | `calm` | 50.6 | 16.8 | 8.5 | 65.0 |
| 2018-10-10 | **35.4** | mild | `pre_crisis` | 19.1 | 19.8 | 3.8 | 65.0 |
| 2018-12-04 | **9.1** | calm | `calm` | 43.7 | 18.1 | 7.9 | 65.0 |
| 2018-12-19 | **76.2** | critical | `pre_crisis` | 4.7 | 29.3 | 1.4 | 30.0 |
| 2018-12-21 | **41.9** | elevated | `pre_crisis` | 15.0 | 18.5 | 2.8 | 65.0 |
| 2018-12-24 | **88.7** | imminent | `active_crisis` | 43.1 | 24.6 | 10.6 | 9.0 |
| 2019-04-15 | **3.8** | calm | `calm` | 61.0 | 7.5 | 4.6 | 65.7 |
| 2019-05-13 | **8.9** | calm | `calm` | 23.9 | 17.8 | 4.3 | 65.0 |
| 2019-08-05 | **87.7** | imminent | `active_crisis` | 47.0 | 18.0 | 8.5 | 45.0 |
| 2019-08-23 | **9.8** | calm | `calm` | 39.1 | 19.7 | 7.7 | 65.0 |
| 2019-10-04 | **8.9** | calm | `calm` | 38.3 | 17.8 | 6.8 | 65.0 |
| 2019-12-31 | **8.3** | calm | `calm` | 52.1 | 16.6 | 8.6 | 65.0 |
| 2020-01-31 | **8.2** | calm | `calm` | 27.3 | 16.3 | 4.4 | 65.0 |
| 2020-02-19 | **4.1** | calm | `calm` | 48.6 | 8.2 | 4.0 | 71.5 |
| 2020-02-21 | **13.7** | calm | `calm` | 37.1 | 27.3 | 10.1 | 65.0 |
| 2020-02-24 | **52.1** | elevated | `pre_crisis` | 15.0 | 28.2 | 4.2 | 65.0 |
| 2020-02-27 | **53.1** | elevated | `pre_crisis` | 15.0 | 29.1 | 4.4 | 65.0 |
| 2020-02-28 | **50.7** | elevated | `pre_crisis` | 15.0 | 26.9 | 4.0 | 65.0 |
| 2020-03-09 | **39.8** | mild | `pre_crisis` | 15.0 | 30.8 | 4.6 | 65.0 |
| 2020-03-16 | **88.6** | imminent | `active_crisis` | 31.4 | 24.3 | 7.6 | 9.0 |
| 2020-03-23 | **89.3** | imminent | `active_crisis` | 0.7 | 28.5 | 0.2 | 22.3 |
| 2020-04-30 | **9.5** | calm | `calm` | 55.0 | 19.0 | 10.4 | 65.0 |
| 2020-06-12 | **42.5** | elevated | `pre_crisis` | 15.0 | 19.0 | 2.9 | 65.0 |
| 2020-09-23 | **49.4** | elevated | `pre_crisis` | 15.0 | 25.6 | 3.8 | 65.0 |
| 2020-10-30 | **41.7** | elevated | `pre_crisis` | 15.0 | 18.3 | 2.7 | 65.0 |
| 2021-01-29 | **39.2** | mild | `pre_crisis` | 15.0 | 15.9 | 2.4 | 65.0 |
| 2021-03-04 | **42.7** | elevated | `pre_crisis` | 15.0 | 19.2 | 2.9 | 65.0 |
| 2021-05-15 | **3.8** | calm | `calm` | 43.9 | 7.5 | 3.3 | 55.5 |
| 2021-07-19 | **7.6** | calm | `calm` | 20.0 | 15.2 | 3.0 | 65.0 |
| 2021-09-20 | **38.9** | mild | `pre_crisis` | 15.0 | 15.6 | 2.3 | 65.0 |
| 2021-11-26 | **46.1** | elevated | `pre_crisis` | 15.0 | 22.5 | 3.4 | 65.0 |
| 2022-01-24 | **39.0** | mild | `pre_crisis` | 15.0 | 15.7 | 2.4 | 65.0 |
| 2022-02-22 | **51.4** | elevated | `pre_crisis` | 15.0 | 27.5 | 4.1 | 65.0 |
| 2022-02-24 | **45.0** | elevated | `pre_crisis` | 27.3 | 19.5 | 5.3 | 22.3 |
| 2022-03-16 | **21.2** | mild | `pre_crisis` | 34.4 | 19.0 | 6.5 | 70.8 |
| 2022-04-29 | **41.9** | elevated | `pre_crisis` | 15.0 | 32.8 | 4.9 | 65.0 |
| 2022-05-09 | **40.6** | elevated | `pre_crisis` | 15.0 | 31.5 | 4.7 | 65.0 |
| 2022-06-15 | **91.6** | imminent | `active_crisis` | 5.1 | 44.2 | 2.3 | 30.0 |
| 2022-06-17 | **88.3** | imminent | `active_crisis` | 43.7 | 21.9 | 9.6 | 30.0 |
| 2022-09-13 | **17.1** | calm | `calm` | 29.8 | 34.1 | 10.2 | 65.0 |
| 2022-09-23 | **51.5** | elevated | `pre_crisis` | 15.0 | 41.9 | 6.3 | 65.0 |
| 2022-09-26 | **90.9** | imminent | `active_crisis` | 2.7 | 39.3 | 1.1 | 9.0 |
| 2022-10-13 | **68.7** | critical | `pre_crisis` | 2.7 | 21.4 | 0.6 | 55.2 |
| 2022-11-10 | **14.0** | calm | `calm` | 57.9 | 28.0 | 16.2 | 65.0 |
| 2023-02-21 | **39.0** | mild | `pre_crisis` | 19.3 | 31.6 | 6.1 | 65.0 |
| 2023-03-08 | **4.1** | calm | `calm` | 44.6 | 8.2 | 3.7 | 65.0 |
| 2023-03-09 | **45.0** | elevated | `pre_crisis` | 23.8 | 20.1 | 4.8 | 22.3 |
| 2023-03-13 | **89.2** | imminent | `active_crisis` | 13.2 | 28.2 | 3.7 | 9.0 |
| 2023-03-15 | **89.2** | imminent | `active_crisis` | 3.0 | 28.2 | 0.8 | 13.2 |
| 2023-03-24 | **48.2** | elevated | `pre_crisis` | 12.3 | 19.5 | 2.4 | 71.9 |
| 2023-04-06 | **4.7** | calm | `calm` | 36.5 | 9.4 | 3.4 | 62.3 |
| 2023-05-04 | **16.1** | calm | `calm` | 39.4 | 32.3 | 12.7 | 65.0 |
| 2023-08-04 | **14.2** | calm | `calm` | 35.7 | 28.3 | 10.1 | 65.0 |
| 2023-09-21 | **15.9** | calm | `calm` | 29.9 | 31.9 | 9.5 | 65.0 |
| 2023-10-09 | **12.2** | calm | `calm` | 25.7 | 24.5 | 6.3 | 71.7 |
| 2023-10-26 | **20.0** | mild | `calm` | 23.6 | 40.0 | 9.4 | 65.0 |
| 2023-12-01 | **38.3** | mild | `pre_crisis` | 59.1 | 32.0 | 18.9 | 65.0 |
| 2023-12-13 | **8.9** | calm | `calm` | 66.1 | 17.8 | 11.8 | 84.2 |
| 2024-02-13 | **39.6** | mild | `pre_crisis` | 48.7 | 34.1 | 16.6 | 65.0 |
| 2024-04-15 | **31.5** | mild | `pre_crisis` | 53.9 | 26.7 | 14.4 | 66.7 |
| 2024-04-19 | **18.8** | calm | `calm` | 27.6 | 37.5 | 10.3 | 65.0 |
| 2024-06-12 | **42.0** | elevated | `pre_crisis` | 70.6 | 34.0 | 24.0 | 65.0 |
| 2024-07-31 | **90.7** | imminent | `active_crisis` | 36.1 | 38.0 | 13.7 | 46.5 |
| 2024-08-01 | **36.0** | mild | `recovery` | 43.4 | 45.0 | 19.5 | 65.0 |
| 2024-08-02 | **89.5** | imminent | `active_crisis` | 36.3 | 30.3 | 11.0 | 30.0 |
| 2024-08-05 | **91.1** | imminent | `active_crisis` | 40.7 | 40.9 | 16.6 | 9.0 |
| 2024-08-14 | **17.7** | calm | `recovery` | 32.5 | 22.1 | 7.2 | 77.9 |
| 2024-08-23 | **6.6** | calm | `calm` | 35.0 | 13.2 | 4.6 | 73.8 |
| 2024-09-06 | **12.1** | calm | `calm` | 20.2 | 24.1 | 4.9 | 65.0 |
| 2024-09-18 | **18.0** | calm | `recovery` | 52.5 | 22.5 | 11.8 | 91.7 |
| 2024-10-01 | **46.6** | elevated | `pre_crisis` | 25.0 | 39.7 | 9.9 | 36.7 |
| 2024-10-30 | **44.7** | elevated | `pre_crisis` | 33.8 | 40.2 | 13.6 | 65.0 |
| 2024-11-04 | **46.6** | elevated | `pre_crisis` | 23.8 | 40.3 | 9.6 | 65.0 |
| 2024-11-05 | **49.0** | elevated | `pre_crisis` | 32.6 | 44.2 | 14.4 | 65.0 |
| 2024-11-15 | **17.7** | calm | `calm` | 35.0 | 35.4 | 12.4 | 65.0 |
| 2024-12-04 | **13.9** | calm | `calm` | 59.2 | 27.8 | 16.5 | 65.0 |
| 2024-12-18 | **89.3** | imminent | `active_crisis` | 43.9 | 28.8 | 12.6 | 38.8 |
| 2025-01-15 | **40.3** | elevated | `pre_crisis` | 50.8 | 34.5 | 17.5 | 65.0 |
| 2025-02-19 | **13.7** | calm | `calm` | 56.2 | 27.3 | 15.3 | 65.0 |
| 2025-03-14 | **14.2** | calm | `calm` | 23.3 | 28.5 | 6.6 | 65.0 |
| 2025-04-15 | **14.4** | calm | `calm` | 55.0 | 28.8 | 15.8 | 65.0 |
| 2025-06-15 | **20.0** | mild | `pre_crisis` | 47.3 | 16.5 | 7.8 | 64.3 |
| 2025-07-15 | **13.0** | calm | `calm` | 45.2 | 26.0 | 11.8 | 65.0 |
| 2025-09-15 | **11.1** | calm | `calm` | 61.0 | 22.1 | 13.5 | 65.0 |
| 2025-11-15 | **14.7** | calm | `calm` | 39.6 | 29.4 | 11.6 | 65.0 |
| 2025-12-15 | **14.9** | calm | `calm` | 40.5 | 29.9 | 12.1 | 65.0 |
| 2026-04-26 | **5.4** | calm | `calm` | 43.3 | 10.8 | 4.7 | 65.0 |
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
| 2026-05-16 | **37.5** | mild | `pre_crisis` | 45.2 | 32.6 | 14.7 | 65.0 |

## 5. 핵심 발견 (Findings)

### 5.1 Phase 별 평균 fragility 단계적 분리

- **calm** 10.7 → **recovery** 23.5 → **pre_crisis** 44.4 → **active_crisis** 89.5

✅ **모노톤 증가 검증** — phase 별 fragility 가 정상 단계적 분리

### 5.2 Latent vector dominance

- Active crisis 의 top vector: **sector_dispersion** (17.3%)
- Pre-crisis 의 top vector: **sector_dispersion** (37.9%)

