# RNN (Recurrent Neural Network)

> 📺 유튜브 강의를 보며 정리 · 2026-09 · [← 개념 목록](README.md)

- 시계열 데이터 처리하기 좋은 뉴럴 네트워크 구조
  - 사용처 : 음성인식, dna 염기서열 분석, 감정 분석, 번역기 등

## CNN vs RNN
- CNN : 이미지 구역별로 같은 weight 공유
- RNN : 시간별로 같은 weight 공유 (현재와 과거는 같은 weight 공유)

## First Order System
- 현재 시간의 상태 ← 이전 시간 상태 관련
- **autonomous system : 이 시스템은 외부입력 없이 자기 혼자서 돌아감 (셀프 피드백)**
  - X<sub>t</sub> = f(X<sub>t-1</sub>)
- 현재 입력도 존재하는 경우 ? (이전시간 상태 + 현재 입력)
  - X<sub>t</sub> = f(X<sub>t-1</sub>, U<sub>t</sub>)

## State-Space Model

![State-Space Model](images/rnn/01_state_space_model.png)

- 어떤 시스템을 해석하기 위한 3요소 : 입력, 상태, 출력
  - X<sub>t</sub>(상태) = f(X<sub>t-1</sub>(상태), U<sub>t</sub>(입력)) → 1차원 시스템 모형, 이때 X<sub>t</sub> 관측 가능?
    - X<sub>t</sub>는 온도, 기후, 등등 이런게 될 수 있고 이 모든걸 다 관측하기는 어려움.
    - but 각 시간에서 관측 가능한 상태 모음 = 출력 Y<sub>t</sub>
    - 이때 X<sub>t</sub>는 스스로 피드백하여도 동작함
    - 상태 X<sub>t</sub> 의미 ? hidden layer의 state. 이전까지의 상태들과 이전까지의 입력을 대표할 수 있는 압축본. 즉 X<sub>t</sub>는 시계열로 들어오는 입력들을 최대한 상세히 표현가능해야 함.
      - X<sub>4</sub> ← X<sub>0~3</sub> && U<sub>1~4</sub>
  - Y<sub>t</sub>(출력) = h(X<sub>t</sub>) (h라는 비선형 함수)

## State-Space Model as RNN

### First-order Markov Model
- 원래 풀고 싶었던 문제 : X<sub>t</sub> = f(U<sub>t</sub>, U<sub>t-1</sub>, U<sub>t-2</sub>, .., U<sub>0</sub>)
  - 입력값들 -직빵→ X<sub>t</sub> : 사실상 힘듦
- 대신해서 풀 문제 : X<sub>t</sub> = f(X<sub>t-1</sub>, U<sub>t</sub>)
  - X<sub>t-1</sub>은 이전 상태들의 압축한 어떠한 "상태"

### State-Space Model 에서 근사하는 함수 : 2개 (f, h)

![f, h 두 함수](images/rnn/02_f_h_functions.png)

- X<sub>t</sub> = f(X<sub>t-1</sub>, U<sub>t</sub>) (검은색 부분)
- Y<sub>t</sub> = h(X<sub>t</sub>) (파란색 부분)
- 이 두 f, h 함수를 근사하기 위해 뉴럴 네트워크 사용 (각각 한개 해서 총 2개 뉴럴네트워크)

### 뉴럴 네트워크에서 State-Space Model ?
- (참고) 뉴럴 네트워크에서 비선형 함수 표현

  ![뉴럴 네트워크의 비선형 함수](images/rnn/03_nn_nonlinear.png)

  - Activation Function : 비선형성 부여
  - w : weight
  - b : bias
- 뉴럴 네트워크 셋팅으로 state-space model 근사 함수

  ![state-space model](images/rnn/04_ssm_nn_before.png)
  →
  ![뉴럴 네트워크로 근사](images/rnn/05_ssm_nn_after.png)

  - 총 5개 parameter matrix

### RNN 기본 구조

![RNN 기본 구조](images/rnn/06_rnn_structure.png)

## RNN 훈련 방식
- ANN, CNN 처럼 back-propagation 이용
- BPTT : Back-propagation through time (시간에 따른 bp)

## RNN 문제 type
- many-to-many : input, output 여러개
  - back-propagation 할때 각각 봄
  - 번역시 사용
  - RNN에서 많이 사용하진 않음
- **many-to-one** ⭐ 주가 예측 논문들이 쓰는 구조 (과거 N일 → 다음 날 1개)
  - back-propagation 할때 마지막만 봄
  - 시계열 예측시에 사용
- one-to-many
  - 생성시 많이 사용 (문장 생성)
- sequence-to-sequence (seq2seq) = many-to-one + one-to-many

  ![seq2seq](images/rnn/07_seq2seq.png)

---
**다음 →** 기울기 소실(vanishing gradient) → LSTM
