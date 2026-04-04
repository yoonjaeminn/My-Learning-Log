'26. 4. 4. 토  
# 주제: Intermediate Machine Learning(XGBoost)
  
### 1. Introduction   
* Ensemble method: 일반적인 렌덤포레스트 방식을 지칭함 / 여러 모델의 예측을 합치는 방식    
* XGBoost: 표준 표 횽식 데이터를 처리하기 위한 선도적인 소프트웨어 라이브러리로 매개변수 조정을 통해 매우 정확한 모델 학습이 가능   
   
### 2. Gradient Boosting   
* 모델을 ensemble에 반복적으로 추가하기위해 사이클을 거치는 방식    
* 예측이 부정확할 수 있는 단일 모델로 ensemble 시작 / 후속적인 추가가 에러를 감소시킴   
* 사이클   
  - 현재 ensemble을 이용하여 각 관측에 대한 예측을 생성하고, 모든 예측을 ensemble에 추가함.   
  - 이 예측들을은 손실 함수를 계산하는 데 사용됨    
  - 손실함수를 사용하여 ensemble에 추가될 새 모델을 fit함.   
  - 구체적으로, 새로운 모델을 ensemble에 추가하는 것이 loss를 감소시킬 수 있도록 parameters를 결정함.    
  - 마지막으로, 새로운 모델을 ensemble에 추가함.   
  - 위 과정을 반복함.    
    
### 3. Example   
```
from xgboost import XGBRegressor

my_model = XGBRegressor()
my_model.fit(X_train, y_train)
```
```
from sklearn.metrics import mean_absolute_error

predictions = my_model.predict(X_valid)
print("Mean Absolute Error: " + str(mean_absolute_error(predictions, y_valid)))
>>>
Mean Absolute Error: 241041.5160392121
```

### 4. Parameter Tuning   
* XGBoost는 정확도와 스피드에 크게 영향을 줄 수 있는 몇개의 파라미터가 있음.   
* `n_estimators`: 얼마나 많은 모델을 앙상블에 추가할 것인가   
  - 너무 낮으면, underfitting    
  - 너무 높으면, overfitting
```
my_model = XGBRegressor(n_estimators=500)
my_model.fit(X_train, y_train)
```
* `earlt_stopping_rounds`: 이상적인 n_estimators를 자동으로 찾는 방식을 제공함.   
  - validation score의 향상이 멈췄을 때, 모델의 반복을 정지함.   
  - 높은 n_estimators와 earlt_stopping_rounds를 사용하는 것은 최적의 타이밍을 찾는데 좋은 방식임.   
  - earlt_stopping_rounds=?: 최소한 ?만큼의 반복 이후에 반복을 정지함.   
  - earlt_stopping_rounds를 사용할때, validation scores를 계산하기 위해 `eval_set`파라미터를 사용해야함.   
```
my_model = XGBRegressor(n_estimators=500)
my_model.fit(X_train, y_train, 
             early_stopping_rounds=5, 
             eval_set=[(X_valid, y_valid)],
             verbose=False)
```

* `learning_rate`: 각각의 예측값을 단순히 추가하지 않고, 작은 숫자(learning rate)를 곱한 뒤 추가하는 방식    
  - 과적합을 피하면서 n_estimators를 크게 설정할 수 있음   
  - earlt_stopping_rounds를 사용하면, 적절한 수의 트리가 자동으로 결정됨   
  - 일반적으로 작은 learning_rate와 큰 n_estimators를 사용하면 정확도는 높아지지만, 시간이 길어짐.   
  - 디폴트값으로 0.1을 가짐.   
```
my_model = XGBRegressor(n_estimators=1000, learning_rate=0.05)
my_model.fit(X_train, y_train, 
             early_stopping_rounds=5, 
             eval_set=[(X_valid, y_valid)], 
             verbose=False)
```

* `n_jobs`: 런타임을 고려하는 대규모 데이터셋에서 병렬처리를 사용하여 모델을 빠르게 구축 가능   
  - 일반적으로 n_jobs를 컴퓨터의 코어수와 동일하게 설정.    
  - 소규모 데이터셋에는 의미 없음.   
```
my_model = XGBRegressor(n_estimators=1000, learning_rate=0.05, n_jobs=4)
my_model.fit(X_train, y_train, 
             early_stopping_rounds=5, 
             eval_set=[(X_valid, y_valid)], 
             verbose=False)
```













