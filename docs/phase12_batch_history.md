# Phase 12 Fragility Index — Batch History 시계열

**생성**: 2026-05-13 02:49  
**Sessions**: 49  
**Date range**: 2000-04-14 ~ 2026-05-07

## 1. Phase 분포 통계

| Phase | n | Avg fragility | Min | Max |
|---|---|---|---|---|
| **active_crisis** | 19 | 91.0 | 87.3 | 93.9 |
| **pre_crisis** | 19 | 60.0 | 33.5 | 82.9 |
| **recovery** | 4 | 27.4 | 18.6 | 34.1 |
| **calm** | 7 | 8.8 | 2.2 | 13.7 |

## 2. Phase 별 Top Latent Vectors (avg %)

### active_crisis

| Vector | Avg contribution |
|---|---|
| credit stress | 29.9% |
| policy uncertainty | 20.1% |
| geopolitical tail | 16.0% |
| catalyst density | 12.5% |
| em fx stress | 8.4% |

### pre_crisis

| Vector | Avg contribution |
|---|---|
| policy uncertainty | 20.3% |
| credit stress | 19.9% |
| geopolitical tail | 19.6% |
| carry leverage | 13.7% |
| em fx stress | 12.9% |

### recovery

| Vector | Avg contribution |
|---|---|
| credit stress | 36.1% |
| policy uncertainty | 15.2% |
| catalyst density | 12.5% |
| em fx stress | 11.9% |
| geopolitical tail | 11.5% |

### calm

| Vector | Avg contribution |
|---|---|
| credit stress | 35.6% |
| sector dispersion | 11.9% |
| em fx stress | 11.7% |
| catalyst density | 10.0% |
| geopolitical tail | 8.8% |

## 3. Crisis 직전 14일 fragility trajectory

- Active crisis sessions: **19**
- 14일 이내 prior session 가 있는 case: **6**

### Per-crisis trajectory

**2020-03-23** (crisis fragility 92.6):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-7 | 2020-03-16 | 90.3 | active_crisis |

**2022-06-17** (crisis fragility 88.9):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-2 | 2022-06-15 | 93.3 | active_crisis |

**2023-03-13** (crisis fragility 89.5):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-4 | 2023-03-09 | 45.0 | pre_crisis |

**2023-03-15** (crisis fragility 91.1):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-2 | 2023-03-13 | 89.5 | active_crisis |
| D-6 | 2023-03-09 | 45.0 | pre_crisis |

**2024-08-02** (crisis fragility 90.4):

| D- | Date | Fragility | Phase |
|---|---|---|---|
| D-2 | 2024-07-31 | 91.7 | active_crisis |

**2024-08-05** (crisis fragility 91.4):

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
| 2019-04-15 | **8.0** | calm | `calm` | 61.0 | 16.0 | 9.8 | 65.7 |
| 2019-08-05 | **87.3** | imminent | `active_crisis` | 47.0 | 15.6 | 7.3 | 45.0 |
| 2020-02-19 | **10.1** | calm | `calm` | 48.6 | 20.1 | 9.8 | 71.5 |
| 2020-03-16 | **90.3** | imminent | `active_crisis` | 31.4 | 35.2 | 11.1 | 9.0 |
| 2020-03-23 | **92.6** | imminent | `active_crisis` | 0.7 | 50.6 | 0.4 | 22.3 |
| 2021-05-15 | **3.8** | calm | `calm` | 43.9 | 7.5 | 3.3 | 55.5 |
| 2022-02-24 | **45.0** | elevated | `pre_crisis` | 27.3 | 29.1 | 7.9 | 22.3 |
| 2022-03-16 | **52.1** | elevated | `pre_crisis` | 34.4 | 46.7 | 16.1 | 70.8 |
| 2022-06-15 | **93.3** | imminent | `active_crisis` | 5.1 | 55.3 | 2.8 | 30.0 |
| 2022-06-17 | **88.9** | imminent | `active_crisis` | 43.7 | 26.0 | 11.4 | 30.0 |
| 2022-10-13 | **58.9** | elevated | `pre_crisis` | 2.7 | 44.9 | 1.2 | 55.2 |
| 2023-03-09 | **45.0** | elevated | `pre_crisis` | 23.8 | 22.8 | 5.4 | 22.3 |
| 2023-03-13 | **89.5** | imminent | `active_crisis` | 13.2 | 30.1 | 4.0 | 9.0 |
| 2023-03-15 | **91.1** | imminent | `active_crisis` | 3.0 | 40.7 | 1.2 | 13.2 |
| 2023-03-24 | **45.0** | elevated | `pre_crisis` | 12.3 | 34.0 | 4.2 | 71.9 |
| 2023-04-06 | **11.7** | calm | `calm` | 36.5 | 23.3 | 8.5 | 62.3 |
| 2023-10-09 | **49.5** | elevated | `pre_crisis` | 25.7 | 43.6 | 11.2 | 71.7 |
| 2023-12-13 | **13.7** | calm | `calm` | 66.1 | 27.4 | 18.1 | 84.2 |
| 2024-04-15 | **33.5** | mild | `pre_crisis` | 53.9 | 28.4 | 15.3 | 66.7 |
| 2024-07-31 | **91.7** | imminent | `active_crisis` | 36.1 | 44.6 | 16.1 | 46.5 |
| 2024-08-02 | **90.4** | imminent | `active_crisis` | 36.3 | 36.2 | 13.1 | 30.0 |
| 2024-08-05 | **91.4** | imminent | `active_crisis` | 40.7 | 42.6 | 17.3 | 9.0 |
| 2024-08-14 | **25.1** | mild | `recovery` | 32.5 | 31.4 | 10.2 | 77.9 |
| 2024-08-23 | **12.1** | calm | `calm` | 35.0 | 24.1 | 8.4 | 73.8 |
| 2024-09-18 | **31.8** | mild | `recovery` | 52.5 | 39.8 | 20.9 | 91.7 |
| 2024-10-01 | **59.9** | elevated | `pre_crisis` | 25.0 | 52.0 | 13.0 | 36.7 |
| 2024-12-18 | **90.6** | imminent | `active_crisis` | 43.9 | 37.5 | 16.5 | 38.8 |
| 2025-06-15 | **56.6** | elevated | `pre_crisis` | 47.3 | 48.9 | 23.1 | 64.3 |
| 2026-04-27 | **54.3** | elevated | `pre_crisis` | 61.2 | 45.1 | 27.6 | 83.9 |
| 2026-04-28 | **69.8** | critical | `pre_crisis` | 69.9 | 56.6 | 39.6 | 81.0 |
| 2026-04-29 | **82.9** | imminent | `pre_crisis` | 64.0 | 68.3 | 43.7 | 82.0 |
| 2026-05-01 | **69.8** | critical | `pre_crisis` | 70.2 | 56.6 | 39.7 | 84.3 |
| 2026-05-04 | **82.6** | imminent | `pre_crisis` | 47.2 | 71.4 | 33.7 | 80.0 |
| 2026-05-05 | **82.5** | imminent | `pre_crisis` | 46.8 | 71.4 | 33.4 | 91.7 |
| 2026-05-06 | **76.8** | critical | `pre_crisis` | 47.3 | 66.3 | 31.4 | 100.0 |
| 2026-05-07 | **77.4** | critical | `pre_crisis` | 50.1 | 66.3 | 33.2 | 91.7 |

## 5. 핵심 발견 (Findings)

### 5.1 Phase 별 평균 fragility 단계적 분리

- **calm** 8.8 → **recovery** 27.4 → **pre_crisis** 60.0 → **active_crisis** 91.0

✅ **모노톤 증가 검증** — phase 별 fragility 가 정상 단계적 분리

### 5.2 Latent vector dominance

- Active crisis 의 top vector: **credit_stress** (29.9%)
- Pre-crisis 의 top vector: **policy_uncertainty** (20.3%)

