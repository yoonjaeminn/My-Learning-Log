'26. 3. 16. 월  
# 주제: seaborn(Scatter Plots)

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

### 1. Load and examine the data
```
insurance_filepath = "../input/insurance.csv"

insurance_data = pd.read_csv(insurance_filepath)

insurance_data.head()
>>>
	age	sex	bmi	children	smoker	region	charges
0	19	female	27.900	0	yes	southwest	16884.92400
1	18	male	33.770	1	no	southeast	1725.55230
2	28	male	33.000	3	no	southeast	4449.46200
3	33	male	22.705	0	no	northwest	21984.47061
4	32	male	28.880	0	no	northwest	3866.85520
```

### 2. Scatter plots
```
sns.scatterplot(x=insurance_data['bmi'], y=insurance_data['charges'])

"""
분산형 차트 작성: x축은 bmi, y축은 charges
"""
```
```
sns.regplot(x=insurance_data['bmi'], y=insurance_data['charges'])

"""
회귀선이 그려진 분산형 차트 작성
"""

### 3. Color-coded scatter plots
```
```
sns.scatterplot(x=insurance_data['bmi'], y=insurance_data['charges'], hue=insurance_data['smoker'])

"""
hue에 지정된 서로 다른 변수들은 서로 다른 색의 점으로 표현
"""
```
```
sns.lmplot(x="bmi", y="charges", hue="smoker", data=insurance_data)

"""
서로 다른 색의 데이터 분산 집합에 대한 회귀선
"""
```
```
sns.swarmplot(x=insurance_data['smoker'], y=insurance_data['charges'])

"""
categorical swamplot 작성: 연속된 변수들이 아닌 categorical한 변수를 메인 축 중 하나에 배치한 분산형 차트
"""
```
