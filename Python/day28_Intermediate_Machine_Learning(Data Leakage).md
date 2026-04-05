'26. 4. 5. 일  
# 주제: Intermediate Machine Learning(Data Leakage)
  
### 1. Introduction   
* `Data leakage`   
  - training data가 타겟에 대한 정보를 포함하지만, 예측할때 비슷한 데이터가 사용가능하지 않으면 발생      
  - training set이나 validation data에는 좋은 성능을 보이지만, 실제로는 성능이 매우 떨어짐   
* `Target leakage`   
  - predictor가 예측할때는 사용할 수 없는 데이터를 포함할때 발생   
  - timing과 chronological order을 고려해야함   
  - 예) 어떤 학습 데이터에 한가지 변수가 결정되면 target이 거의 결정될 때, 모델은 이를 강하게 학습함   
    - 하지만, 실제 데이터에는 그 패턴이 없을 수 있음   
  - 이러한 leakage를 방지하기 위해, 타겟 실현 다음에 업데이트 되는 변수는 제외해야 함   
* `Train-Test Contamination` 
  - training 데이터와 validation 데이터를 잘 구분하지 않아 발생. 즉, 전처리에 validation 데이터가 영향을 줌       
    
### 2. Example   
```
import pandas as pd

# Read the data
data = pd.read_csv('../input/aer-credit-card-data/AER_credit_card_data.csv', 
                   true_values = ['yes'], false_values = ['no'])

# Select target
y = data.card

# Select predictors
X = data.drop(['card'], axis=1)

print("Number of rows in the dataset:", X.shape[0])
X.head()
>>>
Number of rows in the dataset: 1319
	reports	age	income	share	expenditure	owner	selfemp	dependents	months	majorcards	active
0	0	37.66667	4.5200	0.033270	124.983300	True	False	3	54	1	12
1	0	33.25000	2.4200	0.005217	9.854167	False	False	3	34	1	13
2	0	33.66667	4.5000	0.004156	15.000000	True	False	4	58	1	5
3	0	30.50000	2.5400	0.065214	137.869200	False	False	0	25	1	7
4	0	32.16667	9.7867	0.067051	546.503300	True	False	2	64	1	5
```

data가 작음. cross-validation 사용
```
from sklearn.pipeline import make_pipeline
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import cross_val_score

# Since there is no preprocessing, we don't need a pipeline (used anyway as best practice!)
my_pipeline = make_pipeline(RandomForestClassifier(n_estimators=100))
cv_scores = cross_val_score(my_pipeline, X, y, 
                            cv=5,
                            scoring='accuracy')

print("Cross-validation accuracy: %f" % cv_scores.mean())
>>>
Cross-validation accuracy: 0.981052
```

정확도가 매우 높은데, 일반적으로 저정도 정확도는 잘 안 나옴. 즉, data leakage가 있는지 확인 필요   
* card: 1 if credit card application accepted, 0 if not   
* reports: Number of major derogatory reports   
* age: Age n years plus twelfths of a year   
* income: Yearly income (divided by 10,000)   
* share: Ratio of monthly credit card expenditure to yearly income   
* expenditure: Average monthly credit card expenditure   
* owner: 1 if owns home, 0 if rents   
* selfempl: 1 if self-employed, 0 if not   
* dependents: 1 + number of dependents   
* months: Months living at current address   
* majorcards: Number of major credit cards held   
* active: Number of active credit account   
```
expenditures_cardholders = X.expenditure[y]
expenditures_noncardholders = X.expenditure[~y]

print('Fraction of those who did not receive a card and had no expenditures: %.2f' \
      %((expenditures_noncardholders == 0).mean()))
print('Fraction of those who received a card and had no expenditures: %.2f' \
      %(( expenditures_cardholders == 0).mean()))
>>>
Fraction of those who did not receive a card and had no expenditures: 1.00
Fraction of those who received a card and had no expenditures: 0.02
```

결과를 볼때, 카드를 받지 않은 사람은 모두 지출이 없고, 카드를 받은 사람 중 2%만 지출이 없음.   
target leakage로 보임. 
```
# Drop leaky predictors from dataset
potential_leaks = ['expenditure', 'share', 'active', 'majorcards']
X2 = X.drop(potential_leaks, axis=1)

# Evaluate the model with leaky predictors removed
cv_scores = cross_val_score(my_pipeline, X2, y, 
                            cv=5,
                            scoring='accuracy')

print("Cross-val accuracy: %f" % cv_scores.mean())
>>>
Cross-val accuracy: 0.830919
```

정확도가 약간 떨어졌지만, leaky 모델보다 실제 데이터에서 더 잘 작동할 것임.
