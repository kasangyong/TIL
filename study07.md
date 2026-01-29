# OOP

## 상속
- 한 클래스의 속성과 메서드를 다른 클래스가 물려받는 것
- 부모 클래스와 자식 클래스 간의 상하 관계가 형성되고, 위쪽에 있는 부모 클래스가 본인의 속성과 매서드를 아래쪽에 있는 자식에게 넘겨줌
- 속성과 메서드를 자식에게 넘겨주는 과정이 상속 과정

### 상속이 필요한 이유
1. 코드 재사용
    - 상속을 통해 기존 클래스의 속성과 메서드를 재사용할 수 있음
    - 기존 클래스를 수정하지 않고도 기능을 확장할 수 있음
2. 계층 구조
    - 상속을 토해 클래스들 간의 계층 구조를 형성할 수 있음
    - 부모 클래스와 자식 클래스 간의 관계를 표현하고, 더 구체적인 클래스를 만들 수 있음
3. 유지 보수의 용이성
    - 상속을 통해 기존 클래스의 수정이 필요한 경우, 해당 클래스만 수정하면 되므로 유지 보수가 용이해짐
    - 코드의 일관성을 유지하고, 수정이 필요한 범위를 최소화 할 수 있음

```python
# 상속을 사용한 계층구조 변경
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def talk(self):  # 메서드 재사용
        print(f'반갑습니다. {self.name}입니다.')


class Professor(Person):
    def __init__(self, name, age, department):
        self.name = name
        self.age = age
        self.department = department


class Student(Person):
    def __init__(self, name, age, gpa):
        self.name = name
        self.age = age
        self.gpa = gpa


p1 = Professor('박교수', 49, '컴퓨터공학과')
s1 = Student('김학생', 20, 3.5)

# 부모 Person 클래스의 talk 메서드를 활용
p1.talk()  # 반갑습니다. 박교수입니다.

# 부모 Person 클래스의 talk 메서드를 활용
s1.talk()  # 반갑습니다. 김학생입니다.
```

### 메서드 오버라이딩
- 부모 클래스의 메서드를 같은 이름, 같은 파라미터 구조로 재정의 하는 것
- 자식 클래스에서 메서드를 다시 정의하면, 부모 클래스의 메서드 대신 자식 클래스의 메서드가 실행됨
- 오버라이딩은 동일한 이름과 매개변수를 사용하지만, 내부 동작을 원하는 대로 바꿀 수 있게 해줌
- 부모 클래스의 기능을 유지하면서도 일부 동작을 맞춤형으로 바꾸고 싶을 때 유용

```python
class Animal:
    def eat(self):
        print('Animal이 먹는 중')

class Dog(Animal):
    # 오버라이딩 (부모 클래스 Animal의 eat 메서드를 재정의)
    def eat(self):
        print('Dog가 먹는 중')

my_dog = Dog()
my_dog.eat()  # Dog가 먹는 중
```

## 다중 상속
- 둘 이상의 상위 클래스로부터 여러 행동이나 특징을 상속받을 수 있음
- 상속받은 모든 클래스의 요소를 활용 가능
- 중복된 속성이나 메서드가 있는 경우 상속 순서에 의해 결정

```python
# 다중 상속 예시
class Person:
    def __init__(self, name):
        self.name = name
    def greeting(self):
        return f'안녕, {self.name}'


class Mom(Person):
    gene = 'XX'
    def swim(self):
        return '엄마가 수영'


class Dad(Person):
    gene = 'XY'
    def walk(self):
        return '아빠가 걷기'


class FirstChild(Dad, Mom):
    def swim(self):
        return '첫째가 수영'
    def cry(self):
        return '첫째가 응애'

baby1 = FirstChild('아가')
print(baby1.cry())  # 첫째가 응애
print(baby1.swim())  # 첫째가 수영
print(baby1.walk())  # 아빠가 걷기
# print(baby1.gene)  # XY
```

### MRO
- 파이썬이 매서드를 찾는 순서에 대한 규칙(메서드 결정 순서)
#### super()
- MRO에 따라 현재 클래스의 부모 클래스의 메서드나 속성에 접근할 수 있게 해주는 내장 함수
##### 특징
- 단순히 부모 클래스의 메서드를 호출하기 위한 용도 뿐만 아니라
- 다중 상속이 있을 때도 올바른 순서에 따라 상위 클래스의 메서드를 찾아 실행하기 위해 super() 사용
```python
class Person:
    def __init__(self, name, age, number, email):
        self.name = name
        self.age = age
        self.number = number
        self.email = email


class Student(Person):
    def __init__(self, name, age, number, email, student_id):
        # super()를 통해 Person의 __init__ 메서드 호출
        super().__init__(name, age, number, email) # Person.__init__(name, age, number, email)와 같음
        self.student_id = student_id
```
``` python
# 다중 상속
class ParentA:
    def __init__(self):
        super().__init__()  -> 이 부분 중요한 듯
        self.value_a = 'ParentA'

    def show_value(self):
        print(f'Value from ParentA: {self.value_a}')


class ParentB:
    def __init__(self):
        self.value_b = 'ParentB'

    def show_value(self):
        print(f'Value from ParentB: {self.value_b}')


class Child(ParentA, ParentB):
    def __init__(self):
        super().__init__()  # ParentA 클래스의 __init__ 메서드 호출
        self.value_c = 'Child'

    def show_value(self):
        super().show_value()  # ParentA 클래스의 show_value 메서드 호출
        print(f'Value from Child: {self.value_c}')


child = Child()
child.show_value()
"""
Value from ParentA: ParentA
Value from Child: Child
"""

print(child.value_c)  # Child
print(child.value_a)  # ParentA
print(
    child.value_b
)  # AttributeError: 'Child' object has no attribute 'value_b'
```

```
<ParentA에 super().__init__()를 추가하면?>
그 다음으로 ParentB의 __init__가 실행되어 value_b도 초기화할 수 있음
그러면 print(child.value_b)는 ParentB를 출력하게 됨

print(child.value_b)  # ParentB


<Child 클래스의 MRO>
Child -> ParentA -> ParentB

super()는 단순히 “직계 부모 클래스를 가리킨다”가 아니라, 
MRO 순서를 기반으로 “현재 클래스의 다음 순서” 클래스(또는 메서드)를 가리킴

따라서 ParentA에서 super()를 부르면 MRO상 다음 클래스인 ParentB.__init__()가 호출됨




1.1 Child 클래스의 인스턴스를 생성할 때 일어나는 일
    1.	child = Child() 호출 시, Child.__init__()가 실행
    2.	Child.__init__() 내부에서 super().__init__()를 호출
        - 여기서 Child의 super()는 MRO에 의해 ParentA의 __init__()를 가리킴
    3.	ParentA.__init__()로 진입

1.2. ParentA.__init__() 내부
	1.	ParentA.__init__()에는 다시 super().__init__()가 있음
	2.	ParentA를 기준으로 MRO에서 “다음 클래스”는 ParentB, 따라서 ParentA의 super().__init__()는 ParentB.__init__() 호출
    3.	ParentB.__init__()가 실행되면서 self.value_b = 'ParentB'가 설정됨
	4.	ParentB.__init__()가 종료된 후, 다시 ParentA.__init__()로 돌아와 self.value_a = 'ParentA'가 설정됨
	5.	ParentA.__init__() 종료 후, 다시 Child.__init__()로 돌아감
	6.	마지막으로 Child.__init__() 내에서 self.value_c = 'Child'가 설정되고 종료

1.3 결과적으로 child 인스턴스는 value_a, value_b, value_c 세 속성을 모두 갖게 됨
	- child.value_a → 'ParentA'
	- child.value_b → 'ParentB' 
	- child.value_c → 'Child'
```

## 디버깅
### 버그
- 소프트웨어에서 발생하는 오류 또는 결함
- 프로그램의 예상된 동작과 실제 동작 사이의 불일치

### 디버깅
- 소프트웨에서 발생하는 버그를 찾아내고 수정하는 과정
- 프로그램의 오작동 원인을 실별하여 수정하는 작업
- 디버깅은 코드 실행 과정에서 변수 값이나 흐름을 점검하며 문제의 정확한 위치와 원인을 찾아내는 과정
- 효과적인 디버깅을 위해 단계별로 코드를 실행하거나 로그를 출력해 프로그램 상태를 확인
#### 디버깅 방법
1. print 함수 활용
    - 특정 함수 결과, 반복/조건 결과 등 나눠서 생각
    - 코드를 bisection으로 나눠서 생각
2. 개발 환경에서 제공하는 기능 활용
    - breakpoint, 변수 조회 등
3. python tutor 활용
4. 뇌 컴파일, 눈 디버깅 등

## 에러
- 프로그램 실행 중 발생하는 예외 상황
- 프로그램을 실행할 때 예상치 못한 문제가 발생하면 오류가 생김
### 문법 에러
- 프로그램의 구문이 올바르지 않은 경우 발생 
#### 문법 에러 예시
- Invalid syntax(문법 오류)
    while -> 뒤에 : 안붙임
- assign to literal(잘못된 할당)
    5 = 3
- Unterminated string literal -> 보통 문자열이나 문장을 제대로 닫지 않은 상태에서 줄 끝에 다다르면 발생
    print('hello

### 예외
- 프로그램 실행 중 감지되는 에러
#### 내장 예외 예시
- ZeroDivisionError -> 나누기 또는 모듈로 연산의 두 번째 인자가 0일 때 발생
    10/0
- NameError -> 지역 또는 전역 이름을 찾을 수 없을 때 발생
    print(name1) -> name1이 정의되지 않음
- TypeError -> 타입 불일치, 인자 누락, 인자 초과, 인자 타입 불일치
    '2' + 2
    sum(1, 2, 3) -> sum은 2개 변수 받아야됨
    import random / random.sample(1,2) -> sequence 여야 함 (dict or set)
- ValueError
    int('1.5')
- IndexError -> 시퀀스 인덱스가 범위를 벗어남
    empty_list = []/ empty_list[2]
- KeyError
- ModuleNotFoundError -> 모듈 찾을 수 없음
- ImportError
- KeyboardInterrupt -> 사용자가 컨트롤 c 등 누를 때 발생, 강제종료시
- IndentationError -> 잘못된 들여쓰기와 관련된 문법 오류

#### 예외 처리
try
    - 예외가 발생할 수 있는 코드 작성
except
    - 예외가 발생했을 때 실행할 코드 작성
else
    - 예외가 발생하지 않았을 때 실행할 코드 작성
finally
    - 예외 발생 여부와 상관없이 항상 실행할 코드 작성
```python
# try-except
try:
    result = 10/0
except ZeroDivisionError:
    print('0으로 나눌 수 없음')

try:
    num = int(input('숫자를 입력하세요: '))
except ValueError:
    print('그건 숫자가 아니야')

```
###### 예외 처리 주의사항
- 내장 예외 클래스는 상속 계층구조를 가지기 때문에
- except 절로 분기 시 반드시 하위 클래스를 먼저 확인 할 수 있도록 작성해야함

#### 예외 객체 다루기
as 키워드
    - 예외객체: 예외가 발생했을 때 예외에 대한 정보를 담고 있는 객체
    - except 블록에서 예외 객체를 받아 상세한 예외 정보 활용 가능
```py
my_list = []

try:
    number = my_list[1]
except IndexError as error:
    # list index out of range가 발생했습니다.
    print(f'{error}가 발생했습니다.')
```




