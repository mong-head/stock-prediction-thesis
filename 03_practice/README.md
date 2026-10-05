# 🧪 03. 연습 노트북

[개념 노트](../01_knowledge/)에서 정리한 것을 코드로 확인해 본 기록.
로컬 Jupyter · Python

📦 라이브러리 사용법 · 함정 메모 → [LIBRARIES.md](LIBRARIES.md)

⬜ 아직 · 🟡 하는 중 · ✅ 끝 (✍️ 알게 된 것까지 적으면)

## 개념 확인

| | 노트북 | 확인한 것 | 개념 노트 |
|---|---|---|---|
| ✅ | [`data_load_krx`](data_load_krx.ipynb) | OHLCV가 실제로 어떻게 생겼는지 | |
| 🟡 | [`data_load_us`](data_load_us.ipynb) | 미국 데이터는 KRX랑 뭐가 다른지 · 지수 vs ETF | [지수 / ETF](../01_knowledge/index-etf.md) |
| ⬜ | `data_candlestick` | 캔들 하나에 네 값이 어떻게 들어가는지 | [지수 / ETF](../01_knowledge/index-etf.md) |
| ⬜ | `data_sliding_window` | 과거 N일 → 다음 날이 배열로 어떤 모양인지 | [슬라이딩 윈도우](../01_knowledge/sliding-window.md) |
| ⬜ | `model_lstm_minimal` | `input_shape=(20, 1)`이 뭘 의미하는지 | [LSTM](../01_knowledge/lstm.md) |
| ⬜ | `eval_time_split` | 무작위로 나누면 왜 안 되는지 | |
| ⬜ | `eval_metrics` | 정확도의 함정 · F1 · 혼동행렬 | |

## 모델 비교

같은 데이터와 시간순 분할로 고정하고, 간단한 모델부터 올라가며 비교.

| | 노트북 | 확인한 것 | 개념 노트 |
|---|---|---|---|
| ⬜ | `model_baseline` | "내일 = 오늘 종가"보다 나은가 | |
| ⬜ | `model_svm` | 스케일링을 빼면 성능이 얼마나 떨어지는지 | |
| ⬜ | `model_rf` | 스케일링 없이도 되는 이유 | |
| ⬜ | `model_xgboost` | | |
| ⬜ | `model_dnn` | | |
| ⬜ | `model_lstm` | 시퀀스를 주면 달라지는지 | [LSTM](../01_knowledge/lstm.md) |
