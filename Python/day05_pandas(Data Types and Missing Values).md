'26. 3. 11. 수  
# 주제: pandas(Data Types and Missing Values)

|   |  A |  B |    C   |    D   |   E  |
|:--|:---|:---|:-------|:-------|:-----|
| 0 |	35 |    | thiron | fourfi | 28.0 |
| 1 | 41 | 34 | twenni |  sixt  | 26.1 |
| 2 |	20 | 21 |  fourt | thirfi | 18.0 |
```
prac = pd.read_csv("adress", index_col = 0)

"""
pd.set_options('display.max_rows', 5)
>>> 5번째 행까지만 표시. table이 너무 클때 사용
"""
```

### 1. Dtypes
```
prac.E.dtype
>>>
dtype('float64')

"""
E열의 타입 반환. E열은 64비트의 foeating point number임
index도 타입을 가지고 있음
"""
```
```
prac.C.dtype
>>>
dtype('object')

"""
문자열로만 구성된 열은 자체적인 타입을 갖지 않고, object 타입으로 지정됨
"""
```
```
prac.A.astype('float64')
>>>
0  35.0
1  41.0
2  20.0
Name: A, Length: 3, dtype: float64

"""
columns.astype('dtype') -> columns의 type을 dtype으로 변환
"""
```

### 2. Missing data
```
"""
값이 없는 항목(결측값)은 NaN 값이 부여되는데, 기술적인 이유로 NaN 값들은 항상 float64 dtype을 가짐
-> int64 타입의 요소를 갖는 열에 하나라도 NaN이 있으면, 그 열의 타입은 float64로 정의됨
"""
```
```
prac[pd.isnull(prac.B)]
>>>
|   |  A |  B |    C   |    D   |   E  |
| 0 | 35 |    | thiron | fourfi | 28.0 |

"""
B열이 NaN인 행만 반환
"""
```
```
prac[pd.notnull(prac.B)]
>>>
|   |  A |  B |    C   |    D   |   E  |
| 1 | 41 | 34 | twenni |  sixt  | 26.1 |
| 2 | 20 | 21 |  fourt | thirfi | 18.0 |

"""
B열이 NaN이 아닌 행만 반환
"""
```
```
prac.B.fillna("Unknown")
>>>
       B   
0   Unknown 
1     34   
2     21
Name: B, Length: 3, dtype: object

"""
Series.fillna() -> NaN 값을 다른 값으로 치환
"""
```
```
prac.D.replace("fourfi", "twelve")
>>>
0    twelve
1    sixt
2    thirfi
Name: D, Length: 3, dtype: object

"""
Series.replace(A,B) -> A를 B로 치환
"""
```

### 코드 예시1) B열에 NaN이 얼마나 많은지 카운트하기
```
test1 = prac[prac.B.isnull()]
test2 = len(test1)
-----------------or-----------------
test2 = prac.B.isnull.sum()
-----------------or-----------------
test2 = pd.isnull(prac.B).sum()

"""
[행번호] -> 해당 행 반환
[boolean] -> boolean 값이 TRUE인 행 번호 반환
-> prac.B.isnull 와 pd.isnull(prac.B) 가 행번호와 boolean 값을 갖는 Series므로 boolean 연산이 가능
"""
```

### 코드 예시4) B열에 NaN을 "Unknown"으로 치환하고, 각 값이 출현하는 횟수를 세는 시리즈 만들기(내림차순 정렬)
```
test = prac.B.fillna("Unknown").value_counts().sort_values(ascending=False)
```
