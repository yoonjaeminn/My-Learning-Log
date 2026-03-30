'26. 3. 30. 월  
# 주제: Intermediate Machine Learning(Introduction)
  
### 1. Introduction  
- missing values, categorical variables   
- pipeline   
- crossvalidation  
- XGBoost    
- leakage   

### 1. Examples    
```
from sklearn.ensemble import RandomForestRegressor

# Define the models
model_1 = RandomForestRegressor(n_estimators=50, random_state=0)
model_2 = RandomForestRegressor(n_estimators=100, random_state=0)
model_3 = RandomForestRegressor(n_estimators=100, criterion='absolute_error', random_state=0)
model_4 = RandomForestRegressor(n_estimators=200, min_samples_split=20, random_state=0)
model_5 = RandomForestRegressor(n_estimators=100, max_depth=7, random_state=0)

models = [model_1, model_2, model_3, model_4, model_5]

"""
n_estimators=? : 의사결정 트리의 총 개수 -> 숫자가 클수록 성능이 안정화되지만, 학습 시간과 메모리 사용량 늘어남
criterion=? : 나무의 가지를 칠때, 어떤 기준으로 데이터를 나눌지 결정하는 지표
              squared_error(기본값, 평균제곱오차(MSE)를 최소화,큰 오차에 민감), absolute_erroe(평균절대오차(MAE)최소화, 이상치 영향 덜 받음)
min_sample_split=? : 노드(나무의 가지)를 더 나눌지, 멈출지를 결정하는 조건 / 한 노드안에 데이터가 최소 몇 개 이상이어야 분할라지 설정
                     -> 크게 설정할 수록 나무가 깊게 자라는 과적합 방지 효과
max_depth=? : 나무의 최대 깊이 -> 높을수록 복잡한 패턴을 학습하지만, 과적합 위험이 있음
"""
```
```
from sklearn.metrics import mean_absolute_error

# Function for comparing different models
def score_model(model, X_t=X_train, X_v=X_valid, y_t=y_train, y_v=y_valid):
    model.fit(X_t, y_t)
    preds = model.predict(X_v)
    return mean_absolute_error(y_v, preds)

for i in range(0, len(models)):
    mae = score_model(models[i])
    print("Model %d MAE: %d" % (i+1, mae))

>>>
Model 1 MAE: 24015
Model 2 MAE: 23740
Model 3 MAE: 23528
Model 4 MAE: 23996
Model 5 MAE: 23706
```
