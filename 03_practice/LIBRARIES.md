# 📦 라이브러리 메모

연습하면서 **직접 돌려서 확인한 것**만 적는다. 설명서에 없거나 헷갈렸던 것 위주.

| 분류 | 라이브러리 | 버전 | 용도 |
|---|---|---|---|
| [1. 데이터 수집](#1-데이터-수집) | pykrx | 1.2.9 | 한국 주가 (KRX) |
| | finance-datareader | 0.9.202 | 한국·해외 주가, 종목 목록 |
| | yfinance | 1.7.0 | 미국 주가·지수·ETF (Yahoo Finance) |
| [2. 데이터 분석](#2-데이터-분석) | pandas | 2.3.3 | 표 데이터 다루기 |
| | numpy | 2.4.6 | 배열 계산 |
| [3. 시각화](#3-시각화) | matplotlib | 3.11.2 | 그래프 |
| | mplfinance | 0.12.10b0 | 캔들 차트 |
| [4. 모델](#4-모델) | scikit-learn | 1.9.1 | 전통 ML (SVM, RF), 분할, 평가 |
| | xgboost | 3.2.0 | 부스팅 |
| | torch (PyTorch) | 2.14.1+cu126 | 딥러닝 (DNN, LSTM), GPU |

---

# import

아래에 나오는 `stock.`, `fdr.`, `yf.`, `pd.`, `plt.`는 이렇게 불러온 이름이다. 노트북 맨 위 칸에 넣어두면 됨.

데이터 수집:

```python
from pykrx import stock               # 한국 — stock.get_market_ohlcv(...)
import FinanceDataReader as fdr       # 한국 + 미국 — fdr.DataReader(...)
import yfinance as yf                 # 미국 — yf.download(...)
```

분석·시각화·모델 쪽은 쓸 때 추가:

```python
import pandas as pd                   # pd.read_csv(...), pd.concat(...)
import numpy as np
import matplotlib.pyplot as plt       # plt.rcParams[...], plt.show()
import torch
```

---

# 1. 데이터 수집

주가를 받아오는 라이브러리. 결과는 **pandas DataFrame**으로 나옴 → 받은 다음부터는 [2. 데이터 분석](#2-데이터-분석) 영역 (`import pandas` 안 해도 `df.head()` 등이 되는 이유).

## 라이브러리 ≠ 데이터 출처

라이브러리는 출처에서 데이터를 **가져오는 도구**일 뿐. 논문에는 라이브러리 이름이 아니라 **출처**를 적는다 (+ 수정 가격 여부, 받은 날짜·기간).

| 라이브러리 | 실제 출처 | 성격 |
|---|---|---|
| pykrx | **한국거래소(KRX) 정보데이터시스템** | 거래소 공식 사이트에서 가져옴 → 출처 확실 |
| yfinance | **Yahoo Finance** | Yahoo 공식 도구가 아님 (웹에서 가져오는 방식). Yahoo 쪽이 바뀌면 고장 나기도 함 |
| FinanceDataReader | 여러 곳 (KRX, Yahoo, 네이버 등) | 출처를 모아놓은 도구. 미국 데이터는 yfinance와 **값이 똑같음** (같은 Yahoo 데이터) |

| 출처 | 쓰는 곳 |
|---|---|
| Yahoo Finance (무료) | 딥러닝 주가 예측 논문에서 **제일 많이 씀** (Jiang 2021 리뷰). 누구나 받을 수 있어서 재현하기 쉬움 |
| KRX 정보데이터시스템 (무료) | 한국 거래소 원본 |
| CRSP · Compustat · Bloomberg · Refinitiv Datastream (유료) | 금융·경제학 최상위 저널의 표준 |
| FnGuide DataGuide · KIS-VALUE (유료) | 국내 재무·금융 논문 |

## 한눈에: 뭘 받고 싶은가 → 어떻게

🔑 = KRX 로그인 필요 ([아래](#krx-로그인))

| 받고 싶은 것 | 예 | 방법 | 컬럼 |
|---|---|---|---|
| **한국 단일 종목** | 삼성전자 `005930` | pykrx `stock.get_market_ohlcv(시작, 끝, 코드)` | 시가 고가 저가 종가 거래량 등락률 (수정주가) |
| | | ↳ 원래 가격 🔑 `adjusted=False` | + **거래대금** (7열) |
| | | FinanceDataReader `fdr.DataReader(코드, 시작, 끝)` | Open High Low Close Volume Change |
| **한국 ETF** | KODEX 200 `069500` | pykrx 🔑 `stock.get_etf_ohlcv_by_date(시작, 끝, 코드)` | **NAV** 시가 고가 저가 종가 거래량 거래대금 **기초지수** (8열) |
| | | pykrx `stock.get_market_ohlcv(...)`도 되지만 ⚠️ [가격이 다름](#한국-etf-주식-함수-vs-etf-함수) | 6열 |
| | | FinanceDataReader `fdr.DataReader("069500")` | 6열 |
| **한국 지수** | KOSPI 200 `1028` | pykrx 🔑 `stock.get_index_ohlcv(시작, 끝, "1028")` | 시가 고가 저가 종가 거래량 거래대금 **상장시가총액** |
| | | FinanceDataReader `"KS11"`(KOSPI), `"KS200"`(KOSPI 200) | ⚠️ 9월 한 달이 13행뿐 (거래일 20일) → pykrx 권장 |
| **한국 종목 목록** | 이름 → 코드 | FinanceDataReader `fdr.StockListing("KRX")` (약 2,870개) | Code, Name, … |
| | | pykrx 🔑 `stock.get_market_ticker_list(날짜)` | 코드 리스트 |
| **한국 ETF 목록** | "나스닥" 들어간 ETF | FinanceDataReader `fdr.StockListing("ETF/KR")` (약 1,170개) | Symbol, Name, … |
| | | pykrx 🔑 `stock.get_etf_ticker_list(날짜)` | 코드 리스트만 |
| **미국 종목·ETF** | `AAPL`, `SPY`, `QQQ` | [yfinance](#yfinance-미국) `yf.download(...)` 또는 FinanceDataReader | Open High Low Close (Adj Close) Volume |
| **미국 지수** | `^GSPC`(S&P 500) | yfinance / FinanceDataReader (`"US500"`도 됨) | 같음 |

**코드 → 이름**: 종목 `stock.get_market_ticker_name("005930")` → 삼성전자 · ETF 🔑 `stock.get_etf_ticker_name("069500")` → KODEX 200 · 지수 🔑 `stock.get_index_ticker_name("1028")` → 코스피 200

**미국 ↔ 한국 짝**

| | 미국 | 한국 |
|---|---|---|
| 대표 지수 | S&P 500 `^GSPC` | KOSPI 200 `1028` |
| 그 지수를 따라가는 ETF | `SPY` | KODEX 200 `069500` |
| 한국에서 사는 미국 지수 ETF | — | TIGER 미국S&P500 `360750`, TIGER 미국나스닥100 `133690` 등 |

---

## pykrx (한국)

`from pykrx import stock` · 함수 목록 `dir(stock)` · 사용법 `help(stock.get_market_ohlcv)`

### 한국 단일 종목

| 항목 | 내용 |
|---|---|
| **수정주가가 기본값** | `adjusted=True`가 기본 (`help()`에는 안 나옴 → 소스코드에서 확인) |
| 수정주가의 범위 | **가격만 수정, 거래량은 수정 안 됨** (2018년 삼성전자 50:1 분할 전후 거래량이 100배 넘게 뜀) |
| 원래 가격 🔑 | `adjusted=False` → 원래 가격 + **거래대금** 컬럼이 추가됨. 2018년 4월 삼성전자 종가 2,427,000 (수정주가로는 48,540) |
| 거래정지일 | 빈 칸이 아니라 **거래량 0 + 직전 가격**으로 들어옴 → `isna()`로 안 잡힘 |
| 간격 바꾸기 | `freq="d"` 일(기본) / `"m"` 월 / `"y"` 년. 월·연봉은 **등락률 컬럼이 없음** (5열). 일봉을 `resample`로 합친 것과 **같은 결과** |

### 한국 ETF: 주식 함수 vs ETF 함수

KODEX 200(`069500`) 1년치 (2025-10 ~ 2026-09)

| | `get_market_ohlcv` (주식 함수) | `get_etf_ohlcv_by_date` (ETF 함수) 🔑 |
|---|---|---|
| 컬럼 | 6개 | 8개 (+ **NAV**, **거래대금**, **기초지수**) |
| 가격 | **수정된 가격** — 과거 가격이 조금 낮게 나옴 | **실제 거래된 가격** |
| 차이 | 241일 중 **199일** 다름 (2026-07-29 이전). ETF 함수 ÷ 주식 함수 = 1.002 ~ 1.01 | |

- 차이의 원인은 **분배금**(ETF가 주는 배당). 분배금이 나가면 그만큼 가격이 빠지므로, 주식 함수는 **과거 가격을 그만큼 낮춰서** 수익률을 이어 붙임 (미국 `Close` vs `Adj Close`와 같은 원리)
  - 근거: 두 가격의 비율이 **2025-10-30 · 2026-01-29 · 2026-04-29 · 2026-07-30**에 한 칸씩 내려감 (1.010 → 1.0075 → 1.0065 → 1.002 → 1.000) = KODEX 200 분배금 시기(1·4·7·10월 말)와 일치
  - 비율이 1.0075 ↔ 1.0076처럼 오락가락하는 건 수정 가격을 원 단위로 반올림해서 생기는 오차
- `adjusted=False`는 ETF에는 안 됨 (에러) → 실제 가격은 **ETF 함수**로
- **NAV** = ETF가 실제로 들고 있는 자산의 1주당 가치. 종가와 거의 같지만 조금씩 다름 (시장에서 사고파는 가격 ≠ 실제 가치)
- **기초지수** = 그 ETF가 따라가는 지수 값 (KODEX 200이면 KOSPI 200)

### 한국 지수 🔑

| 항목 | 내용 |
|---|---|
| 지수 코드 | `1028` = 코스피 200 (`get_index_ticker_name`으로 확인) |
| 컬럼 | 시가 고가 저가 종가 거래량 거래대금 **상장시가총액** |
| 값 | 포인트 (원이 아님). 코스피 200 ≈ 1,113 (2026-09-22) vs KODEX 200 ≈ 111,880원 → 약 **100배** |

### KRX 로그인

- 사이트: [KRX 정보데이터시스템 (data.krx.co.kr)](https://data.krx.co.kr) — 회원가입(무료) 필요
- 아이디·비밀번호를 Windows **사용자 환경 변수** `KRX_ID`, `KRX_PW`에 넣으면 pykrx를 불러올 때 자동 로그인. 세션 1시간, 만료되면 자동 재로그인
- 환경 변수를 바꾸면 VS Code를 완전히 껐다 켜야 적용됨
- 소셜 로그인 계정은 안 됨 (아이디·비밀번호를 직접 보내는 방식)
- 로그인 안 돼 있으면 `KRX 로그인 실패: …` 메시지 → 한국 단일 종목 일봉(수정주가)만 쓸 거면 무시해도 됨. 🔑 함수는 빈 표나 `KeyError`
- 로그인하면 출력에 `로그인 ID: …`가 찍힘
- ⚠️ 비밀번호를 코드에 쓰지 않는다 (`os.environ["KRX_PW"] = …` ❌)

---

## FinanceDataReader (한국 + 미국)

`import FinanceDataReader as fdr` · 로그인 필요 없음

| 항목 | 내용 |
|---|---|
| 한국 종목 | `fdr.DataReader("005930", "2018-04-25", "2018-05-10")` → pykrx와 **같은 숫자** |
| 한국 ETF | `fdr.DataReader("069500")` |
| 한국 지수 | `"KS11"`, `"KS200"` → ⚠️ 빠진 날이 있음 |
| 종목 / ETF 목록 | `fdr.StockListing("KRX")` / `fdr.StockListing("ETF/KR")` → 이름으로 찾기: `etf[etf["Name"].str.contains("나스닥")]` |
| `"KRX:005930"` 형식 | 한 번에 **최대 2년**까지만 (넘으면 `400 Bad Request`) |
| `"NAVER:005930"` 형식 | 기본 형식과 같은 숫자 |
| 미국 | `fdr.DataReader("AAPL")`, `"SPY"`, `"^GSPC"`/`"US500"` → 컬럼에 `Adj Close` 포함 |
| 종료일 | 미국 데이터는 `end`가 **포함 안 됨** (9/30까지 → `"2026-10-01"`). 한국은 포함됨 |
| ⚠️ 미국 시작일 | 시작일 **하루 전**부터 나옴 — `fdr.DataReader("SPY", "2025-10-01", "2026-10-01")` → 2025-09-30부터 252행 (yfinance는 251행). 받은 다음 `df.loc["2025-10-01":]`로 잘라서 맞추기 |
| 미국 값 | yfinance(`auto_adjust=False`)와 같은 날짜끼리 **완전히 같음** (Close·Open·Volume 차이 0, Adj Close 0.0001 반올림) |

---

## yfinance (미국)

논문 데이터 출처 1위인 **Yahoo Finance**에서 가져옴 (Yahoo 공식 도구는 아님). `import yfinance as yf`

| 항목 | 내용 |
|---|---|
| 일봉 받기 | `yf.download("SPY", start="2025-10-01", end="2026-10-01")` |
| ⚠️ **종료일 미포함** | `end` 날짜는 안 들어감 → 9/30까지 받으려면 `end="2026-10-01"` |
| ⚠️ **컬럼이 2층** | 기본은 `("Close", "SPY")`처럼 종목 이름이 한 층 더 붙음 → `df["Close"]`가 생각대로 안 됨. `multi_level_index=False`로 1층으로 |
| ⚠️ **기본값은 이미 수정된 가격** | 기본 `auto_adjust=True` → `Close`가 **이미 수정 종가**이고 `Adj Close` 컬럼이 없음. 원래 종가도 보려면 `auto_adjust=False` → `Close`(원래) + `Adj Close`(수정) 둘 다 |
| `True`의 `Close` = `False`의 `Adj Close` | 애플·SPY 1년치 **차이 0** |
| ⚠️ **`True`는 OHLC 전부 수정** | `Open`·`High`·`Low`도 그날 `Adj Close ÷ Close` 비율로 같이 바뀜 (애플 2025-10-01: 0.99632, SPY: 0.98933). `Volume`은 같음 → `False`로 받아서 `Open`(원래) + `Adj Close`(수정)를 섞어 쓰면 기준이 어긋남. **OHLC를 같이 쓸 땐 `True`(기본)** |
| ⚠️ **`Close`도 이미 분할 반영** | 애플 2020-08-31 4:1 분할 → 전날(8/28) 실제 약 $499인데 `Close`는 **$124.81**. **거래량도 분할 보정됨** (pykrx는 가격만) → `Close`와 `Adj Close` 차이는 **배당 때문에만** 생김 |
| `Close` vs `Adj Close` (1년치) | 애플 **214일** · SPY **242일** 다름 (배당·분배금). S&P 500 지수 `^GSPC` **0일** (지수는 배당을 무시하고 가격만으로 계산, 분할은 계산식에서 흡수) |
| 배당 포함 지수 | `^SP500TR` — 1년 수익률 15.34% vs `^GSPC` 14.01% → 차이 약 1.3%p = 배당 수익률 |
| ⚠️ 수정 가격은 받은 날짜에 따라 바뀜 | 나중에 배당이 또 나오면 **과거 날짜의 `Adj Close`도 다시 깎임** → 실험 데이터는 파일로 저장하거나 받은 날짜 기록 (재현성) |
| 배당·분할 내역 | `yf.Ticker("AAPL").dividends` (배당락일·금액), `.splits` (분할) · `.history(start=..., end=...)` → `Dividends`, `Stock Splits` 컬럼이 같이 나옴 |
| 진행바 끄기 | `progress=False` (`[****100%****]` 안 찍힘) |
| 지수 티커 | 앞에 `^`: `^GSPC`(S&P 500), `^IXIC`(나스닥), `^DJI`(다우) |
| ETF 티커 | `SPY`(S&P 500), `QQQ`(나스닥 100), `DIA`(다우 30) |

---

## 수정 가격 비교 (라이브러리마다 다름)

| | 가격 분할 보정 | 거래량 분할 보정 | 배당 보정 |
|---|---|---|---|
| pykrx `get_market_ohlcv` (기본) | ✅ | ❌ | ✅ (ETF 분배금) |
| pykrx `get_etf_ohlcv_by_date` | — | — | ❌ (실제 가격) |
| yfinance `Close` (`auto_adjust=False`) | ✅ | ✅ | ❌ |
| yfinance `Adj Close` / 기본 `Close` | ✅ | ✅ | ✅ |

- 받는 방식: pykrx는 `adjusted`로 **둘 중 하나**를 골라 받고, yfinance(`auto_adjust=False`)는 **한 표에 둘 다** 나옴
- 배당 보정 = 배당 **이전** 과거 가격에 `1 − 배당금 ÷ 전날 종가`를 **곱함** (빼면 옛날 가격이 망가지고, 곱해야 수익률이 유지됨). 그 날짜 **이후** 배당만 반영되고, 오래된 날짜일수록 누적
- 비교할 땐 **같은 기준끼리** — 예: 지수(배당 무시) vs SPY는 `auto_adjust=False`의 `Close`로

---

# 2. 데이터 분석

## pandas

`import pandas as pd` (받은 표의 기능만 쓸 땐 없어도 됨 — 아래 참고)

| 하고 싶은 것 | 코드 | 메모 |
|---|---|---|
| 몇 행 몇 열 | `df.shape` | `(241, 6)` = 241행 × 6컬럼 |
| 빈 칸 개수 | `df.isna().sum()` | 컬럼별 개수. 행 자체가 없는 날(휴장일)은 안 잡힘 |
| 요약 통계 | `df.describe()` | count / mean / std / min / 25% / 50% / 75% / max. `e+07` = ×1,000만 |
| 최댓값 vs 그 위치 | `df["등락률"].max()` / `.idxmax()` | max = **얼마**, idxmax = **언제**(인덱스) |
| 특정 행 | `df.loc["2026-07-31"]` | |
| 일봉 → 월봉 | `df.resample("ME").agg({"시가": "first", "고가": "max", "저가": "min", "종가": "last", "거래량": "sum"})` | pykrx `freq="m"`과 **완전히 같은 결과**. 주봉 `"W"`, 연봉 `"YE"` |
| 변화율 | `x["종가"].pct_change() * 100` | 리샘플링 후 등락률은 합치지 말고 새로 계산. `0.000618` = 0.0618% |
| 날짜 범위 자르기 | `df.loc["2025-10-01":"2026-09-30"]` | 라이브러리마다 시작·종료일 처리가 달라서 받은 뒤 직접 맞춤 |
| 1열짜리 표 → 한 줄 | `df["Close"].squeeze()` | yfinance 2층 컬럼이면 `df["Close"]`가 `[251 rows x 1 columns]` 표로 나옴 → 계산 전에 펴기 |
| 첫날 = 1로 맞추기 | `x / x.iloc[0]` | 가격 크기가 다른 것(지수 6,700 vs SPY 660) 움직임 비교 |
| 나란히 붙이기 | `pd.DataFrame({"지수": a, "SPY": b})` | 같은 날짜끼리 자동으로 맞춰짐 |
| 다른 날짜 찾기 | `a.index.difference(b.index)` | a에만 있는 날짜 |
| 같은 날짜끼리 비교 | `(a["Close"] - b["Close"]).abs().max()` | 빼기도 날짜 기준으로 맞춰서 계산 → 0이면 같음 |
| 큰 순서로 보기 | `r.reindex(r["차이"].abs().sort_values(ascending=False).index).head()` | 차이가 컸던 날 TOP |

`df.xxx()`처럼 **이미 받은 표의 기능**은 import 없이 쓸 수 있음. `pd.read_csv()`, `pd.DataFrame()`, `pd.concat()`처럼 **pandas 함수를 직접 부를 때**만 `import pandas as pd` 필요.

## numpy

(아직 없음)

---

# 3. 시각화

## matplotlib

`import matplotlib.pyplot as plt` · `df["종가"].plot()`처럼 pandas에서 바로 그릴 땐 없어도 됨

| 하고 싶은 것 | 코드 |
|---|---|
| 한글 깨짐(□) 해결 | `plt.rcParams["font.family"] = "Malgun Gothic"` |
| 한 그래프에 겹쳐 그리기 | `ax = a.plot()` → `b.plot(ax=ax)` (따로 `.plot()` 하면 그래프가 두 개 생겨서 비교 안 됨) |

## mplfinance

(아직 없음 — `data_candlestick`에서)

---

# 4. 모델

## scikit-learn

(아직 없음 — SVM, RF 할 때)

## xgboost

(아직 없음)

## PyTorch

`import torch`

| 하고 싶은 것 | 코드 | 메모 |
|---|---|---|
| GPU 되나 | `torch.cuda.is_available()` | `True`면 됨 |
| 모델 저장 / 불러오기 | `torch.save(model.state_dict(), "x.pt")` / `model.load_state_dict(torch.load("x.pt"))` | 커널이 꺼져도 다시 학습 안 해도 됨. `*.pt`는 `.gitignore`에 있음 |
