# Phase 12 Fragility Index — Batch History 시계열

**생성**: 2026-05-03 03:16  
**Sessions**: 45  
**Date range**: 2000-04-14 ~ 2026-05-01

## 1. Phase 분포 통계

| Phase | n | Avg fragility | Min | Max |
|---|---|---|---|---|
| **active_crisis** | 19 | 91.0 | 87.3 | 93.9 |
| **pre_crisis** | 15 | 54.3 | 33.3 | 82.9 |
| **recovery** | 5 | 28.7 | 18.6 | 34.1 |
| **calm** | 6 | 10.5 | 2.2 | 20.4 |

## 2. Phase 별 Top Latent Vectors (avg %)

### active_crisis

| Vector | Avg contribution |
|---|---|
| credit stress | 28.4% |
| policy uncertainty | 22.8% |
| geopolitical tail | 15.8% |
| catalyst density | 12.6% |
| sector dispersion | 9.5% |

### pre_crisis

| Vector | Avg contribution |
|---|---|
| credit stress | 25.2% |
| geopolitical tail | 19.6% |
| policy uncertainty | 19.2% |
| carry leverage | 10.1% |
| catalyst density | 9.5% |

### recovery

| Vector | Avg contribution |
|---|---|
| credit stress | 31.1% |
| policy uncertainty | 18.8% |
| em fx stress | 10.9% |
| sector dispersion | 10.5% |
| carry leverage | 10.4% |

### calm

| Vector | Avg contribution |
|---|---|
| credit stress | 31.2% |
| sector dispersion | 17.1% |
| policy uncertainty | 10.8% |
| em fx stress | 6.9% |
| geopolitical tail | 6.6% |

## 3. Crisis 직전 14일 fragility trajectory

- Active crisis sessions: **19**
- 14일 이내 prior session 가 있는 case: **6**

### Per-crisis trajectory

**2020-03-23** (crisis fragility 91.3):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-7 | 2020-03-16 | 89.9 | active_crisis |

**2022-06-17** (crisis fragility 88.9):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-2 | 2022-06-15 | 93.3 | active_crisis |

**2023-03-13** (crisis fragility 88.9):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-4 | 2023-03-09 | 45.0 | pre_crisis |

**2023-03-15** (crisis fragility 91.8):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-2 | 2023-03-13 | 88.9 | active_crisis |
| D-6 | 2023-03-09 | 45.0 | pre_crisis |

**2024-08-02** (crisis fragility 90.8):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-2 | 2024-07-31 | 93.1 | active_crisis |

**2024-08-05** (crisis fragility 91.5):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-3 | 2024-08-02 | 90.8 | active_crisis |
| D-5 | 2024-07-31 | 93.1 | active_crisis |

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
| 2019-04-15 | **8.0** | calm | `calm` | 61.0 | 16.0 | 9.8 | 65.7 |
| 2019-08-05 | **87.3** | imminent | `active_crisis` | 47.0 | 15.6 | 7.3 | 45.0 |
| 2020-02-19 | **33.3** | mild | `pre_crisis` | 48.6 | 28.7 | 13.9 | 71.5 |
| 2020-03-16 | **89.9** | imminent | `active_crisis` | 31.4 | 32.9 | 10.3 | 9.0 |
| 2020-03-23 | **91.3** | imminent | `active_crisis` | 0.7 | 41.7 | 0.3 | 22.3 |
| 2021-05-15 | **3.8** | calm | `calm` | 43.9 | 7.5 | 3.3 | 55.5 |
| 2022-02-24 | **45.0** | elevated | `pre_crisis` | 27.3 | 29.1 | 7.9 | 22.3 |
| 2022-03-16 | **52.1** | elevated | `pre_crisis` | 34.4 | 46.7 | 16.1 | 70.8 |
| 2022-06-15 | **93.3** | imminent | `active_crisis` | 5.1 | 55.3 | 2.8 | 30.0 |
| 2022-06-17 | **88.9** | imminent | `active_crisis` | 43.7 | 26.0 | 11.4 | 30.0 |
| 2022-10-13 | **58.9** | elevated | `pre_crisis` | 2.7 | 44.9 | 1.2 | 55.2 |
| 2023-03-09 | **45.0** | elevated | `pre_crisis` | 23.8 | 23.3 | 5.5 | 22.3 |
| 2023-03-13 | **88.9** | imminent | `active_crisis` | 13.2 | 25.8 | 3.4 | 9.0 |
| 2023-03-15 | **91.8** | imminent | `active_crisis` | 3.0 | 45.0 | 1.4 | 13.2 |
| 2023-03-24 | **53.0** | elevated | `pre_crisis` | 12.3 | 42.4 | 5.2 | 71.9 |
| 2023-04-06 | **14.4** | calm | `calm` | 36.5 | 28.8 | 10.5 | 62.3 |
| 2023-10-09 | **20.4** | mild | `calm` | 25.7 | 40.8 | 10.5 | 71.7 |
| 2023-12-13 | **33.8** | mild | `pre_crisis` | 66.1 | 27.7 | 18.3 | 84.2 |
| 2024-04-15 | **26.5** | mild | `recovery` | 53.9 | 33.1 | 17.8 | 66.7 |
| 2024-07-31 | **93.1** | imminent | `active_crisis` | 36.1 | 53.9 | 19.5 | 46.5 |
| 2024-08-02 | **90.8** | imminent | `active_crisis` | 36.3 | 38.5 | 14.0 | 30.0 |
| 2024-08-05 | **91.5** | imminent | `active_crisis` | 40.7 | 43.0 | 17.5 | 9.0 |
| 2024-08-14 | **32.6** | mild | `recovery` | 32.5 | 40.7 | 13.2 | 77.9 |
| 2024-08-23 | **14.4** | calm | `calm` | 35.0 | 28.9 | 10.1 | 73.8 |
| 2024-09-18 | **31.8** | mild | `recovery` | 52.5 | 39.8 | 20.9 | 91.7 |
| 2024-10-01 | **59.9** | elevated | `pre_crisis` | 25.0 | 52.0 | 13.0 | 36.7 |
| 2024-12-18 | **90.2** | imminent | `active_crisis` | 43.9 | 34.8 | 15.3 | 38.8 |
| 2025-06-15 | **58.2** | elevated | `pre_crisis` | 47.3 | 50.3 | 23.8 | 64.3 |
| 2026-04-27 | **54.3** | elevated | `pre_crisis` | 61.2 | 45.1 | 27.6 | 83.9 |
| 2026-04-28 | **69.8** | critical | `pre_crisis` | 69.9 | 56.6 | 39.6 | 81.0 |
| 2026-04-29 | **82.9** | imminent | `pre_crisis` | 64.0 | 68.3 | 43.7 | 82.0 |
| 2026-05-01 | **69.8** | critical | `pre_crisis` | 70.2 | 56.6 | 39.7 | 84.3 |

## 5. 핵심 발견 (Findings)

### 5.1 Phase 별 평균 fragility 단계적 분리

- **calm** 10.5 → **recovery** 28.7 → **pre_crisis** 54.3 → **active_crisis** 91.0

✅ **모노톤 증가 검증** — phase 별 fragility 가 정상 단계적 분리

### 5.2 Latent vector dominance

- Active crisis 의 top vector: **credit_stress** (28.4%)
- Pre-crisis 의 top vector: **credit_stress** (25.2%)

