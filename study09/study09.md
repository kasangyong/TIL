# SW 문제해결 기본

## 2차원 List

### 2차원 배열
- 2차원 배열의 선언
    - 1차원 list를 묶어놓은 list
    - 2차원 이상의 다차원 list는 차원에 따라 index를 선언
    - 2차원 list의 선언: 세로 길이(행의 개수), 가로 길이(열의 개수)를 필요로 함
    - python에서는 데이터 초기화를 통해 변수 선언과 초기화가 가능함

#### 입력을 2차원 배열에 저장하기
```python
# 입력 예시
# 3
# 1 2 3
# 4 5 6
# 7 8 9

N = int(input())
arr = [list(map(int, input().split())) for _ in range(N)]

값을 붙여서 주면
arr = [list(map(int, input())) for _ in range(N)]

0으로 이루어진 3 X 4 배열 만들기
arr = [[0] * 4 for _ in range(3)]
```

#### 배열 순회
- n X m 배열의 n*m 개의 모든 원소를 빠짐없이 조사하는 방법(n: 행, m: 열)

```python
행 우선 순회 예시
# i 행의 좌표
# j 열의 좌표

n, m = map(int, input().split())
arr = [list(map(int, input().split())) for _ in range(n) ]

s = 0
for i in range(n):
    for j in range(m):
        s += arr[i][j] # 필요한 연산 수행

열 우선 순회 예시
# 열의 합 중 최대

max_v = 0
for j in range(m):
    s = 0
    for i in range(n):
        s += arr[i][j]
    if s>max_v:
        max_v = s

지그재그 순회

for i in range(n):
    for j in range(m):
        f(arr[i][j + (m-1-2*j)*(i%2)])
```

### 델타
- 2차 배열의 한 좌표에서 4방향의 인접 배열 요소를 탐색하는 방법
- 인덱스 (i, j)인 칸의 상하좌우 칸 (ni, nj)

```python
# N X N 배열에서 각 원소를 중심으로, 상하좌우 k칸의 합계 중 최댓값 (k = 2)

max_v = 0
for i in range(N):
    for j in range(N):
        s = arr[i][j]
        for di, dj in [[0, 1], [1, 0], [0, -1], [-1, 0]]: # 각 방향
            for c in range(1, k+1): # 거리별
                ni, nj = i+di*c, j+dj*c
                if 0 <= ni < N and 0 <= nj < N:
                    s += arr[ni][nj]
        if max_v < s:
            max_v = s
```

#### 전치 행렬
- 대칭되는 행렬
```python
# i : 행의 좌표, 행의 크기 len(arr)
# j : 열의 좌표, 열의 크기 len(arr[0])
arr = [[1, 2, 3], [4, 5, 6], [7, 8, 9]] # 3X3 행렬

for i in range(3):
    for j in range(3):   # for j in range(i)인 경우 if문 필요 없음
        if i<j:
            arr[i][j], arr[j][i] = arr[j][i], arr[i][j]


# 대각선의 합

n = int(input())
arr = []
s = 0
for i in range(n):
    s += arr[i][i]
    s =- arr[i][n-1-i]
    if n%2 == 1:
        s -= arr[(n-1)/2][(n-1)/2]
```
![alt text](image.png)

### 부분집합

- 비트 연산자
    - &: 비트 단위로 AND 연산
    - |: 비트 단위로 or 연산
    - <<: 피연산자의 비트 열을 왼쪽으로 이동
    - >>: 피연산자의 비트 열을 오른쪽으로 이동
- <<연산자 활용
    - 1<<n: 2^n 즉, 원소가 n개일 경우의 모든 부분집합의 수를 의미
- &연산자 활용
    - i & (1<<j): i의 j번째 비트가 1인지 아닌지를 검사
```python
# 부분집합 구하기 -> 이해 잘 안되므로 나중에 이해 요망
# 비트 연산을 사용

arr = [3,6,7,1,5,4]
n = len(arr)
for i in range(1<<n): # 1<<n : 부분 집합의 개수
    for j in range(n):  # 원소의 수만큼 비트를 비교
        if i & (1<<j):  # i의 j번 비트가 1인 경우
            print(arr[j], end = ",") # j번 원소 출력
    print()
```

