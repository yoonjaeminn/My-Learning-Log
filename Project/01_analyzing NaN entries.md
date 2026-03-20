'26. 3. 19. 목  
# Analyzing NaN Entries:  
### WHY: 데이터 분석 전처리의 필수 작업으로, 결측치가 많은 열을 제외하거나, 대체하여 분석의 신뢰도를 향상
### kaggle의 "*UCI SECOM Dataset*" 활용
```
import pandas as pd
pd.plotting.register_matplotlib_converters()
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns

my_filepath = "/kaggle/input/datasets/paresh2047/uci-semcom/uci-secom.csv"
my_data = pd.read_csv(my_filepath)

# secom dataset의 결측치의 비율을 "null_counts" Series로 저장
null_counts = my_data.isnull().mean()*100
# 상위 50개의 데이터만 추출
null_top50 = null_counts.sort_values(ascending=False).head(50)

plt.figure(figsize=(12,6))

# 막대그래프 작성
ax = sns.barplot(x=null_top50.index, y=null_top20.values, palette='viridis')

# 각 막대의 위에 값 레이블 표시
ax.bar_label(ax.containers[0], fmt='%.lf%%', padding=3, fontsize=6)
"""
ax.containers[0]: 막대그래프 지칭
fmt='%.lf%%': "%.lf"(소수점 첫째까지 표시), "%%"(뒤에 % 붙임)
padding: 그래프와 간격 띄우기
"""

plt.title("Top 20 Missing Values by Sensor")
plt.xlabel("Sensor ID")
plt.ylabel("Missing percentage(%)")
# x축 눈금의 값을 반시계 방향으로 90도 회전
plt.xticks(rotation=90)
```
>>> output
<img src="https://github.com/user-attachments/assets/1fff947b-8d73-42c2-ac09-8c4a59c0471e" width="80%">

### ACTION: 결측치 46% 이상의 열은 삭제하고, 나머지 결측치에 대해서는 중앙값으로 대체 고려
### 1. 결측치 비율이 46%지점까지는 완만히 감소하다가, 이후 17%로 급격히 감소하는 변곡점 관측 -> 데이터 손실 최소화하면서 분석 신뢰도 확보를 위한 기준을 46% 이상으로 지정함.
### 2. 17% 이하의 결측치를 갖는 열의 NaN entry는 평균값과 중앙값 중에서 대체
