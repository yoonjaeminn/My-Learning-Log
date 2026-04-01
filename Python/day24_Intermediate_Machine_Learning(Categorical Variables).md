'26. 4. 1. 수  
# 주제: Intermediate Machine Learning(Categorical Variables)
  
### 1. Introduction   
* Categorical Variables: 제한된 수의 변수를 갖음   
* Examples   
  - 전혀 아니다, 아니다, 그렇다, 매우 그렇다    
  - 현대, 기아, 혼다, 도요타, 포드        

### 2. Three Approaches    
* `Drop Categorical Variables`   
  - catrgoricl vatiables를 다루를 가장 간단한 방식 -> 단순 제거   
  - 해당 열들이 유용한 정보를 포함하지 않을 때만 사용   
   
* `Ordinal Encoding`   
  - 카테고리 들의 순서를 가정함 e.i.) "Never" (0) < "Rarely" (1) < "Most days" (2) < "Every day" (3)   
  - 이러한 변수들을 `ordinal variables`라고 함   
  - 트리 배이스 모델에서 잘 작동함   
   
* `One-Hot Encoding`    
  - 각각의 값들의 존재 유무를 나타내는 새로운 열을 만듦   
  - Ordinal Encoding과 달리, 카테고리들 간의 순서를 가정하지 않음   
  - 즉, categorical data에 명확한 순서가 없을때 잘 작동함   
  - 직관적인 순위가 없는 categorical variables를 `nominal variables`라고 함   
  - dataset의 크기를 매우 증가시킬 수 있음 -> low cardinality를 갖는 열에만 사용   
  - high cardinality를 갖는 열들은 제거하거나 ordinal encoding 사용

### 3. Example   
```
# Cardinality 란 한 열에서 unique values의 개수를 말함
# 상대적으로 낮은 cardinality를 갖는 열 추출
low_cardinality_cols = [cname for cname in X_train_full.columns if X_train_full[cname].nunique() < 10 and 
                        X_train_full[cname].dtype == "object"]

# numerical columns 추출
numerical_cols = [cname for cname in X_train_full.columns if X_train_full[cname].dtype in ['int64', 'float64']]

my_cols = low_cardinality_cols + numerical_cols
X_train = X_train_full[my_cols].copy()
X_valid = X_valid_full[my_cols].copy()
```
```
from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import mean_absolute_error

# Function for comparing different approaches
def score_dataset(X_train, X_valid, y_train, y_valid):
    model = RandomForestRegressor(n_estimators=100, random_state=0)
    model.fit(X_train, y_train)
    preds = model.predict(X_valid)
    return mean_absolute_error(y_valid, preds)
```
```
# Drop Categorical Variables
drop_X_train = X_train.select_dtypes(exclude=['object'])
drop_X_valid = X_valid.select_dtypes(exclude=['object'])

print("MAE from Approach 1 (Drop categorical variables):")
print(score_dataset(drop_X_train, drop_X_valid, y_train, y_valid))
>>>
MAE from Approach 1 (Drop categorical variables):
175703.48185157913
```
```
# Ordinal Encoding
from sklearn.preprocessing import OrdinalEncoder

# Make copy to avoid changing original data 
label_X_train = X_train.copy()
label_X_valid = X_valid.copy()

# Apply ordinal encoder to each column with categorical data
ordinal_encoder = OrdinalEncoder()
label_X_train[object_cols] = ordinal_encoder.fit_transform(X_train[object_cols])
label_X_valid[object_cols] = ordinal_encoder.transform(X_valid[object_cols])

print("MAE from Approach 2 (Ordinal Encoding):") 
print(score_dataset(label_X_train, label_X_valid, y_train, y_valid))
>>>
MAE from Approach 2 (Ordinal Encoding):
165936.40548390493
```
```
# One-Hot Encoding
from sklearn.preprocessing import OneHotEncoder

# Apply one-hot encoder to each column with categorical data
# handle_unknown='ignore' -> valid data가 training data에 없는 class를 포함할때, 오류를 방지함
# sparse=False -> encoded columns가 sparse matrix가 아닌, numpy array로 반환되도록 함
OH_encoder = OneHotEncoder(handle_unknown='ignore', sparse=False)
OH_cols_train = pd.DataFrame(OH_encoder.fit_transform(X_train[object_cols]))
OH_cols_valid = pd.DataFrame(OH_encoder.transform(X_valid[object_cols]))

# One-hot encoding removed index; put it back
OH_cols_train.index = X_train.index
OH_cols_valid.index = X_valid.index

# Remove categorical columns (will replace with one-hot encoding)
num_X_train = X_train.drop(object_cols, axis=1)
num_X_valid = X_valid.drop(object_cols, axis=1)

# Add one-hot encoded columns to numerical features
OH_X_train = pd.concat([num_X_train, OH_cols_train], axis=1)
OH_X_valid = pd.concat([num_X_valid, OH_cols_valid], axis=1)

# Ensure all columns have string type
OH_X_train.columns = OH_X_train.columns.astype(str)
OH_X_valid.columns = OH_X_valid.columns.astype(str)

print("MAE from Approach 3 (One-Hot Encoding):") 
print(score_dataset(OH_X_train, OH_X_valid, y_train, y_valid))
```
