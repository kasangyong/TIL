# stack

## stack 계산기

### 후위 표기법 변환
- 스택을 이용하여 중위 표기법(infix notation) 수식을 후위 표기법(postfix notation)으로 변환하는 방법.
- 중위 표기법: 연산자가 피연산자 사이에 위치 (예: A + B).
- 후위 표기법: 연산자가 피연산자 뒤에 위치 (예: A B +).
- 변환 알고리즘:
  1. 왼쪽에서 오른쪽으로 수식을 읽음.
  2. 피연산자(숫자 또는 변수)는 바로 출력.
  3. 연산자를 만나면 스택에 push. 단, 스택의 top에 있는 연산자의 우선순위가 현재 연산자보다 높거나 같으면 pop하여 출력 후 push.
  4. 여는 괄호 '('는 스택에 push.
  5. 닫는 괄호 ')'를 만나면 스택에서 '('를 만날 때까지 pop하여 출력.
  6. 수식 끝나면 스택에 남은 연산자 모두 pop하여 출력.
- 예시: (A + B) * C
  - A: 출력 → A
  - (: push (
  - A: 출력 → A A
  - +: push + (스택: (, +)
  - B: 출력 → A A B
  - ): pop + 출력 → A A B +, pop ( 제거
  - *: push * (스택: *)
  - C: 출력 → A A B + C
  - 끝: pop * → A A B + C *

### 후위 표기법 연산
- 후위 표기법 수식을 스택을 이용하여 계산.
- 과정:
  1. 왼쪽에서 오른쪽으로 토큰을 읽음.
  2. 피연산자는 스택에 push.
  3. 연산자를 만나면 스택에서 두 개의 피연산자를 pop, 연산 수행 후 결과를 스택에 push.
  4. 수식 끝나면 스택에 남은 값이 결과.
- 예시: 5 3 + 2 *
  - 5: push 5 (스택: 5)
  - 3: push 3 (스택: 5, 3)
  - +: pop 3, 5 → 5+3=8, push 8 (스택: 8)
  - 2: push 2 (스택: 8, 2)
  - *: pop 2, 8 → 8*2=16, push 16 (스택: 16)
  - 결과: 16

- Python 코드 예시 (스택을 이용한 계산기, 중위 표기법 → 후위 표기법 변환 및 계산):
  ```python
  # 연산자 우선순위를 반환하는 함수
  # +와 -는 우선순위 1, *와 /는 우선순위 2, 그 외는 0
  def precedence(op):
      if op == '+' or op == '-':
          return 1
      if op == '*' or op == '/':
          return 2
      return 0

  # 중위 표기법 수식을 후위 표기법으로 변환하는 함수
  # 스택을 사용하여 연산자와 괄호를 처리
  def infix_to_postfix(expression):
      stack = []  # 연산자를 저장할 스택
      output = []  # 후위 표기법 결과를 저장할 리스트
      tokens = expression.replace(' ', '')  # 공백 제거
      i = 0
      while i < len(tokens):
          if tokens[i].isdigit():  # 숫자인 경우
              num = ''
              while i < len(tokens) and tokens[i].isdigit():
                  num += tokens[i]
                  i += 1
              output.append(num)  # 숫자를 출력 리스트에 추가
              continue
          elif tokens[i] == '(':  # 여는 괄호인 경우
              stack.append(tokens[i])  # 스택에 push
          elif tokens[i] == ')':  # 닫는 괄호인 경우
              while stack and stack[-1] != '(':  # '('를 만날 때까지 pop하여 출력
                  output.append(stack.pop())
              stack.pop()  # '(' 제거
          else:  # 연산자인 경우
              while stack and precedence(stack[-1]) >= precedence(tokens[i]):  # 우선순위 비교하여 pop
                  output.append(stack.pop())
              stack.append(tokens[i])  # 현재 연산자를 스택에 push
          i += 1
      while stack:  # 남은 연산자 모두 출력
          output.append(stack.pop())
      return ' '.join(output)  # 공백으로 구분된 문자열 반환

  # 후위 표기법 수식을 계산하는 함수
  # 스택을 사용하여 피연산자와 연산을 처리
  def evaluate_postfix(expression):
      stack = []  # 계산을 위한 스택
      tokens = expression.split()  # 공백으로 토큰 분리
      for token in tokens:
          if token.isdigit():  # 숫자인 경우
              stack.append(int(token))  # 스택에 push
          else:  # 연산자인 경우
              b = stack.pop()  # 두 번째 피연산자
              a = stack.pop()  # 첫 번째 피연산자
              if token == '+':
                  stack.append(a + b)
              elif token == '-':
                  stack.append(a - b)
              elif token == '*':
                  stack.append(a * b)
              elif token == '/':
                  stack.append(a // b)  # 정수 나눗셈
      return stack[0]  # 최종 결과 반환

  # 전체 계산 함수: 중위 표기법을 후위로 변환 후 계산
  def calculate(expression):
      postfix = infix_to_postfix(expression)  # 변환
      return evaluate_postfix(postfix)  # 계산

  # 예시 실행
  print(calculate("(6+5*(2-8)/2)"))  # 계산 결과 출력
  ```

## stack 응용

### backtracking
- 백트래킹: 가능한 모든 경우를 시도하다가 조건을 만족하지 않으면 이전 단계로 돌아가 다른 경로를 탐색.
- 스택 사용: 재귀 대신 스택으로 구현 가능. 예: 미로 찾기에서 경로 탐색 시, 방문한 경로를 스택에 저장하고 막히면 pop하여 되돌아감.
- 예시: N-Queen 문제에서 퀸 배치 시도, 충돌하면 이전 위치로 백트래킹.
- Python 코드 예시 (스택을 이용한 N-Queen 백트래킹):
  ```python
  def solve_n_queens(n):
      stack = []  # 백트래킹 경로를 저장할 스택
      solutions = []  # 해답을 저장할 리스트
      # 초기 상태: 첫 번째 행에 퀸 배치 시도
      for col in range(n):
          stack.append([(0, col)])  # (row, col) 형태로 경로 저장
      
      while stack:
          path = stack.pop()  # 현재 경로 꺼내기
          row = len(path)  # 현재 행 번호
          if row == n:  # 모든 행에 퀸 배치 완료
              solutions.append(path)  # 해답에 추가
              continue
          
          for col in range(n):  # 현재 행의 모든 열 시도
              # 현재 위치에 퀸을 놓을 수 있는지 확인 (열과 대각선 충돌 검사)
              if all(path[i][1] != col and abs(path[i][1] - col) != row - i for i in range(row)):
                  # 유망하면 다음 행으로 진행
                  new_path = path + [(row, col)]  # 새로운 경로 생성
                  stack.append(new_path)  # 스택에 push
      
      return solutions  # 모든 해답 반환
  
  # 4x4 보드의 해 출력
  solutions = solve_n_queens(4)
  for sol in solutions:
      print(sol)
  ```
  - 설명: 스택에 현재까지의 퀸 배치 경로를 저장. 각 단계에서 다음 행의 모든 열을 시도하며, 충돌 검사 후 유망한 경우 push. 충돌 시 백트래킹.

### 부분집합
- 부분집합 생성: 비트 연산이나 재귀로 가능하지만, 스택으로도 구현.
- 스택 사용: 각 요소를 포함/미포함으로 나누어 스택에 상태 저장.
- 예시: {1,2,3}의 부분집합.
  - 시작: 빈 집합 push.
  - 1 포함: {1} push, 1 미포함: {} 유지.
  - 2 포함: {1,2}, {2} 등 반복.
- Python 코드 예시 (스택을 이용한 부분집합 생성):
  ```python
  def generate_subsets(nums):
      stack = [([], 0)]  # (현재 부분집합 리스트, 다음 처리할 요소 인덱스)
      subsets = []  # 모든 부분집합을 저장할 리스트
      
      while stack:  # 스택이 비어있지 않은 동안 반복
          current, index = stack.pop()  # 현재 부분집합과 인덱스 꺼내기
          if index == len(nums):  # 모든 요소를 처리했으면
              subsets.append(current[:])  # 부분집합 추가 (복사본)
              continue
          
          # 현재 요소를 포함하지 않는 경우
          stack.append((current, index + 1))
          # 현재 요소를 포함하는 경우
          stack.append((current + [nums[index]], index + 1))
      
      return subsets  # 모든 부분집합 반환
  
  # {1,2,3}의 부분집합 생성 예시
  nums = [1, 2, 3]
  subsets = generate_subsets(nums)
  for subset in subsets:
      print(subset)
  ```
  - 설명: 스택에 (현재 부분집합 리스트, 다음 요소 인덱스)를 저장. 각 요소에 대해 포함/미포함 두 가지 경우를 push. 인덱스가 끝이면 부분집합으로 추가.

### 순열1
- 순열 생성: 재귀로 구현, 스택으로도 가능.
- 스택 사용: 현재 순열 상태와 사용된 요소를 스택에 저장.
- 예시: [1,2,3]의 순열.
  - 시작: [] push.
  - 1 추가: [1], 사용: {1}.
  - 2 추가: [1,2], 사용: {1,2}.
  - 3 추가: [1,2,3] 출력, 백트래킹.
- Python 코드 예시 (스택을 이용한 순열 생성):
  ```python
  def generate_permutations(nums):
      stack = [([], set())]  # (현재 순열, 사용된 요소 집합)
      permutations = []
      
      while stack:
          current, used = stack.pop()
          if len(current) == len(nums):
              permutations.append(current[:])
              continue
          
          for num in nums:
              if num not in used:
                  new_current = current + [num]
                  new_used = used | {num}
                  stack.append((new_current, new_used))
      
      return permutations
  
  # [1,2,3]의 순열
  nums = [1, 2, 3]
  perms = generate_permutations(nums)
  for perm in perms:
      print(perm)
  ```
  - 설명: 스택에 (현재 순열 리스트, 사용된 요소 집합)를 저장. 각 단계에서 사용되지 않은 요소를 추가하여 새로운 상태를 push. 길이가 n이면 순열로 추가.

### 가지치기
- 가지치기: 백트래킹에서 불필요한 경로를 미리 제거.
- 스택 사용: 조건 검사 후 유망하지 않으면 pop하지 않고 건너뜀.
- 예시: 합이 10을 넘지 않는 부분집합 찾기에서, 현재 합이 이미 10 초과면 가지치기.

### 순열2
- 순열1과 유사하지만, 중복 허용 또는 특정 조건.
- 스택 사용: 중복 체크를 위해 방문 배열과 함께 스택에 저장.
- 예시: 중복 없는 순열 생성 시, 방문 배열로 가지치기.
- Python 코드 예시 (스택을 이용한 중복 없는 순열, 방문 배열 사용):
  ```python
  def generate_permutations_no_duplicates(nums):
      nums.sort()  # 중복 요소 처리 위해 리스트 정렬
      stack = [([], [False] * len(nums))]  # (현재 순열 리스트, 방문 배열)
      permutations = []  # 모든 순열을 저장할 리스트
      
      while stack:  # 스택이 비어있지 않은 동안 반복
          current, visited = stack.pop()  # 현재 순열과 방문 배열 꺼내기
          if len(current) == len(nums):  # 순열이 완성되었으면
              permutations.append(current[:])  # 순열 추가 (복사본)
              continue
          
          for i in range(len(nums)):  # 모든 인덱스에 대해 시도
              if visited[i]:  # 이미 방문했으면 스킵
                  continue
              # 중복 방지: 이전 요소와 같고 이전이 방문되지 않았으면 스킵 (가지치기)
              if i > 0 and nums[i] == nums[i-1] and not visited[i-1]:
                  continue
              new_current = current + [nums[i]]  # 새로운 순열 생성
              new_visited = visited[:]  # 방문 배열 복사
              new_visited[i] = True  # 현재 인덱스 방문 표시
              stack.append((new_current, new_visited))  # 스택에 push
      
      return permutations  # 모든 순열 반환
  
  # [1,1,2]의 중복 없는 순열 생성 예시
  nums = [1, 1, 2]
  perms = generate_permutations_no_duplicates(nums)
  for perm in perms:
      print(perm)
  ```
  - 설명: 스택에 (현재 순열, 방문 배열)를 저장. 중복 요소가 있을 때, 이전 요소가 사용되지 않았으면 스킵하여 가지치기. 길이가 n이면 순열로 추가.

### 분할정복
- 분할정복: 문제를 작은 부분으로 나누어 해결.
- 스택 사용: 재귀 대신 스택으로 구현, 예: 퀵소트에서 피벗 기준 분할.
- 예시: 병합 정렬에서 스택으로 재귀 피하기, 각 부분을 스택에 push하여 처리.
- Python 코드 예시 (스택을 이용한 퀵소트, 분할정복):
  ```python
  def quicksort_stack(arr):
      stack = [(0, len(arr) - 1)]  # (시작 인덱스, 끝 인덱스) 스택 초기화
      
      while stack:  # 스택이 비어있지 않은 동안 반복
          start, end = stack.pop()  # 현재 정렬 범위 꺼내기
          if start >= end:  # 범위가 유효하지 않으면 스킵
              continue
          
          # 피벗 선택 (중간 값)
          pivot_index = (start + end) // 2
          pivot = arr[pivot_index]
          
          # 피벗을 끝으로 이동
          arr[pivot_index], arr[end] = arr[end], arr[pivot_index]
          pivot_index = end
          
          # 분할: 피벗보다 작은 요소를 왼쪽으로 이동
          i = start - 1
          for j in range(start, end):
              if arr[j] < pivot:
                  i += 1
                  arr[i], arr[j] = arr[j], arr[i]
          
          # 피벗 위치 조정
          arr[i + 1], arr[pivot_index] = arr[pivot_index], arr[i + 1]
          pivot_index = i + 1
          
          # 왼쪽과 오른쪽 부분을 스택에 push (재귀 대신)
          stack.append((start, pivot_index - 1))  # 왼쪽 부분
          stack.append((pivot_index + 1, end))  # 오른쪽 부분
      
      return arr  # 정렬된 배열 반환
  
  # 퀵소트 예시 실행
  arr = [3, 6, 8, 10, 1, 2, 1]
  sorted_arr = quicksort_stack(arr)
  print(sorted_arr)
  ```
  - 설명: 스택에 (시작, 끝) 인덱스를 저장. 각 단계에서 피벗을 기준으로 분할하고, 왼쪽/오른쪽 부분을 스택에 push하여 재귀 없이 처리.
