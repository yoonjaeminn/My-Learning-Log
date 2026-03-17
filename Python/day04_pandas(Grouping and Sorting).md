'26. 3. 10. 화  
# 주제: pandas_(Grouping and Sort)  

|   |  A |  B |    C   |    D   |  E |
|:--|:---|:---|:-------|:-------|:---|
| 0 |	35 | 21 | thiron | fourfi | 28 |
| 1 | 41 | 34 | twenni |  sixt  | 26 |
| 2 |	20 | 21 |  fourt | thirfi | 18 |
```
prac = pd.read_csv("adress", index_col = 0)

"""
pd.set_options('display.max_rows', 5)
>>> 5번째 행까지만 표시. table이 너무 클때 사용
"""
```

### 1. Groupwise analysis
```
prac.groupby(열)

"""
열의 값을 기준으로 같은 값을 갖는 행끼리의 그룹을 생성
"""
```
```
prac.groupby('B').B.count()
>>>
B
21  1
34  2
Name: B, Length: 2, dtype: int64

"""
B열에서 같은 값을 갖는 행을 그룹화하고, B열에서 출현하는 값들의 수를 셈
"""
```
```
prac.groupby('B').A.min()
>>>
B
21  20
34  41
Name: A, Length: 2, dtype: int64

"""
B열에서 같은 값을 갖는 행을 그룹화하고, 각 그룹의 A열의 최소값을 반환
"""
```
```
prac.groupby('B').apply(lambda df: df.A.iloc[0])
>>>
B
21  35
34  41
length: 2, dtype: int64

"""
B열을 기준으로 같은 값을 갖는 행을 그룹화 하고, 각 그룹의 첫 행의 A열 값을 반환
"""
```
```
prac.groupby(['B', 'A']).apply(lambda df: df.loc[df.E.idmax()])
>>>
|   |  B |  A |    C   |    D   |  E |
| 0 |	21 | 35 | thiron | fourfi | 28 |
|   |    | 20 | fourt  | thirfi | 18 | 
| 1 | 34 | 41 | twenni |  sixt  | 26 |

"""
B열을 기준으로 같은 값을 갖는 행을 그룹화 한 뒤, 각 그룹에서 A열을 기준으로 다시 그룹화
이후 각 그룹의 행에서 E열의 값이 최대인 행을 반환
"""
```
```
prac.groupby(['B']).E.agg([len, min, max])
>>>
|   | len | min | max |
| B |	    |     |     |
| 21|  2  |  18 |  28 |
| 34|  1  |  26 |  26 |

"""
Dataframe.agg(함수1, 함수2, ...) -> DataFrame에 다양한 함수를 동시에 적용하여 간단한 요약 테이블을 반환
"""
```
```
prac.groupby(['B']).apply(lambda df: df.loc[df.E.idmax()])
test.reset_index()
>>>
|   |  B |  A |    C   |    D   |  E |
| 0 |	21 | 35 | thiron | fourfi | 28 |
| 1 | 21 | 20 | fourt  | thirfi | 18 | 
| 2 | 34 | 41 | twenni |  sixt  | 26 |

"""
multi_index.reset_index -> 그룹화된 인덱스를 순서는 유지한채로 그룹 해제함.
"""
```

### 코드 예시1) B열을 기준으로 그룹화 한 뒤, 각 그룹의 E열의 최대 값을 반환하고, 이를 인덱스 기준으로 정렬한 Series 만들기
```
test = prac.groupby('B').E.max().sort_index()
```

### 코드 예시2) B열을 기준으로 그룹화 한 뒤, 각 그룹의 E열의 최대값과 최소값을 반환한 DataFrmae 만들기
```
test = prac.groupby('B').E.agg(max, min)
```

### 2. Multi-indexes
```
test = prac.groupby(['B','A']).C.agg([len])

"""
하나 이상의 level을 갖는 인덱스의 타입을 MultiIndex라 함.
"""
```

### 3. Sorting
```
test = prac.groupby(['B','A']).C.agg([len])
test = test.reset_index()
test.sort_values(by='len', ascending=True)
>>>
|   |  B |  A |len|
| 1 | 21 | 20 | 1 |
| 2 | 34 | 41 | 2 |
| 0 |	21 | 35 | 3 |
# 실제로는 len값이 모두 1이지만, 예를 위해 가정
"""
DataFame.sort_value(by=?1, ascending=?2)
-> ?1 값을 기준으로(여러 기준 지정 가능 e.g. ['B', 'A']),
   ?2(False: 내림차순 / True, 생략: 오름차순) 순서대로 정렬
"""
```

### 코드 예시3) 두가지 값을 기준으로 내림차순 정렬하기
```
test1 = prac.groupby('B').A.agg([min, max])
test2 = test1.sort_values(by=['min','max'], ascending=False)
```

### 코드 예시4) 두가지 값을 기준으로 정렬 한뒤, 각 level의 pair의 개수 구하는 Series를 만들어 내림차순 정렬하기
```
test = prac.groupby(['B', 'A']).size().sort_values(ascending=False)

"""
Series는 열이 하나기 때문에 by를 작성할 필요 없음.
"""
```
