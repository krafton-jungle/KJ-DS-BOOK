# 균형 이진 탐색 트리
이진 탐색 트리(BST)의 구조를 유지하면서도, 삽입과 삭제 후에도 트리의 높이가 지나치게 증가하지 않도록 자동으로 균형을 유지하는 트리를 말한다.

| 일반 BST는 정렬된 데이터를 삽입하면 선형 구조(편향 트리)가 되어 탐색 성능이 O(n)까지 떨어진다. 이를 방지하기 위해, 균형 BST는 트리의 높이를 O(log n)으로 유지하여 항상 빠른 탐색·삽입·삭제가 가능하도록 만든 자료구조이다.

# 대표적인 균형 이진 탐색 트리
AVL 트리와 레드 블랙 트리(Red-Black Tree)에 대해 알아보자.
<!-- 자가 균형 이진 탐색 트리(Balanced-BST)인 AVL 트리의 구조와 동작 원리를 이해하고, 삽입과 삭제 연산에서 균형을 유지하는 방법을 학습한다.

| AVL 트리는 각 노드의 균형 인수(Balance Factor)를 유지하며, 삽입과 삭제 시 균형이 깨지면 회전을 통해 O(log n)의 시간 복잡도를 보장한다. 이를 통해 일반 이진 탐색 트리(BST)가 편향되어 성능이 나빠지는 문제를 AVL 트리를 통해 극복할 수 있다 -->

## AVL 트리 주요 키워드
| 항목 | 설명 |
|--------|--------|
| [소제목](https://jungle9.gitbook.io/kj-book-guide/algorithm#key)   | 핵심 개념 및 원리 요약   |
| [코드예시](https://jungle9.gitbook.io/kj-book-guide/algorithm#graph)   | Python 코드 기반 설명  |
| [영상첨부설정](https://jungle9.gitbook.io/kj-book-guide/algorithm#excute) | 영상 및 이미지 첨부 방법 안내 |



## 레드 블랙 트리 주요 키워드
| 항목 | 설명 |
|--------|--------|
| NIL   | Null Leaf에 대한 설명   |
| extra 블랙   | 삭제 시 발생하는 문제에 대해 설명  |

<!-- (https://jungle9.gitbook.io/kj-book-guide/algorithm#key) -->
<!-- (https://jungle9.gitbook.io/kj-book-guide/algorithm#graph) -->