'26. 3. 15. 일  
# 주제: seaborn(Bar Charts and Heatmaps)

```
# 데이터 제공
import pandas as pd

# pandas와 matplotlib이 날짜 데이터를 주고 받을 때 호환되고록 연결, 시계열 데이터 시각화중 오류 방지
pd.plotting.register_matplotlib_converters()

# 기본적인 시각화 라이브러리
import matplotlib.pyplot as plt

# 주피터 노트북에서 차트를 별도 창이 아닌, 셀 바로 아래에 출력
%matplotlib inline

# matplotlib 기반 고급 시각화 라이브러리 / 더 간결한 코드, 더 예쁜 차트 / 통계적 시각화 특화
import seaborn as sns
```

### 1. Load the data
```
flight_filepath = "../input/flight_delays.csv"

flight_data = pd.read_csv(flight_filepath, index_col="Month")

"""
flight_data.index = range(1,13)
flight_data.index.name = "Month"
"""
```

### 2. Bar chart
```
plt.figure(figsize=(10,6))

plt.title("Average Arrival Delay for Spirit Airlines Flights, by Month")

sns.barplot(x=flight_data.index, y=flight_data['NK'])

plt.ylabel("Arrival delay (in minutes)")

"""
sns.barplot(x=flight_data.index, y=flight_data['NK'])
  
  - sns.barplot: seaborn을 이용해 막대그래프 그림
    
  - x=flight_data.index: x축에 index 데이터 사용
    -> flight_data['Month']: 오류 발생, 이미 index로 지정됨
    
  - y=flight_data['NK']: y축에 NK열 데이터 사용
      x,y 축 설정을 반대로 하면, 가로로 누윈 막대그래프 생성
"""
```

### 3. Heatmap
```
plt.figure(figsize=(14,7))

plt.title("Average Arrival Delay for Each Airline, by Month")

sns.heatmap(data=flight_data, annot=True)

plt.xlabel("Airline")

"""
sns.heatmap(data=flight_data, annot=True)

  - sns.heatmap: seaborn을 이용해 히트맵 그림

  - data=flight_data: 차트에 flight_data의 entries를사용함

  - annot=True: 히트맵에 숫자를 표시, 생략시 색만 표시
"""
```
