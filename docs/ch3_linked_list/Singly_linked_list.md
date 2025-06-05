# 단일 연결 리스트 (Singly Linked List)

> 단일 연결 리스트(Singly Linked List)는 각 노드가 데이터를 저장하고, 다음 노드의 주소를 가리키는 포인터로 연결된 선형 자료구조입니다. 마지막 노드는 다음 포인터가 `None`으로 설정되어 리스트의 끝을 나타낸다.

## 목차

| 항목         | 설명                           |
|--------------|-------------------------------|
| [핵심 키워드 정리](#핵심-키워드-정리)   | 알고리즘 개념, 키워드 정리 |
| [자료구조 구조 및 시각화](#자료구조-구조-및-시각화) | 알고리즘을 이해하는데 필요한 개념 |
| [기본 연산](#기본-연산) | 알고리즘을 시각화 자료와 함께 보여준다 |
| [구현 예시 (Python)](#구현-예시-python) | 파이썬으로 구현한 알고리즘 예시 코드 |
| [참고 자료](#참고-자료) | 자료 출처 및 더 찾아보기 |

## 핵심 키워드 정리

* **노드(Node)**: 데이터를 담고 다음 노드를 가리키는 포인터를 가진 기본 단위
* **헤드(Head)**: 리스트의 첫 번째 노드를 참조하는 포인터
* **테일(Tail)**: 리스트의 마지막 노드 (다음 포인터가 `None`인 노드)

## 자료구조 구조 및 시각화

```
Head → [Data|Next] → [Data|Next] → [Data|None]
```

예시:

```
Head → [10|●] → [20|●] → [30|None]
```

## 기본 연산

### 삽입 (Insertion)

#### 맨 앞에 삽입

* 시간복잡도: `O(1)`
* 과정:
  1. 새 노드 생성
  2. 새 노드의 `next`를 현재 헤드로 연결한다 
  3. 헤드를 새 노드로 변경한다 

#### 맨 뒤에 삽입

* 시간복잡도: `O(n)`
* 과정:
  1. 마지막 노드까지 순회한다 
  2. 마지막 노드의 `next`를 새 노드로 연결한다 

#### 중간에 삽입

* 시간복잡도: `O(n)`
* 과정:
  1. 삽입할 위치의 바로 이전 노드까지 순회한다 
  2. 새 노드의 `next`를 기존 다음 노드로 설정한다 
  3. 이전 노드의 `next`를 새 노드로 변경한다 

### 삭제 (Deletion)

#### 맨 앞 노드 삭제

* 시간복잡도: `O(1)`
* 과정:
  1. 헤드를 다음 노드로 변경한다 

#### 특정 값 노드 삭제

* 시간복잡도: `O(n)`
* 과정:
  1. 삭제할 노드의 이전 노드까지 순회한다 
  2. 이전 노드의 `next`를 삭제할 노드의 다음 노드로 연결한다 

### 탐색 (Search)

* 시간복잡도: `O(n)`
* 과정:
  1. 헤드부터 차례로 탐색하여 원하는 데이터를 가진 노드를 찾는다 

## 장단점

### 장점

* 유연한 크기 조정: 필요에 따라 동적으로 크기 조정 가능하다 
* 삽입 및 삭제가 효율적: 맨 앞 연산의 경우 특히 효율적이다 

### 단점

* 임의 접근(random access) 불가능: 특정 위치 접근이 느리다 
* 메모리 사용 증가: 포인터 저장을 위한 추가 메모리 필요하다 

## 배열(Array)과 비교

| 특징        | 배열(Array)   | 단일 연결 리스트   |
| --------- | ----------- | ----------- |
| 메모리 할당 방식 | 연속적         | 비연속적        |
| 크기 조정     | 어렵다         | 쉽다          |
| 임의 접근 속도  | 빠름 (`O(1)`) | 느림 (`O(n)`) |
| 삽입/삭제 효율성 | 낮음          | 높음          |

## 배열 vs 단일 연결 리스트 (GeeksforGeeks 참고)

| 항목               | 배열 (Array)                                                                 | 단일 연결 리스트 (Singly Linked List)                                                                 |
|------------------|------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **메모리 할당 방식** | 연속적(Contiguous): 요소들이 메모리상에 연속적으로 저장된다                           | 비연속적(Non-contiguous): 각 노드가 메모리상 임의의 위치에 저장되며, 포인터로 연결된다                       |
| **메모리 할당 시점** | 컴파일 타임 또는 초기화 시점에 전체 크기 지정 필요하다                                  | 런타임에 동적으로 필요할 때마다 노드를 생성하여 크기 조절 가능하다                                            |
| **임의 접근(Random Access)** | O(1): 인덱스를 통해 즉시 접근 가능하다                                             | O(n): 원하는 위치까지 순차적으로 탐색해야 한다                                                           |
| **삽입/삭제 효율성** | 중간 삽입/삭제 시 O(n): 요소들을 이동시켜야 한다                                      | 중간 삽입/삭제 시 O(1): 포인터만 조정하면 되므로 효율적이다                                                  |
| **캐시 친화성(Cache Friendliness)** | 높음: 연속된 메모리로 인해 캐시 효율성이 높다                                   | 낮음: 비연속적인 메모리 접근으로 인해 캐시 효율성이 낮다                                                  |
| **메모리 오버헤드** | 낮음: 데이터만 저장하면 된다                                                       | 높음: 각 노드마다 포인터를 추가로 저장해야 한다                                                           |
| **사용 용이성**     | 간단: 대부분의 언어에서 기본적으로 지원하며 사용이 쉽다                               | 복잡: 포인터 관리가 필요하며 구현이 복잡할 수 있다                                                      |
| **특징 요약**       | 빠른 접근 속도와 높은 캐시 효율성을 제공하지만, 크기 조절이 어렵고 삽입/삭제가 비효율적이다  | 크기 조절이 유연하고 삽입/삭제가 효율적이지만, 접근 속도가 느리고 메모리 오버헤드가 있다                    |

## 구현 예시 (Python)

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class SinglyLinkedList:
    def __init__(self):
        self.head = None

    def insert_front(self, data):
        new_node = Node(data)
        new_node.next = self.head
        self.head = new_node

    def insert_end(self, data):
        new_node = Node(data)
        if not self.head:
            self.head = new_node
            return
        current = self.head
        while current.next:
            current = current.next
        current.next = new_node

    def delete_value(self, key):
        current = self.head
        prev = None
        while current:
            if current.data == key:
                if prev:
                    prev.next = current.next
                else:
                    self.head = current.next
                return True
            prev = current
            current = current.next
        return False

    def search(self, key):
        current = self.head
        while current:
            if current.data == key:
                return True
            current = current.next
        return False

    def display(self):
        elements = []
        current = self.head
        while current:
            elements.append(str(current.data))
            current = current.next
        print(' → '.join(elements) + ' → None')

# 사용 예시
lst = SinglyLinkedList()
lst.insert_front(30)
lst.insert_front(20)
lst.insert_front(10)
lst.display()  # 출력: 10 → 20 → 30 → None
lst.delete_value(20)
lst.display()  # 출력: 10 → 30 → None
```

## 참고 자료

* GeeksforGeeks: [Singly Linked List – Tutorial](https://www.geeksforgeeks.org/singly-linked-list-tutorial/)
* GeeksforGeeks: [What is Linked List?](https://www.geeksforgeeks.org/what-is-linked-list/)
* GeeksforGeeks: [Linked List vs Array](https://www.geeksforgeeks.org/linked-list-vs-array/)
