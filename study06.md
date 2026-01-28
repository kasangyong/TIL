# OOP 객체 지향 프로그래밍

## 절차 지향과 객체 지향

### 절차 지향 프로그래밍
- 함수와 로직 중심 작성
- 데이터를 순차적으로 처리
    - ex) 물을 끓인다 -> 면을 넣는다 -> 스프를 넣는다 -> 불을 끄고 그릇에 담는다

#### 절차 지향 프로그래밍의 특징
- 입력을 받고 처리하고 결과를 내는 과정이 위에서 아래로 순차적으로 흐르는 형태
- 순차적인 명령어 실행
- 데이터와 함수의 분리
- 함수 호출의 흐름이 중요

#### 절차 지향 프로그래밍의 한계
- 복잡성 증가
    - 프로그램 규모가 커질수록 데이터와 함수의 관리가 어려움
    - 전역 변수의 증가로 인한 관리의 어려움
- 유지보수 문제
    - 코드 수정 시 영향 범위의 파악 어려움

### 객체 지향 프로그래밍 
- 클래스는 설계도, 인스턴스는 실제 물건
    - ex) 자동차를 만드는 설계도 -> class
    - 설계도를 바탕으로 실제 조립한 자동차 한 대 한 대가 인스턴스
    - 인스턴스는 서로 다른 특성을 가질 수 있음
    - 클래스를 정의한 뒤 그 클래스를 바탕으로 여러 개의 인스턴스를 만들 수 있음

#### 객체 지향 프로그래밍의 특징
- 프로그램을 데이터와 그 데이터를 처리하는 함수를 하나의 단위로 묶어서 조직적으로 관리
- 데이터와 메서드를 결합
    - ex) 주방 도구, 재료, 행동을 각각 별개로 생각 x -> '볶음밥 기계' 객체를 만들어 기계가 스스로 행동과 재료를 관리하는 방식

### 객체
- 실제 존재하는 사물을 추상화한 것
- '속성'과 '동작'을 가짐
    - 예를 들어 '강아지'라는 객체는 이름, 종, 나이, 행동 등으로 표현할 수 있음

#### 객체 특징
- 속성 -> 객체의 상태/데이터
- 메서드 -> 객체의 행동/기능
- 고유성 -> 각 객체는 고유한 특성을 지님

### 클래스
- 객체를 만들기 위한 설계도
- 데이터와 기능을 함께 묶는 방법을 제공
- 파이썬에서 타입을 표현하는 방법
- 클래스로부터 여러 개의 객체를 쉽게 만들어 낼 수 있음

#### 클래스의 정의
- 클래스는 관련된 데이터(속성)와 기능(메서드)을 하나의 묶음으로 정의하는 설계도
- class 키워드
- 클래스 이름은 파스칼 케이스 방식으로 작성
```python
class MyClass:
    pass
```
#### 클래스 예시
- __init__ 메서드는 '생성자 메서드'로 불리며, 새로운 객체를 만들 때 필요한 초기값을 설정
- __ -> 매직 메서드, 파이썬이 호출 시점 결정
```python
# 객체 지향 사고
# 예: 사람(객체) 안에 name, age와 이와 관련된 기능(메서드) 포함
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def introduce(self):
        print(f'안녕하세요, {self.name}입니다. 나이는 {self.age}살입니다.')


alice = Person('Alice', 25)
alice.introduce()  # 객체가 자신의 정보를 출력
```

### 인스턴스
- 클래스를 통해 생성된 객체
- 인스턴스는 클래스를 사용해 실제로 만들어진 객체
- 같은 클래스로 여러 인스턴스를 만들 수 있음
- 각 인스턴스는 클래스 구조를 따라 동작하지만 서로 독립된 데이터를 가질 수 있음

#### 인스턴스 예시
- ex) '아이유'는 '가수'클래스의 인스턴스이다.
```python
p1 = Person('Alice', 25) -> 인스턴스
p1.introduce()  # "안녕하세요. 저는 Alice, 나이는 25살입니다."

p2 = Person('Bella', 30) -> 인스턴스
p2.introduce()  # "안녕하세요. 저는 Bella, 나이는 30살입니다."
```

### 클래스와 인스턴스
- 클래스를 정의한다는 것은 공통된 특성과 기능을 가진 틀을 만드는 것
- 실제 활동하는 개별 객체들은 이 틀에서 생성된 인스턴스
- 공통된 특성과 기능을 가진 틀을 만드는 것은 곧 새로운 타입을 만드는 행위

```python
class Singer:
    pass

iu = Singer()
print(type(iu)) # <class '__main__.Singer'>
name = 'Alice'
print(type(name)) # <class 'str'> -> Alice 는 str 클래스의 인스턴스
#split() 은 class str에 정의되어 있음
#append() 는 class list에 정의되어 있음
```

### 클래스 구조
- 생성자 메서드
    - 인스턴스 생성 시 자동 호출되는 특별한 메서드
    - __init__이라는 이름의 메서드로 정의
    - 인스턴스 변수의 초기화 담당
- 인스턴스 변수(속성)
    - 각 인스턴스별 고유한 속성
    - self.변수명 형태로 정의
    - 인스턴스마다 독립적인 값 유지
- 클래스 변수(속성)
    - 모든 인스턴스가 공유하는 속성
    - 클래스 내부에서 직접 정의
```python
class Circle:
    pi = 3.14
    # 생성자 메서드
    def __init__(self, radius):
        self.radius = radius
# 인스턴스 생성
c1 = Circle(1)
c2 = Circle(2)
# 인스턴스 변수(속성) 접근
print(c1.radius) # 1
print(c2.radius) # 2

# 클래스 변수(공통 속성) 접근
print(c1.pi) #3.14
print(c2.pi) #3.14

#파이썬 내에서는 인스턴스 변수와 클래스 변수 구분 못함 -> 인스턴스 변수 먼저 접근
```
### 메서드
#### 인스턴스 메서드
- 클래스 내부에 정의되는 메서드의 기본
- 반드시 첫 번째 인자로 인스턴스 자신(self)을 받음
- 인스턴스의 속성에 접근하거나 변경 가능

#### 생성자 메서드 
- 인스턴스 객체가 생성될 때 자동으로 호출되는 메서드
- 인스턴스 변수들의 초기값을 설정
```python
class Counter:
    def __init__(self):
        self.count = 0

    # 인스턴스 메서드
    def increment(self):
        # 이 메서드를 호출하는 인스턴스의 변수 count를 1 증가 시키는 메서드
        self.count +=1


c = Counter()
c1 = Counter()
# 인스턴스 메서드 호출
print(c.count) # 0
c.increment()
print(c.count) # 1
print(c1.count) # 0
```
#### 클래스 메서드
- 클래스 변수를 조작하거나 클래스 레벨의 동작을 수행
- @classmethod 데코레이터를 사용하여 정의
- 호출 시, 첫번째 인자로 해당 메서드를 호출하는 클래스(cls)가 전달됨
- 클래스를 인자로 받아 클래스 속성을 변경하거나 읽는 데 사용
```python
class Person:
    population = 0

    def __init__(self, name):
        self.name = name
        # 클래스 메서드를 통해 인스턴스가 생성될 때마다 인구 수 증가
        Person.increase_population()

    # 클래스 메서드
    @classmethod
    def increase_population(cls):
        cls.population +=1


# 인스턴스 생성
person1 = Person('Alice')
person2 = Person('Bob')

# 클래스 변수 접근
print(Person.population) # 2
```

#### 스태틱 메서드
- 클래스, 인스턴스에 상관없이 독립적으로 동작하는 메서드
- @staticmethod 데코레이터를 사용하여 정의
- 호출 시 자동으로 전달 받는 인자가 없음(self, cls를 받지 않음)
- 인스턴스나 클래스 속성에 직접 접근하지 않는, '도우미 함수'와 비슷한 역할
```python
class MathUtils:
    # 정적 메서드
    # 클래스나 인스턴스에 의존하지 않고 독립적으로 동작
    # 정적 메서드는 클래스나 인스턴스의 상태를 변경하지 않음
    @staticmethod
    def add(a,b):
        return a+b


# 정적 메서드 호출
print(MathUtils.add(3,5 )) # 8
```

#### 메서드 사용 역할
```python
class MyClass:
    def instance_method(self):
        return 'instance method', self

    @classmethod
    def class_method(cls):
        return 'class method', cls

    @staticmethod
    def static_method():
        return 'static method'


instance = MyClass()
# 클래스가 할 수 있는 것
# 사실 클래스는 모든 메서드를 호출할 수 있음
print(MyClass.instance_method(instance))
print(MyClass.class_method())
print(MyClass.static_method())


# 인스턴스가 할 수 있는 것
# 사실 인스턴스는 모든 메서드를 호출할 수 있음
print(instance.instance_method())
print(instance.class_method())
print(instance.static_method())


# 기술적인 '가능 여부(Can)'와 논리적인 '사용 의도(Should)'를 구분해야 함
# 1. 클래스의 입장
# - 클래스 메서드와 정적 메서드를 호출하는 것이 주된 역할입니다.
print(MyClass.class_method())
print(MyClass.static_method())

# 2. 인스턴스의 입장
# - 인스턴스 메서드는 오직 인스턴스만 호출하는 것이 좋습니다.
print(instance.instance_method())
```

