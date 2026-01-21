# Python Functions

함수 구조  
```python
덧셈 예시
def get_sum(num1, num2):
    '''이것은 두 수를 받아
    두 수의  합을 반환하는 함수입니다.
    '''
    return num1 + num2

result = get_sum(5, 3)
print(result)
>>> 8
```
- num1, num2 -> parameter
- ''' 부분 -> docstring
- return이 없는 함수도 존재

함수 정의
- 함수 정의는 def 키워드로 시작
- def 키워드 이후 함수 이름 작성
- 괄호 안에 매개변수를 정의할 수 있음
- 함수 내에 return 문이 없으면 None이 반환


```python
result2 = print(100)
print(results)
>>> 100
>>> None
```

```python
def my_fun():
    """
    이 함수는 호출되면 터미널에 hello를 출력하는 함수
    """
    print('hello')

result3 = my_fun()
print(result3)
>>>hello
>>>None
```

매개변수
- 정의할 때 함수가 받을 값을 나타내는 변수  

인자
- 함수를 호출할 때 실제로 전달되는 값

다양한 인자의 종류
1. 위치 인자
    - 함수 호출 시 인자의 위치에 따라 전달되는 인지
    - 위치 인자는 함수 호출 시 반드시 값을 전달해야함
```python
def greet(name, age):
    print(f'안녕하세요, {name}님!, {age}살이군요.')
``` 
greet('bob') -> 불가 #age 인자 들어가지 x

2. 기본 인자 값
    - 함수 정의에서 매개변수에 기본 값을 할당하는 것
    - 함수 호출 시 인자를 전달하지 않으면, 기본값이 매개변수에 할당
```python
def greet(name, age=30):
    print(f'안녕하세요, {name}님!, {age}살이군요.')
```
greet('bob') -> 가능 #age에 30 할당  
3. 키워드 인자  
    - 함수 호출 시 이름과 함께 값을 전달하는 인자  
    - 매개 변수와 인자를 일치시키지 않고, 특정 매개변수 할당 가능  
    - 인자의 순서 중요 X  
    - 호출 시 키워드 인자는 위치 인자 뒤에 위치해야함  
greet(age=30, name ='Dave') -> 가능  
greet(age=30, 'Dave') -> 불가능 

4. 임의이 인자 목록
    - 정해지지 않은 개수의 인자를 처리하는 인자
    - 함수 정의 시 매개변수 앞에 *을 붙여 사용
    - 여러 개의 인자를 tuple로 처리
```python
def calculate_sum(*args):
    print(args) #(1, 100, 500, 30)
    print(type(args)) # <class 'tuple'>
```

5. 임의의 키워드 인자 목록
    - 정해지지 않은 개수의 키워드 인자를 처리하는 인자
    - 함수 정의 시 매개 변수 앞에 **을 붙여 사용
    - 여러 개의 인자를 dictionary로 묶어 철
```python
def print_info(**kwargs):
    print(kwargs)

print_info(name='Eve', age=30) 
>>> {'name' : 'Eve', 'age' : 30}
```
args, kwargs는 문법적으로 바꿀 수 있지만 보통 그냥 씀
```python
인자의 모든 종류를 적용한 예시
def func(pos1, pos2, default_arg='default', *args, **kwargs):
    print('pos1:', pos1)
    print('pos2:', pos2)
    print('default_arg:', default_arg)
    print('args:', args)
    print('kwargs:', kwargs)


func(1, 2, 3, 4, 5, 6, key1='value1', key2='value2')
"""
pos1: 1
pos2: 2
default_arg: 3
args: (4, 5, 6)
kwargs: {'key1': 'value1', 'key2': 'value2'}
"""
```

## 재귀함수
- 함수 안에서 자기 자신을 또 호출  
- 1개 이상의 종료되는 상황이 존재하고, 수렴하도록 작성
- 종료 조건 명확히 할 것
- 반복되는 호출이 종료 조건을 향하도록 할 것
```python
def factorial(n):
    if n == 0:
        return 1
    else:
        return n*factorial(n-1)

print(factorial(5))
>>>120
```
## 내장 함수 (Built-in function)
- 파이썬에 기본적으로 내장된 함수
- https://docs.python.org/ko/3.11/library/functions.html 확인
```python
numbers = [1, 2, 3, 4, 5]
print(numbers)  # [1, 2, 3, 4, 5]
print(len(numbers))  # 5
print(max(numbers))  # 5
print(min(numbers))  # 1
print(sum(numbers))  # 15
print(sorted(numbers, reverse=True))  # [5, 4, 3, 2, 1]
```

## 함수와 Scope(영역)
- 파이선의 범위
    - 함수 기준 코드 내부-> local scope 
    - 그 외의 공간 -> global scope
```python
def fun():
    num = 20 -> 지역 변수
    print('local', num) # local 20

print('global', num) #NameError -> 밖에서는 사용 안됨
```
- global 키워드
    - 변수의 스코프를 전역 범위로 지정하기 위해 사용
    - 일반적으로 함수 내에서 전역 변수로 활용하기 위해 사용
```python
num = 0  # 전역 변수
def increment():
    global num  # num를 전역 변수로 선언
    num += 1
print(num)  # 0
increment()
print(num)  # 1
```

## 함수의 규칙
- 약어 사용 지양
- return T/F -> is_로 시작
- 단일 책임 규칙
    - 모든 객체는 하나의 명확한 목적과 책임만을 가져야 함
    - 명확한 목적
        - 함수는 한 가지 작업만 수행
        - 함수 이름으로 목적을 명확히 표현
    - 책임 분리
        - 데이터 검증, 처리, 저장 등을 별도 함수로 분리
        - 각 함수는 독립적으로 동작 가능하도록 설계
    - 유지보수성
        - 작은 단위의 함수로 나누어 확인
        - 코드 수정 시 영향 범위를 최소화

```python
# 잘못된 설계 예시 (여러 책임이 섞인 함수)
def process_user_data(user_data):
    # 책임 1: 데이터 유효성 검사
    if len(user_data['password']) < 8:
        raise ValueError('비밀번호는 8자 이상이어야 합니다')
    # 책임 2: 비밀번호 암호화 및 저장
    user_data['password'] = hash_password(user_data['password'])
    db.users.insert(user_data)
    # 책임 3: 이메일 발송
    send_email(user_data['email'], '가입을 환영합니다!')
# 올바른 설계 예시 (책임을 분리한 함수들)
def validate_password(password):
    """비밀번호 유효성 검사"""
    if len(password) < 8:
        raise ValueError('비밀번호는 8자 이상이어야 합니다')
def save_user(user_data):
    """비밀번호 암호화 및 저장"""
    user_data['password'] = hash_password(user_data['password'])
    db.users.insert(user_data)
def send_welcome_email(email):
    """환영 이메일 발송"""
    send_email(email, '가입을 환영합니다!')
# 메인 함수에서 순차적으로 실행
def process_user_data(user_data):
    validate_password(user_data['password'])
    save_user(user_data)
    send_welcome_email(user_data['email'])
```

## 패킹 언패킹
```python
packed_values = 1, 2, 3, 4, 5
# 언패킹
a, b, c, d, e = packed_values
print(a, b, c, d, e)  # 1 2 3 4 5

# ‘*’ 을 활용한 언패킹 (함수 인자 전달)
def my_function(x, y, z):
    print(x, y, z)
names = ['alice', 'jane', 'peter']
my_function(*names)  # alice jane peter
```











