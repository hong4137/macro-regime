# Macro Regime Dashboard

2-Agent Bayesian Fusion (Liquidity + Geopolitics) 기반 시장 regime 모니터링 대시보드.

## 🔗 Live Dashboard

→ **https://hong4137.github.io/macro-regime/**

매일 자동 갱신 (장 시작 전).

## 구조

- 2 Specialist agents (Liquidity v2, Geopolitics v2) → Fusion → Alert (GREEN/YELLOW/ELEVATED/RED)
- 7 Fusion Mechanisms (M1~M7): weighted avg, catalyst modifier, resonance, single-agent escalation, crisis override, renormalize, asymmetric hysteresis
- Tier 1 leading indicators: SAHM, SLOOS, USDJPY, KRE/SPY, HYG/IEF, SOFR-IORB, BREAKEVEN_10Y
- Catalyst Preprocessor: importance-weighted decay
- USEPU 5d/10d MA spike noise filter

## 검증 결과

- 8 episodes / 47 sessions backtest: ±0.4pp 정확
- 5 random OOS: **5/5 (100%) hit rate**
- Portfolio vs Buy-and-Hold SPY: +4.80% / DD -7.49pp

## 데이터

- `index.html` — daily dashboard (5분 자동 새로고침)
- `data/alert_history_live.jsonl` — 라이브 alert history
- `data/predictions.jsonl` — 예측 + T+1/T+3/T+5 채점 결과

## 라이선스

Personal research project. 투자 자문 아님.
