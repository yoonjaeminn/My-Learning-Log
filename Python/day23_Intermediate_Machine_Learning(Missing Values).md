'26. 3. 31. 화  
# 주제: Intermediate Machine Learning(Missing Values)
  
### 1. Three Approaches  
* A Simple Option: Drop Columns with Missing Values:   
  중요한 정보가 손실될 수 있음
* A Better Option: Imputation   
  - 결측치를 다른 수로 채움    
  - 정확하지는 않지만, 단순 drop보다는 보통 저 정확함    
* An Extension To Imputation    
  - 표준적인 방식으로 보통 잘 작동됨    
  - 결측치를 채우고, 결측치를 가지던 각 열에 대해 추가된 entries의 위치를 보여주는 새로운 열을 더함      

### 2. Examples    
```
from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import mean_absolute_error

# Function for comparing different approaches
def score_dataset(X_train, X_valid, y_train, y_valid):
    model = RandomForestRegressor(n_estimators=10, random_state=0)
    model.fit(X_train, y_train)
    preds = model.predict(X_valid)
    return mean_absolute_error(y_valid, preds)
```
    
#### Score from Approach 1 (`Drop Columns with Missing Values`)
```
# Get names of columns with missing values
cols_with_missing = [col for col in X_train.columns
                     if X_train[col].isnull().any()]

# Drop columns in training and validation data
reduced_X_train = X_train.drop(cols_with_missing, axis=1)
reduced_X_valid = X_valid.drop(cols_with_missing, axis=1)

print("MAE from Approach 1 (Drop columns with missing values):")
print(score_dataset(reduced_X_train, reduced_X_valid, y_train, y_valid))

>>>
MAE from Approach 1 (Drop columns with missing values):
183550.22137772635
```

#### Score from Approach 2 (`Imputation`)
```
from sklearn.impute import SimpleImputer

# Imputation
my_imputer = SimpleImputer()
imputed_X_train = pd.DataFrame(my_imputer.fit_transform(X_train))
imputed_X_valid = pd.DataFrame(my_imputer.transform(X_valid))

# Imputation removed column names; put them back
imputed_X_train.columns = X_train.columns
imputed_X_valid.columns = X_valid.columns

print("MAE from Approach 2 (Imputation):")
print(score_dataset(imputed_X_train, imputed_X_valid, y_train, y_valid))

>>>
MAE from Approach 2 (Imputation):
178166.46269899711

"""
fit_transform(): fit하여 평균으로 transform하는게 기본 설정
transform(): fit_transform에서 fit한 평균을 그대로 사용 -> valid 변수에서 fit하는 과정은 Data Leakage임
"""
```

#### Score from Approach 3 (`An Extension to Imputation`)
```
# Make copy to avoid changing original data (when imputing)
X_train_plus = X_train.copy()
X_valid_plus = X_valid.copy()

# Make new columns indicating what will be imputed
for col in cols_with_missing:
    X_train_plus[col + '_was_missing'] = X_train_plus[col].isnull()
    X_valid_plus[col + '_was_missing'] = X_valid_plus[col].isnull()

# Imputation
my_imputer = SimpleImputer()
imputed_X_train_plus = pd.DataFrame(my_imputer.fit_transform(X_train_plus))
imputed_X_valid_plus = pd.DataFrame(my_imputer.transform(X_valid_plus))

# Imputation removed column names; put them back
imputed_X_train_plus.columns = X_train_plus.columns
imputed_X_valid_plus.columns = X_valid_plus.columns

print("MAE from Approach 3 (An Extension to Imputation):")
print(score_dataset(imputed_X_train_plus, imputed_X_valid_plus, y_train, y_valid))

>>>
MAE from Approach 3 (An Extension to Imputation):
178927.503183954
```
