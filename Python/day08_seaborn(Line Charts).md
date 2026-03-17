'26. 3. 14. 토  
# 주제: seaborn(Line Charts)

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
spotify_filepath = "../input/spotify.csv"

spotify_data = pd.read_csv(spotify_filepath, index_col="Date", parse_dates=True)

"""
parse_dates=True -> index_col로 지정된 row(Date)를 날짜 형식으로 인식
"""
```

### 2. Examine the data
```
spotify_data.head()

"""
head() -> 첫 5행 출력 -> 데이터가 적절히 load 됐는지 확인하기 위해 사용
빈 elements는 NaN으로 출력됨
"""
```
```
spotify_data.tail()

"""
tail() -> 마지막 5행 출력 -> 데이터가 적절히 load 됐는지 확인하기 위해 사용
빈 elements는 NaN으로 출력됨
"""
```

### 3. Plot the data
```
plt.figure(figsize=(14,6))

plt.title("Daily Global Streams of Popular Songs in 2017-2018")

sns.lineplot(data=spotify_data)

"""
plt.figure(figsize=(16,6)): figsize(가로,세로), 단위는 인치, 생략시 기본 작은 사이즈로 출력

plt.title("제목"): 차트에 제목을 입력함, 생략시 제목 없이 출력

sns.lineplot(data=?): 선 그래프
  data=spotify_data: spotify_data를 사용할 DataFrame으로 지정, index가 x-axis, 나머지가 y-axis로 자동 설정

크기 설정 -> 그래프 그리기: 순서 중요
"""
```

### 4. Plot a subset of the data
```
list(spotify_data.columns)

"""
list(DataFrame.columns) -> DataFrame의 모든 열의 이름을 출력
"""
```
```
plt.figure(figsize=(14,6))

plt.title("Daily Global Streams of Popular Songs in 2017-2018")

sns.lineplot(data=spotify_data['Shape of You'], label="Shape of You")

sns.lineplot(data=spotify_data['Despacito'], label="Despacito")

plt.xlabel("Date")

plt.ylabel("Streams")

"""
sns.lineplot(data=spotify_data['Shape of You'], label="Shape of You"):
  spotify-data에서 'Shape of You' 단일 Series에 대한 선그래프 입력
  label="?" -> 해당 그래프의 범례에 ?로 이름 표시
  아래 코드도 동일

plt.xlabel("?") -> x축 명 지정
plt.ylabel("?") -> y축 명 지정
  y축 명 생략시, 가장 처음 입력된 Series의 name이 y축 명으로 지정됨
"""
```
