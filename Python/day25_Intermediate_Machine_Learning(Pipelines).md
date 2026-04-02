'26. 4. 2. 목  
# 주제: Intermediate Machine Learning(Pipelines)
  
### 1. Introduction   
* `Pipelines`: 데이터 전처리와 모델링 코드를 조직화할 수 있는 간단한 방식, 전처리와 모델링 과정을 묶어 하나의 과정처럼 사용할 수 있게 함   
  - `Cleaner Code`: 각 과정에서 training and valid data를 수동으로 추적하지 않아도 됨   
  - `Fewer Bugs`: 전처리나 적용 과정에서 실수할 확률이 적음   
  - `Easier to Productionize`: 모델을 프로토타입에서 대규모로 배포하는데 도움이 됨   
  - `More Options for Model Validation`: cross-validation    

### 3. Example   
```
import pandas as pd
from sklearn.model_selection import train_test_split

# Read the data
data = pd.read_csv('../input/melbourne-housing-snapshot/melb_data.csv')

# Separate target from predictors
y = data.Price
X = data.drop(['Price'], axis=1)

# Divide data into training and validation subsets
X_train_full, X_valid_full, y_train, y_valid = train_test_split(X, y, train_size=0.8, test_size=0.2,
                                                                random_state=0)

# "Cardinality" means the number of unique values in a column
# Select categorical columns with relatively low cardinality (convenient but arbitrary)
categorical_cols = [cname for cname in X_train_full.columns if X_train_full[cname].nunique() < 10 and 
                        X_train_full[cname].dtype == "object"]

# Select numerical columns
numerical_cols = [cname for cname in X_train_full.columns if X_train_full[cname].dtype in ['int64', 'float64']]

# Keep selected columns only
my_cols = categorical_cols + numerical_cols
X_train = X_train_full[my_cols].copy()
X_valid = X_valid_full[my_cols].copy()
```

* Step 1: Define Preprocessing Steps   
```
from sklearn.compose import ColumnTransformer
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import OneHotEncoder

# Preprocessing for numerical data
numerical_transformer = SimpleImputer(strategy='constant')

# Preprocessing for categorical data
categorical_transformer = Pipeline(steps=[
    ('imputer', SimpleImputer(strategy='most_frequent')),
    ('onehot', OneHotEncoder(handle_unknown='ignore'))
])

# Bundle preprocessing for numerical and categorical data
preprocessor = ColumnTransformer(
    transformers=[
        ('num', numerical_transformer, numerical_cols),
        ('cat', categorical_transformer, categorical_cols)
    ])
```

* Step 2: Define the Model   
```
from sklearn.ensemble import RandomForestRegressor
model = RandomForestRegressor(n_estimators=100, random_state=0)
```

* Step 3: Create and Evaluate the Pipeline   
  - 파이프라인을 사용하면, 전처리와 fitting 과정이 코드 한줄로 처리됨   
  - 파이프라인을 사용하면, X_valid가 자동으로 전처리됨 -> valid data 전처리 없이 predict() 가능
```
from sklearn.metrics import mean_absolute_error

# Bundle preprocessing and modeling code in a pipeline
my_pipeline = Pipeline(steps=[('preprocessor', preprocessor),
                              ('model', model)
                             ])

# Preprocessing of training data, fit model 
my_pipeline.fit(X_train, y_train)

# Preprocessing of validation data, get predictions
preds = my_pipeline.predict(X_valid)

# Evaluate the model
score = mean_absolute_error(y_valid, preds)
print('MAE:', score)
>>>
MAE: 160679.18917034855
```
