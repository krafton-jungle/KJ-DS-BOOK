## Linked Stack
- 개념: Linked Stack은 연결 리스트를 이용하여 구현된 스택(Stack)이다.
- 핵심 연산:
    - push(value): 새로운 노드를 생성하고, 그 노드의 next를 현재 top으로 설정 -> top을 새 노드로 갱신
    - pop(): 현재 top 노드의 데이터를 반환하고, top을 top.next로 변경
    - peek(): top 노드의 데이터를 확인
    - is_empty(): top이 None이면 스택이 비어 있음
    - display(): 스택의 모든 데이터를 top -> bottom 순으로 출력
- 배열 vs 연결 리스트:
  - 배열 대신 연결 리스트를 사용하는 가장 큰 장점은 필요에 따라 크기를 줄이거나 늘릴 수 있는 스택을 구현할 수 있다는 것이다.
  - 배열을 사용하면 배열의 최대 용량에 제한이 생겨 스택 오버플로우가 발생할 수 있다. 
  - 연결 리스트에서는 각 새 노드가 동적으로 할당되므로 오버플로우가 발생하지 않는다.

| 항목                      | 배열 기반 스택                          | 연결 리스트 기반 스택                      |
|---------------------------|------------------------------------------|---------------------------------------------|
| 메모리 크기               | 고정 (초기 크기 설정 필요)              | 동적 (필요할 때마다 노드 생성)             |
| 크기 조절 가능성          | 제한됨 (리사이징 필요하거나 불가능)     | 자유롭게 늘어나고 줄어들 수 있음          |
| 오버플로우 가능성         | 있음 (배열 한계 도달 시)                | 거의 없음 (메모리 허용 범위 내에서는 자유) |
| 메모리 낭비 가능성        | 배열 크기보다 적게 쓸 경우 낭비 발생 가능| 없음 (필요한 만큼만 사용)                  |


## 코드 예시 (Python)
- 단일 연결 리스트(singly linked list) 기반의 스택
![단일 연결 리스트 기반 스택](linked_stack_img/singly-linked-list-01.png)
![단일 연결 리스트 기반 스택](linked_stack_img/singly-linked-list-02.png)
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class SinglyLinkedStack:
    def __init__(self):
        self.top = None

    def is_empty(self):
        return self.top is None

    def push(self, value):
        new_node = Node(value)
        new_node.next = self.top
        self.top = new_node

    def pop(self):
        if self.is_empty():
            raise IndexError("Stack is empty")
        popped_node = self.top
        self.top = self.top.next
        return popped_node.data

    def peek(self):
        if self.is_empty():
            raise IndexError("Stack is empty")
        return self.top.data

    def display(self):
        current = self.top
        elements = []
        while current:
            elements.append(current.data)
            current = current.next
        print("Stack (top → bottom):", elements)
```

- 이중 연결 리스트(Doubly Linked List) 기반의 스택

![단일 연결 리스트 기반 스택](linked_stack_img/doubly-linked-list.png)
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.prev = None
        self.next = None

class DoublyLinkedStack:
    def __init__(self):
        self.top = None

    def is_empty(self):
        return self.top is None

    def push(self, value):
        new_node = Node(value)
        new_node.prev = None
        new_node.next = self.top
        if self.top is not None:
            self.top.prev = new_node
        self.top = new_node

    def pop(self):
        if self.is_empty():
            raise IndexError("Stack is empty")
        popped_node = self.top
        self.top = self.top.next
        if self.top is not None:
            self.top.prev = None
        return popped_node.data

    def peek(self):
        if self.is_empty():
            raise IndexError("Stack is empty")
        return self.top.data

    def display(self):
        current = self.top
        elements = []
        while current:
            elements.append(current.data)
            current = current.next
        print("Stack (top → bottom):", elements)
```


## 영상첨부설정


## 참고
- 참고한 자료
  - https://www.geeksforgeeks.org/implement-a-stack-using-singly-linked-list/
  - https://www.geeksforgeeks.org/implementation-of-stack-using-doubly-linked-list/
- 이미지 출처
  - https://www.geeksforgeeks.org/implement-a-stack-using-singly-linked-list/
  - https://www.geeksforgeeks.org/implementation-of-stack-using-doubly-linked-list/
- 추가로 참고하면 좋은 자료