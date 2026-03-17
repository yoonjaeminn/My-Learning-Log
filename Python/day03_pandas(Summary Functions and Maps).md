'26. 3. 9. 월  
# 주제: pandas(Summary Functions and Maps)  

| | A | B | C | D | E |
|:-|:-|:-|:-|:-|:-|
| 0 |	35 | 21 | thiron | fourfi | 28 |
| 1 | 41 | 34 | twenni | sixt | 26 |
| 2 |	25 | 21 | fourt | thirfi | 18 |
```
prac = pd.read_csv("adress", index_col = 0)

"""
pd.set_options('display.max_rows', 5)
>>> 5번째 행까지만 표시. table이 너무 클때 사용
"""
```
### 1. Summaty Functions
```
test = prac.E.describe()
>>>
count  3.000000
mean  24.000000
      ...
75%   생략함
max   28
Name: E, Length: 8, dtype: float64

"""
테이블에 대한 description 제공
"""
```
```
prac.E.mean()
>>>
24

"""
평균값 반환 method
"""
```
```
prac.A.median()
>>>
35.0
"""
중간값 반환 method
"""
```
```
prac.B.unique()
>>>
array([21, 34], dtype=int64)

"""
중복 없이 하나씩만 반환 method
"""
```
```
prac.B.value_counts()
>>>
21 2
34 1
Name: B, Length: 2, dtype:int64

"""
각 값이 등장하는 횟수 반환 method
"""
```
```
prac.A.idxmax()
>>>
1

"""
열에서 최대 값을 갖는 element의 index(행번호)를 반환 method
"""
```
### 코드 예시1) A열에서 A열의 평균을 뺀 값으로 이루어진 Series 만들기
```
test = prac.A - prac.A.mean()
```
### 코드 예시2) B부터 E까지의 증가율이 가장 큰 회차의 A값 구하기
```
index = (prac.E / prac.A).maxidx()
largest_turn = prac.loc[index, 'A']
```
### 2. Maps
```
mean_A = prac.A.mean()
prac.A.map(lambda a: a - mean_A)

"""
Series.map(함수) -> Series의 각 값에 동일한 함수를 적용함. 결과로 새로운 Series 반환 merhod
lambda 매계변수: 변환값 -> 함수를 정의하지 않고 한줄로 작성해 바로 사용하는 간단한 함수
"""
```
```
def remean_score(row):
  row.A = row.A - mean_A
  return row

prac.apply(remean_score, axis='columns')

"""
DataFrame.apply(함수, axis=?) -> DataFrame의 각 행 또는 열에 대해 함수 적용
axis=? -> 0 or 'index' (열 기준으로 적용) / 1 or 'columns' (행 기준으로 적용)
"""
```
```
prac.A - mean_A

"""
pandas는 Series에 대해 벡터화 연산을 지원함 -> 간단한 연산을 빠르게 처리 가능
"""
```
```
prac.C + " - " + prac.D
>>>
0   thiron - fourfi
1   twenni - sixt
2   fourt - thirfi

"""
pandas는 Series에 대해 문자열 벡터 연산도 가능함
"""
```
### 코드 예시3) B열에 't'가 포함된 elements의 수와 'n'이 포함된 elements의 수를 세는 Series 만들기
```
test = pd.Series([
  prac.B.str.contain('t').sum(),
  prac.B.str.contain('n').cum()
], index=['t','n'])

------------or------------

n_t = prac.B.map(lambda spel: 't' in spel).sum()
n_n = prac.B.map(lambda spel: 'n' in spel).sum()
test = pd.Series([n_t, n_n], index=['t', 'n'])
```
### 코드 예시4) A열 값이 15이거나 40 이상이면 3을, 35이상이고 40미만이면 2를, 그 외는 1을 기록하는 Series 만들기
```
test = pd.Series(1, index=prac.index)
test[(prac.A == 15) | (prac.A >= 40)] = 3
test[(prac.A >= 35) & (prac.A < 40)] = 2

"""
Series[논리 조건] -> Boolean Series 생성 -> 참인 값에 대해서만 식 적용
"""
------------or------------

def fn(row):
    if row.A == 15:
        return 3
    elif row.A >= 40:
        return 3
    elif row.A >= 35:
        return 2
    else:
        return 1
    
test = prac.apply(fn, axis='columns')
```
