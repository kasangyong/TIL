# modules

모듈 
- 한 파일로 묶인 함수의 모음  

```python
import math
print(math.pi) #3.141592 -> 모듈명.변수명
print(math.sqrt(4)) #2.0 -> 모듈명.함수명
```
이론
- import 문 사용
    - 같은 이름의 함수가 여러 모듈에 있을 때 충돌 방지
    - .(dot)연산자
        - 점의 왼쪽 객체에서 점의 오른쪽 이름을 찾아라
- from 절 사용
    - 코드가 짧고 간결해짐
    ```python
    from math import pi, sqrt
    print(pi) -> 변수명
    print(sqrt(4)) -> 함수명
    ```
    - 단점
        - 정의된 모듈의 위치를 알기 어려워 명시적이지 않을 수 있음
        - 사용자가 선언한 변수 or 함수가 겹칙게 되어 모듈에서 정의한 값이나 동작이 이루어 지지 않을 수 있음

- as 키워드
    - as 키워드를 사용하여 별칭 부여
    ```python
    import pandas as pd
    import matplot.lib as plt
    ```

- 직접 정의한 모듈 사용
```python
#my_math.py 에서 사용
def add(x, y):
    return x + y

#sample.py 에서 사용
import my_math

# from my_math import add

print(my_math.add(1, 2))
# print(add(1, 2))
``` 

## 파이썬 표준 라이브러리
- 파이썬을 설치하면 자동으로 사용할 수 있는 기본 라이브러리
    - 다양한 기증 내장되어 있어, 복잡한 작업도 쉽게 처리 가능
    - math, random, sys (모듈) 및 json, email (패키지) 등 다양한 모둘과 패키지 포함
    - 별도 설치 없이 import로 사용 가능
## 파이썬 외부 패키지
- 필요한 기능을 사용하기 위해 직접 설치해서 쓰는 패키지
    - 전 세계 개발자들이 만든 다양한 패키지 존재
    - 사용할 패키지 설치할 때 pip 명령어 사용

```python
사용자 정의 패키지
경로: 같은 경로부터 시작 -> my_package 폴더 안 -> math 폴더 안 -> my_math.py

from my_package.math import my_math
from my_package.statistics import tools

print(my_math.add(1, 2))
print(tools.mod(1, 2))
```
## requests 외부 패키지 설치 및 사용 예시 -> 많이 사용
- 파이썬에서 웹에 요청을 보내고 응답을 받는 걸 쉽게 만들어주는 외부 패키지
```python
pip install requests
import requests
# 공휴일 정보 API
url = "https://date.nager.at/api/v3/publicholidays/2026/KR"
response = requests.get(url).json()
print(response)
```

# 제어문
- 코드의 실행 흐름을 제어하는데 사용되는 구문
- 조건에 따라 구문을 실행하거나 반복적으로 코드를 실행

## 조건문
- 주어진 조건식을 평가하여 해당 조건이 참인 경우에만 코드 블록을 실행 or 건너뜀
- if
    - 조건문의 기본 형태
    - if문에 작성된 조건을 만족할 때 내부 코드 실행
    - 작성되는 조건은 표현식으로 작성
- elif
    - 이전의 조건을 만족하지 못하고 추가로 다른 조건이 필요할 때 사용
    - 여러 개의 elif 문 사용 가능
- else
    - 모든 조건들을 만족하지 않으면 실행

- 복수 조건문
    - 조건식을 동시에 검사하는 것이 아니라 ***순차적***으로 비교
    - 조건식의 순서에 따라 원하는 겨로가가 나오지 않을 수 있음을 주의

![alt text](image.png)

## 반복문
- 주어진 코드 블록을 여러 번 반복해서 실행하는 굼누
- for문
    - 반복 가능(iterable)한 객체의 요소들을 반복하는데 주로 사용
    - 주로 반복 가능한 객체 요소의 개수만큼 반복
    - 특징: 반복 횟수가 정해져 있음
    - 반복 가능한 객체
        - 요소를 하나씩 반환할 수 있는 모든 객체
        - list, tuple, str 뿐 아니라 dict, set도 가능
    - for 문 작동 원리
        - 리스트 내 첫 항목이 반복 변수에 할당되고 코드블록이 실행
        - 다음으로 2번째 항목이 할당
        - 끝까지 실행하고 더 이상 리스트에 남은 요소가 없으면 실행 종료
```python
student_list = ['Alice', 'Bob', 'Charlie']
for student in student_list:
    print(f"Hello, {student}!")
```

순회
```
# for문 dictionary 순회
my_dict = {
    'x': 10,
    'y': 20,
    'z': 30,
}

for key in my_dict:
    print(key)
    print(my_dict[key])
# x 10 y 20 z 30

# 인덱스 순회
numbers = [4, 6, 10, -8, 5]

for i in range(len(numbers)):
    numbers[i] = numbers[i] * 2

print(numbers) # [8, 10, 20, -16, 10]

# 중첩 반복문
outers = ['A', 'B']
inners = ['c', 'd']

for outer in outers:
    for inner in inners:
        print(outer, inner)


# 중첩 리스트 순회
elements = [['A', 'B'], ['c', 'd']]

# 1
for elem in elements:
    print(elem)

# 2
for elem in elements:
    for item in elem:
        print(item)

```


- while 문
    - while 조건이 참인 동안 반복
    - 반복 횟수가 정해지지 않은 경우 주로 사용
```python
input_value = ''
while input_value != 'exit':
    input_value = input("Enter a value: ")
    print(input_value)
```

while 문의 반복 원리
- while의 조건식 확인
    - 조건식이 참이면 코드 실행
    - 조건문이 거짓이면 반복 종료
- 코드 블록 실행이 마무리되면 다시 while 조건 확인
- 반드시 종료 조건이 필요

반복 제어
- break
    - 해당 키워드를 만나게 되면 남은 코드를 무시하고 반복 즉시 종료
    - 반복을 끝내야 할 명확한 조건이 있을 때 사용
- continue
    - 해당 키워드를 만나게 되면 다음 코드는 무시하고 다음 반복을 수행 -> pass 시킨다고 생각하면 편할 듯

```python
for i in range(10):
    if i == 5:
        break
    print(i)  # 0 1 2 3 4

for i in range(10):
    if i % 2 == 0:
        continue
    print(i)  # 1 3 5 7 9
```

```python
# break 키워드 예시 (for문)
# 리스트에서 첫번째 짝수만 찾은 후 반복 종료하기
numbers = [1, 3, 5, 6, 7, 9, 10, 11]
found_even = False -> flag 변수

for num in numbers:
    if num % 2 == 0:
        print('첫 번째 짝수를 찾았습니다:', num)
        found_even = True
        break

if not found_even:
    print('짝수를 찾지 못했습니다')
```

pass
- 아무 동작도 하지 않음을 명시적으로 나타내는 키워드
- 코드의 틀을 유지하거나 나중에 내용을 채우기 위한 용도로 사용
- 코드를 오래 비워두면 오류 발생하기 때문에 사용

## map함수
map(function, iterable)
- 반복 가능한 데이터구조(iterable)의 모든 요소에 function을 적용, 그 결과들을 map object로 변환
```python
numbers = [1, 2, 3]
result = map(str, numbers) -> numbers를 문자열로 변환
print(list(result)) #['1', '2', '3']
```
```python
문자열 1 2 3 이 입력됐을 경우
numbers1 = input().split() -> split은 문자열을 기준으로 잘린 문자들을 리스트로 반환
print(numbers1) #['1', '2', '3']
numbers2 = list(map(int, input().split()))
print(numbers2) #[1, 2, 3]
```

zip 함수
```python
girls = ['Jane', 'ashley']
boys = ['Peter', 'jay']
pair = zip(girls, boys)
print(list(pair)) #[('Jane', 'Peter'),('ashley', 'jay')]

# zip 함수 활용
kr_scores = [10, 20, 30, 50]
math_scores = [20, 40, 50, 70]
en_scores = [40, 20, 30, 50]

for student_scores in zip(kr_scores, math_scores, en_scores):
    print(student_scores)

# zip 함수 활용 (전치 행렬)
scores = [
    [10, 20, 30],
    [40, 50, 39],
    [20, 40, 50],
]

for score in zip(*scores):
    print(score)
```

## 핵심 요약

  * `map()`, `range()`, `zip()` 등
    * '이터레이터(Iterator)' 또는 그와 유사한 객체를 반환합니다. 이들은 '지연 평가(Lazy Evaluation)' 원리에 따라 동작합니다.
  * `list(...)`
    * 이터레이터를 '소비'하여 모든 요소를 계산하고, 그 결과를 리스트라는 새로운 자료구조로 '구체화'하는 과정입니다.
  * 기술적 흐름 
    * `함수(이터러블)` → `이터레이터` 생성 → `list()`로 `소비` → 지연되었던 계산 실행 → 결과 `리스트` 반환.
  * 결론: list()로 사용 or 반복문(for)으로 사용







