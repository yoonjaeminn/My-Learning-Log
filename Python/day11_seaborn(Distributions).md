'26. 3. 17. 화  
# 주제: seaborn(Distributions)

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
iris_filepath = "../input/iris.csv"

iris_data = pd.read_csv(iris_filepath, index_col="Id")

iris_data.head()
>>>
	Sepal Length (cm)	Sepal Width (cm)	Petal Length (cm)	Petal Width (cm)	Species
Id					
1	5.1	3.5	1.4	0.2	Iris-setosa
2	4.9	3.0	1.4	0.2	Iris-setosa
3	4.7	3.2	1.3	0.2	Iris-setosa
4	4.6	3.1	1.5	0.2	Iris-setosa
5	5.0	3.6	1.4	0.2	Iris-setosa
```

### 2. Histograms
```
sns.histplot(iris_data['Petal Length (cm)'])

"""
히스토그램차트 작성: 대상이 될 열을 지정 해야함
"""
```

### 3. Density plots
```
sns.kdeplot(data=iris_data['Petal Length (cm)'], shade=True)
  
"""
kernel density estimate (KDE) plot을 작성함
  - shade=True: 곡선 아래 영역를 색으로 채움
"""
```

### 3. 2D KDE plots
```
sns.jointplot(x=iris_data['Petal Length (cm)'], y=iris_data['Sepal Width (cm)'], kind="kde")
  
"""
이차원 KDE 차트 작성
    - kind="kde": 차트를 kde 형식으로 작성함. scatter, reg, hex, hist 등
    - fill=True: 추가 작성하면, 등고선 내부 색으로 채움
"""
```

### 4. Color-coded plots
```
sns.histplot(data=iris_data, x='Petal Length (cm)', hue='Species')

"""
x축에 Petal Length (cm) 데이터를 갖고 Species별로 색을 다르게 지정하는 히스토그램 차트 작성
"""
```
```
sns.kdeplot(data=iris_data, x='Petal Length (cm)', hue='Species', shade=True)

"""
x축에 Petal Length (cm) 데이터를 갖고 Species별로 색을 다르게 지정하며, 곡선 밑을 색으로 채운 KDE 차트 작성
"""
```
