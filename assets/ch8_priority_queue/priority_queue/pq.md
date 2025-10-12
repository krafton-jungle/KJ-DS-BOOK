
## 1\. 우선순위 큐란?

-   **정의**: 각 원소가 우선순위를 가지며, 우선순위가 높은 원소부터 먼저 처리되는 추상 자료형
-   **일반 큐 vs 우선순위 큐**: FIFO → 우선순위 기반 처리
-   **실생활 비유**: 응급실 환자 대기, VIP 고객 서비스, 운영체제 프로세스 스케줄링

## 2\. 우선순위 큐의 특징

-   **핵심 연산**
    -   `insert(element, priority)`: 원소를 우선순위와 함께 삽입
    -   `extractMax/Min()`: 최고/최저 우선순위 원소 제거 후 반환
    -   `peek()`: 최고 우선순위 원소 조회 (제거하지 않음)
    -   `isEmpty()`: 비어있는지 확인
-   **시간복잡도 비교**
    -   배열 구현: 삽입 O(1), 추출 O(n)
    -   연결리스트 구현: 삽입 O(n), 추출 O(1)
    -   **힙 구현**: 삽입 O(log n), 추출 O(log n)
    -   사실 최대 최소값이 아니라 우선순위 기반이기 때문에 역할은 다르다.

## 3\. 내부 동작방식 (힙 기반)

### 3.1 힙(Heap)의 개념

-   **완전이진트리** 구조
-   **힙 속성**: 부모 ≥ 자식 (Max Heap) 또는 부모 ≤ 자식 (Min Heap)
-   **배열 표현**: 인덱스 i의 자식은 2i+1, 2i+2

### 3.2 삽입(Insert) 과정

1.  트리의 마지막 위치에 새 원소 추가
2.  **상향 조정(Heapify Up)**: 부모와 비교하며 힙 속성 복구
3.  시간복잡도: O(log n)

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2F7jvGY%2FbtsPHeOiWRQ%2FAAAAAAAAAAAAAAAAAAAAAGFRxYITfs-QI9TDQwitq5SZFbi1RWlm01z1kQSAtWLp%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1756652399%26allow_ip%3D%26allow_referer%3D%26signature%3DfAJ6KNPAjxOd2716iuO%252Fy5T21oQ%253D">

#### 삽입의 원리

사실 우선순위 큐와 완전이진힙은 빈 노드 없이 좌측부터 채우기때문에  
생각하는 개념과 달리 배열로 구현된다.

그 이유는 본인의 부모노드가 항상 `(인덱스-1) // 2` 이기 때문이고  
반대로 자식노드는 `인덱스*2 +1와 인덱스*2 +2` 이기 때문이다.

따라서 삽입과정 추출과정에서 최대, 최소를 구하는 과정은  
위 인덱스 공식에 따라서 가능한 범위까지 교환이 이루어지는 방식으로 진행된다.

### 3.3 추출(Extract) 과정

1.  루트 노드(최고 우선순위) 제거
2.  마지막 노드를 루트로 이동
3.  **하향 조정(Heapify Down)**: 자식과 비교하며 힙 속성 복구
4.  시간복잡도: O(log n)

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FMGXCM%2FbtsPIJte9Oy%2FAAAAAAAAAAAAAAAAAAAAAMo9jTglW7MgTZhtY9Vo5nt461jKXuRV1hjbHqUYoTuz%2Fimg.webp%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1756652399%26allow_ip%3D%26allow_referer%3D%26signature%3DNRc9WjOFECtt4NjIoC0p7zeAono%253D">

## 4\. 구현 방법 비교

| 구현 방식 | 삽입 | 추출 | 공간복잡도 | 특징 |
| --- | --- | --- | --- | --- |
| 배열 | O(1) | O(n) | O(n) | 구현 간단, 추출 느림 |
| 정렬된 배열 | O(n) | O(1) | O(n) | 삽입 느림, 추출 빠름 |
| 연결리스트 | O(n) | O(1) | O(n) | 동적 크기 |
| **이진힙** | **O(log n)** | **O(log n)** | **O(n)** | **균형잡힌 성능** |
| 피보나치힙 | O(1) | O(log n) | O(n) | 고급 구현 |

## 5\. 코드 구현 예제 (Python)

```python
class PriorityQueue:

    def __init__(self):
        self.values = []

    def enqueue(self, value, priority): # 데이터 삽입
        new_node = PQNode(value, priority)
        self.values.append(new_node)
        self.bubble_up(new_node) # 새로운 노드를 삽입 후 위치 조정

    def bubble_up(self, node):
        idx = len(self.values) - 1
        cur_node = self.values[idx]  # 현재 추가된 노드
        while idx > 0:
            parent_idx = (idx - 1) // 2
            parent_node = self.values[parent_idx]
            if (parent_node.priority <= cur_node.priority):  
            # 만약 현재 노드가 우선순위가 높다면
            # 우선 순위가 낮을수록 먼저처리
                break

            self.values[parent_idx] = cur_node
            self.values[idx] = parent_node
            idx = parent_idx
            # 자리를 바꾼 후 인덱스는 이전 부모인덱스로 대치

    def dequeue(self): # 루트 노드 추출
        priority_node = self.values[0]
        end_node = self.values.pop()
        if len(self.values) > 0:
            self.values[0] = end_node
            self.sinkdown() # 마지막 노드를 상단으로 올려 히피다운

        return priority_node

    def sinkdown(self):
        idx = 0
        length = len(self.values)
        element = self.values[0]

        while True:
            left_child_idx = idx * 2 + 1
            right_child_idx = idx * 2 + 2
            left_child = None
            right_child = None

            swap = None
            if left_child_idx < length: # 좌측노드가 존재하고 우선순위 비교
                left_child = self.values[left_child_idx]
                if left_child.priority < element.priority:
                    swap = left_child_idx
            if right_child_idx < length:
            # 좌측노드가 존재하고 우선순위 비교 + 요소, 좌측과 비교 후 대치
            # 우측노드가 우선이 되야한다.(배열의 마지막)
                right_child = self.values[right_child_idx]
                if (swap == None and right_child.priority < element.priority) or (
                    swap != None and right_child.priority < left_child.priority
                ):
                    swap = right_child_idx
            if swap == None:
                break
            self.values[idx] = self.values[swap]
            self.values[swap] = element
            idx = swap


class PQNode:

    def __init__self, value, priority):
        self.val = value
        self.priority = priority
```

## 주의사항과 한계

-   **동일 우선순위**: 삽입 순서 보장 안됨
-   **동적 우선순위**: 삽입 후 우선순위 변경 어려움
-   **메모리 오버헤드**: 포인터 및 힙 구조 유지 비용
-   **캐시 지역성**: 배열 대비 메모리 접근 패턴 불리

### 동일 우선순위

**카운터를 이용한 순서 보장**  
추가 메모리를 써야하고 로직이 복잡해진다.

## 출처

[나무위키-힙트리](https://namu.wiki/w/%ED%9E%99%20%ED%8A%B8%EB%A6%AC)