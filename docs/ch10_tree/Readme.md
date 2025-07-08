# 이진 트리 (Binary Tree)

## 1. 이진 트리란?

이진 트리는 **컴퓨터 과학**에서 가장 중요한 **자료 구조** 중 하나로, **트리 구조**의 특별한 형태이다.
각 노드가 **최대 2개**의 자식 노드(child_node)만을 가질 수 있으며, 이들을 **왼쪽 자식(left child)**과 **오른쪽 자식(right child)**으로 구분한다.
이진 트리는 검색, 정렬, 데이터베이스 인덱싱, 컴파일러 설계 등 다양한 분야에서 활용된다.

그 전에 자주 쓰는 트리 용어는 [여길](https://en.wikipedia.org/wiki/Tree_(data_structure)) 참고하길 바란다.

---

## 2. 트리의 기본 개념

먼저 우리는 다음과 같은 개념을 알아야 한다.

### 1. 노드 (Node)

트리의 각 원소를 **노드**라고 한다.
모든 노드는 데이터와 최대 2개의 자식 노드 포인터를 가진다.

### 2. 루트 (Root)

트리의 **최상위 노드**를 루트라고 한다. 트리에는 **정확히 하나**의 루트가 존재한다.
즉 루트는 부모가 없는 노드이다.

예를 들어, 다음 트리에서 A가 루트이다:
```
    A
   / \
  B   C
```

### 3. 잎 노드 (Leaf Node)

**자식이 없는** 노드를 잎 노드 또는 리프 노드라고 한다.
위 예시에서 B와 C가 잎 노드이다.

### 4. 부모와 자식 개념

트리 구조에서 **부모-자식 관계**는 노드 간의 계층적 연결을 나타내는 핵심 개념이다.

#### 부모 노드 (Parent Node)
- 다른 노드들과 연결되어 있으면서 **상위 레벨**에 위치한 노드
- 자식 노드들을 가리키는 포인터 또는 참조를 가지고 있음
- 루트 노드를 제외한 모든 노드는 정확히 **하나의 부모**를 가짐

#### 자식 노드 (Child Node)
- 부모 노드의 **하위 레벨**에 위치하며, 부모 노드와 직접 연결된 노드
- 이진 트리에서는 각 노드가 최대 **2개의 자식**만 가질 수 있음
- **왼쪽 자식(Left Child)**: 부모 노드의 왼쪽에 위치한 자식 노드
- **오른쪽 자식(Right Child)**: 부모 노드의 오른쪽에 위치한 자식 노드

#### 부모-자식 관계 예제

다음 이진 트리를 보자:
```
      A
     / \
    B   C
   /   / \
  D   E   F
```

이 트리에서:
- **A**는 **B**와 **C**의 부모 노드
- **B**는 **D**의 부모 노드
- **C**는 **E**와 **F**의 부모 노드
- **B**와 **C**는 **A**의 자식 노드 (B는 왼쪽 자식, C는 오른쪽 자식)
- **D**는 **B**의 자식 노드 (왼쪽 자식)
- **E**와 **F**는 **C**의 자식 노드 (E는 왼쪽 자식, F는 오른쪽 자식)

#### 관계의 특징
- **루트 노드**: 부모가 없는 최상위 노드 (위 예제에서 A)
- **리프 노드**: 자식이 없는 최하위 노드들 (위 예제에서 D, E, F)

---

### 4. 높이 (Height)와 깊이 (Depth)

#### 노드의 깊이 (Depth)
노드의 **깊이**는 루트로부터 해당 노드까지의 경로 길이이다:

$$
\text{depth}(v) = \text{루트에서 노드 } v \text{까지의 간선 수}
$$

예시:
```
    A     (depth = 0)
   / \
  B   C   (depth = 1)
 /
D         (depth = 2)
```

#### 노드의 높이 (Height)
노드의 **높이**는 해당 노드에서 가장 깊은 잎 노드까지의 경로 길이이다:

$$
\text{height}(v) = \max\{\text{노드 } v\text{에서 잎 노드까지의 간선 수}\}
$$

#### 트리의 높이
트리의 높이는 **루트의 높이**와 같다:

$$
\text{height}(T) = \text{height}(\text{root})
$$

---

### 5. 완전 이진 트리 (Complete Binary Tree)

**완전 이진 트리**는 마지막 레벨을 제외한 모든 레벨이 완전히 채워져 있고, 마지막 레벨은 왼쪽부터 순서대로 채워진 이진 트리이다.

$$
\text{Complete Binary Tree}: \forall \text{level } i < h-1, \text{모든 노드가 존재}
$$

예시:
```
       1
      / \
     2   3
    / \ /
   4  5 6
```

#### 완전 이진 트리의 성질

높이가 {% math %}h{% endmath %}인 완전 이진 트리에서:

- **최소 노드 수**: {% math %}2^h{% endmath %}개
- **최대 노드 수**: {% math %}2^{h+1} - 1{% endmath %}개

$$
2^h \leq n \leq 2^{h+1} - 1
$$

여기서 {% math %}n{% endmath %}은 총 노드 수이다.

---

### 6. 전 이진 트리 (Full Binary Tree)

**전 이진 트리**는 모든 노드가 0개 또는 2개의 자식을 가지는 이진 트리이다. 즉, 1개의 자식만 가지는 노드가 존재하지 않는다.

$
\text{Full Binary Tree}: \forall \text{node}, \text{degree} \in \{0, 2\}
$

예시:
```
       1
      / \
     2   3
    / \ 
   4  5 
```



### 7. 포화 이진 트리 (Perfect Binary Tree)

**포화 이진 트리**는 모든 내부 노드가 정확히 2개의 자식을 가지고, 모든 잎 노드가 같은 레벨에 있는 이진 트리이다. 즉, **완전하면서 동시에 포화된** 이진 트리이다.

$
\text{Perfect Binary Tree} = \text{Complete} \cap \text{Full Binary Tree}
$

예시:
```
       1
      / \
     2   3
    / \ / \
   4  5 6  7
```

#### 포화 이진 트리의 성질

포화 이진 트리는 전 이진 트리와 동일한 성질을 가진다:

- **총 노드 수**: {% math %}2^{h+1} - 1{% endmath %}개 (높이가 {% math %}h{% endmath %}일 때)
- **잎 노드 수**: {% math %}2^h{% endmath %}개
- **내부 노드 수**: {% math %}2^h - 1{% endmath %}개


---

## 3. 이진 트리의 순회 (Tree Traversal)

이진 트리의 **순회**는 트리의 모든 노드를 체계적으로 방문하는 과정이다. 주요 순회 방법은 다음과 같다.

### 전위 순회 (Preorder Traversal)

**루트 → 왼쪽 서브트리 → 오른쪽 서브트리** 순서로 방문한다.

```
def preorder(node):
    if node is not None:
        visit(node)           # 루트 방문
        preorder(node.left)   # 왼쪽 서브트리
        preorder(node.right)  # 오른쪽 서브트리
```

#### 예시
```
       1
      / \
     2   3
    / \
   4   5
```

전위 순회 결과: **1 → 2 → 4 → 5 → 3**

---

### 중위 순회 (Inorder Traversal)

**왼쪽 서브트리 → 루트 → 오른쪽 서브트리** 순서로 방문한다.

```
def inorder(node):
    if node is not None:
        inorder(node.left)    # 왼쪽 서브트리
        visit(node)           # 루트 방문
        inorder(node.right)   # 오른쪽 서브트리
```

#### 예시
위와 같은 트리에서 중위 순회 결과: **4 → 2 → 5 → 1 → 3**

**중요**: 이진 탐색 트리에서 중위 순회는 **오름차순**으로 노드를 방문한다.

---

### 후위 순회 (Postorder Traversal)

**왼쪽 서브트리 → 오른쪽 서브트리 → 루트** 순서로 방문한다.

```
def postorder(node):
    if node is not None:
        postorder(node.left)   # 왼쪽 서브트리
        postorder(node.right)  # 오른쪽 서브트리
        visit(node)            # 루트 방문
```

#### 예시
위와 같은 트리에서 후위 순회 결과: **4 → 5 → 2 → 3 → 1**

---

### 레벨 순회 (Level-order Traversal)

트리를 **레벨별로** 왼쪽부터 오른쪽으로 방문한다. **큐(Queue)**를 사용하여 구현한다.

```
def levelorder(root):
    if root is None:
        return
    
    queue = [root]
    while queue:
        node = queue.pop(0)   # 큐에서 제거
        visit(node)
        
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
```

#### 예시
위와 같은 트리에서 레벨 순회 결과: **1 → 2 → 3 → 4 → 5**

---

## 4. 이진 탐색 트리 (Binary Search Tree, BST)

이진 탐색 트리는 다음 조건을 만족하는 **특별한 이진 트리**이다:

### BST 속성

각 노드 {% math %}v{% endmath %}에 대해:

$$
\forall x \in \text{left\_subtree}(v), \; x.\text{key} < v.\text{key}
$$

$$
\forall y \in \text{right\_subtree}(v), \; y.\text{key} > v.\text{key}
$$

즉, **왼쪽 서브트리의 모든 값 < 루트 < 오른쪽 서브트리의 모든 값**이다.

### 예시
```
       8
      / \
     3   10
    / \    \
   1   6    14
      / \   /
     4   7 13
```

이 트리는 BST 속성을 만족한다:
- 8의 왼쪽 서브트리: {1, 3, 4, 6, 7} < 8
- 8의 오른쪽 서브트리: {10, 13, 14} > 8

---

### BST의 기본 연산

#### 1. 탐색 (Search)

값 {% math %}k{% endmath %}를 찾기 위해 루트부터 시작하여:

```
def search(node, key):
    if node is None or node.key == key:
        return node
    
    if key < node.key:
        return search(node.left, key)
    else:
        return search(node.right, key)
```

**시간 복잡도**: {% math %}O(h){% endmath %} (여기서 {% math %}h{% endmath %}는 트리의 높이)

#### 2. 삽입 (Insert)

새로운 값을 BST 속성을 유지하면서 삽입한다:

```
def insert(node, key):
    if node is None:
        return Node(key)
    
    if key < node.key:
        node.left = insert(node.left, key)
    else:
        node.right = insert(node.right, key)
    
    return node
```

#### 3. 삭제 (Delete)

삭제할 노드의 자식 수에 따라 경우를 나눈다:

1. **잎 노드**: 단순히 제거
2. **자식이 1개**: 자식으로 대체
3. **자식이 2개**: 중위 순회 후계자(또는 전임자)로 대체

---

### BST의 성능

#### 균형 잡힌 경우
- **높이**: {% math %}h = O(\log n){% endmath %}
- **탐색, 삽입, 삭제**: {% math %}O(\log n){% endmath %}

#### 편향된 경우 (Skewed Tree)
```
1
 \
  2
   \
    3
     \
      4
```

- **높이**: {% math %}h = O(n){% endmath %}
- **탐색, 삽입, 삭제**: {% math %}O(n){% endmath %}

따라서 BST의 효율성을 위해서는 **균형**을 유지하는 것이 중요하다.

---

## 5. 균형 이진 트리

### AVL 트리

**AVL 트리**는 각 노드에서 왼쪽과 오른쪽 서브트리의 높이 차이가 최대 1인 자가 균형 이진 탐색 트리이다.

#### 균형 인수 (Balance Factor)

$$
\text{BF}(v) = \text{height}(\text{left\_subtree}) - \text{height}(\text{right\_subtree})
$$

AVL 트리에서는 모든 노드에 대해:

$$
\text{BF}(v) \in \{-1, 0, 1\}
$$

#### 회전 (Rotation)

균형이 깨진 경우, **회전** 연산을 통해 균형을 복구한다:

1. **LL 회전**: 오른쪽 회전
2. **RR 회전**: 왼쪽 회전
3. **LR 회전**: 왼쪽-오른쪽 회전
4. **RL 회전**: 오른쪽-왼쪽 회전

---

### 레드-블랙 트리

**레드-블랙 트리**는 각 노드가 빨간색 또는 검은색으로 색칠된 이진 탐색 트리로, 다음 조건을 만족한다:

1. 모든 노드는 빨간색 또는 검은색이다
2. 루트는 검은색이다
3. 모든 잎(NIL)은 검은색이다
4. 빨간 노드의 자식은 모두 검은색이다
5. 임의의 노드에서 후손 잎까지의 경로에 있는 검은 노드의 수는 같다

이러한 조건들로 인해 가장 긴 경로가 가장 짧은 경로의 2배를 넘지 않아 **대략적인 균형**을 유지한다.

---

## 6. 트리의 응용

### 1. 힙 (Heap)

**힙**은 완전 이진 트리로, 다음 힙 속성 중 하나를 만족한다:

#### 최대 힙 (Max Heap)
$$
\forall \text{parent-child pair}, \; \text{parent} \geq \text{child}
$$

#### 최소 힙 (Min Heap)
$$
\forall \text{parent-child pair}, \; \text{parent} \leq \text{child}
$$

힙은 **우선순위 큐** 구현과 **힙 정렬**에 사용된다.

### 2. 표현식 트리

**표현식 트리**는 수학 표현식을 트리로 나타낸 것이다:

- 잎 노드: 피연산자
- 내부 노드: 연산자

예시: {% math %}(a + b) \times (c - d){% endmath %}

```
       ×
      / \
     +   -
    / \ / \
   a  b c  d
```

- **전위 순회**: 전위 표기법 (Prefix)
- **중위 순회**: 중위 표기법 (Infix)
- **후위 순회**: 후위 표기법 (Postfix)

### 3. 결정 트리 (Decision Tree)

**결정 트리**는 의사결정 과정을 트리 형태로 나타낸 것으로, 머신러닝과 데이터 마이닝에서 분류와 회귀에 사용된다.

---

## 7. 트리의 특성과 정리

### 트리의 기본 정리

{% math %}n{% endmath %}개의 노드를 가진 트리에 대해:

$$
\text{간선의 수} = n - 1
$$

**증명**: 루트를 제외한 모든 노드는 정확히 하나의 부모를 가지므로, 각 노드마다 부모로의 간선이 하나씩 존재한다.

### 이진 트리의 성질

#### 성질 1
레벨 {% math %}i{% endmath %}에서 최대 노드 수는 {% math %}2^i{% endmath %}개이다. ({% math %}i \geq 0{% endmath %})

#### 성질 2
높이가 {% math %}h{% endmath %}인 이진 트리의 최대 노드 수는 {% math %}2^{h+1} - 1{% endmath %}개이다.

#### 성질 3
{% math %}n{% endmath %}개의 노드를 가진 이진 트리의 최소 높이는 {% math %}\lfloor \log_2 n \rfloor{% endmath %}이다.

#### 성질 4
잎 노드의 개수를 {% math %}n_0{% endmath %}, 차수가 2인 노드의 개수를 {% math %}n_2{% endmath %}라고 하면:

$$
n_0 = n_2 + 1
$$

**증명**: 간선의 수를 두 가지 방법으로 계산하면:
- 위에서 아래로: {% math %}n_1 + 2n_2{% endmath %}
- 전체 노드에서 루트 제외: {% math %}n_0 + n_1 + n_2 - 1{% endmath %}

따라서 {% math %}n_1 + 2n_2 = n_0 + n_1 + n_2 - 1{% endmath %}이고, 정리하면 {% math %}n_0 = n_2 + 1{% endmath %}이다.

---

## 8. 구현 예시

### 기본 노드 구조

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

### 이진 탐색 트리 클래스

```python
class BST:
    def __init__(self):
        self.root = None
    
    def insert(self, val):
        self.root = self._insert_recursive(self.root, val)
    
    def _insert_recursive(self, node, val):
        if not node:
            return TreeNode(val)
        
        if val < node.val:
            node.left = self._insert_recursive(node.left, val)
        else:
            node.right = self._insert_recursive(node.right, val)
        
        return node
    
    def inorder(self):
        result = []
        self._inorder_recursive(self.root, result)
        return result
    
    def _inorder_recursive(self, node, result):
        if node:
            self._inorder_recursive(node.left, result)
            result.append(node.val)
            self._inorder_recursive(node.right, result)
```

이진 트리는 컴퓨터 과학에서 가장 근본적이고 강력한 자료 구조 중 하나로, 그 응용 범위는 무궁무진하다.