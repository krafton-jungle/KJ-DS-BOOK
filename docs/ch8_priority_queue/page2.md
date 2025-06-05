# Min-Heap (최소 힙)

이 장에서는 Min-Heap 자료구조의 개념, 구현 방법, 그리고 실제 적용 예제를 중심으로 학습한다.

> Min-Heap은 우선순위 큐의 핵심 구현체로, 항상 최솟값을 빠르게 찾을 수 있는 효율적인 자료구조이다.

## 📋 목차

| 항목 | 설명 |
|------|------|
| 핵심 키워드 정리 | Min-Heap을 이해하는데 필요한 개념 |
| Min-Heap이란 | 완전 이진 트리와 힙 속성의 정의 |
| 알고리즘 실행 과정 | 삽입/삭제 과정을 시각화 자료와 함께 보여준다 |
| 구현 예시 (Python) | 파이썬으로 구현한 Min-Heap 코드 |
| 참고 | 자료 출처 및 더 찾아보기 |

> Min-Heap의 핵심 연산인 삽입, 삭제, 힙화 과정을 단계별로 이해할 수 있도록 구성하였다.

## 핵심 키워드 정리

* **정의**: 부모 노드의 값이 항상 자식 노드의 값보다 작거나 같은 완전 이진 트리
* **자료구조**: 완전 이진 트리 (Complete Binary Tree)
* **시간복잡도**: 삽입/삭제 `O(log n)`, 최솟값 조회 `O(1)`
* **공간복잡도**: `O(n)`
* **동작 방식 요약**:
  * **입력**: 정수 또는 비교 가능한 원소들
  * **처리**: 힙 속성을 유지하며 완전 이진 트리에 저장
  * **출력**: 최솟값 (루트 노드)

## Min-Heap이란

* Min-Heap은 **완전 이진 트리**의 형태를 가지며, **힙 속성**을 만족하는 자료구조이다.
* **힙 속성**: `부모 노드 ≤ 자식 노드` (Min-Heap 기준)
* **완전 이진 트리**: 마지막 레벨을 제외하고 모든 레벨이 완전히 채워진 트리
* **배열 표현**: 인덱스 관계를 이용해 효율적으로 저장
  * 부모 인덱스: `(i-1) // 2`
  * 왼쪽 자식: `2*i + 1`
  * 오른쪽 자식: `2*i + 2`

### 어떤 문제 유형에 자주 등장하는가?
* 우선순위 큐: 항상 최솟값을 빠르게 접근
* 힙 정렬: O(n log n) 시간에 정렬
* 다익스트라 알고리즘: 최단 경로 탐색
* K번째 작은/큰 원소 찾기

## 알고리즘 실행 과정

### 1. Min-Heap 구조 예시

```
초기 상태:
    1
   / \
  3   2
 / \ / \
4  7 5  6

배열 표현: [1, 3, 2, 4, 7, 5, 6]
```

### 2. 삽입(Insert) 과정 - 값 0 삽입

**1단계: 마지막 위치에 삽입**
``` 
      1
    /   \
   3     2
  / \   / \
 4   7 5   6
/
0
```
배열: `[1, 3, 2, 4, 7, 5, 6, 0]`

**2단계: Bubble Up (상향 조정)**
```
# 0과 부모 4 비교 → 교환
      1
    /   \
   3     2
  / \   / \
 0   7 5   6
/
4
```

**3단계: 계속 Bubble Up**
```
# 0과 부모 3 비교 → 교환
      1
    /   \
   0     2
  / \   / \
 3   7 5   6
/
4
```

**4단계: 마지막 Bubble Up**
```
# 0과 부모 1 비교 → 교환
      0
    /   \
   1     2
  / \   / \
 3   7 5   6
/
4
```

**최종 결과**: `[0, 1, 2, 3, 7, 5, 6, 4]`

### 3. 삭제(Delete Min) 과정 - 최솟값 0 삭제

**삭제 전 상태**:
```
      0
    /   \
   1     2
  / \   / \
 3   7 5   6
/
4
```
배열: `[0, 1, 2, 3, 7, 5, 6, 4]`

**1단계: 루트(0)를 제거하고 마지막 노드(4)를 루트로 이동**
```
      4
    /   \
   1     2
  / \   / \
 3   7 5   6
```
배열: `[4, 1, 2, 3, 7, 5, 6]`

**2단계: Bubble Down (하향 조정)**
```
# 4와 자식들(1, 2) 비교 → 더 작은 1과 교환
      1
    /   \
   4     2
  / \   / \
 3   7 5   6
```

**3단계: 계속 Bubble Down**
```
# 4와 자식들(3, 7) 비교 → 더 작은 3과 교환
      1
    /   \
   3     2
  / \   / \
 4   7 5   6
```

**최종 결과**: `[1, 3, 2, 4, 7, 5, 6]`

## 구현 예시 (Python)

### 기본 Min-Heap 사용법 (heapq 모듈)

```python
import heapq

# 빈 힙 생성
heap = []

# 원소 삽입
numbers = [4, 1, 3, 2, 16, 9, 10]
for num in numbers:
    heapq.heappush(heap, num)
    print(f"삽입 {num}: {heap}")

# 최솟값 제거
while heap:
    min_val = heapq.heappop(heap)
    print(f"삭제 {min_val}: {heap}")
```

### 배열을 힙으로 변환 (Heapify)

```python
import heapq

# 기존 배열을 힙으로 변환
arr = [4, 1, 3, 2, 16, 9, 10]
print(f"원본 배열: {arr}")

heapq.heapify(arr)  # O(n) 시간에 힙으로 변환
print(f"힙으로 변환: {arr}")

# 최솟값들을 순서대로 추출
result = []
while arr:
    result.append(heapq.heappop(arr))

print(f"정렬된 결과: {result}")
```

### K번째 작은/큰 원소 찾기

```python
import heapq

def find_k_smallest(arr, k):
    """K개의 가장 작은 원소 찾기"""
    return heapq.nsmallest(k, arr)

def find_k_largest(arr, k):
    """K개의 가장 큰 원소 찾기"""
    return heapq.nlargest(k, arr)

# 사용 예시
numbers = [7, 10, 4, 3, 20, 15, 8]
print(f"배열: {numbers}")
print(f"가장 작은 3개: {find_k_smallest(numbers, 3)}")
print(f"가장 큰 3개: {find_k_largest(numbers, 3)}")
```

## 📖 참고

### 📚 참고한 자료
* **『Introduction to Algorithms』** - Thomas H. Cormen
  * Chapter 6: Heapsort


### 🌐 추가로 참고하면 좋은 자료
* **온라인 시각화 도구**
  * [VisuAlgo - Binary Heap](https://visualgo.net/en/heap)
* **문제 해결 사이트**
  * [GeeksforGeeks - Heap](https://www.geeksforgeeks.org/heap-data-structure/)