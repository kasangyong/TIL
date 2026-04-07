# 최소 비용 신장 트리 (Minimum Spanning Tree)

---

## 1. 신장 트리 (Spanning Tree)

**신장 트리**란 그래프의 모든 정점을 포함하면서 **사이클이 없는** 부분 그래프이다.

- N개의 정점, **N-1개의 간선**으로 구성
- 모든 정점이 연결되어 있어야 함
- 사이클이 없어야 함
- 하나의 그래프에서 여러 개의 신장 트리가 존재할 수 있음

```
원래 그래프:              신장 트리 예시:
  1 --- 2                  1 --- 2
  |\ /  |                  |
  | X   |                  1 --- 3
  |/ \  |                  |
  3 --- 4                  3 --- 4
```

---

## 2. 최소 신장 트리 (Minimum Spanning Tree, MST)

**최소 신장 트리**란 신장 트리 중 **간선 가중치의 합이 최소**인 트리이다.

> 활용 예: 도시 간 도로 건설, 네트워크 케이블 배선, 전력망 설계 등

---

## 3. 그래프의 표현

### 3-1. 인접 행렬 (Adjacency Matrix)

V×V 크기의 2차원 배열로 간선을 표현한다.

```
그래프:
  1 --3-- 2
  |       |
  4       2
  |       |
  3 --1-- 4

인접 행렬 (가중치):
     1    2    3    4
1  [ 0,   3,   4,   0 ]
2  [ 3,   0,   0,   2 ]
3  [ 4,   0,   0,   1 ]
4  [ 0,   2,   1,   0 ]
```

```python
V = 4
graph = [[0] * (V + 1) for _ in range(V + 1)]

# 간선 추가 (양방향)
graph[1][2] = graph[2][1] = 3
graph[1][3] = graph[3][1] = 4
graph[2][4] = graph[4][2] = 2
graph[3][4] = graph[4][3] = 1
```

- 장점: 간선 존재 여부 O(1) 확인
- 단점: V² 공간 필요, 희소 그래프에 비효율적

### 3-2. 인접 리스트 (Adjacency List)

각 정점에 연결된 정점과 가중치를 리스트로 관리한다.

```python
from collections import defaultdict

V = 4
graph = defaultdict(list)

# (연결 정점, 가중치)
graph[1] += [(2, 3), (3, 4)]
graph[2] += [(1, 3), (4, 2)]
graph[3] += [(1, 4), (4, 1)]
graph[4] += [(2, 2), (3, 1)]
```

- 장점: 공간 효율적 (O(V + E))
- 단점: 두 정점 간 간선 확인이 O(degree)

---

## 4. MST 찾기

MST를 구하는 대표적인 두 가지 알고리즘:

| 알고리즘 | 방식 | 복잡도 | 적합한 경우 |
|---------|------|--------|------------|
| Prim    | 정점 중심 | O(E log V) | 밀집 그래프 |
| Kruskal | 간선 중심 | O(E log E) | 희소 그래프 |

---

## 5. Prim 알고리즘

### 설명

임의의 시작 정점에서 출발하여 **현재 MST에 연결된 간선 중 가중치가 가장 작은 간선**을 하나씩 추가하며 트리를 확장하는 알고리즘.

**동작 원리:**
1. 임의의 시작 정점을 MST에 포함
2. MST에 포함된 정점들과 연결된 간선 중 **가장 가중치가 작은 간선** 선택
3. 해당 간선의 반대편 정점이 MST에 없으면 추가
4. 모든 정점이 MST에 포함될 때까지 반복

### 적용 예시 (그림)

```
초기 그래프:
        2
  1 -------- 2
  |  \       |
4 |    \ 3   | 5
  |      \   |
  3 -------- 4
        1

간선 목록:
(1,2,2), (1,3,4), (1,4,3), (2,4,5), (3,4,1)
```

```
Step 1: 정점 1에서 시작
MST = {1}
후보 간선: (1-2, 2), (1-3, 4), (1-4, 3)

    1
   /|\
  2  3  4  ← 방문 안 됨


Step 2: 최소 간선 (1-2, 2) 선택
MST = {1, 2}
후보 간선: (1-3, 4), (1-4, 3), (2-4, 5)

    1 ==2== 2
   두 정점 연결됨


Step 3: 최소 간선 (1-4, 3) 선택
MST = {1, 2, 4}
후보 간선: (1-3, 4), (2-4, 5 → 이미 포함), (4-3, 1)

    1 ==2== 2
    |
    3== 4  ← (1-4)


Step 4: 최소 간선 (4-3, 1) 선택
MST = {1, 2, 3, 4}
모든 정점 포함 완료!

최종 MST:
    1 ==2== 2
    |
    3== 4  ← 연결됨 (4-3 간선)
총 비용: 2 + 3 + 1 = 6
```

### 코드 (Python - 우선순위 큐 이용)

```python
import heapq
from collections import defaultdict

def prim(graph, V, start=1):
    visited = [False] * (V + 1)
    # (가중치, 정점)
    heap = [(0, start)]
    total_cost = 0

    while heap:
        cost, u = heapq.heappop(heap)

        if visited[u]:
            continue  # 이미 MST에 포함된 정점
        visited[u] = True
        total_cost += cost
        print(f"정점 {u} 추가, 비용: {cost}")

        for v, w in graph[u]:
            if not visited[v]:
                heapq.heappush(heap, (w, v))

    return total_cost


# 입력 예시
V, E = map(int, input().split())
graph = defaultdict(list)

for _ in range(E):
    u, v, w = map(int, input().split())
    graph[u].append((v, w))
    graph[v].append((u, w))

print("MST 총 가중치:", prim(graph, V))
```

### 코드 (인접 행렬 버전 - O(V²))

```python
import sys
INF = sys.maxsize

def prim_matrix(graph, V, start=1):
    min_edge = [INF] * (V + 1)  # 각 정점까지 최소 간선 가중치
    in_mst = [False] * (V + 1)
    min_edge[start] = 0
    total_cost = 0

    for _ in range(V):
        # MST에 포함되지 않은 정점 중 최소 비용 정점 선택
        u = -1
        for v in range(1, V + 1):
            if not in_mst[v] and (u == -1 or min_edge[v] < min_edge[u]):
                u = v

        in_mst[u] = True
        total_cost += min_edge[u]

        # 선택된 정점과 연결된 정점들의 최소 비용 갱신
        for v in range(1, V + 1):
            if not in_mst[v] and graph[u][v] != 0:
                if graph[u][v] < min_edge[v]:
                    min_edge[v] = graph[u][v]

    return total_cost
```

---

## 6. Kruskal 알고리즘

### 설명

**모든 간선을 가중치 기준으로 정렬**한 후, 사이클을 형성하지 않는 간선을 차례로 선택하는 알고리즘.

사이클 판단에 **Union-Find (Disjoint Set)** 자료구조를 사용한다.

**동작 원리:**
1. 모든 간선을 가중치 오름차순으로 정렬
2. 가장 가중치가 작은 간선부터 선택
3. 해당 간선이 **사이클을 형성하지 않으면** MST에 추가
4. N-1개의 간선이 선택될 때까지 반복

### Union-Find 자료구조

```python
# 경로 압축 + 랭크 기반 합치기
parent = []
rank = []

def init(n):
    global parent, rank
    parent = list(range(n + 1))  # parent[i] = i
    rank = [0] * (n + 1)

def find(x):
    if parent[x] != x:
        parent[x] = find(parent[x])  # 경로 압축
    return parent[x]

def union(x, y):
    px, py = find(x), find(y)
    if px == py:
        return False  # 이미 같은 집합 → 사이클 형성

    # 랭크 기반 합치기
    if rank[px] < rank[py]:
        px, py = py, px
    parent[py] = px
    if rank[px] == rank[py]:
        rank[px] += 1
    return True
```

### 적용 예시 (그림)

```
그래프:
        2
  1 -------- 2
  |  \       |
4 |    \ 3   | 5
  |      \   |
  3 -------- 4
        1

간선 정렬 (가중치 오름차순):
(3-4, 1), (1-2, 2), (1-4, 3), (1-3, 4), (2-4, 5)
```

```
Step 1: (3-4, 가중치 1)
사이클? NO → MST에 추가
집합: {1} {2} {3,4}

    3 === 4


Step 2: (1-2, 가중치 2)
사이클? NO → MST에 추가
집합: {1,2} {3,4}

    1 === 2       3 === 4


Step 3: (1-4, 가중치 3)
사이클? NO → MST에 추가
집합: {1,2,3,4}

    1 === 2
    |
    3 === 4  (1과 4 연결 → 집합 합체)


Step 4: (1-3, 가중치 4)
사이클? YES (1과 3은 이미 같은 집합) → 건너뜀


간선 3개 선택 완료 (V-1 = 3)

최종 MST:
    1 ==2== 2
    |
    3== 4
총 비용: 1 + 2 + 3 = 6
```

### 코드 (Python)

```python
import sys
input = sys.stdin.readline

def find(x):
    if parent[x] != x:
        parent[x] = find(parent[x])
    return parent[x]

def union(x, y):
    px, py = find(x), find(y)
    if px == py:
        return False
    if rank[px] < rank[py]:
        px, py = py, px
    parent[py] = px
    if rank[px] == rank[py]:
        rank[px] += 1
    return True


V, E = map(int, input().split())
edges = []

for _ in range(E):
    u, v, w = map(int, input().split())
    edges.append((w, u, v))  # (가중치, 정점1, 정점2)

# 가중치 오름차순 정렬
edges.sort()

parent = list(range(V + 1))
rank = [0] * (V + 1)

total_cost = 0
edge_count = 0

for w, u, v in edges:
    if union(u, v):  # 사이클 없으면 MST에 추가
        total_cost += w
        edge_count += 1
        print(f"간선 ({u}-{v}, {w}) 추가")

        if edge_count == V - 1:  # N-1개 선택 완료
            break

print("MST 총 가중치:", total_cost)
```

### 연습 문제

> **문제**: 다음 그래프에서 MST를 Kruskal 알고리즘으로 구하시오.
>
> 정점: 5개  
> 간선: (1-2, 6), (1-3, 1), (1-4, 5), (2-3, 5), (2-5, 3), (3-4, 5), (3-5, 6), (4-5, 2)

```
그래프 시각화:
    1
   /|\
  6/ |1\ 5
  /  |  \
 2   3   4
  \3/|\5 /2
   5 | 5/
    \|/
     5
```

<details>
<summary>풀이 보기</summary>

**간선 정렬 (가중치 오름차순):**

| 순서 | 간선 | 가중치 | 사이클? | 선택 |
|------|------|--------|--------|------|
| 1 | 1-3 | 1 | NO | ✅ |
| 2 | 4-5 | 2 | NO | ✅ |
| 3 | 2-5 | 3 | NO | ✅ |
| 4 | 1-4 | 5 | NO | ✅ |
| 5 | 2-3 | 5 | YES (2,3은 이미 연결) | ❌ |
| 6 | 3-4 | 5 | YES | ❌ |

**선택된 간선:** (1-3, 1) → (4-5, 2) → (2-5, 3) → (1-4, 5)

**최종 MST:**
```
  1 --1-- 3
  |
  5(가중치)
  |
  4 --2-- 5 --3-- 2
```
**총 비용: 1 + 2 + 3 + 5 = 11**

```python
# 검증 코드
edges = [(1,1,3),(2,4,5),(3,2,5),(5,1,4),(5,2,3),(5,3,4),(6,1,2),(6,3,5)]
parent = list(range(6))
rank = [0]*6

def find(x):
    if parent[x]!=x: parent[x]=find(parent[x])
    return parent[x]

def union(x,y):
    px,py=find(x),find(y)
    if px==py: return False
    if rank[px]<rank[py]: px,py=py,px
    parent[py]=px
    if rank[px]==rank[py]: rank[px]+=1
    return True

total, cnt = 0, 0
for w,u,v in edges:
    if union(u,v):
        total+=w; cnt+=1
        print(f"({u}-{v}, {w}) 선택")
        if cnt==4: break
print("총 비용:", total)  # 출력: 11
```
</details>

---

# 최단 경로 (Shortest Path)

---

## 1. 최단 경로 알고리즘의 종류

| 알고리즘 | 특징 | 음수 간선 | 복잡도 |
|---------|------|----------|--------|
| Dijkstra | 단일 출발지, 음수 간선 불가 | X | O(E log V) |
| Bellman-Ford | 단일 출발지, 음수 사이클 감지 | O | O(VE) |
| Floyd-Warshall | 모든 쌍 최단 경로 | O | O(V³) |

---

## 2. Dijkstra 알고리즘

### 상세 설명

**다익스트라(Dijkstra)** 알고리즘은 **음수 가중치가 없는 그래프**에서 **단일 출발지**로부터 모든 정점까지의 최단 경로를 구하는 알고리즘이다.

**핵심 아이디어 (탐욕법, Greedy):**
- 현재까지 알려진 최단 거리가 가장 작은 정점을 먼저 처리
- 처리된 정점의 최단 거리는 이후 변하지 않음 (확정)
- 음수 가중치가 있으면 이 성질이 깨지므로 사용 불가

**동작 원리:**
1. 시작 정점의 거리 = 0, 나머지 = ∞ 으로 초기화
2. 방문하지 않은 정점 중 거리가 가장 작은 정점 u 선택
3. u의 인접 정점 v에 대해: `dist[u] + weight(u,v) < dist[v]` 이면 갱신 (Relaxation)
4. u를 방문 처리
5. 모든 정점 방문할 때까지 2~4 반복

### 적용 예시 (그림)

```
그래프 (출발: 1번 정점):

         2
  1 ---------- 2
  |             |
  | 5      1   | 3
  |             |
  3 ---------- 4 ---------- 5
         2           1
```

```
초기 상태:
dist = [∞, 0, ∞, ∞, ∞, ∞]  (1-indexed)
방문  = [F, F, F, F, F, F]
```

```
Step 1: dist 최소 정점 = 1 (dist=0) 선택, 방문 처리
  → 정점 2: dist[1]+2=2 < ∞ → dist[2]=2
  → 정점 3: dist[1]+5=5 < ∞ → dist[3]=5

dist = [∞, 0, 2, 5, ∞, ∞]
방문  = [F, T, F, F, F, F]
```

```
Step 2: dist 최소 미방문 정점 = 2 (dist=2) 선택, 방문 처리
  → 정점 4: dist[2]+3=5 < ∞ → dist[4]=5

dist = [∞, 0, 2, 5, 5, ∞]
방문  = [F, T, T, F, F, F]
```

```
Step 3: dist 최소 미방문 정점 = 3 또는 4 (dist=5) → 3 선택
  → 정점 4: dist[3]+2=7 > 5 (dist[4]) → 갱신 안 함

dist = [∞, 0, 2, 5, 5, ∞]
방문  = [F, T, T, T, F, F]
```

```
Step 4: dist 최소 미방문 정점 = 4 (dist=5) 선택, 방문 처리
  → 정점 5: dist[4]+1=6 < ∞ → dist[5]=6

dist = [∞, 0, 2, 5, 5, 6]
방문  = [F, T, T, T, T, F]
```

```
Step 5: 정점 5 (dist=6) 선택, 방문 처리
  → 더 이상 갱신 없음

최종 최단 거리:
  1→1 : 0
  1→2 : 2
  1→3 : 5
  1→4 : 5
  1→5 : 6
```

### 적용 예시 (코드)

```python
import heapq
import sys
input = sys.stdin.readline
INF = float('inf')

def dijkstra(graph, V, start):
    dist = [INF] * (V + 1)
    dist[start] = 0

    # (거리, 정점)
    heap = [(0, start)]

    while heap:
        d, u = heapq.heappop(heap)

        if d > dist[u]:
            continue  # 이미 처리된 정점 (오래된 정보)

        for v, w in graph[u]:
            if dist[u] + w < dist[v]:
                dist[v] = dist[u] + w
                heapq.heappush(heap, (dist[v], v))

    return dist


# 입력 처리
V, E = map(int, input().split())
graph = [[] for _ in range(V + 1)]

for _ in range(E):
    u, v, w = map(int, input().split())
    graph[u].append((v, w))  # 단방향

start = int(input())
dist = dijkstra(graph, V, start)

for i in range(1, V + 1):
    print(f"{start} → {i} : {'도달불가' if dist[i] == INF else dist[i]}")
```

### 경로 추적 (Path Reconstruction)

최단 경로 자체를 출력해야 할 때는 `prev` 리스트를 추가한다.

```python
import heapq
INF = float('inf')

def dijkstra_with_path(graph, V, start):
    dist = [INF] * (V + 1)
    prev = [-1] * (V + 1)  # 이전 정점 기록
    dist[start] = 0

    heap = [(0, start)]

    while heap:
        d, u = heapq.heappop(heap)

        if d > dist[u]:
            continue

        for v, w in graph[u]:
            if dist[u] + w < dist[v]:
                dist[v] = dist[u] + w
                prev[v] = u  # 경로 기록
                heapq.heappush(heap, (dist[v], v))

    return dist, prev


def get_path(prev, end):
    path = []
    v = end
    while v != -1:
        path.append(v)
        v = prev[v]
    path.reverse()
    return path


# 사용 예시
dist, prev = dijkstra_with_path(graph, V, start=1)
print("경로:", get_path(prev, end=5))  # [1, 2, 4, 5] 등
```

### 복잡도 분석

| 구현 방식 | 시간 복잡도 | 공간 복잡도 |
|---------|------------|------------|
| 배열 (선형 탐색) | O(V²) | O(V) |
| 우선순위 큐 (힙) | O((V+E) log V) | O(V+E) |

- 밀집 그래프(E ≈ V²): 배열 방식이 유리
- 희소 그래프(E ≈ V): 우선순위 큐가 유리

---

### 연습 문제 1 (기본)

> 정점 6개, 아래 방향 그래프에서 정점 1로부터 각 정점까지 최단 거리를 구하시오.
>
> 간선: (1→2, 1), (1→3, 4), (2→3, 2), (2→4, 5), (3→5, 1), (4→6, 2), (5→4, 1), (5→6, 5)

```
그래프 시각화:

  1 --1→ 2 --5→ 4 --2→ 6
  |      |        ↑
  4↓    2↓       1|
  |      3 --1→ 5
  └──────┘      |
                5↓
                6
```

<details>
<summary>풀이 보기</summary>

**단계별 dist 변화:**

| Step | 선택 정점 | dist[1] | dist[2] | dist[3] | dist[4] | dist[5] | dist[6] |
|------|----------|---------|---------|---------|---------|---------|---------|
| 초기 | -        | 0       | ∞       | ∞       | ∞       | ∞       | ∞       |
| 1    | 1        | 0       | **1**   | **4**   | ∞       | ∞       | ∞       |
| 2    | 2        | 0       | 1       | **3**   | **6**   | ∞       | ∞       |
| 3    | 3        | 0       | 1       | 3       | 6       | **4**   | ∞       |
| 4    | 5        | 0       | 1       | 3       | **5**   | 4       | **9**   |
| 5    | 4        | 0       | 1       | 3       | 5       | 4       | **7**   |
| 6    | 6        | 0       | 1       | 3       | 5       | 4       | 7       |

**최종 답:**
- 1→1: **0**
- 1→2: **1**
- 1→3: **3** (경로: 1→2→3)
- 1→4: **5** (경로: 1→2→3→5→4)
- 1→5: **4** (경로: 1→2→3→5)
- 1→6: **7** (경로: 1→2→3→5→4→6)

```python
import heapq
INF = float('inf')

V = 6
graph = [[] for _ in range(V + 1)]
graph[1] += [(2,1),(3,4)]
graph[2] += [(3,2),(4,5)]
graph[3] += [(5,1)]
graph[4] += [(6,2)]
graph[5] += [(4,1),(6,5)]

dist = [INF]*(V+1)
dist[1] = 0
heap = [(0,1)]

while heap:
    d, u = heapq.heappop(heap)
    if d > dist[u]: continue
    for v, w in graph[u]:
        if dist[u]+w < dist[v]:
            dist[v] = dist[u]+w
            heapq.heappush(heap,(dist[v],v))

print(dist[1:])  # [0, 1, 3, 5, 4, 7]
```
</details>

---

### 연습 문제 2 (응용 - BOJ 1753 스타일)

> 방향 그래프에서 주어진 시작점에서 모든 정점으로의 최단 경로를 구하라.  
> 도달할 수 없는 정점은 "INF" 출력.

```python
import heapq
import sys
input = sys.stdin.readline
INF = float('inf')

def solve():
    V, E = map(int, input().split())
    start = int(input())

    graph = [[] for _ in range(V + 1)]
    for _ in range(E):
        u, v, w = map(int, input().split())
        graph[u].append((v, w))

    dist = [INF] * (V + 1)
    dist[start] = 0
    heap = [(0, start)]

    while heap:
        d, u = heapq.heappop(heap)
        if d > dist[u]:
            continue
        for v, w in graph[u]:
            if dist[u] + w < dist[v]:
                dist[v] = dist[u] + w
                heapq.heappush(heap, (dist[v], v))

    for i in range(1, V + 1):
        print("INF" if dist[i] == INF else dist[i])

solve()
```

<details>
<summary>입출력 예시</summary>

**입력:**
```
5 6
1
5 1 1
1 2 2
1 3 3
2 3 4
2 4 5
3 4 6
```

**출력:**
```
0
2
3
7
INF
```

**설명:** 정점 5는 출발지이므로 0, 정점 5에서는 도달할 수 없는 경로가 없지만  
(위 예시에서 출발이 1이므로) 5는 도달 불가 → INF
</details>

---

## 정리 비교표

| 구분 | Prim | Kruskal | Dijkstra |
|------|------|---------|---------|
| 목적 | MST | MST | 최단 경로 |
| 방식 | 정점 중심 | 간선 중심 | 정점 중심 |
| 자료구조 | 우선순위 큐 (heapq) | 정렬 + Union-Find | 우선순위 큐 (heapq) |
| 음수 간선 | - | - | 불가 |
| 시간복잡도 | O(E log V) | O(E log E) | O(E log V) |
| Python 핵심 | `heapq.heappush/pop` | `list.sort()` + `find/union` | `heapq.heappush/pop` |
