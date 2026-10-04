# 📦 라이브러리 메모

연습하면서 **직접 돌려서 확인한 것**만 적는다. 설명서에 없거나 헷갈렸던 것 위주.

> 🤖 처음 내용은 Claude가 실행해서 확인한 결과로 채움 (2026-10-04). 버전이 바뀌면 동작이 달라질 수 있음.

| 분류 | 라이브러리 | 버전 | 용도 |
|---|---|---|---|
| [1. 데이터 수집](#1-데이터-수집) | pykrx | 1.2.9 | 한국 주가 (KRX) |
| | finance-datareader | 0.9.202 | 한국·해외 주가, 종목 목록 |
| [2. 데이터 분석](#2-데이터-분석) | pandas | 2.3.3 | 표 데이터 다루기 |
| | numpy | 2.4.6 | 배열 계산 |
| [3. 시각화](#3-시각화) | matplotlib | 3.11.2 | 그래프 |
| | mplfinance | 0.12.10b0 | 캔들 차트 |
| [4. 모델](#4-모델) | scikit-learn | 1.9.1 | 전통 ML (SVM, RF), 분할, 평가 |
| | xgboost | 3.2.0 | 부스팅 |
| | torch (PyTorch) | 2.14.1+cu126 | 딥러닝 (DNN, LSTM), GPU |

---

# 1. 데이터 수집

주가를 받아오는 라이브러리. 결과는 **pandas DataFrame**으로 나옴 → 받은 다음부터는 [2. 데이터 분석](#2-데이터-분석) 영역 (`import pandas` 안 해도 `df.head()` 등이 되는 이유).

## pykrx

| 항목 | 내용 |
|---|---|
| 일봉 받기 | `stock.get_market_ohlcv("20251001", "20260930", "005930")` → 컬럼 `시가 고가 저가 종가 거래량 등락률`, 인덱스는 `날짜` |
| **수정주가가 기본값** | `adjusted=True`가 기본 (`help()`에는 안 나옴 → 소스코드에서 확인) |
| 수정주가의 범위 | **가격만 수정, 거래량은 수정 안 됨** (2018년 삼성전자 50:1 분할 전후 거래량이 100배 넘게 뜀) |
| 거래정지일 | 빈 칸이 아니라 **거래량 0 + 직전 가격**으로 들어옴 → `isna()`로 안 잡힘 |
| 원래 가격 | `adjusted=False` → **KRX 로그인 필요**. 로그인 없으면 빈 표 |
| 간격 바꾸기 | `freq="d"` 일(기본) / `"m"` 월 / `"y"` 년. 월·연봉은 **등락률 컬럼이 없음** (5열) |
| 코드 → 이름 | `stock.get_market_ticker_name("005930")` → `'삼성전자'` (로그인 없이 됨) |
| 전체 종목 목록 | `stock.get_market_ticker_list(...)` → **KRX 로그인 필요**. 로그인 없으면 빈 목록이나 `IndexError` |
| 함수 목록 / 사용법 | `dir(stock)` / `help(stock.get_market_ohlcv)` |

**KRX 로그인**
- data.krx.co.kr 계정을 환경 변수 `KRX_ID`, `KRX_PW`에 넣으면 불러올 때 자동 로그인. 세션 1시간, 만료되면 자동 재로그인
- 소셜 로그인 계정은 안 됨 (아이디·비밀번호를 직접 보내는 방식)
- 로그인 안 돼 있으면 `KRX 로그인 실패: …` 메시지 → 일봉(수정주가)만 쓸 거면 무시해도 됨
- 로그인하면 출력에 `로그인 ID: …`가 찍힘
- ⚠️ 비밀번호를 코드에 쓰지 않는다 (`os.environ["KRX_PW"] = …` ❌)

## FinanceDataReader

| 항목 | 내용 |
|---|---|
| 일봉 받기 | `fdr.DataReader("005930", "2018-04-25", "2018-05-10")` → 컬럼 `Open High Low Close Volume Change` |
| pykrx와 비교 | 같은 기간 삼성전자 종가·거래량이 **pykrx와 같은 숫자** |
| **전체 종목 목록** | `fdr.StockListing("KRX")` → 로그인 없이 됨 (약 2,870개) |
| 이름 → 코드 | `krx[krx["Name"] == "SK하이닉스"]` → `Code` 칸 |
| `"KRX:005930"` 형식 | 한 번에 **최대 2년**까지만 (넘으면 `400 Bad Request`) |
| `"NAVER:005930"` 형식 | 기본 형식과 같은 숫자 |

---

# 2. 데이터 분석

## pandas

| 하고 싶은 것 | 코드 | 메모 |
|---|---|---|
| 몇 행 몇 열 | `df.shape` | `(241, 6)` = 241행 × 6컬럼 |
| 빈 칸 개수 | `df.isna().sum()` | 컬럼별 개수. 행 자체가 없는 날(휴장일)은 안 잡힘 |
| 요약 통계 | `df.describe()` | count / mean / std / min / 25% / 50% / 75% / max. `e+07` = ×1,000만 |
| 최댓값 vs 그 위치 | `df["등락률"].max()` / `.idxmax()` | max = **얼마**, idxmax = **언제**(인덱스) |
| 특정 행 | `df.loc["2026-07-31"]` | |
| 일봉 → 월봉 | `df.resample("ME").agg({"시가": "first", "고가": "max", "저가": "min", "종가": "last", "거래량": "sum"})` | pykrx `freq="m"`과 **완전히 같은 결과**. 주봉 `"W"`, 연봉 `"YE"` |
| 변화율 | `x["종가"].pct_change() * 100` | 리샘플링 후 등락률은 합치지 말고 새로 계산 |

`df.xxx()`처럼 **이미 받은 표의 기능**은 import 없이 쓸 수 있음. `pd.read_csv()`, `pd.DataFrame()`, `pd.concat()`처럼 **pandas 함수를 직접 부를 때**만 `import pandas as pd` 필요.

## numpy

(아직 없음)

---

# 3. 시각화

## matplotlib

| 하고 싶은 것 | 코드 |
|---|---|
| 한글 깨짐(□) 해결 | `plt.rcParams["font.family"] = "Malgun Gothic"` |

## mplfinance

(아직 없음 — `data_candlestick`에서)

---

# 4. 모델

## scikit-learn

(아직 없음 — SVM, RF 할 때)

## xgboost

(아직 없음)

## PyTorch

| 하고 싶은 것 | 코드 | 메모 |
|---|---|---|
| GPU 되나 | `torch.cuda.is_available()` | `True`면 됨 |
| 모델 저장 / 불러오기 | `torch.save(model.state_dict(), "x.pt")` / `model.load_state_dict(torch.load("x.pt"))` | 커널이 꺼져도 다시 학습 안 해도 됨. `*.pt`는 `.gitignore`에 있음 |
