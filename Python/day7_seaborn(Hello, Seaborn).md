'26. 3. 13. 금  
# 주제: seaborn(Hello, Seaborn)

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
fifa_filepath = "../input/fifa.csv"

fifa_data = pd.read_csv(fifa_filepath, index_col="Date", parse_dates=True)

"""
parse_dates=True -> index_col로 지정된 row(Date)를 날짜 형식으로 인식
"""
```

### 2. Examine the data
```
fifa_data.head()

"""
head() -> 첫 5행 출력 -> 데이터가 적절히 load 됐는지 확인하기 위해 사용
"""
```

### 3. Plot the data
```
plt.figure(figsize=(16,6))

sns.lineplot(data=fifa_data)

"""
figure(figsize=(16,6)): figsize(가로,세로), 단위는 인치, 생략시 기본 작은 사이즈로 출력

lineplot: 선 그래프
data=fifa_data: fifa_data를 사용할 DataFrame으로 지정, index가 x-axis, 나머지가 y-axis로 자동 설정

크기 설정 -> 그래프 그리기: 순서 중요
"""
```
