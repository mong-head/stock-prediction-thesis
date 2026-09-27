# 📖 용어집

> 내가 정리한 개념 노트에서 뽑은 용어 · `Ctrl+F`로 검색 · 자세한 건 각 노트로

## RNN · 시스템
| 용어 | 뜻 | 노트 |
|---|---|---|
| RNN (Recurrent Neural Network) | 시계열 데이터 처리하기 좋은 뉴럴 네트워크 구조. 시간별로 같은 weight 공유 | [RNN](rnn.md) |
| CNN (RNN과 비교) | 이미지 구역별로 같은 weight 공유 | [RNN](rnn.md) |
| First Order System | 현재 시간의 상태 ← 이전 시간 상태 관련 | [RNN](rnn.md) |
| Autonomous system | 외부입력 없이 자기 혼자서 돌아감 (셀프 피드백), X<sub>t</sub> = f(X<sub>t-1</sub>) | [RNN](rnn.md) |
| State-Space Model | 어떤 시스템을 해석하기 위한 3요소 : 입력, 상태, 출력 | [RNN](rnn.md) |
| 상태 X<sub>t</sub> (state) | hidden layer의 state. 이전까지의 상태들과 이전까지의 입력을 대표할 수 있는 압축본 | [RNN](rnn.md) |
| First-order Markov Model | X<sub>t</sub> = f(U<sub>t</sub>, …, U<sub>0</sub>) 대신 X<sub>t</sub> = f(X<sub>t-1</sub>, U<sub>t</sub>)를 푸는 것 | [RNN](rnn.md) |
| Activation Function | 비선형성 부여 | [RNN](rnn.md) |
| w / b | weight / bias | [RNN](rnn.md) |
| BPTT | Back-propagation through time (시간에 따른 bp) | [RNN](rnn.md) |

## RNN 문제 type
| 용어 | 뜻 | 노트 |
|---|---|---|
| many-to-many | input, output 여러개. 번역시 사용 | [RNN](rnn.md) |
| many-to-one | back-propagation 할때 마지막만 봄. 시계열 예측시에 사용 | [RNN](rnn.md) |
| one-to-many | 생성시 많이 사용 (문장 생성) | [RNN](rnn.md) |
| seq2seq | many-to-one + one-to-many | [RNN](rnn.md) |

## Gradient 문제
| 용어 | 뜻 | 노트 |
|---|---|---|
| Exploding gradient | gradient = 무한대. loss 가 inf 로 뜨면 학습 더 이상 진행 불가 | [Vanishing Gradient](vanishing-gradient.md) |
| Gradient clipping | gradient 상한과 하한을 정하여 학습시킴. 근본적인 해결책은 아님 | [Vanishing Gradient](vanishing-gradient.md) |
| Vanishing gradient | gradient = 0. 학습이 종료된건지 vg문제인건지 파악이 어려움 | [Vanishing Gradient](vanishing-gradient.md) |
| Gated RNNs | LSTM / GRU. gated : input / output 을 얼마나 열고 닫을것인지 수도꼭지 역할 | [Vanishing Gradient](vanishing-gradient.md) |

## LSTM · GRU
| 용어 | 뜻 | 노트 |
|---|---|---|
| LSTM | gradient flow를 제어할 수 있는 밸브 역할 | [LSTM](lstm.md) |
| forget gate | 이 이전 상태와 입력을 얼마나 사용(잊어버릴것)할 것인지 결정 | [LSTM](lstm.md) |
| input gate | 이전상태와 입력을 얼마나 활용할 것인지 결정 | [LSTM](lstm.md) |
| cell | forget, input gate에서 나온 데이터 섞기 | [LSTM](lstm.md) |
| output gate | 위 정보들을 모두 종합해서 다음 상태 결정 | [LSTM](lstm.md) |
| GRU | LSTM이 너무 복잡하여 나온 간단한 모델. cell 이 없는 구조 | [GRU](gru.md) |
| BiLSTM | LSTM 확장형. 시간에 따라서 뒤로 가는 방향도 있음 | [BiLSTM](bilstm.md) |
