# Python의 스택 (Stack)

## 스택이란?
스택(Stack)은 컴퓨터 과학에서 사용되는 기본적인 자료구조 중 하나로, **LIFO (Last In First Out)** 방식으로 작동합니다. 즉, 마지막에 들어온 데이터가 가장 먼저 나오는 구조입니다. 스택은 함수 호출, 브라우저의 뒤로가기 기능, 또는 알고리즘 구현 등 다양한 곳에서 사용됩니다.

## Python에서의 스택 구현
Python에는 내장된 스택 자료형이 없지만, **리스트(List)**를 사용하여 쉽게 스택을 구현할 수 있습니다. 리스트의 `append()` 메서드로 요소를 추가하고, `pop()` 메서드로 요소를 제거할 수 있습니다.

### 주요 연산
- **push(item)**: 스택에 요소를 추가 (리스트의 `append()` 사용)
- **pop()**: 스택의 맨 위 요소를 제거하고 반환 (리스트의 `pop()` 사용)
- **peek()**: 스택의 맨 위 요소를 확인 (리스트의 `[-1]` 인덱스 사용)
- **is_empty()**: 스택이 비어있는지 확인 (리스트의 길이가 0인지 확인)

## 간단한 예시
다음은 Python 리스트를 사용하여 스택을 구현한 간단한 예시입니다:

```python
# 스택 초기화
stack = []

# 요소 추가 (push)
stack.append(1)
stack.append(2)
stack.append(3)

print("스택:", stack)  # 출력: [1, 2, 3]

# 맨 위 요소 확인 (peek)
if stack:
    print("맨 위 요소:", stack[-1])  # 출력: 3

# 요소 제거 (pop)
popped = stack.pop()
print("제거된 요소:", popped)  # 출력: 3
print("스택:", stack)  # 출력: [1, 2]

# 스택이 비어있는지 확인
print("스택이 비어있나요?", len(stack) == 0)  # 출력: False
```

이 예시에서는 리스트를 스택처럼 사용하여 요소를 추가하고 제거하는 기본적인 동작을 보여줍니다. 실제 응용에서는 이러한 연산들을 함수로 캡슐화하여 사용하기도 합니다.

## 스택 기반 문제 해결 기법
스택은 다양한 알고리즘과 문제 해결에 유용하게 사용됩니다. 특히, 순서가 중요하거나 역순 처리가 필요한 경우에 효과적입니다. 아래에 몇 가지 대표적인 예시를 살펴보겠습니다.

### 1. 괄호 짝 맞추기 (Parentheses Matching)
괄호가 올바르게 짝을 이루는지 확인하는 문제입니다. 여는 괄호를 스택에 넣고, 닫는 괄호를 만나면 스택에서 꺼내어 짝이 맞는지 확인합니다.

#### 예시 코드
```python
def is_valid_parentheses(s):
    stack = []
    mapping = {')': '(', '}': '{', ']': '['}
    
    for char in s:
        if char in mapping:
            # 닫는 괄호인 경우
            top_element = stack.pop() if stack else '#'
            if mapping[char] != top_element:
                return False
        else:
            # 여는 괄호인 경우
            stack.append(char)
    
    return not stack

# 테스트
print(is_valid_parentheses("()"))      # True
print(is_valid_parentheses("()[]{}"))  # True
print(is_valid_parentheses("(]"))      # False
print(is_valid_parentheses("([)]"))    # False
```

#### 해결 방법 설명
- 여는 괄호를 만나면 스택에 추가합니다.
- 닫는 괄호를 만나면 스택의 맨 위 요소와 짝이 맞는지 확인합니다.
- 문자열을 모두 처리한 후 스택이 비어있어야 유효합니다.

### 2. 후위 표기법 계산 (Postfix Evaluation)
후위 표기법(역 폴란드 표기법)에서 수식을 계산하는 문제입니다. 피연산자는 스택에 넣고, 연산자를 만나면 스택에서 두 개의 피연산자를 꺼내어 계산합니다.

#### 예시 코드
```python
def evaluate_postfix(expression):
    stack = []
    
    for token in expression.split():
        if token.isdigit():
            stack.append(int(token))
        else:
            # 연산자인 경우
            b = stack.pop()
            a = stack.pop()
            if token == '+':
                stack.append(a + b)
            elif token == '-':
                stack.append(a - b)
            elif token == '*':
                stack.append(a * b)
            elif token == '/':
                stack.append(a // b)  # 정수 나눗셈
    
    return stack[0]

# 테스트
print(evaluate_postfix("2 1 + 3 *"))  # (2 + 1) * 3 = 9
print(evaluate_postfix("4 13 5 / +")) # 4 + (13 / 5) = 6
```

#### 해결 방법 설명
- 숫자를 만나면 스택에 추가합니다.
- 연산자를 만나면 스택에서 두 개의 숫자를 꺼내어 연산을 수행하고 결과를 다시 스택에 넣습니다.
- 최종적으로 스택에 남은 하나의 값이 계산 결과입니다.

이러한 기법들은 스택의 LIFO 특성을 활용하여 문제를 효율적으로 해결할 수 있습니다. 더 복잡한 문제에서도 스택을 적절히 사용하면 코드의 간결성과 효율성을 높일 수 있습니다.

## 추가 알고리즘 개념

### 1. 재귀호출 (Recursion)
재귀호출은 함수가 자기 자신을 호출하는 프로그래밍 기법입니다. 문제를 더 작은 하위 문제로 나누어 해결하며, 종료 조건이 필요합니다.

#### 동작 원리
- 함수가 자신을 호출하면서 문제를 점점 작게 만듭니다.
- 기본 케이스(base case)에 도달하면 재귀를 멈추고 결과를 반환합니다.
- 스택을 사용하여 호출된 함수들을 관리합니다.

#### 예시: 팩토리얼 계산
```python
def factorial(n):
    if n == 0 or n == 1:  # 기본 케이스
        return 1
    else:
        return n * factorial(n - 1)

print(factorial(5))  # 120
```

#### 이해하기 쉬운 그림 (ASCII Art)
```
factorial(5)
├── 5 * factorial(4)
│   ├── 4 * factorial(3)
│   │   ├── 3 * factorial(2)
│   │   │   ├── 2 * factorial(1)
│   │   │   │   └── 1 (기본 케이스)
│   │   │   └── 2 * 1 = 2
│   │   └── 3 * 2 = 6
│   └── 4 * 6 = 24
└── 5 * 24 = 120
```

#### 장점
- 코드가 간결하고 읽기 쉽습니다.
- 복잡한 문제를 자연스럽게 분해할 수 있습니다.
- 단점: 스택 오버플로우 위험이 있고, 메모리 사용이 많을 수 있습니다.

### 2. 배열 원소 검색 (Array Element Search)
배열에서 특정 원소를 찾는 알고리즘입니다. 선형 검색과 이진 검색이 대표적입니다.

#### 동작 원리 (선형 검색)
- 배열의 처음부터 끝까지 하나씩 비교합니다.
- 찾는 원소가 나오면 인덱스를 반환합니다.

#### 예시: 선형 검색
```python
def linear_search(arr, target):
    for i in range(len(arr)):
        if arr[i] == target:
            return i
    return -1

arr = [3, 1, 4, 1, 5, 9, 2, 6]
print(linear_search(arr, 5))  # 4
```

#### 이해하기 쉬운 그림 (선형 검색)
```
배열: [3, 1, 4, 1, 5, 9, 2, 6]
       ↑  ↑  ↑  ↑  ↑  ← 찾음!
인덱스: 0  1  2  3  4
```

#### 장점
- 구현이 간단합니다.
- 정렬되지 않은 배열에서도 사용할 수 있습니다.
- 단점: 큰 배열에서 비효율적입니다 (O(n) 시간 복잡도).

### 3. 피보나치 수열 (Fibonacci Sequence)
피보나치 수열은 각 숫자가 앞의 두 숫자의 합인 수열입니다 (0, 1, 1, 2, 3, 5, 8, ...).

#### 동작 원리 (기본 재귀)
- F(n) = F(n-1) + F(n-2), F(0)=0, F(1)=1

#### 예시: 기본 재귀 피보나치
```python
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

print(fibonacci(10))  # 55
```

#### 메모이제이션 (Memoization)
이전에 계산한 결과를 저장하여 중복 계산을 피합니다.

##### 동작 원리
- 딕셔너리나 배열에 계산 결과를 저장합니다.
- 같은 입력에 대해 저장된 값을 반환합니다.

##### 예시: 메모이제이션 피보나치
```python
memo = {}
def fibonacci_memo(n):
    if n in memo:
        return memo[n]
    if n <= 1:
        memo[n] = n
    else:
        memo[n] = fibonacci_memo(n-1) + fibonacci_memo(n-2)
    return memo[n]

print(fibonacci_memo(10))  # 55
```

##### 이해하기 쉬운 그림
```
F(5) 호출
├── F(4) + F(3)
│   ├── F(3) + F(2)  ← 메모에 저장
│   │   ├── F(2) + F(1)  ← 메모에 저장
│   │   │   ├── F(1) + F(0)  ← 메모에 저장
│   │   │   │   └── 1 + 0 = 1
│   │   │   └── 1 + 1 = 2
│   │   └── 2 + 1 = 3
│   └── 3 + 2 = 5
└── 5 + 3 = 8
```

##### 장점
- 중복 계산을 제거하여 효율적입니다 (O(n) 시간).
- 재귀의 장점을 유지하면서 성능을 향상시킵니다.

#### 동적 계획법 (Dynamic Programming, DP)
하위 문제의 결과를 저장하며 상향식으로 해결합니다.

##### 동작 원리
- 작은 문제부터 해결하며 결과를 배열에 저장합니다.
- 큰 문제를 작은 문제의 조합으로 해결합니다.

##### 예시: DP 피보나치
```python
def fibonacci_dp(n):
    if n <= 1:
        return n
    dp = [0] * (n + 1)
    dp[1] = 1
    for i in range(2, n + 1):
        dp[i] = dp[i-1] + dp[i-2]
    return dp[n]

print(fibonacci_dp(10))  # 55
```

##### 이해하기 쉬운 그림
```
dp 배열: [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
인덱스:    0  1  2  3  4  5  6   7   8   9  10
```

##### 장점
- 시간 복잡도가 O(n)으로 효율적입니다.
- 메모리 사용이 최적화됩니다.
- 재귀 호출 스택 오버플로우를 피할 수 있습니다.

#### 깊이 우선 탐색 (Depth-First Search, DFS)
그래프나 트리를 탐색할 때 한 방향으로 깊게 탐색하는 방법입니다. 스택을 사용하여 구현할 수 있습니다.

##### 동작 원리
- 시작 노드부터 인접한 노드를 스택에 넣고 방문합니다.
- 더 이상 갈 곳이 없으면 스택에서 꺼내어 이전 노드로 돌아갑니다.

##### 예시: DFS (그래프 탐색)
```python
def dfs(graph, start, visited=None):
    if visited is None:
        visited = set()
    visited.add(start)
    print(start, end=' ')
    for neighbor in graph[start]:
        if neighbor not in visited:
            dfs(graph, neighbor, visited)

graph = {
    'A': ['B', 'C'],
    'B': ['A', 'D', 'E'],
    'C': ['A', 'F'],
    'D': ['B'],
    'E': ['B', 'F'],
    'F': ['C', 'E']
}

dfs(graph, 'A')  # A B D E F C
```

##### 이해하기 쉬운 그림 (트리 형태)
```
     A
    / \
   B   C
  / \   \
 D   E   F
      \
       F
```

##### 장점
- 구현이 간단합니다 (스택 사용).
- 메모리 사용이 적습니다 (한 경로만 저장).
- 미로 찾기나 퍼즐 해결에 유용합니다.
- 단점: 최단 경로를 보장하지 않습니다.
