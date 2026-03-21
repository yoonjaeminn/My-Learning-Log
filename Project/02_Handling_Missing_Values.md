'26. 3. 21. 토  
# Deleting and Replacing NaN entries:  
### WHY: 
SECOM 데이터는 방대한 columns를 포함하고 있어, 유의미한 분석을 위해 불필요한 열을 제거하고 데이터의 무결성을 확보하는 과정이 필수적. 상수열과 결측치가 많은 열을 제거하거나 대체하여 분석의 신뢰도를 향상
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
"""
>>>
상수 열의 개수: 116
상수 열 목록: ['5', '13', '42', '49', '52', '69', '97', '141', '149', '178', '179', '186', '189', '190', '191', '192', '193', '194', '226', '229', '230', '231', '232', '233', '234', '235', '236', '237', '240', '241', '242', '243', '256', '257', '258', '259', '260', '261', '262', '263', '264', '265', '266', '276', '284', '313', '314', '315', '322', '325', '326', '327', '328', '329', '330', '364', '369', '370', '371', '372', '373', '374', '375', '378', '379', '380', '381', '394', '395', '396', '397', '398', '399', '400', '401', '402', '403', '404', '414', '422', '449', '450', '451', '458', '461', '462', '463', '464', '465', '466', '481', '498', '501', '502', '503', '504', '505', '506', '507', '508', '509', '512', '513', '514', '515', '528', '529', '530', '531', '532', '533', '534', '535', '536', '537', '538']
제거된 상수 열 개수: 116
원본 열 개수: 592
제거 후 열 개수: 476
"""

# 결측치 45.6% 이상 columns 제거
cols_to_drop = null_percent[null_percent >= 45.6].index

print(f"결측치 45.6% 이상 열 개수: {len(cols_to_drop)}")

my_data_dropped2 = my_data_dropped1.drop(columns=cols_to_drop)

print(f"원본 열 개수: {my_data.shape[1]}")
print(f"최종 열 개수: {my_data_dropped2.shape[1]}")
"""
>>>
결측치 45.6% 이상 열 개수: 32
원본 열 개수: 592
최종 열 개수: 444
"""

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

-> 562열과 564열의 분포를 볼떄, 17.4%의 중앙값 대체가 더 이뤄진 걸 고려하더라도, 스파이크가 매우 심함. 563열의 중앙값을 확인한 결과, 중앙값 대체로 발생한 거짓 쌍봉형 데이터가 생성됨. 이러한 데이터들은 데이터 분석 관점에서 쓸모 없거나 방해되는 데이터가 될 수 있음.   
     
>>>   
### ACTION: 스파이크 리스트를 추출하고, 각 열들의 불량 데이터도 모두 스파이크 지점에 몰려 있는지 확인하여, 각 열의 제거/유지 고려 필요
