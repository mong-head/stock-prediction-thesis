# 📈 시계열 주가 예측 — 석사 논문 연구 노트

석사 논문을 준비하며 공부한 개념, 읽은 논문, 연구 과정을 기록하는 저장소

**연구 방향** : 딥러닝 기반 시계열 주가 예측 (RNN · LSTM · Attention 계열)

## 🗂️ 구성
| 폴더 | 내용 |
|---|---|
| [📚 01_knowledge](01_knowledge/) | 논문을 읽기 위한 배경지식 정리 |
| [📄 02_papers](02_papers/) | 관련 논문 — 서지 · 링크 · 느낀 점 |
| [✍️ 03_my_thesis](03_my_thesis/) | 내 논문 (주제 확정 후) |

## 📚 개념 정리
- **순서 데이터 모델** : FNN · [RNN](01_knowledge/rnn.md) · [Vanishing Gradient](01_knowledge/vanishing-gradient.md) · [LSTM](01_knowledge/lstm.md) · [GRU](01_knowledge/gru.md) · [BiLSTM](01_knowledge/bilstm.md)
- **데이터 기초** : [스칼라 / 벡터 / 행렬 / 텐서](01_knowledge/tensor.md)
- **시계열 실험 규칙** : [슬라이딩 윈도우](01_knowledge/sliding-window.md) · 시간 순서 분할 · 데이터 누수 · 정상성 · 생존편향
- **평가 지표** : MAE / RMSE / MAPE · 방향 정확도 · Sharpe / MDD
- **금융 기초** : OHLCV · 수익률 · [지수 / ETF](01_knowledge/index-etf.md) · 효율적 시장 가설 · Naive baseline
- **읽으면서** : 1D CNN · Attention · Transformer · GNN · ARIMA / GARCH / 로지스틱 회귀

→ 전체 목록 : [01_knowledge](01_knowledge/README.md) · 📖 [용어집](01_knowledge/glossary.md)

## 📄 읽은 논문
| 연도 | 논문 | 키워드 |
|---|---|---|
| 2021 | [Jiang — 딥러닝 주가 예측 리뷰](02_papers/2021_Jiang_DL-stock-review.md) | 리뷰 |
| 2018 | [Fischer & Krauss — LSTM 금융 예측](02_papers/2018_Fischer_LSTM-financial.md) | LSTM |
| 2020 | [Lu et al. — CNN-LSTM](02_papers/2020_Lu_CNN-LSTM.md) | CNN-LSTM |
| 2023 | [Zhang et al. — CNN-BiLSTM-Attention](02_papers/2023_Zhang_CNN-BiLSTM-Attention.md) | Attention |
| 2026 | [정동균 — GA-ESN 주가 예측](02_papers/2026_Jeong_GA-ESN.md) | GA, ESN |

→ 전체 목록 : [02_papers](02_papers/README.md)
