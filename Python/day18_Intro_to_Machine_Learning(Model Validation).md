'26. 3. 24. 화  
# 주제: Intro to Machine Learning(Model Validation)
  
### 1. What is Model Validation   
- Mean Absolute Error(MAE): error = actual - predicted   
- measure of model quality = mean(MAE)
```
from sklearn.metrics import mean_absolute_error

predicted_home_prices = melbourne_model.predict(X)
mean_absolute_error(y, predicted_home_prices)
```
    
### 2. The Problem with "In-Sample" Scores  
- 모델의 가치는는 새로운 데이터의 예측 능력에 달려있음   
- 따라서, 모델 제작에 사용되지 않은 데이터로 퍼포먼스를 측정해야함   
- 가장 확실한 방법은 모델링 과정에서 약간의 데이터를 배제하고, 이것을 모델의 정확도 테스트에 활용하는 것   
- 이러한 데이터를 **validation data**라고 함   
   
### 3. Coding It  
**train_test_split**: 데이터를 두 조각으로 나누어서, 하나는 훈련용, 하나는 성능 측정용으로 사용   
```
from sklearn.model_selection import train_test_split

# split data into training and validation data, for both features and target
# The split is based on a random number generator. Supplying a numeric value to
# the random_state argument guarantees we get the same split every time we
# run this script.
train_X, val_X, train_y, val_y = train_test_split(X, y, random_state = 0)
# Define model
melbourne_model = DecisionTreeRegressor()
# Fit model
melbourne_model.fit(train_X, train_y)

# get predicted prices on validation data
val_predictions = melbourne_model.predict(val_X)
print(mean_absolute_error(val_y, val_predictions))
```
