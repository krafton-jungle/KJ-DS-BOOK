# 레드 블랙 트리(Red-Black Tree)
자가 균형 이진 탐색 트리(Balanced-BST)인 레드 블랙 트리의 구조와 동작 원리를 이해하고, 삽입과 삭제 연산에서 균형을 유지하는 방법을 학습한다.

| 레드 블랙 트리는 최악의 경우에도 O(log n)의 성능을 보장하는 균형 트리를 사용하여 이진 탐색 트리(BST)의 단점을 레드 블랙 트리를 통해 극복할 수 있다.

# 주요 키워드
| 항목 | 설명 |
|--------|--------|
| [소제목](https://jungle9.gitbook.io/kj-book-guide/algorithm#key)   | 핵심 개념 및 원리 요약   |
| [코드예시](https://jungle9.gitbook.io/kj-book-guide/algorithm#graph)   | Python 코드 기반 설명  |
| [영상첨부설정](https://jungle9.gitbook.io/kj-book-guide/algorithm#excute) | 영상 및 이미지 첨부 방법 안내 |

## 레드 블랙 트리란
개념 : 이진 탐색 트리(BST)의 일종인 레드-블랙 트리는 각 노드가 `빨간색` 또는 `검은색`의 추가 속성을 가진 `self-balancing 바이너리 검색 트리`이다. 이러한 트리의 주요 목표는 삽입 및 삭제 시 균형을 유지하여 효율적인 데이터 검색 및 조작을 보장한다.


## 레드 블랙 트리의 속성
1. 노드 색상 : 각 노드는 레드 혹은 블랙 색상이다.
2. 루트 노드 : 루트는 언제나 블랙 색상이다.
3. 레드 노드 : 레드 노드는 자식으로 레드를 가질 수 없다. (레드 노드의 자식은 검정색이다)
4. 어떤 노드에서 그 자손의 모든 리프 노드(즉, null 노드)로 가는 모든 경로에는 같은 수의 검은색 노드가 존재해야 한다.

```
	         (B)
	      /       \
	    (R)       (B)
	   /             \
     (B)             (R)
  /       \        /     \
null(B) null(B) null(B) null(B)
```

5. 리프 노드 : NIL(Null Leaf)은 언제나 블랙이다.

## 왜 레드 블랙 트리일까?
모든 삽입 및 삭제 후에도 트리의 높이가 O(log n)로 유지되도록 하면 이러한 모든 작업에 대해 O(log n)의 상한을 보장할 수 있습니다. Red-Black 트리의 높이는 항상 O(log n)이며, 여기서 n은 트리의 노드 수 이다.

| **알고리즘** | **시간 복잡도** |
| --- | --- |
| 검색 | O(log n) |
| 삽입 | O(log n) |
| 삭제 | O(log n) |

## AVL와 비교
AVL 트리는 레드 블랙 트리에 비해 균형이 더 잘 잡혀 있다. 

삽입과 삭제가 더 자주 있다면 AVL 트리 보다, 레드 블랙 트리 사용하는 것이 좋다.

검색이 주요 작업이라면 AVL 트리 사용하는 것이 좋다.

하지만 지속적인 삽입, 삭제가 발생한다면 레드 블랙 트리 사용도 좋지 않을 수 있다.

## 레드 블랙 트리의 기본 동작

1. 삽입
2. 검색
3. 삭제
4. 회전

### 삽입

**삽입 방법**

1. BST 삽입 : BST 처럼 새로운 노드를 삽입 (삽입되는 노드의 색은 항상 레드이다)
2. 위반 시 수정
- **케이스 1: 삼촌이 레드일 때**: 할아버지와 삼촌의 컬러를 블랙으로 변경하고 할아버지 노드를 레드로 바꾼다.
    
    ![RBT_1](../../assets/ch13_balanced_binary_search_tree/RBT_1.png)
    
    단, 할아버지 노드가 root이면 블랙을 유지한다.
    
- **케이스 2: 삼촌이 블랙일 때**:
    - 서브 케이스 2.1: 삽입 노드가 오른쪽일 때: Left-Right 회전
        
        ![삽입 위반 수정 케이스 2 오른쪽  자식](../../assets/ch13_balanced_binary_search_tree/RBT_2.png)
        
    - 서브 케이스 2.2: 삽입 노드가 왼쪽 일 때: Right 회전
        
        ![삽입 위반 수정 케이스 2 왼쪽  자식](../../assets/ch13_balanced_binary_search_tree/RBT_3.png)
        

### 검색

레드 블랙 트리의 검색은 BST와 비슷하다.

**검색 방법**

1. 루트에서 시작
2. 트리를 순회
    - 타겟 값과 현재 노드가 같을 때 완료
    - 타겟 값이 현재 노드 보다 작다면 왼쪽으로 이동
    - 타겟 값이 현재 노드 보다 크다면 오른쪽으로 이동
3. 반복 : 타겟 값을 찾을 때까지 반복하거나 NIL(Null Leaf)에 도달할 때까지 반복한다.

### 삭제

삭제 방법

1. BST 삭제: 노드 삭제 시 BST 규칙을 따른다.
2. 레드 노드: 레드 노드 일 경우 BST 규칙을 따르며 삭제 한다.
3. 더블 블랙 수정: 블랙 노드가 삭제될 시, “더블 블랙” 조건이 발생한다.

> `더블 블랙이란?`
레드 블랙 트리에서 노드를 삭제하면, 트리의 블랙 노드가 부족해지는 상황을 말한다.
**지금 black height가 맞지 않음을 알려주는 마커** 같은 역할을 한다.
> 
> 
> 이건 "**여기 아직 검정이 하나 부족해**"라는 신호이자, 이후의 보정을 유도하는 장치다.
> 

> **extra 블랙**
삭제되는 색이 블랙이고, 4번 위반일 때 `extra 블랙 부여`
이를 부여 받은 노드는 `더블 블랙` 혹은 `레드 앤드 블랙`이 된다.

대체한 노드가 레드 앤드 블랙이 되었다면 블랙으로 바꿔주면 끝이다.
> 
> - 30(B)를 삭제했어.
> - 그 자리를 대체한 25(R)는 원래 red였기 때문에, 그대로는 black height가 부족해.
> - 그래서 25에 **extra black을 부여해서 "red-and-black" 상태**로 만들어.
> - 그 상태에서는 black height가 2가 되므로, **일시적으로는 속성 4를 만족하게 된다.
> 
> extra 블랙은 임시 봉합이다.**

- 케이스 1: 형제 노드가 레드일 때: 할아버지를 회전 시키고, 할아버지와 형제 노드의 색상을 변경한다.

![삭제 1](../../assets/ch13_balanced_binary_search_tree/RBT_4.png)

- 케이스 2: 형제 노드가 블랙일 때:
    - 서브케이스 2.1: 형제 노드의 자식이 블랙일 때: 형제의 컬러를 바꾸고 더블 블랙을 위로 보낸다.
        
        ![삭제 2.1](../../assets/ch13_balanced_binary_search_tree/RBT_5.png)
        
    - 2.2: 형제의 자식 중 하나라도 레드일 때:
        - 2.2.1 형제의 먼 자식이 레드일 때: 할아버지와 형제를 회전하고 알맞게 컬러를 수정한다.
            
            작동: 이 조건일 때 `오른쪽 형제`는 할아버지의 색으로, `오른쪽 형제의 오른쪽 자녀`는 블랙으로, 할아버지는 블랙으로 바꾼 후 `할아버지`를 기준으로 왼쪽 회전한다.  (오른쪽 왼쪽 바꿔도 성립)
            
            ![삭제 2.2.1 형제 노드의 자식 중 하나라도 레드](../../assets/ch13_balanced_binary_search_tree/RBT_6.png)
            
        - 2.2.2 형제의 가까운 자식이 레드일 때: 형제와 형제의 자식을 회전 시킨 후 케이스(2.2.1)를 통해 알맞게 수정한다.
        
        ![삭제 2.2.2 형제 노드의 자식 중 하나라도 레드](../../assets/ch13_balanced_binary_search_tree/RBT_7.png)
## 사용 예시
1. maps & sets
2. 우선순위 큐
3. 파일 시스템
4. 인-메모리 데이터베이스
5. 그래픽과 게임 개발

# 코드예시
아래는 C 언어로 작성한 자료구조 예시입니다.

```c
#include <iostream>
using namespace std;

// 노드 클라스
struct Node {
    int data;
    string color;
    Node *left, *right, *parent;

    Node(int data)
        : data(data)
        , color("RED")
        , left(nullptr)
        , right(nullptr)
        , parent(nullptr)
    {
    }
};

// 레드 블랙 트리 클라스
class RedBlackTree {
private:
    Node* root;
    Node* NIL;

    // 왼쪽 회전
    void left_rotate(Node* x)
    {
        Node* y = x->right;
        x->right = y->left;
        if (y->left != NIL) {
            y->left->parent = x;
        }
        y->parent = x->parent;
        if (x->parent == nullptr) {
            root = y;
        }
        else if (x == x->parent->left) {
            x->parent->left = y;
        }
        else {
            x->parent->right = y;
        }
        y->left = x;
        x->parent = y;
    }

    // 오른쪽 회전
    void right_rotate(Node* x)
    {
        Node* y = x->left;
        x->left = y->right;
        if (y->right != NIL) {
            y->right->parent = x;
        }
        y->parent = x->parent;
        if (x->parent == nullptr) {
            root = y;
        }
        else if (x == x->parent->right) {
            x->parent->right = y;
        }
        else {
            x->parent->left = y;
        }
        y->right = x;
        x->parent = y;
    }

    // 삽입 후 수정
    void fix_insert(Node* k)
    {
        while (k != root && k->parent->color == "RED") {
            if (k->parent == k->parent->parent->left) {
                Node* u = k->parent->parent->right; // 삼촌
                if (u->color == "RED") {
                    k->parent->color = "BLACK";
                    u->color = "BLACK";
                    k->parent->parent->color = "RED";
                    k = k->parent->parent;
                }
                else {
                    if (k == k->parent->right) {
                        k = k->parent;
                        leftRotate(k);
                    }
                    k->parent->color = "BLACK";
                    k->parent->parent->color = "RED";
                    rightRotate(k->parent->parent);
                }
            }
            else {
                Node* u = k->parent->parent->left; // 삼촌ㅋ
                if (u->color == "RED") {
                    k->parent->color = "BLACK";
                    u->color = "BLACK";
                    k->parent->parent->color = "RED";
                    k = k->parent->parent;
                }
                else {
                    if (k == k->parent->left) {
                        k = k->parent;
                        rightRotate(k);
                    }
                    k->parent->color = "BLACK";
                    k->parent->parent->color = "RED";
                    leftRotate(k->parent->parent);
                }
            }
        }
        root->color = "BLACK";
    }

    // 순서 이동 도우미
    void inorder_helper(Node* node)
    {
        if (node != NIL) {
            inorderHelper(node->left);
            cout << node->data << " ";
            inorderHelper(node->right);
        }
    }

    // 검색 도우미
    Node* search_helper(Node* node, int data)
    {
        if (node == NIL || data == node->data) {
            return node;
        }
        if (data < node->data) {
            return searchHelper(node->left, data);
        }
        return searchHelper(node->right, data);
    }

public:
    // Constructor
    RedBlackTree()
    {
        NIL = new Node(0);
        NIL->color = "BLACK";
        NIL->left = NIL->right = NIL;
        root = NIL;
    }

    // 삽입
    void insert(int data)
    {
        Node* new_node = new Node(data);
        new_node->left = NIL;
        new_node->right = NIL;

        Node* parent = nullptr;
        Node* current = root;

        // BST insert
        while (current != NIL) {
            parent = current;
            if (new_node->data < current->data) {
                current = current->left;
            }
            else {
                current = current->right;
            }
        }

        new_node->parent = parent;

        if (parent == nullptr) {
            root = new_node;
        }
        else if (new_node->data < parent->data) {
            parent->left = new_node;
        }
        else {
            parent->right = new_node;
        }

        if (new_node->parent == nullptr) {
            new_node->color = "BLACK";
            return;
        }

        if (new_node->parent->parent == nullptr) {
            return;
        }

        fix_insert(new_node);
    }

    // 순서 이동
    void inorder() { inorder_helper(root); }

    // 검색
    Node* search(int data)
    {
        return search_helper(root, data);
    }
};

int main()
{
    RedBlackTree rbt;

    rbt.insert(10);
    rbt.insert(20);
    rbt.insert(30);
    rbt.insert(15);

    // 순서 이동
    cout << "Inorder traversal:" << endl;
    rbt.inorder(); // Output: 10 15 20 30

    // 노드 검색
    cout << "\nSearch for 15: "
         << (rbt.search(15) != rbt.search(0))
         << endl; // Output: 1 (true)
    cout << "Search for 25: "
         << (rbt.search(25) != rbt.search(0))
         << endl; // Output: 0 (false)

    return 0;
}
```

# 영상첨부설정
글로 설명하기 어려운 경우, 영상이나 이미지로 보충한다.

> 예시)      
[자료구조 설명 영상](https://youtube.com)

> 예시)    
![자료구조 다이어그램](/assets/style_guide/krafton-jungle.png)

# 문제 풀어보기
1. 레드 블랙 트리의 특성 5가지를 적으시오.
2. extra 블랙은 어떤 규칙을 위반 하였을 때 발생하는가?

# 참고
참고한 자료

GeeksforGeeks의 Red-Black Tree

이미지 출처

크래프톤 정글 이미지

외부 이미지 사용 시 라이선스 확인 필요

추가로 참고하면 좋은 자료

Visualgo

GitHub 검색: Red-Black Tree

주의사항

외부 콘텐츠를 인용할 경우, 반드시 출처를 명시하고 링크를 제공할 것

이미지, 도표 등은 직접 제작하거나, 라이선스가 허용된 자료만 사용할 것