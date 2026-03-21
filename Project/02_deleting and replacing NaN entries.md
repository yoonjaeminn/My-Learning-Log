'26. 3. 21. 토  
# Deleting and Replacing NaN entries:  
### WHY: 데이터 분석 전처리의 필수 작업으로, 결측치가 많은 열을 제외하거나, 대체하고 상수열을 제거하여 분석의 신뢰도를 향상
### kaggle의 "*UCI SECOM Dataset*" 활용
```
# constant columns 제거
n_unique = my_data.nunique()
constant_cols = n_unique[n_unique <= 1].index

print(f"상수 열의 개수: {len(constant_cols)}")
print(f"상수 열 목록: {constant_cols.tolist()}")
""".tolist(): 판다스나 넘파이의 인덱스나 시리즈 객체를 파이썬 기본 리스트 형태로 변환
"""

my_data_dropped1 = my_data.drop(columns=constant_cols)

print(f"제거된 상수 열 개수: {len(constant_cols)}")
print(f"원본 열 개수: {my_data.shape[1]}")
print(f"제거 후 열 개수: {my_data_dropped.shape[1]}")
""".shape[?}: 0 -> 행이 개수, 1 -> 열의 개수
"""

# 결측치 45.6% 이상 columns 제거
cols_to_drop = null_percent[null_percent >= 45.6].index

print(f"결측치 45.6% 이상 열 개수: {len(cols_to_drop)}")

my_data_dropped2 = my_data_dropped1.drop(columns=cols_to_drop)

print(f"원본 열 개수: {my_data.shape[1]}")
print(f"최종 열 개수: {my_data_dropped2.shape[1]}")

# 나머지 결측치 중앙값으로 대체: 반도체 제조 공정의 센서 데이터를 고려할떄, 이상치와 평균의 함정의 리스크를 피하기 위해 중앙값으로 대체
final_data = my_data_dropped2.fillna(my_data_dropped2.median(numeric_only=True))
""".fillna(?): dataframe 내의 NaN으로 표시된 모든 칸을 ?로 대체함
   numeric_only=True: 숫자 데이터만 계산함 -> 문자 데이터 계산 시도로 인한 에러 방지
"""

print(f"결측치 수: {final_data.isnull().sum().sum()}")
final_data.head(10)

# final_data의 최대 결측치를 가졌던 columns 3개의 데이터 분포 조사
fig, axes = plt.subplots(2, 2, figsize=(16, 12))
"""
fig: plot ouput 전체를 관리하는 객체
axes: 각각의 칸들을 답고 있는 리스트
plt.subplot(a, b): a -> 행 개수, b -> 열 개수  
"""
sns.histplot(final_data['562'], kde=True, ax=axes[0, 0], color='skyblue')
axes[0, 0].set_title("Distribution of Column '562'", fontsize=14)
axes[0, 0].set_xlabel("Value")

sns.histplot(final_data['563'], kde=True, ax=axes[0, 1], color='salmon')
axes[0, 1].set_title("Distribution of Column '563'", fontsize=14)
axes[0, 1].set_xlabel("Value")

sns.histplot(final_data['564'], kde=True, ax=axes[1, 0], color='lightgreen')
axes[1, 0].set_title("Distribution of Column '564'", fontsize=14)
axes[1, 0].set_xlabel("Value")

# 마지막 칸[1,1] 숨기기
axes[1, 1].axis('off')

# 그래프 간 간격 자동 조절
plt.tight_layout()
plt.show()
```
>>> output
<img width="627" height="469" alt="02_deleting and replacing NaN" src="https://github.com/user-attachments/assets/25f6380d-aa91-454a-bfc9-66bbb0c5d023" />

-> 중앙값 집중이 매우 높은 것으로 보이나, 중앙값 대체는 각 열마다 최대 17.4%로 이루어 졌으므로, 대체로 인한 중앙값 스파이크가 아닌, 원본데이터가 중앙값에 매우 집중돼 있었음을 알 수 있으므로, 문제 없음.  
>>>   
### ACTION: 데이터(Pass/Fail) 불균형 분석   
- pass와 fail의 비율을 확인하여 데이터 불균형 정도를 확인 
- 분석 결과에 따라 향후 샘플링 전략(SMOTE 등) 도입 여부 검토. / fail이 너무 적을시, 일반적 분석으로 일반화하기 어려움       
