 '26. 3. 7. 토
# 주제: pandas(Creating, Reading and Writing)
## **주요 개념**  
## 1. **import pandas as pd**: pandas를 'pd'라는 별명으로 불러옴  
## 2. **pd.DataFrame()**: **table** 형태의 객체  
   ```
   prac = pd.DataFrame({'A':[35,41],'B':[21,34]}, index = ['1','2'])
   -------------------------------------or-------------------------------------
   prac = pd.DataFrame([[35,21],[41,34]], columns=['A','B'], index = ['1', '2'])
   ```
**실행결과** 
| | A | B |
|:--- |:--- |:--- |
| 1 |	35 | 21 |
| 2 |	41 | 34 |

## 3. **pd.Series()**: 데이터 값의 연속. **list** 형태의 객체
   ```
   prac = pd.Series([30, 35, 40], index=['A', 'B', 'C'], name='Sales')
   ```
**실행결과**
|A|30|
|:-|:-|
|B|35|
|C|40|

Name: Sales, dtype: int64

## 4. **pd.read_csv()**: csv파일을 불러옴
   ```
   prac = pd.read_csv("파일주소", index_col=0)
   ``` index_col: 열번호 또는 필드명을 입력해, 인덱스로 사용할 수 있음```
   ```
## 5. **shape**: (records수, columns수) 반환
   ```
   prac = pd.read_csv("파일 주소")
   prac.shape
   >>> (129971, 14)
   ```
 ## 6. **prac.head()**: prac의 첫 5행을 반환함.  
 ## 7. **prac.to_csv("이름")**: prac을 "이름"의 csv파일로 저장함
