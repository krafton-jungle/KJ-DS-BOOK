# 트리 (Tree)

> 트리(Tree)는 계층적(Hierarchical) 데이터 구조로, 각 요소(노드)가 부모-자식 관계로 연결되는 비선형 구조이다. 트리는 루트(Root) 노드에서 시작되어 여러 개의 가지(브랜치)로 분기되며, 각 가지는 재귀적으로 서브트리를 이룬다. 트리는 그래프의 일종이지만, **순환(cycle)이 없는 연결 비순환 그래프(Directed Acyclic Graph)** 이다.

## 목차

| 항목         | 설명                           |
|--------------|-------------------------------|
| [핵심 키워드 정리](#핵심-키워드-정리)   | 알고리즘 개념, 키워드 정리 |
| [자료구조 구조 및 시각화](#자료구조-구조-및-시각화) | 알고리즘을 이해하는데 필요한 개념 |
| [기본 연산](#기본-연산) | 알고리즘을 시각화 자료와 함께 보여준다 |
| [구현 예시 (Python)](#구현-예시-python) | 파이썬으로 구현한 알고리즘 예시 코드 |
| [자료구조 비교](#자료구조-비교) | 트리를 다른 구조와 비교 |
| [주요 트리 종류](#자료구조-비교) | 다양한 트리의 종류 |
| [참고 자료](#참고-자료) | 자료 출처 및 더 찾아보기 |

## 핵심 키워드 정리

* **노드(Node)**: 데이터를 포함하고 자식 노드에 대한 포인터를 가지는 트리의 기본 구성 단위.
* **루트(Root)**: 트리의 최상단에 위치한 유일한 노드로, 부모가 없다.
* **부모(Parent)**: 다른 노드를 자식으로 가지는 노드.
* **자식(Child)**: 어떤 노드에 연결된 하위 노드.
* **형제(Sibling)**: 같은 부모를 공유하는 노드들.
* **리프(Leaf)**: 자식이 없는 말단 노드.
* **내부 노드(Internal Node)**: 자식을 가지는 노드.
* **서브트리(Subtree)**: 특정 노드를 루트로 하는 트리.
* **레벨(Level)**: 루트로부터의 거리 (루트: 0, 그 자식: 1 ...).
* **깊이(Depth)**: 루트에서 해당 노드까지 도달하는 경로 길이.
* **높이(Height)**: 노드로부터 가장 멀리 떨어진 리프 노드까지의 경로 길이.

## 자료구조 구조 및 시각화

```
        [1]
       /   \
     [2]   [3]
    /   \     \
  [4]  [5]    [6]
```

* 루트: 1
* 노드 2와 3은 루트의 자식
* 노드 4, 5, 6은 리프 노드
* 서브트리 예시: 노드 2를 루트로 하는 `[2] → [4], [5]`

## 기본 연산

### 삽입 (Insertion)

트리는 선형 자료구조와 다르게 **삽입 위치가 트리의 종류에 따라 달라진다.**

- **일반 트리**: 자식 리스트에 추가
- **이진 탐색 트리(BST)**: 
  - 왼쪽 서브트리: 현재 노드보다 작은 값
  - 오른쪽 서브트리: 현재 노드보다 큰 값
- **힙(Heap)**: 완전 이진 트리 구조 유지 + 최대/최소 조건 유지

**시간 복잡도**  
- 균형 잡힌 트리: `O(log n)`  
- 비균형 트리: `O(n)`

### 삭제 (Deletion)

1. **리프 노드 삭제**: 단순 제거  
2. **하나의 자식을 가진 노드 삭제**: 자식을 부모 노드에 연결  
3. **두 자식을 가진 노드 삭제**: 중위 후계자 또는 중위 선행자를 찾아 대체

### 탐색 (Search)

- 일반 트리: 순회 방식으로 전체 탐색 필요  
- 이진 탐색 트리(BST): `O(log n)` (균형 유지 시)

### 순회 (Traversal)

1. **전위 순회 (Preorder)**: 루트 → 왼쪽 → 오른쪽  
2. **중위 순회 (Inorder)**: 왼쪽 → 루트 → 오른쪽  
3. **후위 순회 (Postorder)**: 왼쪽 → 오른쪽 → 루트  
4. **레벨 순서 순회 (Level Order)**: BFS 방식으로 층별 접근

## 구현 예시 (Python)

```python
class Node:
    def __init__(self, key):
        self.key = key
        self.left = None
        self.right = None

class BinarySearchTree:
    def __init__(self):
        self.root = None

    def insert(self, key):
        def _insert(node, key):
            if node is None:
                return Node(key)
            if key < node.key:
                node.left = _insert(node.left, key)
            else:
                node.right = _insert(node.right, key)
            return node
        self.root = _insert(self.root, key)

    def inorder(self, node):
        if node:
            self.inorder(node.left)
            print(node.key, end=' ')
            self.inorder(node.right)

    def search(self, key):
        def _search(node, key):
            if node is None or node.key == key:
                return node
            if key < node.key:
                return _search(node.left, key)
            return _search(node.right, key)
        return _search(self.root, key)

# 사용 예시
bst = BinarySearchTree()
for value in [50, 30, 70, 20, 40, 60, 80]:
    bst.insert(value)
bst.inorder(bst.root)  # 출력: 20 30 40 50 60 70 80
```

## 자료구조 비교

### 트리 vs 그래프 vs 연결 리스트

| 항목               | 트리 (Tree)                                                   | 그래프 (Graph)                                                            | 연결 리스트 (Linked List)                                         |
|------------------|---------------------------------------------------------------|------------------------------------------------------------------------|---------------------------------------------------------------|
| **구조 유형**        | 비선형, 계층적 구조                                              | 비선형, 일반적인 노드 간 관계 표현                                             | 선형, 일렬로 연결된 노드들                                          |
| **노드 간 연결**     | 부모 → 자식 (단방향), 루트로부터 확장                                   | 노드 간 자유로운 방향 연결 (단방향, 양방향 가능), 순환 허용                         | 각 노드가 다음 노드 하나만 가리킴                                      |
| **순환(Cycle)**      | 없음 (비순환 구조가 기본 조건)                                      | 존재할 수 있음 (순환 가능, 순환 그래프/비순환 그래프 구분됨)                        | 없음 (단순 선형 구조)                                               |
| **루트 노드**       | 반드시 하나 존재                                                | 없음 (모든 노드가 대등하거나, 시작 노드 임의 지정)                                  | 첫 번째 노드를 헤드(head)로 정함                                       |
| **자식 노드 수**     | 최대 n개 (이진트리는 2개까지)                                       | 제한 없음                                                              | 항상 1개 (단일 연결 리스트 기준)                                     |
| **자료 표현**       | 계층적 관계 표현에 적합                                           | 복잡한 관계나 연결망 표현에 적합                                           | 단순 순차적 데이터 표현에 적합                                        |
| **탐색 방식**       | DFS, BFS, 전위/중위/후위 순회 등 존재                               | DFS, BFS 기반의 탐색 (특수 순회 없음)                                    | 선형 순차 탐색만 가능                                              |
| **사용 예시**       | 조직도, 파일시스템, 트라이, 힙, 컴파일 파스 트리 등                          | 네트워크, 소셜 그래프, 경로 탐색, 추천 시스템 등                              | 큐, 스택, 단순 시퀀스 자료 저장 등                                     |

## 주요 트리 종류

| 트리 종류                               | 설명                                                    |
| ----------------------------------- | ----------------------------------------------------- |
| **이진 트리 (Binary Tree)**             | 각 노드가 최대 두 개의 자식을 가진다                                  |
| **완전 이진 트리 (Complete Binary Tree)** | 마지막 레벨을 제외하고 모두 채우고, 왼쪽부터 채운다                          |
| **포화 이진 트리 (Full Binary Tree)**     | 모든 노드가 0개 또는 2개의 자식을 가진다                               |
| **이진 탐색 트리 (BST)**                  | 왼쪽 서브트리는 작은 값, 오른쪽 서브트리는 큰 값                          |
| **균형 이진 트리 (AVL, Red-Black)**       | 삽입/삭제 시 균형 유지하여 성능 안정화                                |
| **힙 (Heap)**                        | 최대 힙 / 최소 힙으로 나뉘며, 완전 이진 트리 구조로 부모 노드가 항상 자식보다 크거나 작다 |
| **트라이 (Trie)**                      | 문자열 탐색 최적화. 키를 문자 단위로 분할하여 트리 구조화                     |
| **N-진 트리 (N-ary Tree)**             | 하나의 노드가 최대 N개의 자식을 가질 수 있다                            |
| **B트리 / B+트리**                      | 고차원 인덱싱 자료구조. 다수의 자식 가능, 데이터베이스/파일시스템 등에서 사용된다         |


## 참고 자료

* GeeksforGeeks: [Introduction to Tree Data Structure](https://www.geeksforgeeks.org/introduction-to-tree-data-structure/)
* GeeksforGeeks: [Types of Trees in Data Structures](https://www.geeksforgeeks.org/types-of-trees-in-data-structures/)
* GeeksforGeeks: [Applications of Tree Data Structure](https://www.geeksforgeeks.org/applications-of-tree-data-structure/)
