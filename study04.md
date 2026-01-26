# 데이터 구조 

## 메서드
- 객체에 속한 함수
- 프로그래밍에서 메서드는 객체가 특정 작업을 수행하도록 정의된 함수
- 메서드는 어딘가에 속해 있는 함수이며, 각 데이터 타입별로 다양한 기능을 가진 메서드가 존재

구조 -> 데이터 타입 객체.매서드()  
```python
# 문자열 메서드 예시
print('hello'.capitalize()) # Hello  
# 리스트 메서드 예시
numbers = [1,2,3]
numbers.append(4)
print(numbers) # [1, 2, 3, 4]
```
 
공통 시퀀스 메서드  
s.index(x) -> 시퀀스에서 첫 번째로 일치하는 항목 x의 인덱스를 반환, 없으면 value error 발생  
s.count(x) -> 시퀀스 s에서 등장하는 항목 x의 개수 반환  

```python
# index
text = 'banana'
print(text.index("a"))  # 1

my_list = [1, 2, 3]
print(my_list.index(2))  # 1


# count
text = 'banana'
print(text.count("a"))  # 3

my_list = [1, 2, 2, 3, 3, 3]
print(my_list.count(3))  # 3
```

## 문자열 탐색 및 검증 메서드

s.find(x) -> x의 첫 번째 위치를 반환, 없으면 -1 반환  
s.isupper() -> 문자열 내의 모든 케이스 문자가 대문자인지 확인  
s.islower() -> 문자열 내의 모든 케이스 문자가 소문자인지 확인  
s.isalpha() -> 문자열의 모든 문자가 알파벳이고 하나 이상의 문자가 포함되어 있으면 True 반환  
```python
# find
text = 'banana'
print(text.find('a'))  # 1
print(text.find('z'))  # -1

# isupper
string1 = 'HELLO'
string2 = 'Hello'
print(string1.isupper())  # True
print(string2.isupper())  # False

# islower
print(string1.islower())  # False
print(string2.islower())  # False

# isalpha
string1 = 'Hello'
string2 = '123heis98576ssh'
print(string1.isalpha())  # True
print(string2.isalpha())  # False
```
## 문자열 조작 메서드(새로운 문자열 반환)

```python
.replace(old, new[,count]) -> 기존 문자열에서 "old" 라는 부분 문자열이 "new"로 모두 바뀐 문자열을 치환
# replace
text = 'Hello, world! world world'
new_text1 = text.replace('world', 'Python')
new_text2 = text.replace('world', 'Python', 1)
print(new_text1)  # Hello, Python! Python Python
print(new_text2)  # Hello, Python! world world
```
```python
.strip([chars])
-> 선행과 후행 문자가 제거된 문자열의 복사본을 돌려줌
-> chars 인자는 제거할 문자 집합을 지정하는 문자열
-> 생략되거나 None이면, chars 인자의 기본값은 공백을 제거
# strip
# 사용자 입력 등에서 불필요한 공백이 포함된 경우
text = '   Hello    World   '

# 1. 아무것도 지정하지 않으면 '공백(띄어쓰기, 탭, 엔터)'을 제거
clean_text = text.strip()

print(clean_text)
# 결과: 'Hello    World'
# (주의: 문자열 중간의 공백은 제거되지 않음)


# 2. 제거할 문자를 지정하는 경우
text = '!!!Hello World!!!'
print(text.strip('!'))
# 결과: 'Hello World'


# [심화] 문자열 집합으로 제거 (순서 상관 없음)
# 'w', '.', 'c', 'o', 'm' 중 하나라도 양쪽 끝에 있으면 계속 제거
url = 'www.example.com'
print(url.strip('w.com'))
# 결과: 'example'
# (왼쪽의 'www.'과 오른쪽의 '.com'이 모두 제거됨)
```

```python
.split(sep=None, maxsplit = -1)
-> sep을 구분자 문자열로 사용하여 문자열에 있는 단어들의 리스트 반환
-> maxsplit이 주어지면 최대 maxsplit번의 분할이 수행
# split
# 1. 공백을 기준으로 분리 (기본 동작)
# - 여러 개의 공백도 하나로 처리하며, 앞뒤 공백은 무시함
text = '  Hello    Python  '
print(text.split())
# 결과: ['Hello', 'Python’]


# 2. 특정 문자를 기준으로 분리
# - 지정한 문자를 기준으로 '엄격하게' 분리함 (빈 문자열 발생 가능)
data = '10,20,,30'
print(data.split(sep=','))
# 결과: ['10', '20', '', '30']


# 3. 분할 횟수 제한 (maxsplit)
# - 앞에서부터 1번만 자르고 나머지는 그대로 둠
path = 'User/admin/documents'
print(path.split('/', maxsplit=1))
# 결과: ['User', 'admin/documents']
```
```python
.join(iterable)
-> iterable의 문자열을 연결한 문자열을 반환
# join
words = ['Python', 'is', 'awesome']

sentence1 = ' '.join(words)
sentence2 = '-'.join(words)

print(sentence1)  # Python is awesome
print(sentence2)  # Python-is-awesome
```
```python
#기타 문자열

# capitalize
text = 'heLLo, woRld!'
new_text1 = text.capitalize()
print(new_text1)  # Hello, world!

# title
new_text2 = text.title()
print(new_text2)  # Hello, World!

# upper
new_text3 = text.upper()
print(new_text3)  # HELLO, WORLD!

# lower
new_text4 = text.lower()
print(new_text4)  # hello, world!

# swapcase
new_text5 = text.swapcase()
print(new_text5)  # HEllO, WOrLD!
```

## 가변 시퀀스 메서드(리스트 전용)
### 리스트 값 추가 및 삭제 메서드
```python
# .append(x)
-> 리스트의 마지막에 항목 x를 추가
my_list = [1, 2, 3]
my_list.append(4)
print(my_list)  # [1, 2, 3, 4]
# append는 None을 반환합니다.
print(my_list.append(4))  # None

# .extend(iterable)
-> 리스트에 다른 반복 가능한 객체의 모든 항목을 추가
my_list = [1, 2, 3]
my_list.extend([4, 5, 6])
print(my_list)  # [1, 2, 3, 4, 5, 6]

# extend와 append의 비교
my_list.append([5, 6, 7])
print(my_list)  # [1, 2, 3, 4, 5, 6, [5, 6, 7]]

# my_list.extend(100)  # TypeError: 'int' object is not iterable

# .insert(i, x)
-> x를 지정한 인덱스 i 위치에 삽입
my_list = [1, 2, 3]
my_list.insert(1, 5)
print(my_list)  # [1, 5, 2, 3]

# .remove(x)
-> 리스트에서 첫 번째로 일치하는 항목을 삭제
my_list = [1, 2, 3, 2, 2, 2]
my_list.remove(2)
print(my_list)  # [1, 3, 2, 2, 2]

# .pop(i)
-> 리스트에서 지정한 인덱스의 항목을 제거하고 반환
-> 작성하지 않을 경우 마지막 항목을 제거
my_list = [1, 2, 3, 4, 5]
item1 = my_list.pop()
item2 = my_list.pop(0)

print(item1)  # 5
print(item2)  # 1
print(my_list)  # [2, 3, 4]

# .clear()
-> 리스트의 모든 항목을 제거
my_list = [1, 2, 3]
my_list.clear()
print(my_list)  # []
```
### 리스트 탐색 및 정렬 메서드
```python
# .reverse()
-> 리스트의 순서를 역순으로 변경(정렬X)
my_list = [1, 3, 2, 8, 1, 9]
my_list.reverse()
# reverse는 None을 반환합니다.
# print(my_list.reverse())  # None
# reverse는 원본 리스트를 변경합니다.
print(my_list)  # [9, 1, 8, 2, 3, 1]

# .sort()
-> 원본 리스트를 오름차순으로 정렬
my_list = [3, 2, 100, 1]
my_list.sort()
# sort는 None을 반환합니다.
# print(my_list.sort())  # None
# sort는 원본 리스트를 변경합니다.
print(my_list)  # [1, 2, 3, 100]

# sort(내림차순 정렬)
my_list.sort(reverse=True)
print(my_list)  # [100, 3, 2, 1]

sort vs sorted
sort -> 리스트의 메서드, 반환 결과가 없음
sorted -> 내장 함수, 반환 결과가 있음
```

## 객체와 참조
변수 할당의 의미
- 파이썬에서 변수 할당은 객체애 대한 참조를 생성하는 과정
```python
# 가변(mutable) 객체 예시
print('가변(mutable) 객체 예시')
a = [1, 2, 3, 4]
b = a
b[0] = 100

print(f'a의 값: {a}')  # [100, 2, 3, 4]
print(f'b의 값: {b}')  # [100, 2, 3, 4]
print(f'a와 b가 같은 객체를 참조하는가? {a is b}')  # True

# 불변(immutable) 객체 예시
print('\n불변(immutable) 객체 예시')
a = 20
b = a
b = 10

print(f'a의 값: {a}')  # 20
print(f'b의 값: {b}')  # 10
print(a is b)  # False

# id() 함수를 사용한 메모리 주소 확인
print('\n메모리 주소 확인')
x = [1, 2, 3]
y = x
z = [1, 2, 3]

print(f'x와 y는 같은 객체인가? {x is y}') # True
print(f'x와 z는 같은 객체인가? {x is z}') # False
```
가변 객체
- 생성 후에도 그 내용을 수정할 수 있음
- 객체의 내용이 변경되어도 같은 메모리 주소를 유지

불변 객체
- 생성 후 그 값을 변경할 수 없음
- 새로운 값을 할당하면 새로운 객체가 생성되고, 변수는 새 객체를 참조하게 됨

### 가변/불변 메모리 관리 방식의 이유
성능 최적화
- 불변 객체: 변경이 불가능하므로, 여러 변수가 동일한 객체를 안전하게 공유 가능
- 가변 객체: 내용 수정이 빈번할 때, 새로운 객체를 생성하는 대신 기존 객체를 직접 수정할 수 있음, 이로 인해 객체 생성 및 삭제애 드는 비용을 절감하여 성능을 향상시킴

메모리 효율성
- 불변 객체: 동일한 값을 가진 여러 변수가 같은 객체를 참조할 수 있어 메모리 사용을 최소화할 수 있음
- 가변 객체: 크기가 큰 데이터를 효율적으로 수정할 수 있음

### 복사
얕은 복사
- 객체의 최상위 요소만 새로운 메모리에 복사하는 방법
- 내부에 중첩된 객체가 있다면 그 객체의 참조만 복제됨

```python
# 1차원 리스트에서의 얕은 복사 (리스트 슬라이싱)
a = [1, 2, 3]
b = a[:]

print(a)  # [1, 2, 3]
print(b)  # [1, 2, 3]

# 1차원 리스트에서의 얕은 복사 (copy 메서드)
a = [1, 2, 3]
b = a.copy()

print(a)  # [1, 2, 3]
print(b)  # [1, 2, 3]


# 1차원 리스트에서의 얕은 복사 (list() 함수)
a = [1, 2, 3]
d = list(a)
a[0] = 100
print(a)  # [100, 2, 3]
print(d)  # [1, 2, 3]


# 얕은 복사의 한계
print('\n다차원 리스트 얕은 복사의 한계')
a = [1, 2, [3, 4, 5]]
b = a[:]

b[0] = 999
print(a)  # [1, 2, [3, 4, 5]]
print(b)  # [999, 2, [3, 4, 5]]

b[2][1] = 100
print(a)  # [1, 2, [3, 100, 5]]
print(b)  # [999, 2, [3, 100, 5]]

print(f'a[2]와 b[2]가 같은 객체인가? {a[2] is b[2]}')  # True
```

깊은 복사
- 객체의 모든 수준의 요소를 새로운 메모리에 복사하는 방법
- 중첩된 객체까지 모두 새로운 객체로 생성됨
```python
# 깊은 복사
import copy

print('깊은 복사 예시')
a = [1, 2, [3, 4, 5]]
b = copy.deepcopy(a)

b[2][1] = 100

print(a)  # [1, 2, [3, 4, 5]]
print(b)  # [1, 2, [3, 100, 5]]
print(f'a[2]와 b[2]가 같은 객체인가? {a[2] is b[2]}')  # False

# 복잡한 중첩 객체 예시
print('복잡한 중첩 객체 깊은 복사')
original = {
    'a': [1, 2, 3],
    'b': {'c': 4, 'd': [5, 6]},
}
copied = copy.deepcopy(original)

copied['a'][1] = 100
copied['b']['d'][0] = 500

print(f'원본: {original}')  # {'a': [1, 2, 3], 'b': {'c': 4, 'd': [5, 6]}}
print(f'복사본: {copied}')  # {'a': [1, 100, 3], 'b': {'c': 4, 'd': [500, 6]}}
print(
    f"original['b']와 copied['b']가 같은 객체인가? {original['b'] is copied['b']}"
)  # False
```

## List Comprehension

```python
# 사용 전
numbers = [1, 2, 3, 4, 5]
squared_numbers = []

for num in numbers:
    squared_numbers.append(num**2)

print(squared_numbers)


# 사용 후
squared_numbers2 = [num**2 for num in numbers]
# squared_numbers2 = list(num**2 for num in numbers)
print(squared_numbers2)


# List Comprehension 활용 예시
# "2차원 배열 생성 시 (인접행렬 생성 시)"
data1 = [[0] * (5) for _ in range(5)]
print(data1)
# 또는
data2 = [[0 for _ in range(5)] for _ in range(5)]
print(data2)

"""
[[0, 0, 0, 0, 0],
 [0, 0, 0, 0, 0],
 [0, 0, 0, 0, 0],
 [0, 0, 0, 0, 0],
 [0, 0, 0, 0, 0]]
"""
```

## 메서드 체이닝
- 여러 메서드를 연속해서 호출하는 방식

```python
# 문자열 메서드 체이닝
text = 'heLLo, woRld!'
new_text = text.swapcase().replace('l', 'z')
print(new_text)  # HEzzO, WOrLD!

# 리스트 메서드 체이닝

# 1. 리스트 메서드 체이닝의 흔한 실수
numbers = [3, 1, 4, 1, 5, 9, 2]

# [실수 1] sort()는 None을 반환하므로, 변수에 아무것도 저장되지 않음
result = numbers.copy().sort()
print(result)  # None (정렬된 리스트를 기대했으나 None 출력)

# [실수 2] append()가 반환한 None 뒤에 메서드를 이을 수 없음
numbers.append(7).extend(
    [8, 9]
)  # AttributeError: 'NoneType' object has no attribute 'extend'


# 2. 올바른 해결책 (Good Cases)
numbers = [3, 1, 4, 1, 5, 9, 2]

# [해결 1] 내장 함수 sorted() 사용 (추천)
# sorted()는 정렬된 '새로운 리스트'를 반환하므로 변수에 할당 가능
# (원본을 건드리지 않으므로 copy()를 굳이 쓸 필요 없음)
sorted_numbers = sorted(numbers)
print(sorted_numbers)  # [1, 1, 2, 3, 4, 5, 9]

# [해결 2] 메서드를 단계별로 나누어 작성
# 체이닝을 포기하고, 명확하게 한 줄씩 실행
copied_list = numbers.copy()
copied_list.sort()
print(copied_list)  # [1, 1, 2, 3, 4, 5, 9]
```






