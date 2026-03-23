'26. 3. 23. 월  
# 주제: Intro to Machine Learning(Your First Machine Learning Model)
  
### 1. Selecting Data for Modeling
```
import pandas as pd

melbourne_file_path = '../input/melbourne-housing-snapshot/melb_data.csv'
melbourne_data = pd.read_csv(melbourne_file_path) 
melbourne_data.columns

>>>
Index(['Suburb', 'Address', 'Rooms', 'Type', 'Price', 'Method', 'SellerG',
       'Date', 'Distance', 'Postcode', 'Bedroom2', 'Bathroom', 'Car',
       'Landsize', 'BuildingArea', 'YearBuilt', 'CouncilArea', 'Lattitude',
       'Longtitude', 'Regionname', 'Propertycount'],
      dtype='object')
```
```
melbourne_data = melbourne_data.dropna(axis=0)

"""
.dropna(axis=?): axis=0 -> NaN이 있는 행 삭제 / axis=1 -> NaN이 있는 열 삭제
"""
```

### 2. Selecting The Prediction Target   
예측하고자 하는 열을 선택하기 위해 dot notation을 사용하고, 그 열을 **prediction target**이라 함.   
```
y = melbourne_data.Price
```

### 3. Choosing "Features"  
**features**: 모델에 입력되고 예측을 위해 사용되는 열들   
```
melbourne_features = ['Rooms', 'Bathroom', 'Landsize', 'Lattitude', 'Longtitude']

X = melbourne_data[melbourne_features]

X.describe()
X.head()

"""
describe와 head를 이용해 데이터를 시각정으로 점검하는 것은 매우 중요함. 종종 특이사항을 발견 할 수 있음.
"""
```

### 4. Building Your Model   
**scikit-learn library**: sklearn으로 부르며, 데이터프레임에 저장된 데이터를 모델링 하는 유명한 라이브러리임.   
- define: 어떤 종류의 모델을 사용할지 결정   
- fit: 패턴 파악. 가장 중요한 부분   
- predict: 예측하기   
- evaluation: 모델 예측의 정확도 평가
```
from sklearn.tree import DecisionTreeRegressor

# Define model. Specify a number for random_state to ensure same results each run
melbourne_model = DecisionTreeRegressor(random_state=1)

# Fit model
melbourne_model.fit(X, y)

# Prediction & Evaluation
print("Making predictions for the following 5 houses:")
print(X.head())
print("The predictions are")
print(melbourne_model.predict(X.head()))

>>>
Making predictions for the following 5 houses:
   Rooms  Bathroom  Landsize  Lattitude  Longtitude
1      2       1.0     156.0   -37.8079    144.9934
2      3       2.0     134.0   -37.8093    144.9944
4      4       1.0     120.0   -37.8072    144.9941
6      3       2.0     245.0   -37.8024    144.9993
7      2       1.0     256.0   -37.8060    144.9954
The predictions are
[1035000. 1465000. 1600000. 1876000. 1636000.]
```
