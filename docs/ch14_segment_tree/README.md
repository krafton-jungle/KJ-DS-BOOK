# 세그먼트 트리

## 정의
세그먼트 트리(Segment Tree)는 배열 간격에 대한 정보를 이진 트리에 저장하는 자료구조이다.



예를 들어, 배열 [1,2,3,4,5]를 생각해보자.



## 구현
### Build
세그먼트 트리를 만드는 작업을 한다. 재귀를 사용하여 구축할 수 있다.

트리를 순회하며 작업을 진행한다. 리프 노드라면 배열 A의 요소를 저장하고, 내부 노드라면 구간 정보를 저장한다. 예를 들어, 구간 정보가 구간 합이라면 두 자식 노드의 합을 저장한다.

```python
def build(node, start, end):
	if start == end:
		# 리프 노드라면 원소를 저장한다.
		tree[node] = A[start]
		return
	
	mid = (start + end) // 2
	# 왼쪽 자식으로 재귀
	build(node*2, start, mid)
	# 오른쪽 자식으로 재귀
	build(node*2+1, mid+1, end)
	# 내부 노드라면 두 자식 노드의 합을 저장한다.
	tree[node] = tree[node*2] + tree[node*2+1]
	return
```

### Query
세그먼트 트리에서 구간 정보를 가져온다.

트리를 순회하며,

* 노드가 나타내는 범위가 지정된 범위 밖에 있다면 0을 반환한다.
* 노드가 나타내는 범위가 지정된 범위 내에 있다면 값을 반환한다.
* 노드가 나타내는 범위가 지정된 범위 일부만 포함한다면 왼쪽 자식과 오른쪽 자식의 합을 반환한다.

```python
def query(node, start, end, left, right):
	if right < start or end < left:
		# 노드가 지정된 범위 밖에 있는 경우
		return 0

	if left <= start and end <= right:
		# 노드가 지정된 범위 안에 있는 경우
		return tree[node]

	# 노드가 지정된 범위 일부에 있는 경우
	mid = (start + end)//2
	left_child = query(node*2, start, mid, left, right)
	right_child = query(node*2+1, mid+1, end, left, right)
	
	return left_child + right_child
```

### Update
배열 A의 index번째 값을 var 값으로 변경한다. (A[index] = var)

트리를 순회하면서,

* index가 포함된 구간을 가지고 있는 자식 노드로 재귀한다.
* 리프 노드라면 배열 값을 변경한다.
* 내부 노드라면 구간 정보를 저장한다.

```python
def update(node, start, end, index, val):
	if start == end:
		# 리프 노드라면 배열 값을 변경한다.
		tree[node] = val
		return
	
	mid = (start + end) // 2
	# index가 포함된 구간을 가진 자식 노드로 재귀한다.
	if start <= index and index <= mid:
		update(2*node, start, mid, index, val)
	else:
		updaet(2*node+1, mid+1, end, index, val)
	# 내부 노드라면 두 자식 노드의 합을 저장한다.
	tree[node] = tree[node*2] + tree[node*2+1]
	return
```

또는 변화량(diff)을 사용하여 업데이트하는 방법이 있다.

* 배열 A의 index번째 값을 val로 변경할 때 변화량은 diff = val - A[index]이다.
* index가 포함된 구간을 가진 노드들만 diff만큼 증가시키는 방식으로 구현한다.

```python
def update(node, start, end, index, diff):
	if index < start or end < index:
		#index가 노드 범위 밖이면 탐색을 중단한다.
		return
	
	# 노드를 diff 만큼 증가시킨다.
	tree[node] += diff

	if start != end:
		# 리프 노드가 아닌 경우 자식 노드를 update해준다.
		mid = (start + end) // 2
		update(node*2, start, mid, index, diff)
		update(node*2+1, mid+1, end, index, diff)
	return
```