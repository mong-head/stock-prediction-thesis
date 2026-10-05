# 수정주가 (adjusted price)

> [← 개념 목록](README.md)

- 분배락 : 분배금만큼 ETF 순자산이 차감 되는 것 (주식의 배당락)
  - 해당 분배락만큼 ETF 가격과 차트에 반영됨
- 수정주가 : 증자, 배당(분배), 액면분할 등이 발생시, 가격의 연속성 유지를 위해 과거 가격을 같은 비율로 수정
  - 예를들어 삼성전자의 경우 2018년에 200만원이던 주가를 액면분할하여 한주당 10만원정도로 낮추었음. 하지만 현재 차트에서 볼경우 2018년 이전 가격도 10만원 이하의 가격이 나오는데 이게 수정주가가 반영된 것.

---

## 👉 코드로 확인

- [data_load_krx.ipynb](../03_practice/data_load_krx.ipynb) — 2018년 삼성전자 액면분할 원래 가격 vs 수정주가, KODEX 200 분배금
- [data_load_us.ipynb](../03_practice/data_load_us.ipynb) — 애플·SPY `Close` vs `Adj Close`
