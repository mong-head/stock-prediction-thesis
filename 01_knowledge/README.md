# 📚 01. 관련 지식

대표 논문을 읽는 데 필요한 개념 정리. ✅ 정리 완료 · ⬜ 아직

> 📖 짧게 뜻만 찾을 땐 → [용어집](glossary.md)

## 순서가 있는 데이터를 다루는 모델
| | 개념 | 한 줄 요약 | 나오는 논문 |
|---|---|---|---|
| ✅ | [RNN](rnn.md) | 이전 시점 정보를 은닉 상태로 넘기는 신경망 | 전반 |
| ✅ | [기울기 소실 (vanishing gradient)](vanishing-gradient.md) | RNN이 긴 과거를 잊는 이유 → LSTM 등장 배경 | 전반 |
| ✅ | [LSTM](lstm.md) | 게이트 3개(forget/input/output)로 기억 조절 | Fischer, Lu, Zhang |
| ✅ | [BiLSTM](bilstm.md) | 양방향으로 읽는 LSTM | Zhang |
| ✅ | [GRU](gru.md) | LSTM 간소화 버전 | Jiang |

## 시계열 실험 규칙
| | 개념 | 한 줄 요약 | 나오는 논문 |
|---|---|---|---|
| ⬜ | 슬라이딩 윈도우 | 과거 N일 → 다음 날 형태로 입력 자르기 | 전반 |
| ⬜ | 셔플 금지 · 시간 순서 분할 | 무작위 split 하면 미래로 과거를 맞히는 반칙 | 전반 |
| ⬜ | walk-forward 검증 | 시계열용 교차검증 | Fischer |
| ⬜ | 데이터 누수 (look-ahead bias) | 스케일러는 train으로만 fit | 전반 |
| ⬜ | 정상성 (stationarity) | 가격 대신 수익률을 쓰는 이유 | 전반 |

## 평가 지표
| | 개념 | 한 줄 요약 | 나오는 논문 |
|---|---|---|---|
| ⬜ | MAE / RMSE / MAPE / R² | 회귀 오차 지표 | Lu, Zhang |
| ⬜ | 방향 정확도 | 오를지 내릴지 맞힌 비율 | — |
| ⬜ | 수익률 / Sharpe / MDD | 트레이딩 성과 지표 | Fischer, 정동균 |

## 금융 기초
| | 개념 | 한 줄 요약 | 나오는 논문 |
|---|---|---|---|
| ⬜ | OHLCV | 시가·고가·저가·종가·거래량 | 전반 |
| ⬜ | 가격 vs 수익률 | 진지한 연구는 대부분 수익률 예측 | 전반 |
| ⬜ | 지수 / ETF | S&P500, KOSPI200, SPY·QQQ | Fischer, 정동균 |
| ⬜ | 효율적 시장 가설 / 랜덤워크 | 주가는 원래 예측하기 어렵다 | Jiang, Fischer |
| ⬜ | 롱숏 포트폴리오 | 오를 종목 매수 + 내릴 종목 공매도 | Fischer |
| ⬜ | ⭐ Naive baseline | "내일 = 오늘 종가"와 비교했는가? | 전반 |

## 읽으면서 채울 것
| | 개념 | 한 줄 요약 | 나오는 논문 |
|---|---|---|---|
| ⬜ | 1D CNN | 2D 필터를 시간축 한 줄로 | Lu, Zhang |
| ⬜ | Attention | 어느 날짜가 중요한지 가중치 | Zhang |
| ⬜ | Transformer | Attention만으로 만든 모델 | Jiang |
| ⬜ | 기술적 지표 (MA, GC, Envelope, RSI) | | 정동균 |
| ⬜ | 유전 알고리즘 (GA) | 선택·교차·돌연변이로 최적화 | 정동균 |
| ⬜ | ESN / Reservoir Computing | 출력층만 학습하는 RNN | 정동균 |
