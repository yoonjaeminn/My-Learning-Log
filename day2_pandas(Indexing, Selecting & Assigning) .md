'26. 3. 8. 일  
# 주제: pandas(Indexing, Selecting & Assigning)  
## **주요 개념**  
| | A | B | C | D | E |
|:-|:-|:-|:-|:-|:-|
| first |	35 | 21 | 30 | 45 | 28 |
| second | 41 | 34 | 29 | 60 | 26 |
| third |	25 | 31 | 40 | 35 | 18 |
```
prac = pd.read_csv("adress", index_col = 0)
```
### 1. prac의 C열 반환
```
test = prac.C

     = prac['C']
```
### 2. iloc[ ] / loc[ ]
```
test = prac.iloc[0]

     = prac.loc['first']

>>> prac의 첫 행을 가져와 test에 할당.

"""
iloc[행번호,열번호] / loc[행이름,열이름] / 열값 생략 가능

iloc[0:9] >>> 0 ~ 8 / loc[0:9] >>> 0 ~ 9
"""
```
### 3. data 할당
```
prac['A'] = '0'
prac['A']
>>>
first	0
second	0
third	0
Name: 'A', Length: 3, dtype: int64
```
### 코드 예시1) C열 첫번째 행 값 반환
```
test = prac['C'][0]

     = prac.C.iloc[0]

     = prac.C.loc['first']

     = prac.C[0]
```
### 코드 예시2) A열 첫 3번째 행까지 값 반환
```
test = prac.A[:3]

     = prac.A.head(3)

     = prac.A.iloc[:3]

     = prac.iloc[:3, 0]
```
### 코드 예시3) A열, B열, C열의 첫번째, 세번째 행 값 반환
```
test = prac.iloc[[0,2],[0,1,2]]

     = prac.iloc[[0,2],:3]

     = prac.loc[['first', 'third'],['A','B','C']]

     = prac.loc[['first', 'third'],'A':'C']
```
### 코드 예시4) A열의 값이 35거나 41이고, D열의 값이 50 이상인 값 반환
```
test = prac.loc[prac.A.isin([35,41]) & (prac.D >= 50)]

     = prac.loc[(prac.A == 35 | prac.A == 41) & (prac.D >= 50)]

"""
isin([]): [] 값이 영역에 존재하면 선택

isnull(): 영역이 공백이면 선택 / notnull: 영역이 공백이 아니면 선택

e.g.) prac.loc[prac.A.notnull()]: 테이블의 A이 공백이 아닌 행을 선택
"""
```
