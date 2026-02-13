# 큐

## 정의
**큐(Queue)**는 선입선출(FIFO: First In First Out) 원칙을 따르는 자료구조이다. 가장 먼저 들어온 데이터가 가장 먼저 나가는 구조로, 일상에서 대기열을 생각하면 된다.

## 개념
큐는 데이터가 한쪽 끝에서만 삽입되고, 다른 한쪽 끝에서만 삭제되는 구조이다. 이는 스택(stack)이 후입선출(LIFO)인 것과 대조된다.
- **입구(rear)**: 데이터가 삽입되는 곳
- **출구(front)**: 데이터가 삭제되는 곳

## 구조
```
┌─────┬─────┬─────┬─────┬─────┬─────┐
│     │     │  1  │  2  │  3  │     │
└─────┴─────┴─────┴─────┴─────┴─────┘
  front                         rear
```
- **front**: 첫 번째 요소(삭제될 위치)를 가리키는 포인터
- **rear**: 마지막 요소(삽입될 위치)를 가리키는 포인터

## 기본 연산
| 연산 | 설명 |
|------|------|
| `enQueue(item)` | 큐의 rear에 데이터 삽입 |
| `deQueue()` | 큐의 front에서 데이터 삭제 및 반환 |
| `peek()` | front의 데이터 확인 (삭제하지 않음) |
| `isEmpty()` | 큐가 비어있는지 확인 |
| `isFull()` | 큐가 가득 찼는지 확인 |

## 연산 과정

### 초기 상태 (공백 큐)
```
front = -1, rear = -1
┌─────┬─────┬─────┬─────┬─────┐
│     │     │     │     │     │
└─────┴─────┴─────┴─────┴─────┘
front=-1, rear=-1
```

### 데이터 1 삽입 (enQueue)
```
rear = rear + 1 → rear = 0
queue[rear] = 1
┌─────┬─────┬─────┬─────┬─────┐
│  1  │     │     │     │     │
└─────┴─────┴─────┴─────┴─────┘
     front=0, rear=0
```

### 데이터 2 삽입 (enQueue)
```
rear = rear + 1 → rear = 1
queue[rear] = 2
┌─────┬─────┬─────┬─────┬─────┐
│  1  │  2  │     │     │     │
└─────┴─────┴─────┴─────┴─────┘
     front=0, rear=1
```

### 데이터 3 삽입 (enQueue)
```
rear = rear + 1 → rear = 2
queue[rear] = 3
┌─────┬─────┬─────┬─────┬─────┐
│  1  │  2  │  3  │     │     │
└─────┴─────┴─────┴─────┴─────┘
     front=0, rear=2
```

### 데이터 삭제 (deQueue) - 1번 데이터
```
item = queue[front] → item = 1
front = front + 1 → front = 1
┌─────┬─────┬─────┬─────┬─────┐
│     │  2  │  3  │     │     │
└─────┴─────┴─────┴─────┴─────┘
          front=1, rear=2
```

---

# 선형큐

## 개념
**선형큐(Linear Queue)**는 데이터를 일렬로 저장하는 가장 기본적인 큐 구조이다. 배열을 사용하여 구현하며, front와 rear가 각각 삭제와 삽입 위치를 나타낸다.

## 구현
```python
class LinearQueue:
    def __init__(self, size):
        self.queue = [None] * size
        self.front = -1  # 삭제 위치
        self.rear = -1   # 삽입 위치
        self.size = size
    
    def isEmpty(self):
        return self.front == self.rear
    
    def isFull(self):
        return self.rear == self.size - 1
    
    def enQueue(self, item):
        if self.isFull():
            print("Queue is Full!")
            return
        self.rear += 1
        self.queue[self.rear] = item
    
    def deQueue(self):
        if self.isEmpty():
            print("Queue is Empty!")
            return None
        self.front += 1
        return self.queue[self.front]
    
    def peek(self):
        if self.isEmpty():
            return None
        return self.queue[self.front + 1]
```

## 연습문제

### 문제: 데이터 1, 2, 3을 차례로 큐에 삽입하고, 세 개의 데이터를 차례로 꺼내서 출력

```python
queue = LinearQueue(10)
queue.enQueue(1)
queue.enQueue(2)
queue.enQueue(3)

print(queue.deQueue())  # 출력: 1
print(queue.deQueue())  # 출력: 2
print(queue.deQueue())  # 출력: 3
```

### 연산 과정 시각화
| 단계 | 연산 | front | rear | 큐 상태 |
|------|------|-------|------|---------|
| 초기 | - | -1 | -1 | [ , , , ] |
| 삽입1 | enQueue(1) | -1 | 0 | [1, , , ] |
| 삽입2 | enQueue(2) | -1 | 1 | [1,2, , ] |
| 삽입3 | enQueue(3) | -1 | 2 | [1,2,3, ] |
| 삭제1 | deQueue() | 0 | 2 | [ ,2,3, ] → 1 출력 |
| 삭제2 | deQueue() | 1 | 2 | [ , ,3, ] → 2 출력 |
| 삭제3 | deQueue() | 2 | 2 | [ , , , ] → 3 출력 |

---

# 원형큐

## 개념
**원형큐(Circular Queue)**는 선형큐의 한계점을 극복하기 위해 도입된 자료구조이다.

### 선형큐의 한계점
1. **메모리 낭비**: 삭제 연산으로 인해 앞부분이 비어있어도, rear가 끝에 도달하면 더 이상 삽입할 수 없음
2. **배열 이동 문제**: 이를 해결하려면 모든 데이터를 앞으로 이동시켜야 하며, O(n)의 시간 복잡도 발생

### 원형큐의 해결 방법
원형큐는 배열의 마지막 위치에서 첫 번째 위치로 순환하는 구조이다. 이를 통해:
- 삭제된 공간을 재사용할 수 있음
- 데이터를 이동할 필요 없음
- 메모리를 효율적으로 사용

## 구조

### 초기 공백 상태
```
front = 0, rear = 0
┌─────┬─────┬─────┬─────┬─────┐
│     │     │     │     │     │
└─────┴─────┴─────┴─────┴─────┘
  front=0, rear=0
```

### 인덱스의 순환
```
(rear + 1) % size → 다음 삽입 위치
(front + 1) % size → 다음 삭제 위치
```
예: size = 5인 경우
- rear = 4에서 삽입 → rear = (4+1) % 5 = 0
- front = 4에서 삭제 → front = (4+1) % 5 = 0

### front 변수
원형큐에서 front는 항상 **삭제할 요소의 바로 전 위치**를 가리킨다.
- `queue[front]` : 삭제된 자리 (사용 안함)
- `queue[(front + 1) % size]` : 삭제될 데이터

## 선형큐와 원형큐의 비교
| 구분 | 선형큐 | 원형큐 |
|------|--------|--------|
| **삽입 위치** | rear가 항상 증가 | rear가 순환하며 증가 |
| **삭제 위치** | front가 항상 증가 | front가 순환하며 증가 |
| **메모리 효율** | 낮음 (앞부분 공간 재사용 불가) | 높음 (공간 순환 재사용) |
| **구현 복잡도** | 간단함 | 약간 복잡함 (연산 필요) |
| **공백 조건** | front == rear | front == rear |
| **포화 조건** | rear == size - 1 | (rear + 1) % size == front |

## 연산 과정

### 초기 상태 (공백)
```
size = 5
front = 0, rear = 0
┌─────┬─────┬─────┬─────┬─────┐
│     │     │     │     │     │
└─────┴─────┴─────┴─────┴─────┘
  f=0, r=0
```

### 데이터 1, 2, 3 삽입

**enQueue(1)**: rear = (rear + 1) % 5 = 1
```
┌─────┬─────┬─────┬─────┬─────┐
│     │  1  │     │     │     │
└─────┴─────┴─────┴─────┴─────┘
  f=0  r=1
```

**enQueue(2)**: rear = (rear + 1) % 5 = 2
```
┌─────┬─────┬─────┬─────┬─────┐
│     │  1  │  2  │     │     │
└─────┴─────┴─────┴─────┴─────┘
  f=0  r=2
```

**enQueue(3)**: rear = (rear + 1) % 5 = 3
```
┌─────┬─────┬─────┬─────┬─────┐
│     │  1  │  2  │  3  │     │
└─────┴─────┴─────┴─────┴─────┘
  f=0  r=3
```

### 데이터 삭제 (1, 2, 3 순서)

**deQueue()** - 1 삭제: front = (front + 1) % 5 = 1
```
┌─────┬─────┬─────┬─────┬─────┐
│     │     │  2  │  3  │     │
└─────┴─────┴─────┴─────┴─────┘
       f=1  r=3
```

**deQueue()** - 2 삭제: front = (front + 1) % 5 = 2
```
┌─────┬─────┬─────┬─────┬─────┐
│     │     │     │  3  │     │
└─────┴─────┴─────┴─────┴─────┘
       f=2  r=3
```

**deQueue()** - 3 삭제: front = (front + 1) % 5 = 3
```
┌─────┬─────┬─────┬─────┬─────┐
│     │     │     │     │     │
└─────┴─────┴─────┴─────┴─────┘
       f=3  r=3 (공백 상태)
```

### 공간 재사용
데이터 4, 5, 6을 더 삽입하면:
```
┌─────┬─────┬─────┬─────┬─────┐
│  6  │     │     │  3  │  4  │
└─────┴─────┴─────┴─────┴─────┘
  f=3  r=4
```

계속 삽입하면 포화 상태:
```
┌─────┬─────┬─────┬─────┬─────┐
│  6  │  5  │  4  │  3  │  4  │
└─────┴─────┴─────┴─────┴─────┘
  f=4  r=0 (포화 상태)
```

## 구현
```python
class CircularQueue:
    def __init__(self, size):
        self.queue = [None] * size
        self.front = 0  # 삭제 위치 (실제 데이터 이전)
        self.rear = 0   # 삽입 위치
        self.size = size
    
    def isEmpty(self):
        return self.front == self.rear
    
    def isFull(self):
        return (self.rear + 1) % self.size == self.front
    
    def enQueue(self, item):
        if self.isFull():
            print("Queue is Full!")
            return
        self.rear = (self.rear + 1) % self.size
        self.queue[self.rear] = item
    
    def deQueue(self):
        if self.isEmpty():
            print("Queue is Empty!")
            return None
        self.front = (self.front + 1) % self.size
        return self.queue[self.front]
    
    def peek(self):
        if self.isEmpty():
            return None
        return self.queue[(self.front + 1) % self.size]
    
    def display(self):
        if self.isEmpty():
            print("Queue is Empty")
            return
        print("Queue contents:", end=" ")
        i = self.front
        while True:
            i = (i + 1) % self.size
            print(self.queue[i], end=" ")
            if i == self.rear:
                break
        print()


# 사용 예시
cq = CircularQueue(5)
cq.enQueue(1)
cq.enQueue(2)
cq.enQueue(3)
cq.display()  # Queue contents: 1 2 3

print(cq.deQueue())  # 1
print(cq.deQueue())  # 2
print(cq.deQueue())  # 3

# 추가 삽입 (원형큐의 공간 재사용 확인)
cq.enQueue(4)
cq.enQueue(5)
cq.enQueue(6)
cq.display()  # Queue contents: 4 5 6
```

### 포화 상태 확인 공식
```
(rear + 1) % size == front
```

---

# 연결 큐

## 개념
**연결 큐(Linked Queue)**는 연결 리스트를 사용하여 구현한 큐이다. 배열 기반 큐와 달리 크기가 고정되지 않고 동적으로 데이터를 저장할 수 있다.
- 메모리를 동적으로 할당받아 사용
- 큐의 크기가 제한되지 않음
- 데이터를 이동할 필요 없음

## 구조

### 단순 연결 리스트를 이용한 큐
```
┌───┐    ┌───┐    ┌───┐    ┌───┐
│ 1 │───>│ 2 │───>│ 3 │───>│   │
└───┘    └───┘    └───┘    └───┘
front                     rear
```
- **front**: 첫 번째 노드(삭제될 위치)를 가리키는 포인터
- **rear**: 마지막 노드(삽입될 위치)를 가리키는 포인터

### 상태 표현

**공백 상태**: front = None, rear = None

**데이터 존재 상태**:
```
front → [data|next] → [data|next] → [data|next] → None
                                              rear
```

## 연산 과정

### 초기 상태
```
front = None, rear = None
```

### enQueue(1) - 첫 번째 데이터 삽입
```
새 노드 생성: [1|None]
rear = 새 노드
front = 새 노드 (첫 번째 삽입이므로)

front ─┐
       ▼
     [1|None]
rear ──┘
```

### enQueue(2) - 두 번째 데이터 삽입
```
기존 rear의 next = 새 노드
rear = 새 노드

front ──────► [1|next] ────► [2|None]
                        rear
```

### enQueue(3) - 세 번째 데이터 삽입
```
rear.next = 새 노드
rear = 새 노드

front ──────► [1|next] ────► [2|next] ────► [3|None]
                                    rear
```

### deQueue() - 데이터 삭제
```
data = front.data
front = front.next
만약 front가 None이 되면 rear도 None으로 설정

front ──────► [1|next] ────► [2|next] ────► [3|None]
↑삭제                         rear
```

## 구현
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedQueue:
    def __init__(self):
        self.front = None  # 첫 번째 노드 (삭제 위치)
        self.rear = None   # 마지막 노드 (삽입 위치)
    
    def isEmpty(self):
        return self.front is None
    
    def enQueue(self, item):
        new_node = Node(item)
        if self.isEmpty():
            self.front = new_node
            self.rear = new_node
        else:
            self.rear.next = new_node
            self.rear = new_node
    
    def deQueue(self):
        if self.isEmpty():
            print("Queue is Empty!")
            return None
        item = self.front.data
        self.front = self.front.next
        if self.front is None:
            self.rear = None
        return item
    
    def peek(self):
        if self.isEmpty():
            return None
        return self.front.data
    
    def display(self):
        if self.isEmpty():
            print("Queue is Empty")
            return
        current = self.front
        while current:
            print(current.data, end=" ")
            current = current.next
        print()


# 사용 예시
lq = LinkedQueue()
lq.enQueue(1)
lq.enQueue(2)
lq.enQueue(3)
lq.display()  # 출력: 1 2 3

print(lq.deQueue())  # 출력: 1
print(lq.deQueue())  # 출력: 2
print(lq.deQueue())  # 출력: 3
```

---

# 덱

## 개념
**덱(Deque, Double-Ended Queue)**은 양쪽 끝에서 삽입과 삭제가 모두 가능한 자료구조이다.
- 앞쪽과 뒤쪽 모두에서 데이터 삽입/삭제 가능
- 스택과 큐의 기능을 모두 포함

## 연산

### 기본 연산
| 연산 | 설명 |
|------|------|
| `append(item)` | 뒤쪽에 데이터 삽입 |
| `appendleft(item)` | 앞쪽에 데이터 삽입 |
| `pop()` | 뒤쪽에서 데이터 삭제 및 반환 |
| `popleft()` | 앞쪽에서 데이터 삭제 및 반환 |
| `peek()` | 뒤쪽 데이터 확인 |
| `peekleft()` | 앞쪽 데이터 확인 |

### 사용 예시
```python
from collections import deque

dq = deque()

# 뒤쪽 삽입
dq.append(1)
dq.append(2)
dq.append(3)
print(list(dq))  # [1, 2, 3]

# 앞쪽 삽입
dq.appendleft(0)
print(list(dq))  # [0, 1, 2, 3]

# 뒤쪽 삭제
print(dq.pop())  # 3

# 앞쪽 삭제
print(dq.popleft())  # 0
```

### 직접 구현
```python
class Deque:
    def __init__(self):
        self.items = []
    
    def isEmpty(self):
        return len(self.items) == 0
    
    def append(self, item):
        self.items.append(item)
    
    def appendleft(self, item):
        self.items.insert(0, item)
    
    def pop(self):
        if self.isEmpty():
            return None
        return self.items.pop()
    
    def popleft(self):
        if self.isEmpty():
            return None
        return self.items.pop(0)
```

---

# 우선순위 큐

## 개념
**우선순위 큐(Priority Queue)**는 데이터에 우선순위를 부여하고, 우선순위가 높은 데이터가 먼저 나가는 자료구조이다.
- 일반 큐와 달리 삽입 순서가 아닌 우선순위에 따라 삭제
- 힙(Heap)을 사용하여 구현하는 것이 일반적
- 스케줄링, 작업 스케줄링 등에 사용

## 연산

### 기본 연산
| 연산 | 설명 |
|------|------|
| `enQueue(item, priority)` | 데이터와 우선순위를 함께 삽입 |
| `deQueue()` | 가장 높은 우선순위의 데이터 삭제 및 반환 |
| `peek()` | 가장 높은 우선순위의 데이터 확인 |

### 삽입 연산 과정
예: 우선순위가 숫자가 작을수록 높다고 가정
```
enQueue(A, 3)  → [A(3)]
enQueue(B, 1)  → [B(1), A(3)]  # B가 우선순위 높음
enQueue(C, 2)  → [B(1), C(2), A(3)]  # C는 B와 A 사이
```

### 삭제 연산 과정
```
deQueue() → B 반환
[C(2), A(3)] → [A(3)]  # C가 가장 앞으로 이동
```

## 배열을 이용한 우선순위 큐 구현
```python
class PriorityQueue:
    def __init__(self):
        self.queue = []  # (데이터, 우선순위) 튜플 저장
    
    def isEmpty(self):
        return len(self.queue) == 0
    
    def enQueue(self, item, priority):
        self.queue.append((item, priority))
        self.queue.sort(key=lambda x: x[1])  # 우선순위 순으로 정렬
    
    def deQueue(self):
        if self.isEmpty():
            print("Queue is Empty!")
            return None
        item, priority = self.queue.pop(0)
        return item
    
    def peek(self):
        if self.isEmpty():
            return None
        return self.queue[0][0]
    
    def display(self):
        for item, priority in self.queue:
            print(f"{item}(우선순위: {priority})", end=" ")
        print()


# 사용 예시
pq = PriorityQueue()
pq.enQueue("Task A", 3)
pq.enQueue("Task B", 1)  # 가장 높음
pq.enQueue("Task C", 2)

print("현재 큐:")
pq.display()
# 출력: Task B(우선순위: 1) Task C(우선순위: 2) Task A(우선순위: 3)

print(f"삭제: {pq.deQueue()}")  # Task B
print(f"삭제: {pq.deQueue()}")  # Task C
print(f"삭제: {pq.deQueue()}")  # Task A
```

### 시간 복잡도
| 연산 | 시간 복잡도 |
|------|-------------|
| 삽입(enQueue) | O(n) - 정렬 필요 |
| 삭제(deQueue) | O(1) - 맨 앞 데이터 |
| 힙 사용 시 | O(log n) |

---

# 큐의 활용

## 1. 버퍼(Buffer)

### 버퍼의 개념
**버퍼(Buffer)**는 데이터를 한 곳에서 다른 곳으로 전송할 때, 속도차를 조정하기 위해 임시로 데이터를 저장하는 공간이다. 큐는 버퍼를 구현하는 가장 적합한 자료구조이다.

### 버퍼의 자료 구조
버퍼는 일반적으로 FIFO(선입선출) 구조를 사용하여 구현된다.
- 데이터가 빠른 속도로 들어올 때 버퍼에 임시 저장
- 처리가 느린 경우 버퍼에서 데이터를 가져와 처리

### 버퍼의 예시 - 키보드 버퍼
키보드를 빠르게 입력할 때, 문자들이 키보드 버퍼에 저장되고, 컴퓨터가 이를 순서대로 처리한다.

```
입력: A B C D (빠르게 입력)
버퍼: [A] → [B] → [C] → [D]
처리: A → B → C → D (순서대로)
```

### 마이쮸 나눠주기 시뮬레이션
```python
from collections import deque

def distribute_candy(N, M, kids):
    queue = deque(range(1, M + 1))
    results = [0] * M
    
    for i in range(M):
        kid = queue.popleft()
        candy = kids[i]
        results[kid - 1] = candy
        print(f"아이 {kid}에게 마이쮸 {candy}개 나눠줌")
        queue.append(kid)
    
    return results

# 사용 예시
N = 20
M = 3
kids = [5, 3, 4]

result = distribute_candy(N, M, kids)
print(f"결과: {result}")
```

---

## 2. BFS (Breadth-First Search)

### BFS의 개념
**BFS(너비 우선 탐색)**는 그래프에서 가장 가까운 노드부터 순서대로 방문하는 탐색 알고리즘이다. 큐를 사용하여 구현하며, 최단 경로를 찾을 때 사용된다.

### BFS 탐색 순서
예시 그래프:
```
      1
     / \
    2   3
   / \   \
  4   5   6
```

BFS 탐색 순서 (시작점: 1):
```
1 → 2 → 3 → 4 → 5 → 6
```

### BFS 알고리즘
```
BFS(G, v):
    큐 Q에 v를 삽입
    visited[v] = true
    
    while Q가 비어있지 않으면:
        x = Q에서 삭제
        출력 (방문 처리)
        
        for each 인접 노드 y of x:
            if visited[y] == false:
                Q에 y를 삽입
                visited[y] = true
```

### BFS 예제
```python
from collections import deque

def bfs(graph, start):
    visited = set()
    queue = deque([start])
    visited.add(start)
    result = []
    
    while queue:
        node = queue.popleft()
        result.append(node)
        
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
    
    return result

# 그래프 정의
graph = {
    1: [2, 3],
    2: [1, 4, 5],
    3: [1, 6],
    4: [2],
    5: [2],
    6: [3]
}

print(bfs(graph, 1))  # 출력: [1, 2, 3, 4, 5, 6]
```

### 미로탐색 예제
```python
from collections import deque

def bfs_maze(maze, start, end):
    n = len(maze)
    m = len(maze[0])
    visited = [[False] * m for _ in range(n)]
    queue = deque([(start[0], start[1], 0)])
    visited[start[0]][start[1]] = True
    
    dx = [-1, 1, 0, 0]
    dy = [0, 0, -1, 1]
    
    while queue:
        x, y, dist = queue.popleft()
        
        if (x, y) == end:
            return dist
        
        for i in range(4):
            nx = x + dx[i]
            ny = y + dy[i]
            
            if 0 <= nx < n and 0 <= ny < m:
                if maze[nx][ny] == 0 and not visited[nx][ny]:
                    visited[nx][ny] = True
                    queue.append((nx, ny, dist + 1))
    
    return -1

# 미로 예제 (0: 통로, 1: 벽)
maze = [
    [0, 1, 1, 1, 1],
    [0, 0, 0, 1, 1],
    [1, 1, 0, 0, 0],
    [1, 1, 1, 1, 0],
    [0, 0, 0, 0, 0]
]

result = bfs_maze(maze, (0, 0), (4, 4))
print(f"최단 거리: {result}")  # 출력: 8
```

---

# 요약

| 구분 | 선형큐 | 원형큐 | 연결큐 | 덱 | 우선순위큐 |
|------|--------|--------|--------|-----|------------|
| 구조 | 일렬 | 원형 | 연결 리스트 | 양방향 | 우선순위 기반 |
| 공간 재사용 | 불가능 | 가능 | 가능 | 가능 | 가능 |
| 구현 | 간단 | 순환 연산 필요 | 포인터 필요 | 양쪽 연산 | 정렬 필요 |
| 사용 상황 | 간단한 경우 | 메모리 효율 | 동적 크기 | 양쪽 삽입/삭제 | 우선순위 필요 |

큐는 BFS(너비 우선 탐색), 작업 스케줄링, 프린터 대기열 등 다양한 분야에서 활용되는 중요한 자료구조이다.
