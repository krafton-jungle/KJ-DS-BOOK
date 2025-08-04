# 세그먼트 트리 (Segment Tree)

## 1. 정의 및 필요성

세그먼트 트리(Segment Tree)는 배열과 같은 선형 자료구조 위에서 특정 **구간(Segment)**에 대한 연산(예: 구간 합, 최댓값, 최솟값, 구간 곱 등)을 효율적으로 처리하기 위한 트리 기반의 자료구조이다.

배열의 특정 구간에 대한 연산을 반복적으로 수행해야 할 때, 매번 구간 내의 모든 원소를 순회하는 방식($O(N)$)은 비효율적이다. 세그먼트 트리는 이러한 **구간 쿼리(Range Query)**와 **점 업데이트(Point Update)**를 모두 로그 시간 복잡도($O(\log N)$)로 처리할 수 있도록 한다.

## 2. 핵심 원리 및 구조

세그먼트 트리는 원본 배열의 구간 정보를 계층적으로 저장하는 **이진 트리**이다.

- **루트 노드**: 배열 전체 구간에 대한 정보를 나타낸다. (예: `A[0...N-1]`의 합)
- **내부 노드**: 특정 구간의 정보를 나타낸다. 이 노드의 정보는 두 자식 노드의 정보를 조합하여 계산된다. (예: 부모 노드의 합 = 왼쪽 자식 노드의 합 + 오른쪽 자식 노드의 합)
- **리프 노드**: 배열의 각 원소 값을 직접 나타낸다.

예를 들어, 배열 `A = [3, 5, 1, 9, 4, 2, 7, 6]`에 대한 **구간 합** 세그먼트 트리는 다음과 같이 시각화할 수 있다. 각 노드는 `[구간 정보]` 형태로 표현한다.

```
                          [37] (0-7)
                        /          \
                 [18] (0-3)          [19] (4-7)
                /      \            /      \
            [8] (0-1) [10] (2-3)   [6] (4-5) [13] (6-7)
           /   \      /   \       /   \      /   \
        [3](0) [5](1) [1](2) [9](3) [4](4) [2](5) [7](6) [6](7)
```

- 트리의 높이는 $O(\log N)$이 된다.
- 트리 구현 시, 일반적으로 배열을 사용하여 표현하며 노드의 수는 약 $2N$개에서 $4N$개 사이의 공간이 필요하다. 인덱스 1을 루트로 사용하면 특정 노드의 자식과 부모를 쉽게 계산할 수 있다.
  - 왼쪽 자식: `node * 2`
  - 오른쪽 자식: `node * 2 + 1`
  - 부모: `node // 2`

## 3. 주요 연산 및 구현

### 3.1. 트리 구축 (Build)

세그먼트 트리를 구축하는 연산이다. 재귀를 사용하여 각 노드의 값을 채워나간다.

- **리프 노드**에 도달하면 (구간의 시작과 끝이 같아지면), 배열의 원본 값을 저장한다.
- **내부 노드**는 왼쪽 자식과 오른쪽 자식 노드를 재귀적으로 구축한 후, 두 자식의 값을 조합하여 자신의 값을 결정한다.

**시간 복잡도**: 모든 노드를 한 번씩 방문하므로 $O(N)$이다.

```python
# tree: 세그먼트 트리를 저장할 배열
# A: 원본 배열
# node: 현재 노드 인덱스
# start, end: 현재 노드가 담당하는 구간

def build(node, start, end):
    # 리프 노드인 경우
    if start == end:
        tree[node] = A[start]
        return tree[node]
    
    # 재귀적으로 자식 노드들의 값을 계산
    mid = (start + end) // 2
    left_sum = build(node * 2, start, mid)
    right_sum = build(node * 2 + 1, mid + 1, end)
    
    # 내부 노드는 자식 노드들의 합으로 값을 갱신
    tree[node] = left_sum + right_sum
    return tree[node]
```

### 3.2. 구간 쿼리 (Query)

주어진 구간 `[left, right]`에 대한 연산 결과를 반환한다.

- 현재 노드의 구간이 쿼리 구간과 **전혀 겹치지 않으면**, 연산에 영향을 주지 않는 항등원(합의 경우 0, 곱의 경우 1)을 반환한다.
- 현재 노드의 구간이 쿼리 구간에 **완전히 포함되면**, 현재 노드의 값을 반환한다.
- 현재 노드의 구간이 쿼리 구간과 **일부만 겹치면**, 왼쪽과 오른쪽 자식으로 재귀 호출을 수행하여 결과를 조합해 반환한다.

**시간 복잡도**: 트리의 높이만큼 탐색하므로 $O(\log N)$이다.

```python
def query(node, start, end, left, right):
    # 쿼리 구간이 현재 노드 구간과 겹치지 않는 경우
    if right < start or end < left:
        return 0 # 합의 항등원

    # 쿼리 구간이 현재 노드 구간을 완전히 포함하는 경우
    if left <= start and end <= right:
        return tree[node]

    # 일부만 겹치는 경우, 자식에게 위임하여 결과를 조합
    mid = (start + end) // 2
    left_child = query(node * 2, start, mid, left, right)
    right_child = query(node * 2 + 1, mid + 1, end, left, right)
    
    return left_child + right_child
```

### 3.3. 점 업데이트 (Update)

배열의 특정 인덱스(`index`)의 값을 새로운 값(`val`)으로 변경하고, 이에 따라 영향을 받는 트리 노드들을 갱신한다.

- `index`를 포함하는 경로를 따라 리프 노드까지 내려간다.
- 리프 노드의 값을 변경한다.
- 재귀적으로 함수를 빠져나오면서, 경로상의 모든 부모 노드들의 값을 자식 노드의 정보에 맞춰 다시 계산한다.

**시간 복잡도**: 트리의 높이만큼 갱신이 필요하므로 $O(\log N)$이다.

```python
def update(node, start, end, index, val):
    # 리프 노드에 도달하면 값을 직접 변경
    if start == end:
        tree[node] = val
        return

    mid = (start + end) // 2
    # 변경할 인덱스가 포함된 자식 노드로 재귀
    if start <= index <= mid:
        update(node * 2, start, mid, index, val)
    else:
        update(node * 2 + 1, mid + 1, end, index, val)
    
    # 자식 노드의 값이 변경되었으므로, 현재 노드의 값도 갱신
    tree[node] = tree[node * 2] + tree[node * 2 + 1]
```

## 4. 고급 기법: Lazy Propagation

점 업데이트가 아닌 **구간 업데이트** (예: `A[i...j]`의 모든 원소에 `k`를 더하기)를 효율적으로 처리하기 위한 기법이다.

구간 업데이트 시 매번 모든 리프 노드를 수정하면 $O(N \log N)$이 걸려 비효율적이다. Lazy Propagation은 **"변경 사항을 당장 적용하지 않고, 나중에 필요할 때까지 미루는"** 전략을 사용한다.

- `lazy` 배열을 추가로 사용하여 각 노드에 "미뤄진" 변경 값을 저장한다.
- `update` 시, 현재 노드가 업데이트 구간에 완전히 포함되면, 노드 값만 갱신하고 `lazy` 배열에 변경 사항을 기록한 뒤 자식에게 전파하지 않고 종료한다.
- `query` 또는 `update`로 특정 노드에 접근할 때, 해당 노드에 `lazy` 값이 있다면, 이 값을 자신과 자식 노드에 적용하고 자신의 `lazy` 값은 초기화한다. 이 과정을 **`propagate`** (전파)라고 한다.

**시간 복잡도**: 구간 업데이트와 구간 쿼리 모두 $O(\log N)$으로 수행된다.

```python
# lazy: lazy 값을 저장할 배열
lazy = [0] * (4 * N)

# lazy 값을 자식에게 전파하는 함수
def propagate(node, start, end):
    if lazy[node] != 0:
        # 현재 노드에 lazy 값이 있다면, 값을 적용
        tree[node] += (end - start + 1) * lazy[node]
        # 리프 노드가 아니라면, 자식에게 lazy 값을 전파
        if start != end:
            lazy[node * 2] += lazy[node]
            lazy[node * 2 + 1] += lazy[node]
        # 자신의 lazy 값은 처리했으므로 초기화
        lazy[node] = 0

def update_range(node, start, end, left, right, diff):
    propagate(node, start, end) # 항상 propagate 먼저 호출

    if right < start or end < left:
        return

    if left <= start and end <= right:
        tree[node] += (end - start + 1) * diff
        if start != end:
            lazy[node * 2] += diff
            lazy[node * 2 + 1] += diff
        return

    mid = (start + end) // 2
    update_range(node * 2, start, mid, left, right, diff)
    update_range(node * 2 + 1, mid + 1, end, left, right, diff)
    tree[node] = tree[node * 2] + tree[node * 2 + 1]

def query_lazy(node, start, end, left, right):
    propagate(node, start, end) # 항상 propagate 먼저 호출

    if right < start or end < left:
        return 0

    if left <= start and end <= right:
        return tree[node]

    mid = (start + end) // 2
    left_child = query_lazy(node * 2, start, mid, left, right)
    right_child = query_lazy(node * 2 + 1, mid + 1, end, left, right)
    return left_child + right_child
```

## 5. 복잡도 분석 및 활용

| 연산 | 시간 복잡도 | 공간 복잡도 |
| :--- | :---: | :---: |
| **Build** (구축) | $O(N)$ | $O(N)$ |
| **Query** (구간 쿼리) | $O(\log N)$ | - |
| **Update** (점 업데이트) | $O(\log N)$ | - |
| **Range Update** (Lazy Propagation) | $O(\log N)$ | $O(N)$ (lazy 배열) |

### 활용 사례
- **알고리즘 경진대회**: 구간에 대한 쿼리가 빈번하게 발생하는 문제에서 필수적으로 사용된다.
- **데이터베이스 인덱싱**: 특정 범위의 데이터를 집계하는 데 활용될 수 있다.
- **계산기하학**: 기하학적 객체들의 포함 관계나 교차를 찾는 문제에 응용된다.

```