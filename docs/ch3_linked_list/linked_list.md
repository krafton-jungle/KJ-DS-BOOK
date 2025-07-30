# 연결 리스트(Linked List)

연결 리스트는 노드(Node)라 불리는 원소들이 포인터(Pointer)를 통해 선형으로 연결된 자료구조이다. 각 노드는 데이터를 저장하는 필드와 다음 노드를 가리키는 포인터 필드로 구성된다.

배열(Array)과 달리 메모리 상에 연속적으로 배치될 필요가 없으며, 동적으로 크기를 조절할 수 있다는 특징이 있다. 이는 삽입과 삭제가 빈번한 경우 성능상 유리한 구조이다.

- **C 언어**에서는 구조체(struct)를 이용하여 각 노드를 정의하고, 포인터를 통해 연결한다.
- **Java**에서는 포인터의 개념 대신 참조(Reference)를 사용하여 노드를 연결한다.

---

## 종류

연결 리스트는 포인터의 구성 방식에 따라 다음과 같이 나뉜다.

### 단일 연결 리스트(Singly Linked List)

각 노드는 다음 노드를 가리키는 `next` 포인터만을 가진다. 순방향으로만 탐색이 가능하다. 구조가 단순하여 메모리 사용량이 적으나, 이전 노드에 대한 접근은 불가능하다.

### 이중 연결 리스트(Doubly Linked List)

각 노드는 앞 노드를 가리키는 `prev` 포인터와 다음 노드를 가리키는 `next` 포인터를 모두 가진다. 양방향 탐색이 가능하며, 특정 노드의 삭제 시 이전 노드를 참조할 수 있어 효율적이다. 다만 포인터가 두 개이므로 메모리 사용량이 증가한다.

### 원형 연결 리스트(Circular Linked List)

단일 또는 이중 연결 리스트에서 마지막 노드의 `next`가 다시 첫 번째 노드인 `head`를 가리키도록 구성한 순환 구조이다. 리스트의 끝과 처음이 연결되어 있어 순환 처리가 필요한 경우에 유용하다.

---

## 기본 연산

연결 리스트에서의 주요 연산과 그 시간 복잡도는 다음과 같다.

### 검색(Search)

- 임의의 인덱스 i번째 원소에 직접 접근할 수 없다.
- 첫 노드부터 순차적으로 순회해야 하므로 시간 복잡도는 O(n)이다.

### 삽입(Insert)

- **맨 앞 삽입**: 새 노드의 `next`를 기존 `head`로 설정한 뒤, `head`를 새 노드로 갱신하면 된다. 시간 복잡도는 O(1)이다.
- **맨 뒤 삽입**:
  - `tail` 포인터가 있다면 O(1) 시간에 가능하다.
  - 없다면 리스트를 끝까지 순회해야 하므로 O(n)이다.
- **중간 삽입**: 삽입할 위치의 직전 노드까지 순회한 뒤 포인터를 재구성한다. 시간 복잡도는 O(n)이다.

### 삭제(Delete)

- 삭제 방식은 삽입과 유사하다.
- **맨 앞 삭제**: `head`를 다음 노드로 갱신하면 된다. O(1)
- **중간/끝 삭제**: 삭제할 노드의 직전 노드를 알아야 하므로 순회가 필요하다. 최악의 경우 O(n)이다.

---

## 특징 및 장단점

### 장점

- **삽입과 삭제에 유리**: 배열에서는 데이터를 삽입하거나 삭제할 경우, 해당 인덱스 이후의 모든 요소를 이동시켜야 한다. 반면 연결 리스트는 포인터 조작만으로 삽입과 삭제가 가능하다.
- **동적 크기 조절 가능**: 메모리 할당이 동적으로 이루어지므로, 배열처럼 초기 크기를 고정할 필요가 없다.
- **스택(Stack)과 큐(Queue)** 등의 자료구조의 내부 구현에 자주 사용된다.

### 단점

- **임의 접근이 불가능**: 배열과 달리 인덱스를 기반으로 한 직접 접근이 불가능하며, 원하는 위치까지 선형 탐색이 필요하다.
- **포인터 저장 공간 필요**: 각 노드가 포인터를 하나 이상 저장하므로 메모리 오버헤드가 존재한다.
- **메모리 비연속성**: 노드들이 메모리 상에서 비연속적으로 존재하므로 CPU 캐시 적중률(Cache Hit Rate)이 낮아져 성능이 저하될 수 있다.

---

## 요약

| 연산 | 배열(Array) | 연결 리스트(Linked List) |
|------|--------------|---------------------------|
| 임의 접근 | O(1) | O(n) |
| 삽입/삭제 (중간) | O(n) | O(n) |
| 삽입/삭제 (앞) | O(n) | O(1) |
| 메모리 효율 | 연속 | 비연속 (포인터 필요) |

---

## 활용 사례

- 메모리 크기가 자주 변하는 상황에서의 **동적 메모리 관리**
- **스택**, **큐**, **해시 테이블의 체이닝(Chaining)** 구현
- **그래프(Graph)** 에서 인접 리스트 표현 방식
- 음악 재생 목록, Undo/Redo 기능처럼 이전 또는 다음 항목으로의 이동이 필요한 프로그램 구조

---
## 코드예시
아래는 Python 언어로 작성한 예시입니다.
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None
      
class SinglyLinkedList:
    def __init__(self):
        self.head = None

    def append(self, data):
        new_node = Node(data)
        if not self.head:
            self.head = new_node
            return
        curr = self.head
        while curr.next:
            curr = curr.next
        curr.next = new_node

    def prepend(self, data):
        new_node = Node(data)
        new_node.next = self.head
        self.head = new_node

    def delete(self, key):
        curr = self.head
        prev = None
        while curr and curr.data != key:
            prev = curr
            curr = curr.next
        if not curr: # 못찾음
            return
        if prev:
            # 이전 노드와 다음 노드를 연결하면 현재 노드는 연결이 끊김 (삭제)
            prev.next = curr.next 
        else:
            self.head = curr.next # 삭제할 노드가 헤드 일 경우 while문 돌지않음

    def find(self, key):
        curr = self.head
        while curr:
            if curr.data == key:
                return curr
            curr = curr.next
        return None
```