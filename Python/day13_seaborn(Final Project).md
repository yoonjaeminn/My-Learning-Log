'26. 3. 19. 목  
# 주제: seaborn(Final Project)

### 1. Kaggle Datasets
```
https://www.kaggle.com/datasets

"""
위 url을 통해서 캐글의 datasets collection을 사용할 수 있음. 개인 프로젝트에 활용
"""
```

### 2. Use your own dataset
```
캐글의 Data Visualization, Final Project, exercise 에서 개인 데이터를 사용하기 위해서는
kaggle.com/datasets
위 url에서 "New Dataset" 버튼을 클릭하여 업로드 할 수 있음.

캐글 Datasets에 업로드한 file은 자동으로 CSV 형식으로 변환됨.
```

### 3. My own Project(secom dataset): 데이터셋의 결측치 시각화
```
# my_data(secom dataset)의 결측치의 비율을 null_counts Series로 저장
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
