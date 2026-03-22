'26. 3. 22. 일  
# 주제: Intro to Machine Learning(Basic Data Exploration)
  
### 1. Using Pandas to Get Familiar With Your Data
```
import pandas as pd
melbourne_file_path = '../input/melbourne-housing-snapshot/melb_data.csv'
melbourne_data = pd.read_csv(melbourne_file_path) 
melbourne_data.describe()
```

### 2. Practice Codes
```
# What is the average lot size (rounded to nearest integer)?
avg_lot_size = round(home_data.describe().loc['mean','LotArea'])

# As of today, how old is the newest home (current year - the date in which it was built)
newest_home_age = 2026 - home_data.describe().loc['max','YearBuilt']
```
