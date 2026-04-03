'26. 4. 3. 금  
# 주제: Intermediate Machine Learning(Cross-Validation)
  
### 1. Introduction   
* 일반적으로, validation set이 클수록 모델 성능 측정에 randomness(noise)가 적고 신뢰도가 높음    

### 2. What is cross-validation?   
* cross-validation을 위해서, 모델링을 데이터의 서로 다른 subset에서 진행함 -> 모델 품질의 다양한 측정을 얻을 수 있음   
* 예를들어, 데이터를 20%씩 할당해 5개로 나누고, 각각의 조각을 `fold`라고 함 -> 이후 각각의 fold에 실험   
  - 1회차: 1번 fold(검증) / 나머지 fold(학습)   
  - 2회차: 2번 fold(검증) / 나머지 fold(학습) ...   
  - 모든 데이터에 대해 모델 검증 가능    

### 3. When should you use cross-validation?   
* cross-validationd은 모델 품질의 더 정확한 측정이 가능 -> 아주 많은 모델 decisions을 만들때 특히 중요함   
* 여러 모델을 평가해야하기 때문에, 작동 시간이 오래 걸림   
* So, when?   
  - 추가적인 계산이 부담되지 않는 작은 데이터셋   
  - 큰 데이터는 단일 검증으로 충분함. 교차검증보다 더 빠르고 데이터가 충분함.   
  - 모델이 몇분 내로 작동 된다면, cross-validationd을 사용하는게 나음.    
* 만약 각각의 평가가 같은 결과는 반환한다면, 단일 검증으로 충분한 것임.

### 3. Example   
```
from sklearn.ensemble import RandomForestRegressor
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer

my_pipeline = Pipeline(steps=[('preprocessor', SimpleImputer()),
                              ('model', RandomForestRegressor(n_estimators=50, random_state=0))])
```
```
from sklearn.model_selection import cross_val_score

# Multiply by -1 since sklearn calculates *negative* MAE
# cv -> fold 수 결정
# "scoring" parameter -> 모델 평가 방식 선택
scores = -1 * cross_val_score(my_pipeline, X, y,
                              cv=5,
                              scoring='neg_mean_absolute_error')

print("MAE scores:\n", scores)
>>>
MAE scores:
 [301628.7893587  303164.4782723  287298.331666   236061.84754543
 260383.45111427]

"""
scikit learn은 클수록 우수한 것으로 평가함
위 규칙을 만족하기 위해서 오차값에는 -를 붙여 출력함
익숙한 양수 형태로 만들기위해 -1을 곱함
"""
```
```
print("Average MAE score (across experiments):")
print(scores.mean())
>>>
Average MAE score (across experiments):
277707.3795913405
```

### 3. 코드 예시: 최적의 tree수 파악   
```
from sklearn.ensemble import RandomForestRegressor
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer

my_pipeline = Pipeline(steps=[
    ('preprocessor', SimpleImputer()),
    ('model', RandomForestRegressor(n_estimators=50, random_state=0))
])

def get_score(n_estimators):
    """Return the average MAE over 3 CV folds of random forest model.
    
    Keyword argument:
    n_estimators -- the number of trees in the forest
    """
    my_pipeline = Pipeline(steps=[
        ('preprocessor', SimpleImputer()),
        ('model', RandomForestRegressor(n_estimators, random_state=0))
    ])
    scores = -1 * cross_val_score(my_pipeline, X, y,
                              cv=3,
                              scoring='neg_mean_absolute_error')
    return scores.mean()

results = {}

for i, val in enumerate([50,100,150,200,250,300,350,400]):
    results[val] = get_score(val)

import matplotlib.pyplot as plt
%matplotlib inline

plt.plot(list(results.keys()), list(results.values()))
plt.show()
```
