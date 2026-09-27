# 📖 용어집

> 논문 읽다가 모르는 단어를 찾는 짧은 사전 · `Ctrl+F`로 검색 · 깊은 정리는 [개념 노트](README.md)
>
> 📝 = 개념 노트 있음

## 순서 데이터 모델
| 영어 | 한국어 | 한 줄 뜻 |
|---|---|---|
| Time series | 시계열 | 시간 순서대로 기록된 데이터 (예: 매일의 종가) |
| Sequence | 시퀀스 | 순서가 있는 데이터 묶음 |
| RNN (Recurrent Neural Network) 📝 [노트](rnn.md) | 순환 신경망 | 이전 시점의 정보를 은닉 상태로 다음 시점에 넘기는 신경망 |
| Hidden state | 은닉 상태 | 지금까지 본 입력들을 압축해 담아둔 벡터 (RNN의 "기억") |
| BPTT (Back-Propagation Through Time) | 시간 역전파 | RNN을 시간 축으로 펼쳐서 하는 역전파 |
| Many-to-one | 다대일 | 여러 시점 입력 → 출력 1개 (과거 N일 → 다음 날) |
| Vanishing gradient | 기울기 소실 | 역전파 중 기울기가 점점 0에 가까워져 먼 과거를 학습하지 못하는 문제 |
| Exploding gradient | 기울기 폭주 | 반대로 기울기가 너무 커져 학습이 불안정해지는 문제 |
| LSTM (Long Short-Term Memory) | 장단기 메모리 | 게이트로 기억할 것과 버릴 것을 조절해 기울기 소실을 줄인 RNN |
| Gate (forget / input / output) | 게이트 (망각 / 입력 / 출력) | LSTM 안에서 정보를 얼마나 통과시킬지 정하는 0~1 값 |
| Cell state | 셀 상태 | LSTM이 장기 기억을 실어 나르는 통로 |
| GRU (Gated Recurrent Unit) | 게이트 순환 유닛 | LSTM을 게이트 2개로 단순화한 버전 |
| BiLSTM (Bidirectional LSTM) | 양방향 LSTM | 앞→뒤, 뒤→앞 두 방향으로 읽어 합친 LSTM |
| 1D CNN (1D Convolution) | 1차원 합성곱 | 필터를 시간 축 한 방향으로만 미끄러뜨리는 CNN, 짧은 패턴 추출에 사용 |
| Attention | 어텐션 (주의 메커니즘) | 입력 중 어느 시점이 더 중요한지 가중치를 주는 방법 |
| Transformer | 트랜스포머 | RNN 없이 어텐션만으로 순서 데이터를 처리하는 모델 |
| Hybrid model | 하이브리드 모델 | 여러 모델을 이어 붙인 것 (예: CNN-LSTM) |
| ESN (Echo State Network) | 에코 상태 네트워크 | 내부(저수지)는 무작위로 고정하고 출력층만 학습하는 RNN |
| Reservoir computing | 저수지 컴퓨팅 | ESN 계열을 부르는 이름 |

## 전통적 방법 (논문 서론에 자주 나옴)
| 영어 | 한국어 | 한 줄 뜻 |
|---|---|---|
| ARIMA | 자기회귀 누적 이동평균 모형 | 과거 값과 과거 오차의 선형 조합으로 예측하는 통계 모형 |
| GARCH | — | 변동성(분산)이 시간에 따라 변하는 것을 모델링하는 통계 모형 |
| SVM / SVR | 서포트 벡터 머신 / 회귀 | 머신러닝 분류·회귀 모델, 딥러닝 이전 비교 대상 |
| Random Forest | 랜덤 포레스트 | 결정 트리 여러 개의 투표, 딥러닝 논문의 단골 비교 대상 |
| GA (Genetic Algorithm) | 유전 알고리즘 | 선택·교차·돌연변이를 반복해 좋은 해를 찾는 최적화 방법 |

## 시계열 실험
| 영어 | 한국어 | 한 줄 뜻 |
|---|---|---|
| Look-back window / Sliding window | 관측 구간 / 슬라이딩 윈도우 | "과거 N일"만큼 잘라서 입력으로 쓰는 것, 한 칸씩 밀며 샘플 생성 |
| Lag | 시차 | 몇 시점 전의 값인지 (lag 1 = 어제 값) |
| Forecast horizon | 예측 구간 | 얼마나 먼 미래를 예측하는지 (1일 후, 5일 후…) |
| Train / Validation / Test set | 학습 / 검증 / 시험 데이터 | 시계열에서는 반드시 시간 순서대로 나눔 |
| Walk-forward validation | 전진 검증 | 학습 구간을 시간 순서대로 밀면서 반복 평가하는 시계열용 교차검증 |
| Look-ahead bias | 미래 정보 편향 | 예측 시점에는 알 수 없는 미래 정보가 입력에 섞이는 실수 |
| Data leakage | 데이터 누수 | 시험 데이터 정보가 학습에 새어 들어가는 것 (예: 전체 데이터로 스케일러 fit) |
| Normalization (Min-Max) | 정규화 | 값을 0~1 범위로 맞추기 |
| Standardization (Z-score) | 표준화 | 평균 0, 표준편차 1로 맞추기 |
| Stationarity | 정상성 | 평균·분산이 시간에 따라 변하지 않는 성질, 주가는 비정상 → 수익률로 변환 |
| Feature | 특성 / 변수 | 모델 입력 항목 (종가, 거래량, 지표 등) |
| Overfitting | 과적합 | 학습 데이터에만 잘 맞고 새 데이터에는 못 맞추는 상태 |
| Dropout | 드롭아웃 | 학습 중 뉴런 일부를 무작위로 꺼서 과적합을 막는 방법 |
| Early stopping | 조기 종료 | 검증 손실이 더 이상 줄지 않으면 학습을 멈추기 |
| Hyperparameter | 하이퍼파라미터 | 사람이 정하는 설정값 (윈도우 길이, 층 수, 학습률 등) |
| Epoch / Batch | 에폭 / 배치 | 전체 데이터 1회 학습 / 한 번에 넣는 샘플 묶음 |
| Learning rate | 학습률 | 한 번에 가중치를 얼마나 바꿀지 |
| Loss function | 손실 함수 | 모델이 얼마나 틀렸는지 재는 식 (회귀는 보통 MSE) |

## 평가 지표
| 영어 | 한국어 | 한 줄 뜻 |
|---|---|---|
| MAE (Mean Absolute Error) | 평균 절대 오차 | 오차 절댓값의 평균 |
| MSE / RMSE | 평균 제곱 오차 / 그 제곱근 | 오차를 제곱해 평균, 큰 오차에 더 민감 |
| MAPE | 평균 절대 백분율 오차 | 오차를 실제값 대비 %로 |
| R² (Coefficient of determination) | 결정계수 | 모델이 변동을 얼마나 설명하는지 (1에 가까울수록 좋음) |
| Directional accuracy / Hit rate | 방향 정확도 | 오를지 내릴지 맞힌 비율 |
| Baseline / Benchmark | 기준 모델 / 비교 대상 | 제안 모델이 이겨야 하는 비교 상대 |
| Naive forecast / Random walk | 단순 예측 / 랜덤워크 | "내일 = 오늘" 예측, 가격 예측에서 의외로 이기기 어려운 기준 ⭐ |

## 금융
| 영어 | 한국어 | 한 줄 뜻 |
|---|---|---|
| OHLCV | 시가·고가·저가·종가·거래량 | Open, High, Low, Close, Volume, 일별 주가 데이터의 기본 형태 |
| Closing price / Adjusted close | 종가 / 수정 종가 | 장 마감 가격 / 배당·액면분할을 반영해 보정한 종가 |
| Return | 수익률 | (오늘 가격 − 어제 가격) / 어제 가격 |
| Log return | 로그 수익률 | log(오늘 / 어제), 더하기로 누적 가능해서 연구에서 많이 씀 |
| Volatility | 변동성 | 수익률이 얼마나 출렁이는지 (보통 표준편차) |
| Index | 지수 | 여러 종목을 묶은 시장 대표값 (S&P 500, KOSPI 200) |
| ETF | 상장지수펀드 | 지수를 따라가도록 만든 상품 (SPY, QQQ, DIA) |
| EMH (Efficient Market Hypothesis) | 효율적 시장 가설 | 가격에 정보가 이미 반영돼 있어 초과수익 예측이 어렵다는 가설 |
| Technical analysis | 기술적 분석 | 과거 가격·거래량 패턴으로 예측하는 방법 |
| Fundamental analysis | 기본적 분석 | 재무제표·경제지표로 기업 가치를 평가하는 방법 |
| Technical indicator | 기술적 지표 | 가격으로 계산한 보조 지표 (MA, RSI, MACD 등) |
| MA (Moving Average) | 이동평균 | 최근 N일 평균 가격 |
| Golden cross | 골든크로스 | 단기 이동평균이 장기 이동평균을 위로 뚫는 것 (매수 신호로 해석) |
| Envelope | 엔벨로프 | 이동평균 위아래로 일정 % 띠를 그린 지표 |
| RSI (Relative Strength Index) | 상대강도지수 | 최근 상승폭과 하락폭 비율로 과매수·과매도 판단 (0~100) |
| Sentiment analysis | 감성 분석 | 뉴스·SNS 글의 긍정/부정을 분석해 입력으로 쓰는 것 |
| Market regime | 시장 상태 | 상승장·하락장·횡보장 같은 시장 국면 |
| Long / Short | 매수 / 공매도 | 오를 것에 베팅 / 내릴 것에 베팅 |
| Long-short portfolio | 롱숏 포트폴리오 | 오를 종목은 사고 내릴 종목은 공매도해서 차이로 수익 |
| Backtesting | 백테스팅 | 과거 데이터로 전략을 모의 실행해보는 것 |
| Transaction cost | 거래 비용 | 수수료·세금 등, 빼고 나면 수익이 사라지는 경우가 많음 |
| Sharpe ratio | 샤프 지수 | 위험(변동성) 대비 수익, 높을수록 좋음 |
| MDD (Maximum Drawdown) | 최대 낙폭 | 최고점 대비 가장 크게 떨어진 비율 |
| Buy & Hold | 매수 후 보유 | 사서 그냥 들고 있는 전략, 트레이딩 전략의 기본 비교 대상 |

## ✍️ 읽다가 추가한 단어
| 영어 | 한국어 | 한 줄 뜻 | 어느 논문 |
|---|---|---|---|
| | | | |
