# 이진 힙(Binary Heap)

이진 힙(Binary heap)은 이진 트리(binary tree)의 형태를 가지는 힙(heap) 자료 구조로, 일반적으로 우선순위 큐(priority queue)를 구현하는 데 사용된다
이진 힙은 1964년에 J. W. J. Williams에 의해 힙 정렬(heapsort)을 구현하기 위한 자료 구조로 도입되었다.

이진 힙은 다음 두 가지 추가 제약 조건을 만족하는 이진 트리로 정의된다.

- 형태 속성(Shape property): 이진 힙은 완전 이진 트리(complete binary tree)이어야 한다. 즉, 가장 아래 레벨(깊이가 가장 큰 레벨)을 제외한 모든 레벨이 완전히 채워져야 하며, 마지막 레벨의 노드들은 왼쪽에서 오른쪽으로 차례대로 채워져야 한다.
- 힙 속성(Heap property): 노드에 저장된 키(key)는 그 자식 노드들의 키보다 크거나 같거나(≥) 작거나 같아야(≤) 한다. 이 비교는 전순서(total order)에 따라 이루어진다.

부모 키(parent key)가 자식 키(child key)보다 크거나 같은 경우에는 최대 힙(max-heap)이라고 하고, 부모 키가 자식 키보다 작거나 같은 경우에는 최소 힙(min-heap)이라고 한다.

이진 힙에서 우선순위 큐를 구현하기 위해서는 다음 두 가지 연산이 효율적(즉, 로그 시간 복잡도)으로 알려져 있다.

1. 요소 삽입하기
2. 최대 힙에서 가장 큰 요소를 제거하거나 최소 힙에서 가장 작은 요소를 제거하기

이진 힙은 힙 정렬 알고리즘에서도 자주 사용되며, 배열을 사용해 암시적 자료 구조로 구현할 수 있기 때문에 제자리 알고리즘으로 동작할 수 있다. 배열 내에서의 상대적 위치를 이용해 부모–자식 관계를 표현할 수 있기 때문이다.

이진 힙의 구조를 알아보기 전에, 완전 이진 트리에 대해 간략히 설명한다.

## 완전 이진 트리 (Complete Binary Tree)

완전 이진 트리란 이진 트리의 일종으로 트리의 모든 레벨이 완전히 채워져 있으며, 마지막 레벨은 왼쪽부터 채워져 있는 구조를 말한다.

<div align="center">
  <img src="https://media.geeksforgeeks.org/wp-content/uploads/20220414154428/complete-200x132.jpg"/>
  <p style="font-size: 10px;">완전 이진 트리 (Complete binary tree)</p>
</div>

# 힙 연산 (Heap Operation)

삽입(insert) 및 제거(remove) 연산은 먼저 형태 속성(shape property)을 보존(preserve)하기 위해 힙의 끝(end)에서 요소를 추가하거나 제거한다.

그 다음 힙 속성(heap property)을 복원하기 위해 위 또는 아래로 이동하면서 재구성(reheapify)을 수행한다.

두 연산 모두 시간 복잡도는 $O(\log n)$이다.

> 아래부터 설명할 힙 연산들은 모두 최대 힙 구조를 가정하고 진행한다.

## 삽입(Insert)

요소를 힙에 삽입하기 위한 단계는 다음과 같다.

1. 힙의 맨 아래 레벨의 왼쪽에서 가장 먼저 비어 있는 공간에 요소를 추가한다.
2. 추가된 요소와 그 부모를 비교하여, 이미 올바른 순서라면 종료한다.
3. 그렇지 않으면, 요소와 부모를 교환하고 2단계로 돌아가 재검사한다.

> 2단계와 3단계는 힙 속성을 복원하기 위해 노드를 부모 노드와 비교하고, 필요하다면 교환하는 과정이며, 이 과정을 업힙(up-heap) 연산이라고 부른다.

> 다음과 같이 불리기도 한다.
>
> - 업-힙 오퍼레이션(up-heap operation)
> - 버블업(bubble-up)
> - 퍼콜레이트업(percolate-up)
> - 시프트업(sift-up)
> - 트리클업(trickle-up)
> - 스윔업(swim-up)

새 요소가 힙 속성을 만족하기 위해 위로 이동해야 하는 레벨의 수에 의해서만 연산 횟수가 결정된다. 따라서 삽입 연산의 최악 시간 복잡도는 $O(\log n)$이다.

무작위 힙(random heap)이나 반복적인 삽입 시나리오에서는 평균 시간 복잡도가 $O(1)$로 나타난다.

### 삽입 과정 예시

이진 힙의 삽입 과정의 예시로, 최대 힙에서 삽입 과정이 이루어진다고 가정한다.

다음과 같은 최대 힙 구조가 있다고 하자.

<div align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/ac/Heap_add_step1.svg/300px-Heap_add_step1.svg.png" width="200"/>
</div>

15를 X로 표시된 위치에 추가하고 싶다고 하면, 다음과 같은 순서를 따라 진행된다.

1. 우선 숫자 15를 X 표시된 위치에 삽입한다.

2. 그러나 힙 속성에 위배되기 때문에 15와 그 부모 노드 8을 교환한다.

<div align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/16/Heap_add_step2.svg/300px-Heap_add_step2.svg.png" width="200"/>
</div>

3. 아직 힙 속성에 위배되기 때문에 15와 그 부모 노드 11을 교환한다.

<div align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/51/Heap_add_step3.svg/300px-Heap_add_step3.svg.png" width="200"/>
</div>

4. 이 시점에서 힙 속성이 만족된다.

## 추출(Extract)

최대 힙에서 최대 요소나 최소 힙에서 최소 요소를 추출하여 힙 속성을 유지하는 절차는 다음과 같다.

1. 힙의 루트를 힙의 마지막 레벨의 마지막 요소로 대체한다.
2. 새 루트와 그 자식 노드를 비교하여, 이미 올바른 순서라면 종료한다.
3. 올바르지 않다면, 루트와 자식 노드 중 힙 속성을 만족하도록 적절한 자식(최소 힙에서는 더 작은 자식, 최대 힙에서는 더 큰 자식)과 교환(swap)하고 2단계로 돌아가 다시 검사한다.

> 단계와 3단계는 힙 속성을 복원하기 위해 노드를 자식 노드 중 하나와 비교하고, 필요하다면 교환하는 과정이며, 이 과정을 다운힙(down-heap) 연산이라고 부른다.

> 다운 힙 연산은 다음과 같이 불리기도 한다.
>
> - 버블다운(bubble-down)
> - 퍼콜레이트다운(percolate-down)
> - 시프트다운(sift-down)
> - 싱크다운(sink-down)
> - 트리클다운(trickle-down)
> - 힙화(heapify-down)
> - 캐스케이드다운(cascade-down)
> - 익스트랙트-민/맥스(extract-min/extract-max)

### 추출 과정 예시

> 이진 힙에서의 max 값 추출 과정의 예시로, 최대 힙에서 이루어진다고 가정한다.

다음과 같은 최대 힙 구조가 있다고 가정하자.

<div align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/1c/Heap_delete_step0.svg/300px-Heap_delete_step0.svg.png" width="200"/>
</div>

11을 없애고 4로 교체한다.

<div align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/ee/Heap_remove_step1.svg/300px-Heap_remove_step1.svg.png" width="200"/>
</div>

이제 8이 4보다 크기 때문에 힙 속성이 위배되었다. 이 경우, 두 요소인 4와 8을 교환하는 것만으로 힙 속성이 복원되며, 더 이상 요소를 교환할 필요가 없다.

<div align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/22/Heap_remove_step2.svg/300px-Heap_remove_step2.svg.png" width="200"/>
</div>

> 최대 힙에서는 내려가는 노드가 자식들 중 더 큰 노드와 교환된다. (최소 힙에서는 더 작은 자식 노드와 교환된다). 이 과정은 해당 노드가 새로운 위치에서 힙 속성을 만족할 때까지 반복된다.

## 삽입 후 추출(Insert then extract)

힙에 요소를 삽입한 뒤 추출하는 작업은, 단순히 위에서 정의된 insert와 extract 함수를 순서대로 호출하는 것보다 더 효율적으로 수행할 수 있다.

기존 방식은 업힙(upheap)과 다운힙(downheap) 연산을 모두 필요로 하지만, 다음과 같은 방식으로 다운힙 연산만 수행하도록 최적화할 수 있다.

1. (최대 힙을 가정할 때) 삽입하려는 요소와 힙의 루트 요소(가장 위 요소)를 비교합니다.
2. 만약 힙의 루트가 더 크다면 루트를 새 요소로 교체하고, 그렇지 않다면 루트부터 다운힙 연산을 수행한다.
3. 그렇지 않다면, 삽입하려던 요소를 그대로 반환한다.

> Python에서는 이러한 기능을 제공하는 함수가 **heappushpop**으로 존재한다.

## 탐색(Search)

임의의 요소를 찾는 데에는 O(n) 시간이 걸린다.

## 키 감소(Decrease key) / 키 증가(Increase key) 연산

키 감소(Decrease key) 연산은 특정 노드의 값을 더 작은 값으로 바꾸는 작업이며,
키 증가(Increase key) 연산은 특정 노드의 값을 더 큰 값으로 바꾸는 작업이다.

이 연산들은 다음과 같은 절차로 이루어진다.

- 주어진 값을 가진 노드를 찾고,
- 값을 변경한 다음,
- 힙 속성을 복원하기 위해 다운힙(down-heapify) 또는 업힙(up-heapify) 연산을 수행한다.

> (최대 힙 기준)

키 감소(Decrease key)는 다음과 같이 수행된다

1. 수정하고자 하는 요소의 인덱스를 찾습니다.
2. 해당 노드의 값을 더 작은 값으로 변경합니다.
3. 다운힙 연산을 통해 힙 속성을 복원합니다.

> (최대 힙 기준)

키 증가(Increase key)는 다음과 같이 수행된다.

1. 수정하고자 하는 요소의 인덱스를 찾는다.
2. 해당 노드의 값을 더 큰 값으로 변경한다.
3. 업힙 연산을 통해 힙 속성을 복원한다.

# 이진 힙의 응용 분야

## 우선 순위 큐 (Priority Queue)

## 힙 정렬 (Heap Sort)

# Reference

- https://en.wikipedia.org/wiki/Binary_heap
- https://www.geeksforgeeks.org/binary-heap/
