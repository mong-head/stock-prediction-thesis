# Vanishing / Exploding Gradient (RNN의 단점)

> 📺 유튜브 강의를 보며 정리 · [← 개념 목록](README.md) · 이전 : [RNN](rnn.md) · 다음 : [LSTM](lstm.md)

## RNN의 단점/문제/한계점?
- exploding / vanishing gradient
  - 구조상 X<sub>t</sub> 상태에는 W<sub>xx</sub>가 곱해지나 이 값이 **절댓값 1보다 크거나 작다면**,,
  - 무한대로 발산하거나 아예 0으로 수렴하는 문제 발생

## exploding gradient 문제
- gradient = 무한대
- 학습 도중 loss 가 inf 로 뜰 경우, 학습 더 이상 진행 불가
- 해결책? gradient clipping
  - gradient 상한과 하한을 정하여 학습시킴
  - 근본적인 해결책은 아님

## vanishing gradient 문제
- gradient = 0
- 학습 도중 파악이 어려움 (학습이 종료된건지 vg문제인건지…)
- 초기화 간결하게 해주는 방법 존재하지만, 다른 네트워크 구조 제안이 훨씬 편함
  - Gated RNNs : [LSTM](lstm.md) / [GRU](gru.md)
  - gated : input / output 을 얼마나 열고 닫을것인지 수도꼭지 역할
