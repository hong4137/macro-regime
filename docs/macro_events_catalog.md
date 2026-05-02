# 매크로 이벤트 캘린더 — 자동 수집 + 대시보드 가이드

**용도**: 별도 프로젝트의 매크로 스케줄 dashboard 구축용 self-contained 자료
**작성일**: 2026-05-03
**범위**: 시장에 영향을 미치는 모든 정형/비정형 catalyst + 자동 수집 로직

---

## 0. 사용 가이드

이 문서는 다른 프로젝트에서 매크로 이벤트 캘린더 dashboard 를 만들 때 그대로 가져갈 수 있게 작성됨. 다음 순서로 활용:

1. **Section 1-3**: 어떤 이벤트를 추적할지 결정 (카테고리 + 우선순위)
2. **Section 4**: 무엇을 어떻게 수집할지 (데이터 소스 + API)
3. **Section 5**: 자동 수집 코드 template (catalyst_calendar.py)
4. **Section 6**: dashboard 시각화 컴포넌트 (HTML + JSON schema)
5. **Section 7**: 무료 vs 유료 ROI

---

## 1. 13개 이벤트 카테고리 (importance ★)

### 1.1 통화정책 (★★★★★) — 가장 강력한 mover

| 이벤트 | 빈도 | 시간(ET) | 영향 자산 |
|---|---|---|---|
| **Fed FOMC 회의 + 성명** | 8회/년 (1·3·5·6·7·9·11·12 주요 수요일) | 14:00 | VIX, USD, yields, equities 전반 |
| **Fed 의장 기자회견** | FOMC 직후 | 14:30 | 즉각 단기 변동성 |
| **Fed Dot Plot** | 분기 (3·6·9·12) | FOMC 자료 | rate path 기대 |
| **FOMC Minutes** | FOMC 3주 후 | 14:00 | 정책 detail |
| **Beige Book** | FOMC 2주 전 | 14:00 | 지역 경기 |
| **잭슨홀 심포지엄** | 8월 마지막 목-토 | 다양 | 정책 시그널 |
| **SLOOS** (lending standards) | 분기 (1·4·7·10) | various | 신용 환경 |
| **BoJ 회의 + YCC** | 8회/년 | 11:00 JST | USDJPY 캐리 |
| **ECB 회의** | 8회/년 (목) | 13:45 CET | EUR, EU yields |
| **BoE 회의** | 8회/년 (목) | 12:00 BST | GBP |
| **PBoC LPR** | 매월 20일 | 09:30 CST | CNY, EM |
| **Fed governor speeches** | 수시 (week-day) | 다양 | 단기 변동 |

### 1.2 미국 거시 지표 (★★★★)

| 이벤트 | 빈도 | 시간(ET) | 비고 |
|---|---|---|---|
| **NFP (고용)** | 매월 첫 금 | 8:30 | 시장 가장 큰 mover |
| **CPI** | 매월 중순 화-목 | 8:30 | Fed 가장 중요 |
| **Core CPI** | CPI 와 동일 | Fed 핵심 지표 |
| **PPI** | CPI 1일 후 | 8:30 | 생산자 물가 |
| **PCE / Core PCE** | 매월 말 | 8:30 | Fed 선호 인플레 |
| **GDP advance/prelim/final** | 분기 (3 releases) | 8:30 | 가장 정확 |
| **ISM PMI 제조** | 매월 1-3일 | 10:00 | 경기 leading |
| **ISM PMI 서비스** | PMI 제조 + 2일 | 10:00 | 70% 경제 |
| **Retail Sales** | 매월 중순 | 8:30 | 소비 |
| **Initial Claims** | 매주 목 | 8:30 | 고용 leading |
| **JOLTS** | 매월 중순 화 | 10:00 | 고용 detail |
| **ADP Employment** | NFP 2일 전 수 | 8:15 | NFP preview |
| **Michigan Sentiment** | 매월 prelim+final | 10:00 | 소비 심리 |
| **Conference Board CCI** | 매월 마지막 화 | 10:00 | 소비 신뢰 |
| **Atlanta Fed GDPNow** | 수시 (real-time) | n/a | nowcast |
| **Building Permits / Housing Starts** | 매월 중순 | 8:30 | 부동산 |
| **Existing Home Sales** | 매월 중순 | 10:00 | 부동산 |
| **Trade Balance** | 매월 첫 화 | 8:30 | 글로벌 흐름 |
| **PPI / Import Price Index** | 매월 | 8:30 | 인플레 leading |

### 1.3 글로벌 거시 (★★★)

| 이벤트 | 빈도 | 비고 |
|---|---|---|
| **중국 NBS PMI** | 매월 1일 | 정부 발표 |
| **중국 Caixin PMI** | NBS + 5일 | 민간 (small biz 강한 weight) |
| **중국 무역수지** | 매월 7일 | export/import |
| **중국 CPI / PPI** | 매월 9일 | 글로벌 인플레 |
| **유럽 IFO** | 매월 25일 | 독일 기업심리 |
| **유럽 ZEW** | 매월 셋째 화 | 투자자 심리 |
| **일본 Tankan** | 분기 (4·7·10·12) | 대기업 심리 |
| **OPEC+ meeting** | 매월 첫 일 (or 수시) | 유가 |
| **호주 RBA** | 11회/년 | AUD, 위험자산 proxy |
| **캐나다 BoC** | 8회/년 | CAD, 원자재 |

### 1.4 빅테크 실적 (★★★★) — Magnificent 7

| 기업 | 분기 실적 timing | 별도 이벤트 |
|---|---|---|
| **AAPL** | Late 1·4·7·10월 | WWDC (6월 첫주), iPhone event (9월) |
| **MSFT** | 분기말 +1개월 | Build (5월), Ignite (11월) |
| **GOOGL** | 분기말 +1개월 | **I/O (5월 둘째 화)** ★ |
| **AMZN** | 2·4·7·10월 | re:Invent (12월), Prime Day (7월) |
| **META** | 1·4·7·10월 | Connect (9-10월) |
| **NVDA** | 2·5·8·11월 | **GTC (3월 셋째 주)** ★ |
| **TSLA** | 1·4·7·10월 | Investor Day, delivery (분기 첫날) |

**기타 시장 mover**:
- JPM/BAC/C/WFC/GS (earnings season 첫주)
- Netflix (보통 첫주, growth bellwether)
- Berkshire Hathaway (Buffett 연차 5월 첫 토)
- Bitcoin halving (4년마다, 약 4월)

### 1.5 Tech 컨퍼런스 / 제품 발표 (★★★)

| 이벤트 | 시기 (대략) | 영향 |
|---|---|---|
| CES 라스베가스 | 1월 첫 화-금 | 가전 + AI 트렌드 |
| MWC 바르셀로나 | 2월 마지막 주 | 모바일/통신 |
| **NVIDIA GTC** | 3월 셋째 주 | AI 인프라 ★ |
| Google Cloud Next | 4월 둘째 주 | 클라우드 |
| Microsoft Build | 5월 둘째 주 | 개발자 생태계 |
| **Google I/O** | 5월 둘째 화-수 | Android + AI ★ |
| **Apple WWDC** | 6월 첫 월-금 | iOS + Vision ★ |
| Apple "September event" | 9월 둘째 화 | iPhone (AAPL stock 큰 mover) |
| Meta Connect | 9-10월 | VR/AR |
| **OpenAI DevDay** | 11월 첫 주 | GenAI ★ |
| AWS re:Invent | 11월 마지막 주 | AMZN cloud |
| **NeurIPS** | 12월 둘째 주 | AI 학계 |
| ICLR | 4-5월 | AI 학계 |
| ICML | 7월 | AI 학계 |

### 1.6 정치/선거 (★★★★)

| 이벤트 | 빈도 | 영향 |
|---|---|---|
| 미 대선 | 4년 (11월 첫 화) | 정책 대전환 |
| 미 중간선거 | 2년 | 의회 권력 |
| 미 의회 (debt ceiling, CR, shutdown) | 수시 | 정부 기능 |
| **트럼프/Biden 관세 발표** | 수시 high impact | 무역 |
| G7 정상회담 | 6월 | 글로벌 정책 |
| G20 정상회담 | 11월 | 글로벌 정책 |
| 미중 정상회담 | 수시 | 무역/지정학 |
| 유엔 총회 (UNGA) | 9월 셋째 화 | 외교 |
| **다보스 포럼 (WEF)** | 1월 셋째 주 | 정·재계 |
| 국정연설 (SOTU) | 1월/2월 | 정책 방향 |
| Fed 의장 재임명 청문 | 4년 | Fed 인사 |

### 1.7 지정학 tail risk (★★★★★) — real-time

| 이벤트 type | 키워드 (영문 + 한글) |
|---|---|
| **이란-이스라엘** | "Iran missile", "Hezbollah", "Houthis", "이란", "escalation" |
| **러시아-우크라이나** | "Russia", "Ukraine", "ceasefire", "Putin", "sanctions" |
| **중국-대만** | "PLA exercise", "Taiwan strait", "PLA", "남중국해" |
| **북한 미사일** | "DPRK", "ICBM", "missile launch", "북한" |
| **미중 관세** | "tariff", "Section 301", "Section 232", "export ban" |
| **반도체 통제** | "CHIPS Act", "export control", "Huawei", "SMIC" |
| **사이버 공격** | "ransomware", "infrastructure attack", "SCADA" |
| **테러** | "terrorist attack", "explosion", "active shooter" |

### 1.8 시장 구조 (Quant flows) (★★★)

| 이벤트 | 빈도 | 영향 |
|---|---|---|
| **Triple Witching** | 분기 (3·6·9·12 셋째 금) | 단기 변동성 spike |
| **Quadruple Witching** | 동일 | 옵션·선물·지수옵션·single-stock 동시 만기 |
| **Monthly Opex** | 매월 셋째 금 | 옵션 만기 |
| **MSCI rebalance** | 분기 (2·5·8·11) | passive 자금 이동 |
| **Russell rebalance** | 6월 마지막 금 | 소형주 |
| **VIX 만기** | 매월 셋째 수 | volatility 거래 |
| **CME FX rollover** | 분기 셋째 수 | FX 선물 |

### 1.9 채권 시장 (★★★)

| 이벤트 | 빈도 | 비고 |
|---|---|---|
| **2Y 국채 입찰** | 매월 마지막 화 | short-end |
| **5Y 국채 입찰** | 매월 마지막 수 | mid-curve |
| **10Y 국채 입찰** | 매월 둘째 수 | long-end ★ |
| **30Y 국채 입찰** | 매월 둘째 목 | long-end ★ |
| **TIPS 입찰** | 분기 (1·4·7·10) | inflation hedge |
| **Treasury Quarterly Refunding** | 분기 (1·4·7·10 첫 수) | Treasury 정책 ★ |
| **Fed H.4.1 (Balance Sheet)** | 매주 목 16:30 | QT 추적 |
| **TGA 잔고** | daily | 단기 유동성 |
| **Reverse Repo balance** | daily 16:00 | RRP |

### 1.10 신용 / 은행 (★★★)

| 이벤트 | 빈도 | 비고 |
|---|---|---|
| **Bank earnings season 첫주** | 분기 | JPM/BAC/C/WFC |
| **Fed Stress Test 결과** | 매년 6월 마지막 목 | 자본 적정성 |
| **CCAR 결과** | 분기 | 은행 capital plan |
| **Basel 규제 변경** | 수시 | 신중 모니터링 |
| **FDIC quarterly bank report** | 분기 | 부실 은행 |

### 1.11 자금 흐름 / 포지셔닝 (★★★★)

| 이벤트 | 빈도 | 데이터 |
|---|---|---|
| **CFTC COT** | 매주 금 15:30 | 헤지펀드 포지션 |
| **AAII Sentiment** | 매주 수 17:00 | 개인투자자 |
| **NAAIM** | 매주 목 16:00 | active 매니저 |
| **ETF flows** | daily 18:00 | passive 흐름 |
| **EPFR Global Funds** | 매주 (paid) | 글로벌 fund flow |
| **Put/Call ratio (CBOE)** | daily 17:00 | 옵션 포지션 |
| **VIX Term Structure** | daily | vol 곡선 |

### 1.12 M&A / IPO / 미시 (★★)

| 이벤트 |
|---|
| 메가 M&A (>$50B) |
| 빅 IPO (Stripe/SpaceX/ByteDance 같은 unicorn) |
| 큰 buyback announcement |
| Spin-off (Honeywell, GE 등) |
| 큰 corporate guidance cut/raise |
| 큰 dividend cut |

### 1.13 자연재해 / 공급망 (★★)

| 이벤트 |
|---|
| 허리케인 시즌 (8-10월, 미 정유시설) |
| 대만 지진 (반도체 공급망) |
| 일본 지진 (자동차 부품) |
| 수에즈 운하 차단 |
| 파나마 운하 가뭄 |
| Houthis 홍해 공격 |
| 큰 사이버 공격 (콜로니얼 파이프라인 같은) |

---

## 2. importance 등급 시스템 (1-5)

| Level | 정의 | 예시 |
|---|---|---|
| **5** | tier-1 — 시장 전체 mover | FOMC, NFP, CPI, 빅테크 실적, 대선, 잭슨홀 |
| **4** | tier-2 — 큰 단기 변동 | PMI, GDP, BoJ, GTC, iPhone event, OPEC+ |
| **3** | tier-3 — sector mover | Tech 컨퍼런스, Bank earnings, Treasury auction, Initial claims |
| **2** | tier-4 — niche | regional Fed speeches, mid-tier 데이터, MBS auctions |
| **1** | tier-5 — informational | minor data, second-tier earnings |

---

## 3. 우선순위 매트릭스 (작업량 vs 가치)

| Tier | 작업량 | 가치 | 권장 |
|---|---|---|---|
| **A** (FOMC + NFP + CPI + 빅테크 7개) | 1 day | ★★★★★ | 즉시 |
| **B** (PMI, GDP, Treasury auction, Tech 컨퍼런스) | 3 days | ★★★★ | 1주 |
| **C** (글로벌 PMI, 정치 일정, 채권 입찰 detail) | 1 week | ★★★ | 2주 |
| **D** (지정학 keyword detection — news scraping) | 2 weeks | ★★★★ | 별도 |
| **E** (M&A, IPO, sector earnings) | 1 month | ★★ | 후속 |

---

## 4. 데이터 소스 (무료 우선)

### 4.1 공식 RSS / API (모두 무료)

| 소스 | URL | 카테고리 | 형식 |
|---|---|---|---|
| **Federal Reserve** | https://www.federalreserve.gov/feeds/press_monetary.xml | FOMC, statements | RSS |
| **Federal Reserve speeches** | https://www.federalreserve.gov/feeds/speeches.xml | governor speeches | RSS |
| **BLS** | https://www.bls.gov/feed/news_release/lifeapdf.rss | NFP, CPI, PPI | RSS |
| **BEA** | https://www.bea.gov/rss/rss.xml | GDP, PCE, trade | RSS |
| **Treasury Direct** | https://www.treasurydirect.gov/RSS/upcoming.xml | 입찰 일정 | RSS |
| **BoJ** | https://www.boj.or.jp/en/whatsnew/news_xml.xml | 일본 통화정책 | RSS |
| **ECB** | https://www.ecb.europa.eu/rss/press.html | 유럽 중앙은행 | RSS |
| **BoE** | https://www.bankofengland.co.uk/rss/news | 영국 통화정책 | RSS |
| **OECD** | https://www.oecd.org/newsroom/feeds/news.xml | 글로벌 거시 | RSS |
| **IMF** | https://www.imf.org/en/News/Rss-feeds | 거시 | RSS |
| **Atlanta Fed GDPNow** | https://www.atlantafed.org/cqer/research/gdpnow.aspx | nowcast | scrape |
| **CFTC COT** | https://www.cftc.gov/MarketReports/CommitmentsofTraders/index.htm | positioning | XML |
| **CBOE** | https://www.cboe.com/us/options/market_statistics/ | options flow | scrape |

### 4.2 무료 API (제한 있음)

| 서비스 | 용도 | 한계 |
|---|---|---|
| **yfinance** (Python) | earnings calendar, prices | rate limit, free |
| **Investing.com economic calendar** | 모든 거시 일정 | scraping 필요 |
| **NewsAPI** | 뉴스 keyword scan | 100 req/day free |
| **Reuters RSS** | 뉴스 | 무료 |
| **Financial Times RSS** | 뉴스 | 무료 |
| **MarketWatch RSS** | 뉴스 | 무료 |
| **Twitter / X API** | 실시간 발언 | basic tier 1500 read/month |
| **Reddit r/investing** | sentiment proxy | 무료 |

### 4.3 유료 (high value, optional)

| 서비스 | 비용/년 | 용도 |
|---|---|---|
| **Bloomberg Terminal** | $30K | 모든 것, real-time |
| **Refinitiv Eikon** | $22K | 모든 것 |
| **FactSet** | $12K | earnings, 거시 |
| **Trading Economics** | $99/월 | 정형 거시 일정 + API |
| **Polygon.io** | $99/월 | 시장 데이터 + news |
| **Alpha Vantage** | $50/월 | 모든 것 (제한적) |
| **AlphaSense** | enterprise | sentiment + filings |

---

## 5. 자동 수집 코드 template

### 5.1 catalyst_calendar.py (메인 모듈)

```python
"""
catalyst_calendar.py — 매크로 이벤트 캘린더 자동 수집

사용:
  python catalyst_calendar.py --days 30
  python catalyst_calendar.py --json upcoming.json
  python catalyst_calendar.py --refresh
"""

from __future__ import annotations
import argparse
import json
from datetime import datetime, timedelta
from pathlib import Path
import xml.etree.ElementTree as ET

import requests
import feedparser
import yfinance as yf

OUTPUT_DIR = Path("./catalysts")
OUTPUT_DIR.mkdir(exist_ok=True)

# ════════════════════════════════════════════════════════════════
# 카테고리별 fetcher
# ════════════════════════════════════════════════════════════════

def fetch_fed_fomc() -> list[dict]:
    """Fed FOMC 일정 (federalreserve.gov RSS)"""
    rss = "https://www.federalreserve.gov/feeds/press_monetary.xml"
    feed = feedparser.parse(rss)
    events = []
    for entry in feed.entries[:20]:
        events.append({
            "title": entry.title,
            "date": entry.published,
            "url": entry.link,
            "category": "fed_fomc",
            "importance": 5,
            "source": "federalreserve.gov",
        })
    return events


def fetch_bls_releases() -> list[dict]:
    """BLS NFP/CPI/PPI 일정"""
    rss = "https://www.bls.gov/feed/news_release/lifeapdf.rss"
    feed = feedparser.parse(rss)
    return [{
        "title": e.title,
        "date": e.published,
        "url": e.link,
        "category": "bls_data",
        "importance": 5 if any(k in e.title for k in ["Employment", "CPI", "PPI"]) else 3,
        "source": "bls.gov",
    } for e in feed.entries[:30]]


def fetch_treasury_auctions() -> list[dict]:
    """Treasury 국채 입찰 일정"""
    rss = "https://www.treasurydirect.gov/RSS/upcoming.xml"
    feed = feedparser.parse(rss)
    return [{
        "title": e.title,
        "date": e.published,
        "url": e.link,
        "category": "treasury_auction",
        "importance": 3 if "10-Year" in e.title or "30-Year" in e.title else 2,
        "source": "treasurydirect.gov",
    } for e in feed.entries[:20]]


def fetch_earnings_calendar(tickers: list[str]) -> list[dict]:
    """yfinance earnings calendar"""
    events = []
    for ticker in tickers:
        try:
            t = yf.Ticker(ticker)
            cal = t.calendar
            if cal is not None and "Earnings Date" in cal:
                date = cal["Earnings Date"][0] if hasattr(cal["Earnings Date"], "__iter__") else cal["Earnings Date"]
                events.append({
                    "title": f"{ticker} Earnings",
                    "date": str(date),
                    "url": f"https://finance.yahoo.com/quote/{ticker}",
                    "category": "earnings",
                    "importance": 5 if ticker in ["AAPL", "MSFT", "GOOGL", "AMZN", "META", "NVDA", "TSLA"] else 3,
                    "source": "yfinance",
                })
        except Exception as e:
            continue
    return events


# ════════════════════════════════════════════════════════════════
# Hand-curated DB
# ════════════════════════════════════════════════════════════════

TECH_CONFERENCES = [
    {"title": "CES Las Vegas", "month": 1, "day": 7, "category": "tech_conf", "importance": 4},
    {"title": "MWC Barcelona", "month": 2, "day": 26, "category": "tech_conf", "importance": 3},
    {"title": "NVIDIA GTC", "month": 3, "day": 18, "category": "tech_conf", "importance": 5, "ticker": "NVDA"},
    {"title": "Google Cloud Next", "month": 4, "day": 9, "category": "tech_conf", "importance": 4, "ticker": "GOOGL"},
    {"title": "Microsoft Build", "month": 5, "day": 14, "category": "tech_conf", "importance": 4, "ticker": "MSFT"},
    {"title": "Google I/O", "month": 5, "day": 14, "category": "tech_conf", "importance": 5, "ticker": "GOOGL"},
    {"title": "Apple WWDC", "month": 6, "day": 9, "category": "tech_conf", "importance": 5, "ticker": "AAPL"},
    {"title": "Apple September Event (iPhone)", "month": 9, "day": 9, "category": "tech_conf", "importance": 5, "ticker": "AAPL"},
    {"title": "Meta Connect", "month": 9, "day": 25, "category": "tech_conf", "importance": 4, "ticker": "META"},
    {"title": "OpenAI DevDay", "month": 11, "day": 6, "category": "tech_conf", "importance": 5},
    {"title": "AWS re:Invent", "month": 12, "day": 2, "category": "tech_conf", "importance": 4, "ticker": "AMZN"},
    {"title": "NeurIPS", "month": 12, "day": 9, "category": "ai_conf", "importance": 3},
]

POLITICAL = [
    {"title": "G7 Summit", "month": 6, "category": "political", "importance": 4},
    {"title": "G20 Summit", "month": 11, "category": "political", "importance": 4},
    {"title": "UN General Assembly", "month": 9, "day": 17, "category": "political", "importance": 3},
    {"title": "World Economic Forum (Davos)", "month": 1, "day": 21, "category": "political", "importance": 4},
    {"title": "State of the Union", "month": 2, "category": "political", "importance": 3},
]

# Quad witching: 분기 셋째 금
def quad_witching_dates(year: int) -> list[dict]:
    events = []
    for m in [3, 6, 9, 12]:
        # 셋째 금 계산
        d = datetime(year, m, 1)
        while d.weekday() != 4:  # Friday = 4
            d += timedelta(days=1)
        d += timedelta(weeks=2)
        events.append({
            "title": "Quadruple Witching",
            "date": d.strftime("%Y-%m-%d"),
            "category": "market_structure",
            "importance": 3,
            "source": "calculated",
        })
    return events


# ════════════════════════════════════════════════════════════════
# Aggregator
# ════════════════════════════════════════════════════════════════

def fetch_all(days_ahead: int = 30) -> list[dict]:
    """모든 sources 통합 fetch."""
    events = []

    # 1. RSS feeds
    events.extend(fetch_fed_fomc())
    events.extend(fetch_bls_releases())
    events.extend(fetch_treasury_auctions())

    # 2. Earnings (Magnificent 7)
    mag7 = ["AAPL", "MSFT", "GOOGL", "AMZN", "META", "NVDA", "TSLA"]
    events.extend(fetch_earnings_calendar(mag7))

    # 3. Hand-curated tech conferences (this year)
    year = datetime.now().year
    for conf in TECH_CONFERENCES:
        date = datetime(year, conf["month"], conf.get("day", 15))
        if date >= datetime.now():
            events.append({**conf, "date": date.strftime("%Y-%m-%d"), "source": "curated"})

    # 4. Political events
    for ev in POLITICAL:
        date = datetime(year, ev["month"], ev.get("day", 15))
        if date >= datetime.now():
            events.append({**ev, "date": date.strftime("%Y-%m-%d"), "source": "curated"})

    # 5. Quad witching
    events.extend(quad_witching_dates(year))
    events.extend(quad_witching_dates(year + 1))

    # Filter: only future events within window
    cutoff = datetime.now() + timedelta(days=days_ahead)
    filtered = [
        e for e in events
        if datetime.fromisoformat(e["date"][:10] if "T" in e.get("date", "") else e.get("date", "1970-01-01"))
        <= cutoff
    ]

    # Sort by date
    filtered.sort(key=lambda e: e.get("date", ""))
    return filtered


def main():
    ap = argparse.ArgumentParser()
    ap.add_argument("--days", type=int, default=30)
    ap.add_argument("--json", default=str(OUTPUT_DIR / "upcoming.json"))
    args = ap.parse_args()

    events = fetch_all(args.days)
    with open(args.json, "w", encoding="utf-8") as f:
        json.dump(events, f, indent=2, ensure_ascii=False)

    print(f"\n[+] {len(events)} events saved to {args.json}\n")
    for e in events[:20]:
        print(f"  {e.get('date', '?'):12s}  importance {e.get('importance', '?')}  "
              f"{e.get('category', '?'):20s}  {e.get('title', '?')}")


if __name__ == "__main__":
    main()
```

### 5.2 dependency 설치

```bash
pip install feedparser requests yfinance
```

---

## 6. Dashboard 시각화 컴포넌트

### 6.1 JSON 출력 schema

```json
{
  "generated_at": "2026-05-03T01:50:00",
  "events": [
    {
      "title": "Fed FOMC Meeting + Statement",
      "date": "2026-05-12",
      "datetime_et": "14:00 ET",
      "category": "fed_fomc",
      "importance": 5,
      "source": "federalreserve.gov",
      "url": "https://www.federalreserve.gov/...",
      "ticker": null,
      "expected_impact": ["VIX", "USD", "yields", "equities"],
      "consensus": "+25bp"
    }
  ]
}
```

### 6.2 HTML 컴포넌트 sketch (vanilla JS)

```html
<div id="catalyst-feed" class="catalyst-grid">
  <div class="filter-bar">
    <label>최소 importance: <select id="min-imp">
      <option value="3">3+</option>
      <option value="4" selected>4+ (tier-2)</option>
      <option value="5">5 (tier-1만)</option>
    </select></label>
    <label>카테고리: <select id="category-filter">
      <option value="">전부</option>
      <option>fed_fomc</option>
      <option>bls_data</option>
      <option>earnings</option>
      <option>tech_conf</option>
    </select></label>
  </div>
  <ul id="event-list"></ul>
</div>

<style>
.catalyst-grid { background: #0b0f17; color: #e6edf7; padding: 20px; }
.event-card { padding: 12px; border-left: 4px solid; margin: 6px 0; background: #121826; }
.imp-5 { border-color: #ef4444; }
.imp-4 { border-color: #f97316; }
.imp-3 { border-color: #eab308; }
.imp-2 { border-color: #60a5fa; }
.event-date { font-size: 11px; color: #8fa3c0; }
.event-title { font-size: 14px; font-weight: 600; }
.event-meta { font-size: 11px; color: #8fa3c0; margin-top: 4px; }
.imminent { background: rgba(239, 68, 68, 0.05); }
</style>

<script>
fetch('catalysts/upcoming.json')
  .then(r => r.json())
  .then(data => {
    const list = document.getElementById('event-list');
    const today = new Date().toISOString().slice(0, 10);
    const minImp = parseInt(document.getElementById('min-imp').value);

    data.events
      .filter(e => e.importance >= minImp)
      .forEach(e => {
        const d = new Date(e.date);
        const daysAway = Math.ceil((d - new Date()) / 86400000);
        const imminent = daysAway <= 3 ? 'imminent' : '';

        list.innerHTML += `
          <li class="event-card imp-${e.importance} ${imminent}">
            <div class="event-date">📅 ${e.date}  (D+${daysAway})  ${e.datetime_et || ''}</div>
            <div class="event-title">${e.title}</div>
            <div class="event-meta">
              ${e.category} · importance ${e.importance} ·
              <a href="${e.url}" target="_blank">출처</a>
              ${e.ticker ? '· ' + e.ticker : ''}
            </div>
          </li>
        `;
      });
  });
</script>
```

### 6.3 핵심 view 4가지

1. **Today + 7d view** — 다음 1주일 imminent_high 이벤트 list
2. **Calendar view** — 월별 calendar 형식 (heatmap by importance)
3. **Category view** — Fed / BLS / Earnings / Tech 별 filter
4. **Ticker view** — 종목별 (AAPL, NVDA 등) 다음 catalyst

---

## 7. 비용 / ROI 평가

### 7.1 Free tier (무료, 권장 시작점)

**커버되는 것**:
- 모든 Fed/BLS/BEA/Treasury 공식 일정
- 빅테크 7개 + 주요 50개 기업 earnings
- Hand-curated tech 컨퍼런스 50+
- Quad witching, 정치 일정
- News RSS scraping (지정학)

**한계**:
- earnings detail (consensus EPS, surprise history) 없음
- 글로벌 거시 일정 부분적
- 실시간 news flash 부분적

**예상 가치**: 80% 의 매크로 catalyst 커버

### 7.2 Trading Economics ($99/월) — 권장 첫 유료

**추가되는 것**:
- 모든 글로벌 거시 일정 (60+ 국)
- 정확한 시간 (분 단위)
- consensus + previous + actual 자동 비교
- API 접근 (programmatic)

**ROI**: ★★★★ (글로벌 매크로 커버 시 필수)

### 7.3 Polygon.io ($99/월) — earnings + 시장 데이터

**추가되는 것**:
- earnings detail (consensus, surprise, guidance)
- corporate filings (8-K)
- 시장 데이터 + news 통합
- Real-time WebSocket

**ROI**: ★★★ (개별 종목 영향 추적 시)

### 7.4 Bloomberg ($30K/년) — full enterprise

**추가되는 것**: 모든 것 + real-time

**ROI**: ★★ (개인/소규모 팀에 over-spec)

---

## 8. 실전 권장 stack

### 8.1 개인 / 소규모 (Free)

```
├── catalyst_calendar.py        # 본 문서 Section 5 코드
├── tech_calendar.json          # Hand-curated (분기 update)
├── upcoming.json               # daily refresh output
└── dashboard/index.html        # 본 문서 Section 6 viz
```

**갱신 빈도**: daily cron (06:00 KST)

### 8.2 중급 ($99-200/월)

위 stack + Trading Economics API + NewsAPI premium

### 8.3 프로 ($300-500/월)

위 + Polygon.io + Alpha Vantage premium + Refinitiv 부분 라이선스

---

## 9. 5월 12일 주요 이벤트 시뮬레이션 (현재 기준)

| 날짜 | 이벤트 | importance |
|---|---|---|
| 2026-05-05 (월) | NFP - April | 5 |
| 2026-05-08 (목) | BoE 회의 | 4 |
| 2026-05-12 (화) | **Fed FOMC + Powell 기자회견** | 5 ★ |
| 2026-05-13 (수) | **CPI - April** | 5 ★ |
| 2026-05-14 (목) | Microsoft Build day-1 | 4 |
| 2026-05-14 (목) | **Google I/O day-1** | 5 ★ |
| 2026-05-15 (금) | Retail Sales - April | 4 |
| 2026-05-19 (화) | NVDA Earnings | 5 ★ |
| 2026-05-30 (금) | PCE - April | 5 |

→ 다음 30일 안에 **importance 5 이벤트 6개** + **importance 4 이벤트 4개** = catalyst density 매우 높음.

---

## 10. 다음 단계 체크리스트

이 문서를 다른 프로젝트로 이식할 때:

- [ ] `catalyst_calendar.py` 복사 + `pip install feedparser requests yfinance`
- [ ] `tech_calendar.json` 분기별 update (기업 IR 캘린더 + 컨퍼런스)
- [ ] daily cron: `python catalyst_calendar.py --days 30 --json catalysts/upcoming.json`
- [ ] `dashboard/index.html` 또는 React/Vue 컴포넌트로 시각화
- [ ] (선택) Trading Economics API 연동
- [ ] (선택) NewsAPI 로 지정학 keyword detection
- [ ] (선택) Twitter/X API 로 Fed governor speeches 실시간

이 문서는 self-contained 이므로 다른 프로젝트로 그대로 복사 + 수정하면 됨.
