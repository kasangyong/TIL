# 트리 (Tree)

트리는 비선형 자료구조로, 노드들이 부모-자식 관계를 이루는 계층 구조입니다.

---

## 1. 트리의 개념과 정의

### 1.1 트리의 정의

**트리(Tree)**는 노드(node)와 간선(edge)으로 구성된 비선형 자료구조입니다. 트리는 다음과 같은 특성을 가집니다:

- **계층 구조**: 노드들이 위아래 관계를 가짐
- **순환 없음**: 한 노드에서 다른 노드로 가는 경로가 유일함
- **그래프의 일종**: 연결된 비순환 그래프

### 1.2 트리의 용어 정리

```
            1(루트)
           / \
          2   3
         / \ / \
        4  5 6  7
       /        /
      8        9
```

| 용어 | 설명 | 예시 (위 그림) |
|------|------|----------------|
| **노드(Node)** | 트리의 기본 구성 요소 | 1, 2, 3, 4, 5, 6, 7, 8, 9 |
| **간선(Edge)** | 노드를 연결하는 선 | 1-2, 1-3, 2-4 등 |
| **루트 노드(Root Node)** | 최상위 노드, 부모가 없는 노드 | 1 |
| **형제 노드(Sibling Node)** | 같은 부모를 가진 노드 | 2와 3, 4와 5 |
| **조상 노드(Ancestor Node)** | 특정 노드에서 루트까지의 경로에 있는 모든 노드 | 4의 조상: 2, 1 |
| **서브 트리(Subtree)** | 트리의 일부를 이루는 트리 | 2를 루트로 하는 부분 |
| **자손 노드(Descendant Node)** | 특정 노드 아래에 있는 모든 노드 | 2의 자손: 4, 5, 8 |
| **노드의 차수(Degree)** | 해당 노드의 자식 수 | 1의 차수: 2, 4의 차수: 1 |
| **트리의 차수** | 트리 내 모든 노드의 차수 중 최대값 | 위 트리의 차수: 2 |
| **단말 노드(Leaf Node)** | 자식이 없는 노드 | 4, 5, 6, 7, 8, 9 |
| **노드의 높이(Height)** | 루트에서 해당 노드까지의 경로의 간선 수 | 4의 높이: 2 |
| **트리의 높이** | 루트에서 가장 깊은 단말 노드까지의 높이 | 3 |

---

## 2. 이진 트리 (Binary Tree)

### 2.1 이진 트리의 개념

**이진 트리**는 각 노드가 최대 2개의 자식 노드를 가지는 트리입니다. 왼쪽 자식과 오른쪽 자식을 구분합니다.

### 2.2 이진 트리의 특성

- 각 노드는 최대 2개의 자식을 가짐
- 특정 노드의 왼쪽 자식과 오른 자식은 서로 다름
- 이진 트리의 높이 h에서 최소 노드 수: h + 1
- 이진 트리의 높이 h에서 최대 노드 수: 2^(h+1) - 1

### 2.3 이진 트리의 종류

```
포화 이진 트리 (Perfect Binary Tree)
        ○
       / \
      ○   ○
     / \ / \
    ○  ○ ○  ○

완전 이진 트리 (Complete Binary Tree)
        ○
       / \
      ○   ○
     / \ /
    ○  ○ ○

편향 이진 트리 (Skewed Binary Tree)
    ○
   /
  ○
 /
○
```

| 종류 | 설명 |
|------|------|
| **포화 이진 트리** | 모든 레벨이 완전히 채워진 트리 (모든 노드가 2개의 자식) |
| **완전 이진 트리** | 마지막 레벨을 제외하고 모두 채워지고, 마지막 레벨은 왼쪽부터 채워짐 |
| **편향 이진 트리** | 모든 노드가 한 방향으로만 치우친 트리 |

---

## 3. 이진 트리의 표현 1: 배열

### 3.1 배열을 이용한 이진 트리의 표현

배열을 사용하여 이진 트리를 표현할 수 있습니다. 인덱스를 활용하여 부모와 자식 관계를 나타냅니다.

```
배열 인덱스:
        1(인덱스 1)
       / \
   2(인덱스2)  3(인덱스3)
   /  \      /  \
4(인덱스4) 5(인덱스5) 6(인덱스6) 7(인덱스7)

배열: [0, 1, 2, 3, 4, 5, 6, 7]
인덱스: 0  1  2  3  4  5  6  7
```

### 3.2 노드의 성질

배열에서 인덱스 관계:

- **부모 노드 인덱스**: i의 부모 = i // 2
- **왼쪽 자식 인덱스**: i의 왼쪽 자식 = i * 2
- **오른쪽 자식 인덱스**: i의 오른쪽 자식 = i * 2 + 1

```
인덱스 2의 부모: 2 // 2 = 1
인덱스 4의 부모: 4 // 2 = 2
인덱스 1의 왼쪽 자식: 1 * 2 = 2
인덱스 1의 오른쪽 자식: 1 * 2 + 1 = 3
```

---

## 4. 이진 트리의 표현 2: 연결 리스트

### 4.1 부모 번호를 인덱스로 자식 번호를 저장

```python
left = [0] * (N+1)   # 부모를 인덱스로 왼쪽자식 저장
right = [0] * (N+1)  # 부모를 인덱스로 오른쪽자식 저장

for i in range(E):
    p, c = arr[i*2], arr[i*2+1]
    if left[p] == 0:    # 왼쪽자식이 아직 없으면
        left[p] = c
    else:               # 왼쪽자식이 있는 경우
        right[p] = c
```

### 4.2 자식 번호를 인덱스로 부모 번호를 저장

```python
par = [0] * (N+1)       # 자식을 인덱스로 부모 저장

for i in range(E):
    p, c = arr[i*2], arr[i*2+1]
    par[c] = p                  # 자식을 인덱스로 부모 저장
```

### 4.3 루트 찾기

```python
root = 1
for i in range(1, N+1):
    if par[i] == 0: # 부모 정점이 없으면 루트
        root = i
        break
```

### 4.4 조상 찾기

```python
def find_ancestor(node):
    ancestors = []
    while par[node] != 0:
        node = par[node]
        ancestors.append(node)
    return ancestors
```

### 4.5 배열 표현의 단점

- **공간 낭비**: 편향 트리에서 많은 빈 공간 발생
- **크기 제한**: 트리 크기가 배열 크기를 초과하면 재할당 필요

### 4.6 연결리스트를 이용한 트리 표현

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None
```

---

## 5. 순회 (Traversal)

### 5.1 순회의 개념

트리의 모든 노드를 체계적으로 방문하는 작업입니다. 각 노드를 한 번씩 방문합니다.

### 5.2 3가지 기본 순회 방법

```
        1
       / \
      2   3
     / \ / \
    4  5 6  7
```

#### 전위 순회 (Pre-order Traversal)

**순서**: 부모 → 왼쪽 서브트리 → 오른쪽 서브트리

```
결과: 1 2 4 5 3 6 7
```

```python
def pre_order(T):   # 전위순회, 방문한 정점(부모) 먼저 처리
    if T:   # 0이 아니면 (존재하는 정점이면)
        print(T)    # visit(T) T에서 할일 처리
        pre_order(left[T])  # 왼쪽 자식(서브트리)로 이동
        pre_order(right[T]) # 오른쪽 자식(서브트리)로 이동
```

#### 중위 순회 (In-order Traversal)

**순서**: 왼쪽 서브트리 → 부모 → 오른쪽 서브트리

```
결과: 4 2 5 1 3 6 7
```

```python
def in_order(T):
    if T:   # 0이 아니면 (존재하는 정점이면)
        in_order(left[T])  # 왼쪽 자식(서브트리)로 이동
        print(T)  # visit(T) T에서 할일 처리
        in_order(right[T]) # 오른쪽 자식(서브트리)로 이동
```

#### 후위 순회 (Post-order Traversal)

**순서**: 왼쪽 서브트리 → 오른쪽 서브트리 → 부모

```
결과: 4 5 2 7 6 3 1
```

```python
def post_order(T):
    if T:   # 0이 아니면 (존재하는 정점이면)
        post_order(left[T])  # 왼쪽 자식(서브트리)로 이동
        post_order(right[T]) # 오른쪽 자식(서브트리)로 이동
        print(T)  # visit(T) T에서 할일 처리
```

### 5.3 순회 방법 비교표

| 순회 방법 | 순서 | 사용 예시 |
|-----------|------|-----------|
| 전위 | 부모 → 왼쪽 → 오른쪽 | 파일 시스템 탐색, 트리 복사 |
| 중위 | 왼쪽 → 부모 → 오른쪽 | 이진 탐색 트리 정렬 출력 |
| 후위 | 왼쪽 → 오른쪽 → 부모 | 디렉토리 크기 계산, 수식 계산 |

### 5.4 수식 트리

수식 트리는 수식을 트리 형태로 표현합니다.

```
수식: (2 + 3) * 4

       *
      / \
     +   4
    / \
   2   3
```

- **전위 표기**: * + 2 3 4
- **중위 표기**: 2 + 3 * 4
- **후위 표기**: 2 3 + 4 *

### 5.5 수식 트리의 순회

```python
# 후위 순회로 수식 계산
def evaluate(node):
    if node.left is None and node.right is None:
        return int(node.value)
    
    left_val = evaluate(node.left)
    right_val = evaluate(node.right)
    
    if node.value == '+':
        return left_val + right_val
    elif node.value == '-':
        return left_val - right_val
    elif node.value == '*':
        return left_val * right_val
    elif node.value == '/':
        return left_val / right_val
```

---

## 6. 연습문제: ex.py 코드 분석

### 6.1 문제 설명

주어진 노드 수 N과 간선 정보를 사용하여 트리를 구성하고, 전위, 중위, 후위 순회 결과를 출력합니다.

### 6.2 입력 예시

```
13
1 2 1 3 2 4 3 5 3 6 4 7 5 8 5 9 6 10 6 11 7 12 11 13
```

### 6.3 코드 설명

```python
# 입력 처리
N = int(input())        # 1번부터 N번까지인 정점
E = N - 1               # 간선 수
arr = list(map(int, input().split()))

# 배열 초기화
left = [0] * (N+1)      # 부모를 인덱스로 왼쪽자식 저장
right = [0] * (N+1)     # 부모를 인덱스로 오른쪽자식 저장
par = [0] * (N+1)       # 자식을 인덱스로 부모 저장

# 트리 구성
for i in range(E):
    p, c = arr[i*2], arr[i*2+1]
    par[c] = p                  # 자식을 인덱스로 부모 저장
    if left[p] == 0:    # 왼쪽자식이 아직 없으면
        left[p] = c
    else:               # 왼쪽자식이 있는 경우
        right[p] = c

# 루트 찾기
root = 1
for i in range(1, N+1):
    if par[i] == 0: # 부모 정점이 없으면 루트
        root = i
        break
```

### 6.4 구성되는 트리

```
        1
       / \
      2   3
     / \ / \
    4  5 6  7
   /      \ /
  8       12
         /
        13
```

### 6.5 순회 결과

- **전위 순회**: 1 2 4 7 12 3 5 8 9 6 10 11 13
- **중위 순회**: 12 7 4 2 1 8 5 9 3 10 6 13 11
- **후위 순회**: 12 7 4 2 8 9 5 10 13 11 6 3 1

---

## 7. 이진 탐색 트리 (Binary Search Tree, BST)

### 7.1 BST의 개념 및 특성

**이진 탐색 트리**는 이진 트리의 일종으로, 다음 특성을 가집니다:

- **왼쪽 서브트리**: 모든 노드가 부모보다 작음
- **오른쪽 서브트리**: 모든 노드가 부모보다 큼
- 재귀적으로도 동일한 특성 유지

```
        8
       / \
      3   10
     / \    \
    1   6    14
      / \   /
     4   7 13
```

### 7.2 연결리스트 vs BST

| 연결리스트 | BST |
|-----------|-----|
| O(n) 탐색 | O(log n) 평균 탐색 |
| 단순 구조 | 균형 필요 |
| 삽입 순서 무관 | 삽입 순서에 영향 |

### 7.3 탐색 연산

```python
def search(root, key):
    if root is None or root.val == key:
        return root
    
    if key < root.val:
        return search(root.left, key)
    else:
        return search(root.right, key)
```

### 7.4 삽입 연산

```python
def insert(root, key):
    if root is None:
        return Node(key)
    
    if key < root.val:
        root.left = insert(root.left, key)
    else:
        root.right = insert(root.right, key)
    
    return root
```

### 7.5 BST의 성능

| 연산 | 평균 | 최악 |
|------|------|------|
| 탐색 | O(log n) | O(n) |
| 삽입 | O(log n) | O(n) |
| 삭제 | O(log n) | O(n) |

> **참고**: 트리가 편향된 경우(연속적으로 증가/감소하는 값 삽입) 성능이 O(n)으로 저하됩니다.

### 7.6 삭제 연산

삭제 연산은 세 가지 경우를 고려합니다:

1. **단말 노드 삭제**: 단순히 노드 제거
2. **자식이 하나인 노드 삭제**: 자식을 삭제 위치로 이동
3. **자식이 두 개인 노드 삭제**: **중위 후속자(in-order successor)** 또는 **중위 선행자(in-order predecessor)**로 대체

```python
def delete(root, key):
    if root is None:
        return root
    
    if key < root.val:
        root.left = delete(root.left, key)
    elif key > root.val:
        root.right = delete(root.right, key)
    else:
        # 자식이 하나인 경우 또는 단말 노드
        if root.left is None:
            return root.right
        elif root.right is None:
            return root.left
        
        # 자식이 두 개인 경우
        min_larger_node = find_min(root.right)
        root.val = min_larger_node.val
        root.right = delete(root.right, min_larger_node.val)
    
    return root
```

---

## 8. 힙 (Heap)

### 8.1 힙의 개념 및 특성

**힙**은 완전 이진 트리 기반의 자료구조로, 다음 특성을 가집니다:

- **완전 이진 트리**: 마지막 레벨을 제외하고 모든 레벨이 채워짐
- **힙 속성**: 부모 노드는 항상 자식 노드보다 크거나 작음

### 8.2 최대 힙 (Max Heap)

```
        50
       /  \
     30    20
    /  \   / \
   15  10 8  16

특성: 부모 ≥ 자식 (50 ≥ 30, 50 ≥ 20, 30 ≥ 15, ...)
```

### 8.3 최소 힙 (Min Heap)

```
        10
       /  \
     20    30
    /  \   / \
   40  50 60  80

특성: 부모 ≤ 자식 (10 ≤ 20, 10 ≤ 30, 20 ≤ 40, ...)
```

### 8.4 힙의 예 vs 힙이 아닌 이진 트리

```
힙 (Max Heap)              힙이 아님
      50                        50
     /  \                      /  \
   30    40                   60   40
   (30 ≤ 40 ✓)              (60 > 50 ✗)
```

### 8.5 힙 연산 - 삽입

```python
def insert(heap, value):
    heap.append(value)
    heapify_up(heap, len(heap) - 1)

def heapify_up(heap, index):
    while index > 0:
        parent = (index - 1) // 2
        if heap[index] > heap[parent]:  # 최대 힙
            heap[index], heap[parent] = heap[parent], heap[index]
            index = parent
        else:
            break
```

**삽입 과정 예시 (최대 힙)**:
```
삽입 전:     50          삽입 60:
           /  \              /  \
         30    40    →     30    50
                            \
                            60
```

### 8.6 힙 연산 - 삭제 (최대값 삭제)

```python
def delete_max(heap):
    if len(heap) == 0:
        return None
    
    max_val = heap[0]
    heap[0] = heap[-1]
    heap.pop()
    heapify_down(heap, 0)
    
    return max_val

def heapify_down(heap, index):
    size = len(heap)
    while True:
        largest = index
        left = 2 * index + 1
        right = 2 * index + 2
        
        if left < size and heap[left] > heap[largest]:
            largest = left
        if right < size and heap[right] > heap[largest]:
            largest = right
        
        if largest != index:
            heap[index], heap[largest] = heap[largest], heap[index]
            index = largest
        else:
            break
```

**삭제 과정 예시 (최대 힙)**:
```
삭제 전:     50          삭제 후:
           /  \              /  \
         30    40    →     30    40
                           
```

### 8.7 힙을 이용한 우선순위 큐

힙은 우선순위 큐를 구현하는 가장 효율적인 방법입니다.

```python
import heapq

# 최소 힙으로 우선순위 큐 구현
pq = []
heapq.heappush(pq, (2, 'task2'))
heapq.heappush(pq, (1, 'task1'))
heapq.heappush(pq, (3, 'task3'))

print(heapq.heappop(pq))  # (1, 'task1') - 가장 높은 우선순위
```

| 연산 | 시간 복잡도 |
|------|-------------|
| 삽입 | O(log n) |
| 삭제 (최대/최소) | O(log n) |
| 조회 (최대/최소) | O(1) |

---

## 요약

| 자료구조 | 특성 | 사용처 |
|----------|------|--------|
| **트리** | 계층 구조, 비선형 | 파일 시스템, DOM |
| **이진 트리** | 각 노드 최대 2개 자식 | 표현 용이 |
| **BST** | 정렬된 탐색, O(log n) | 탐색, 정렬 |
| **힙** | 최대/최소 값 접근 O(1) | 우선순위 큐, 정렬 |

---

*본 문서는 알고리즘 학습을 위해 작성되었습니다.*
