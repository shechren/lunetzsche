---
title: Python - Plotly (2)
date: 2023-11-17 22:39:58 +/-09:00
category: [python/pandas]
tag: [python, pandas, plotly]
---

이번에는 json처럼 된 데이터를 분석해보자.

Bar형으로 보기 위해서는 평소랑 똑같이 쓰면 된다. 다만 행과 열을 바꿔줘야 시각적으로 더 좋다.

```python
import csv
import pandas as pd
import plotly.express as px # 시각화 라이브러리

data = {
    '이나르': {'국어': 90, '수학': 100, '사회': 80, '과학': 100},
    '라피스': {'국어': 100, '수학': 90, '사회': 100, '과학': 80},
    '스노우': {'국어': 85, '수학': 95, '사회': 85, '과학': 95},
    '스페이드': {'국어': 95, '수학': 80, '사회': 95, '과학': 80}
}

df = pd.DataFrame(data)

# 행과 열을 바꾸어 줌
df = df.T

# 인덱스 이름 설정
df.index.name = '이름'

# plotly를 사용하여 바 그래프 그리기
fig = px.bar()

# 그래프 출력
fig.show()
```

![assets/postingImage/python-plotly-score1.png](/assets/postingImage/python-plotly-score1.png)

선 그래프를 그리는 방법은 조금 더 복잡하다. graph_objects를 불러오고, 

```python
import csv
import pandas as pd
import plotly.graph_objects as go

# 데이터 생성
data = {
    '이나르': {'국어': 90, '수학': 100, '사회': 80, '과학': 100},
    '라피스': {'국어': 100, '수학': 90, '사회': 100, '과학': 80},
    '스노우': {'국어': 85, '수학': 95, '사회': 85, '과학': 95},
    '스페이드': {'국어': 95, '수학': 80, '사회': 95, '과학': 80}
}

# 데이터프레임 생성
df = pd.DataFrame(data)

# 행과 열을 바꾸어 줌
df = df.T

# 인덱스 이름 설정
df.index.name = '이름'

# plotly를 사용하여 선 그래프 그리기
fig = go.Figure()

for name in df.index:
    fig.add_trace(go.Scatter(x=df.columns, y=df.loc[name], mode='lines', name=name))

# 그래프 레이아웃 설정
fig.update_layout(title='과목별 성적',
                  xaxis_title='과목',
                  yaxis_title='점수')

# 그래프 출력
fig.show()
```
1. go.Figure()로 그릴 인스턴스를 생성한다.
2. for문을 통해 인덱스(이름)를 반복한다. 
3. add_trace로 새로운 트레이스를 추가한다. go.Scatter는 선 그래프를 나타낸다. x축은 과목, y축은 성적이다.
4. layout을 설정해준다.

![assets/postingImage/python-plotly-score2.png](/assets/postingImage/python-plotly-score2.png)

---