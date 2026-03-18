'26. 3. 18. 수  
# 주제: seaborn(Choosing Plot Types and Custom Styles)

```
# 1. Summary
Trends: 변화의 패턴
  sns.lineplot - Line charts are best to show trends over a period of time, and multiple lines can be used to show trends in more than one group.

Relationship
  sns.barplot - Bar charts are useful for comparing quantities corresponding to different groups.
  sns.heatmap - Heatmaps can be used to find color-coded patterns in tables of numbers.
  sns.scatterplot - Scatter plots show the relationship between two continuous variables; if color-coded, we can also show the relationship with a third categorical variable.
  sns.regplot - Including a regression line in the scatter plot makes it easier to see any linear relationship between two variables.
  sns.lmplot - This command is useful for drawing multiple regression lines, if the scatter plot contains multiple, color-coded groups.
  sns.swarmplot - Categorical scatter plots show the relationship between a continuous variable and a categorical variable.

Distribution
  sns.histplot - Histograms show the distribution of a single numerical variable.
  sns.kdeplot - KDE plots (or 2D KDE plots) show an estimated, smooth distribution of a single numerical variable (or two numerical variables).
  sns.jointplot - This command is useful for simultaneously displaying a 2D KDE plot with the corresponding KDE plots for each individual variable.
```
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

### 2. Changing styles with seaborn
```
sns.set_style("dark")

plt.figure(figsize=(12,6))
sns.lineplot(data=spotify_data)

"""
sns.set_style("?"): ?(darkgrid, whitegrid, dark, white, ticks)theme으로 차트 작성
    - dark: 그림영역 어둡게
    - white: 그림영역 흰색
    - white or dark + grid: 격자선 추가
    - ticks: white에 축 눈금 추가
"""
```
