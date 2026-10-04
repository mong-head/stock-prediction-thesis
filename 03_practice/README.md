# 🧪 03. 연습 노트북

개념 노트([01_knowledge](../01_knowledge/))에서 정리한 것을 **코드로 한 번 확인**하는 곳.
따로 공부하는 게 아니라 **개념 정리의 마지막 단계**.

> 개념 노트 10장 읽는 것보다 10줄 돌려보는 게 빠를 때가 있다.
> `input_shape=(20, 1)`이 "20일치 × 변수 1개"라는 건 에러를 맞아봐야 몸에 들어온다.

실행 환경: **로컬** (Colab 아님) · Jupyter 노트북 `.ipynb` · VS Code에서 열기

---

## 노트북 목록

| 순서 | 노트북 | 확인하는 것 | 짝 개념 노트 | 상태 |
|---|---|---|---|---|
| 1 | `data_load_krx` | OHLCV가 실제로 어떻게 생겼는지 | — | ⬜ |
| 2 | `data_candlestick` | 캔들 하나에 4개 값이 어떻게 들어가는지 | — | ⬜ |
| 3 | `data_sliding_window` | "과거 20일 → 다음날"이 배열로 어떤 모양인지 | ⬜ 슬라이딩 윈도우 | ⬜ |
| 4 | `model_lstm_minimal` | `input_shape=(20, 1)`의 의미 | [lstm.md](../01_knowledge/lstm.md) ✅ | ⬜ |
| 5 | `eval_time_split` | 왜 무작위로 나누면 안 되는지 | ⬜ 시간 순서 분할 | ⬜ |
| 6 | `eval_metrics` | 정확도의 함정 · F1 · 혼동행렬 | ⬜ 평가 지표 | ⬜ |

> **순서는 이 표가 관리한다.** 파일명에 번호를 붙이지 않는다 — 순서가 바뀌어도 파일명을 안 고치려고.

---

## 파일 이름 규칙

폴더로 나누지 않고 **접두어로 묶는다.** 접두어는 3개만 유지.

| 접두어 | 범위 |
|---|---|
| `data_` | 받기 · 탐색 · 전처리 · 윈도우 |
| `model_` | 모델 돌려보기 |
| `eval_` | 분할 · 평가지표 · 검증 |

```
data_sliding_window.ipynb
model_lstm_minimal.ipynb
eval_time_split.ipynb
```

- 알파벳 정렬이라 `data_` → `eval_` → `model_` 순으로 보인다. 실제 작업 순서와 다르지만 **순서는 위 표가 담당**하므로 괜찮다.
- 접두어를 억지로 비틀어 정렬을 맞추지 않는다. 이름이 어색해지는 게 더 손해.
- **노트북이 한 접두어에 3개 이상 쌓이면** 그때 폴더로 나눈다. 접두어가 그대로 폴더명이 되므로 이동이 기계적이다.

---

## 노트북 한 개의 구조

**30분 안에 끝나는 크기**로 유지한다. 한 노트북에 한 가지만 확인.

```
[마크다운]  📖 개념: [lstm.md](../01_knowledge/lstm.md)
           ❓ 확인할 것: input_shape=(20, 1)이 뭘 의미하나

[코드]      import
[코드]      데이터 또는 더미 배열 만들기
[코드]      핵심 한 가지만 실행
[코드]      결과 출력 — shape 찍어보기

[마크다운]  ✍️ 알게 된 것: (본인이 한두 줄)
```

> **마지막 칸이 제일 중요하다.** 돌려만 보고 끝내면 안 남는다.
> "20이 타임스텝이고 1이 변수 개수구나" 한 줄 적는 게 이 노트북의 결과물.

---

## 🔗 링크 규칙 — 양방향

개념 노트와 노트북은 **서로** 링크를 건다. 둘 다 **상대경로**로. (VS Code 로컬에서도, GitHub에서도 동작)

**개념 노트 맨 아래 — 항상 같은 자리, 같은 제목으로**

```markdown
---

## 👉 코드로 확인

- [model_lstm_minimal.ipynb](../03_practice/model_lstm_minimal.ipynb)
```

**노트북 첫 셀 (마크다운)**

```markdown
📖 개념: [lstm.md](../01_knowledge/lstm.md)
❓ 확인할 것: input_shape=(20, 1)이 뭘 의미하나
```

형식을 고정해두는 이유: **나중에 폴더로 나눌 때 `sed` 한 줄로 일괄 수정**하려고.

```bash
# 예: 노트북이 03_practice/model/ 로 들어갔을 때
grep -rl "03_practice/model_lstm_minimal" ../01_knowledge/   | xargs sed -i 's|03_practice/model_lstm_minimal|03_practice/model/lstm_minimal|g'
grep -rl "\.\./01_knowledge" model/   | xargs sed -i 's|\.\./01_knowledge|../../01_knowledge|g'
```

---

## ⚠️ 커밋 전 확인

- **데이터 파일은 커밋하지 않는다** — `.gitignore`에 `*.csv`, `data/`, `.ipynb_checkpoints/`
- 노트북 출력에 **개인정보가 찍히지 않았는지** 확인 (경로에 계정명이 나올 수 있음)
