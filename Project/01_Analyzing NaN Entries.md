'26. 3. 19. 목  
# Purpose: 반도체 공정에서 수율을 떨어뜨리는 변수를 찾아내는 모델 제작   
## -> 불량 발생 사전 예방 / 불량품 하나가 발생하면 같은 배치 전체가 문제일 수 있음  
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

# 그래프 그리기(결측치 비율 top50 columns를 bar plot으로 시각화)

# secom dataset의 결측치의 비율을 "null_counts" Series로 저장
null_percent = my_data.isnull().mean()*100
# 상위 50개의 데이터만 추출
null_top50 = null_percent.sort_values(ascending=False).head(50)

plt.figure(figsize=(16,8))

# 막대그래프 작성
ax = sns.barplot(x=null_top50.index, y=null_top50.values, palette='viridis')

# 각 막대의 위에 값 레이블 표시
for container in ax.containers:
    ax.bar_label(container, fmt='%.1f%%', padding=3, fontsize=6)
"""
ax.containers: 막대그래프 지칭
fmt='%.1f%%': "%.1f"(소수점 첫째까지 표시), "%%"(뒤에 % 붙임)
padding: 그래프와 간격 띄우기
"""

plt.title("Top 50 Missing Values by Sensor")
plt.xlabel("Sensor ID")
plt.ylabel("Missing percentage(%)")
# x축 눈금의 값을 반시계 방향으로 90도 회전
plt.xticks(rotation=90)

plt.show()
```
>>> output
<img width="80%" alt="01_analyzing NaN entries_output" src="https://github.com/user-attachments/assets/2d69baef-bb28-40f9-866f-56953c5d5995" />

### ACTION: 결측치 45.6% 이상의 열은 삭제하고, 나머지 결측치에 대해서는 중앙값으로 대체 고려
### 1. 결측치 비율이 45.6%지점까지는 완만히 감소하다가, 이후 17.4%로 급격히 감소하는 변곡점 관측 -> 데이터 손실 최소화하면서 분석 신뢰도 확보를 위한 기준을 46.5% 이상으로 지정함.
### 2. 17.4% 이하의 결측치를 갖는 열의 NaN entry는 평균값과 중앙값 중에서 대체
