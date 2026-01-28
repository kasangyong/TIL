# 데이터 구조2

## 비시퀀스 데이터 구조

### 딕셔너리 
키와 값을 짝지어 저장하는 자료구조

- 딕셔너리는 내부적으로 해시 테이블을 사용하여 키-값 쌍을 관리
- 키를 통한 값의 삽입, 삭제, 검색이 데이터의 크기와 관계없이 매우 빠름
- 키는 hashable한 고유 값이어야 하지만, 값은 중복 가능하고, 어떠한 자료형도 저장 가능

#### 딕셔너리 메서드
```python
# get(key[,default]) -> [,default]는 선택 인자 표시
-> 키와 연결된 값을 반환하거나 키가 없으면 None 혹은 기본 값을 반환
person = {'name': 'Alice', 'age': 25}
print(person.get('name')) # Alice
print(person.get('country')) # None
print(person.get('country', 'Unknown')) # Unknown
# print(person['country'])  # KeyError: 'country'

# keys()
-> 딕셔너리 키를 모은 객체를 반환
person = {'name': 'Alice', 'age': 25}
print(person.keys())  # dict_keys(['name', 'age'])
for item in person.keys():
    print(item) # name\n age

# values()
-> 딕셔너리 값을 모은 객체를 반환
person = {'name': 'Alice', 'age': 25}
print(person.values())  # dict_values(['Alice', 25])
for item in person.values():
    print(item) # Alice\n 25

# items()
-> 딕셔너리 키/값 쌍을 모은 객체를 반환
person = {'name': 'Alice', 'age': 25}
print(person.items())  # dict_items([('name', 'Alice'), ('age', 25)])
for key, value in person.items():
    print(key, value) # name Alice \n age 25

# pop(key[,default])
-> 키를 제거하고 연결됐던 값을 반환(없으면 에러나 default를 반환)
person = {'name': 'Alice', 'age': 25}
print(person.pop('age'))  # 25
print(person)  # {'name': 'Alice'}
print(person.pop('country',None))  # None
# print(person.pop('country'))  # KeyError: 'country'

# clear()
-> 딕셔너리의 모든 키/값 쌍을 제거
person = {'name': 'Alice', 'age': 25}
person.clear()
print(person) # {}

# setdefault(key[, default])
-> 키와 연결된 값을 반환
-> 키가 없다면 default와 연결한 키를 딕셔너리에 추가하고 default를 반환
person = {'name': 'Alice', 'age': 25}
print(person.setdefault('country', 'KOREA'))  # KOREA
print(person)  # {'name': 'Alice', 'age': 25, 'country': 'KOREA'}

# update([other])
-> other가 제공하는 키/값 쌍으로 딕셔너리를 갱신하고 기존 키는 덮어씀
person = {'name': 'Alice', 'age': 25}
other_person = {'name': 'Jane', 'country': 'KOREA'}

person.update(other_person)
print(person)  # {'name': 'Jane', 'age': 25, 'country': 'KOREA'}

person.update(age=100, address='SEOUL')
print(
    person
)  # {'name': 'Jane', 'age': 100, 'country': 'KOREA', 'address': 'SEOUL'}

```

### 세트
고유한 항목들의 정렬되지 않은 컬렉션
- set은 내부적으로 해시 테이블을 사용하여 데이터를 저장
- 이로 인해 항목의 고유성을 효율적으로 보장하며, 항목의 추가, 삭제, 존재 여부 확인이 데이터의 크기에 관계없이 매우 빠름
- 합집합, 교집합, 차집합 등 수학적인 집합 연산을 간편하게 수행할 수 있음

#### 세트 메서드

```python
# add()
-> 세트에 x를 추가
my_set = {'a', 'b', 'c', 1, 2, 3}
my_set.add(4)
print(my_set) # {1, 'b', 4, 'c', 3, 2, 'a'}

my_set.add(4)
print(my_set) # {1, 'b', 4, 'c', 3, 2, 'a'}

# update(iterable)
-> 세트에 다른 iterable 요소를 추가
my_set = {'a', 'b', 'c', 1, 2, 3}
my_set.update([1, 4, 5])
print(my_set)  # {'c', 2, 3, 1, 'b', 4, 5, 'a'}

# clear()
-> 세트의 모든 항목 제거
my_set = {'a', 'b', 'c', 1, 2, 3}
my_set.clear()
print(my_set)  # set()

# remove(x)
-> 세트에서 항목 x를 제거, 항목 x가 없을 경우 keyerror
my_set = {'a', 'b', 'c', 1, 2, 3}
my_set.remove(2)
print(my_set) # {'c', 3, 1, 'b', 'a'}
# my_set.remove(10)  # KeyError: 10

# pop()
-> 세트에서 임의의 요소를 제거하고 반환
my_set = {'a', 'b', 'c', 1, 2, 3}
element = my_set.pop()
print(element) # 'b'
print(my_set) # {'a', 3, 2, 'c', 1}

# discard(x)
-> 세트 s에서 항목 x를 제거, remove와 달리 에러 없음
my_set = {'a', 'b', 'c', 1, 2, 3}
my_set.discard(2)
print(my_set) # {'a', 'c', 3, 1, 'b'}
my_set.discard(10)

# 집합 메서드
set1 = {0, 1, 2, 3, 4}
set2 = {1, 3, 5, 7, 9}
set3 = {0, 1}

print(set1.difference(set2))  # {0, 2, 4} set1 - set2
print(set1.intersection(set2))  # {1, 3} set1 & set2
print(set1.issubset(set2))  # False set1 <= set2
print(set3.issubset(set1))  # True  set3 <= set1
print(set1.issuperset(set2))  # False set1 >= set2 
print(set1.union(set2))  # {0, 1, 2, 3, 4, 5, 7, 9} set1 | set2
```
### 딕셔너리 확장
#### defaultdict
파이썬 내장 모듈 collections에서 제공하는 딕셔너리의 확장판
- 딕셔너리에 존재하지 않은 키를 조회할 때, 자동으로 기본 값을 생성해주는 자료 구조
- keyerror 걱정 없이 값을 수정하거나 추가할 때 매우 유용

```python
# 기본 딕셔너리 vs defaultdict
# 기존: 기본 딕셔너리
text = 'banana'
counts = {}

for char in text:
    if char not in counts:  # 키가 있는지 매번 확인해야 함
        counts[char] = 0
    counts[char] += 1

print(counts)  # {'b': 3, 'a': 3, 'n': 2}


# 개선: defaultdict 활용
from collections import defaultdict

text = 'banana'
counts = defaultdict(int)  # 기본값을 정수(0)로 설정

for char in text:
    # 키 확인 불필요! 없으면 0으로 자동 생성 후 +1
    counts[char] += 1

print(counts)  # defaultdict(<class 'int'>, {'b': 1, 'a': 3, 'n': 2})


# 활용 예제
# 색상별 과일 분류
fruits = [('red', 'apple'), ('yellow', 'banana'), ('red', 'cherry')]
fruit_by_color = defaultdict(list)

for color, fruit in fruits:
    # color 키가 없으면 빈 리스트 생성 -> append 바로 가능
    pass

print(
    fruit_by_color
)  # defaultdict(<class 'list'>, {'red': ['apple', 'cherry'], 'yellow': ['banana']})
print(
    dict(fruit_by_color)
)  # {'red': ['apple', 'cherry'], 'yellow': ['banana']}
```

defaultdict의 주의사항
- defaultdict는 조회만 해도 키가 생성됨
- 따라서 단순히 키가 있는지 확인할 때는 주의해서 사용해야함 -> get사용
- .setdefault와의 차이점
    - setdefault는 매번 호출할 때마다 기본값을 넣어줘야 하지만, defaultdict는 객체를 생성할 때 한 번만 설정해도 됨

### 해시 테이블 
- 해시 테이블은 키와 값을 짝지어 저장하는 자료 구조

#### 해시 테이블의 원리
1. 키를 해시 함수를 통해 해시 값으로 변환
2. 변환된 해시 값을 인덱스로 삼아 데이터를 저장하거나 찾음
3. 이로 인해 검색, 삽입, 삭제를 매우 빠르게 수행
#### 해시
- 임의의 크기를 가진 데이터를 고정된 크기의 고유한 값으로 변환한 것
- 생성된 해시 값(고유한 정수)은 해당 데이터를 식별하는 지문 역할을 함
- 파이썬에서는 이 해시 값을 이용해 해시 테이블에 데이터 저장
- 이 변환을 수행하는 것이 해시 함수

#### 해시 함수
- 임의 길이 데이터를 입력 받아 고정 길이로 변환해 주는 함수, 이 정수가 해시 값
- 주로 해시 테이블을 구현할 때, 매우 빠른 검색 및 데이터 저장 위치 결정을 위해 활용



#### set의 요소 & dict의 키와 해시 테이블의 관계
set
- 각 요소를 해시 함수로 변환해 나온 해시 값에 맞춰 해시 테이블 내부 버킷에 위치시킴
- 그래서 순서라기보다 버킷 위치(인덱스)가 요소의 위치를 결정
- 따라서 set는 순서를 보장하지 않음  
- 문자열은 해시 계산 시 파이썬의 해시 난수화가 적용 -> 실행마다 순서가 달라질 수 있음  
dict
- 키 - 해시 함수 - 해시 값 - 해시 테이블에 저장
- set와 달리 삽입 순서는 유지한다는 것이 언어 사양에 따라 보장 됨
    - 즉 키를 추가한 순서대로 반복문 순회할 때 나오게 됨
    - 사용자에게 보여지는 키 순서는 삽입 순서가 유지되도록 설계한 것

### 파이썬 문법 규격
BNF -> 프로그래밍 언어의 문법을 표현하기 위한 표기법  
EBNF -> 메타기호를 추가  
[] -> 선택적 요소  
{} -> 0번 이상 반복  
() -> 그룹화  

