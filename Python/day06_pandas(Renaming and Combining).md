'26. 3. 12. 목  
# 주제: pandas(Renaming and Combining)

|   |  A |  B |    C   |    D   |   E  |
|:--|:---|:---|:-------|:-------|:-----|
| 0 |	35 | 21 | thiron | fourfi | 28.0 |
| 1 | 41 | 34 | twenni |  sixt  | 26.1 |
| 2 |	20 | 21 |  fourt | thirfi | 18.0 |
| 3 |	55 | 56 | sixttw | eighni | 13.8 |
```
prac = pd.read_csv("adress", index_col = 0)

"""
pd.set_options('display.max_rows', 5)
>>> 5번째 행까지만 표시. table이 너무 클때 사용
"""
```

### 1. Renaming
```
prac.rename(columns={'A': 'a'})
>>>
|   |  a |  B |    C   |    D   |   E  |
| 0 | 35 | 21 | thiron | fourfi | 28.0 |
| 1 | 41 | 34 | twenni |  sixt  | 26.1 |
| 2 | 20 | 21 |  fourt | thirfi | 18.0 |
| 3 | 55 | 56 | sixttw | eighni | 13.8 |

----------------------------------------

prac.rename(index={0: 'first', 1: 'second'})
>>>
|        |  A |  B |    C   |    D   |   E  |
|  first | 35 | 21 | thiron | fourfi | 28.0 |
| second | 41 | 34 | twenni |  sixt  | 26.1 |
|    2   | 20 | 21 |  fourt | thirfi | 18.0 |
|    3   | 55 | 56 | sixttw | eighni | 13.8 |

"""
rename()은 DataFrame이나 Series의 행 또는 열의 이름을 변경함
"""
"""
dict(A='a', B='b') == {'A': 'a', 'B': 'b'}
-> dict는 딕셔너리를 만드는 또 다른 방법임.
"""
```
```
prac.rename_axis("time", axis='rows').rename_axis("score", axis='columns')
>>>
|  score |  A |  B |    C   |    D   |   E  |
|  time  |    |    |        |        |      |
|    0   | 35 | 21 | thiron | fourfi | 28.0 |
|    1   | 41 | 34 | twenni |  sixt  | 26.1 |
|    2   | 20 | 21 |  fourt | thirfi | 18.0 |
|    3   | 55 | 56 | sixttw | eighni | 13.8 |

"""
rename_axis() -> 행 인덱스 축 이름과, 열 인덱스 축 이름을 바꿈
"""
```
```
prac.set_index("A")
>>>
|    |  B |    C   |    D   |   E  |
|  A |    |        |        |      |
| 35 | 21 | thiron | fourfi | 28.0 |
| 41 | 34 | twenni |  sixt  | 26.1 |
| 20 | 21 |  fourt | thirfi | 18.0 |
| 55 | 56 | sixttw | eighni | 13.8 |

"""
set_index() -> 특정 열을 인덱스로 설정
"""
```

### 2. Combining
```
# 서로 같은 열을 가지고 있는 경우(위아래 연결)
prac1 = pd.read_csv("파일1")
prac2 = pd.read_csv("파일2")

pd.concat([prac1, prac2])

---------------------------------------------

# 서로 다른 열을 가지고 있는 경우(좌우로 연결)
prac1 = pd.read_csv("파일1")
prac2 = pd.read_csv("파일2")

pd.concat([prac1, prac2], axis=1)

"""
concat은 좌우, 위아래로 단순 열결함.
"""
```
```
left = prac1.set_index(['A', 'B'])
right = prac2.set_index(['A', 'B'])

left.join(right, lsuffix='_1', rsuffix='_2')

"""
join()은 서로 다른 두 데이터를 합치기 위해 사용.
concat()과 다르게 인덱스를 기준으로 매칭하며, 기본적으로 옆으로 연결함.

여기서 lsuffix과 rsuffux은 left와 right가 동일한 열을 가지고 있을때 구분하기 위해 사용하며, 열이 서로 다를 경우 생략해도 됨
-> column_name_1, column_name_2 처럼 열 이름이 수정됨
"""
```
