# Linked Queue

---
### 목차

- [Linked Queue란?](#Linke-Queue란?)
- [Queue란?](#Queue란?)
    - [단방향 리스트를 활용한 큐](#단방향-리스트를-활용한-큐)
    - [링크드 리스트를 활용한 큐 구현](#링크드-리스트를-활용한-큐-구현)
- [Deque란?](#Deque란?)
    - [양방향 리스트를 활용한 큐](#양방향-리스트를-활용한-큐)
    - [양방향 리스트를 활용한 덱 구현](#양방향-리스트를-활용한-덱-구현)
- [마무리](#마무리)
---
## Linked Queue란?

링크드 큐는 연결 리스트를 기반으로 큐의 기능을 구현한 자료구조이다. 처음 접하면 다소 생소하게 느껴질 수 있으나, 연결 리스트의 특성을 활용하여 효율적인 큐 연산이 가능하다.

---
## Queue

![큐 구조.png](Linked%20Queue%20206b3875796480558d42e9d3e2b43042/image.png)

- 큐(Queue)는 먼저 들어온 데이터가 먼저 나가는(FIFO, First In First Out) 구조의 자료구조다. 마치 줄을 서서 기다리는 상황처럼, 먼저 들어온 데이터가 먼저 처리된다.
- **Front**: 데이터를 꺼내는 쪽 (가장 오래된 데이터가 있는 곳)
- **Rear**: 데이터를 넣는 쪽 (가장 최근에 들어온 데이터가 쌓이는 곳)
- **배열**, **연결 리스트**로 구현 가능하다.

## Linked List

![링크드 리스트 구조.png](Linked%20Queue%20206b3875796480558d42e9d3e2b43042/image%201.png)

- 각 노드가 데이터를 담고, 다음 노드를 가리키는 포인터를 가지는 자료 구조이다.

![링크드 리스트 예시.png](Linked%20Queue%20206b3875796480558d42e9d3e2b43042/image%202.png)

append() 메소드를 통해 데이터를 추가 한다고 하면 다음과 그림과 같을 것이다.

![링크드 리스트 추가기능 예시.png](Linked%20Queue%20206b3875796480558d42e9d3e2b43042/image%203.png)

popleft() 메소드를 통해 선입되었던 데이터를 삭제한다고 한다면 다음과 그림과 같을 것이다.

![링크드 리스트 삭제기능 예시.png](Linked%20Queue%20206b3875796480558d42e9d3e2b43042/image%204.png)

---
### 링크드 리스트를 활용한 큐 구현

```c
#include <stdio.h>
#include <stdlib.h>

// 노드 구조체 정의
typedef struct Node {
    int data;
    struct Node* next;
} Node;

// 큐 구조체 정의
typedef struct Queue {
    Node* front;
    Node* rear;
} Queue;

// 큐 초기화
void initQueue(Queue* q) {
    q->front = NULL;
    q->rear = NULL;
}

// 큐가 비었는지 확인
int isEmpty(Queue* q) {
    return q->front == NULL;
}

// 큐 출력
void printQueue(Queue* q) {
    if (isEmpty(q)) {
        printf("Queue is empty\n");
        return;
    }

    Node* temp = q->front;
    printf("Current Queue: ");
    while (temp != NULL) {
        printf("%d ", temp->data);
        temp = temp->next;
    }
    printf("\n");
}

// 큐 삽입
void enqueue(Queue* q, int new_data) {
    Node* new_node = (Node*)malloc(sizeof(Node));
    new_node->data = new_data;
    new_node->next = NULL;

    if (isEmpty(q)) {
        q->front = q->rear = new_node;
        printQueue(q);
        return;
    }

    q->rear->next = new_node;
    q->rear = new_node;
    printQueue(q);
}

// 큐 삭제
void dequeue(Queue* q) {
    if (isEmpty(q)) {
        printf("Queue is empty. Cannot dequeue.\n");
        return;
    }

    Node* temp = q->front;
    q->front = q->front->next;

    if (q->front == NULL) {
        q->rear = NULL;
    }

    free(temp);
    printQueue(q);
}

// 메인 함수
int main() {
    Queue q;
    initQueue(&q);

    enqueue(&q, 10);
    enqueue(&q, 20);

    dequeue(&q);  // 10 삭제
    dequeue(&q);  // 20 삭제

    enqueue(&q, 30);
    enqueue(&q, 40);
    enqueue(&q, 50);

    dequeue(&q);  // 30 삭제

    return 0;
}

```

- 큐(Queue)**에서 `dequeue()` 시 `head` 포인터가 이동하지만, 마지막 노드를 제거했을 경우에는 `tail`도 `None`으로 설정해야한다.
- 이를 생략하면 tail이 이미 제거된 노드를 가리키는 잘못된 상태가 되어, 이후 enqueue 시 오류가 발생한다.

---
## Deque

- Deque는 양쪽에서 삽입과 삭제가 가능한 자료구조이다.
- Linked List로 구현할 경우, 각 노드는 prev와 next 포인터를 가지며, front와 rear 포인터로 전체 구조를 추적한다.
- front는 덱의 앞쪽, 즉 가장 왼쪽 노드를 가리키고, rear는 덱의 뒤쪽, 즉 가장 오른쪽 노드를 가리킨다.
- insert_front()는 head 앞에 새 노드를 추가하고, insert_rear()는 tail 뒤에 새 노드를 추가한다.
- 삭제 연산은 해당 위치의 노드를 제거한 후 포인터를 적절히 갱신하여 구조를 유지한다.

![양방향 리스트 구조1.png](Linked%20Queue%20206b3875796480558d42e9d3e2b43042/image%205.png)

![양방향 리스트 구조2.png](Linked%20Queue%20206b3875796480558d42e9d3e2b43042/image%206.png)

Deque를 구현하려면 front와 rear 두 포인터를 추적해야 한다.

Deque의 앞쪽이나 뒤쪽에 항목을 삽입하고, 양쪽에서 항목을 꺼낸다.

Node 타입의 front와 rear 포인터를 선언하며, Node는 이중 연결 리스트의 노드를 나타낸다.

두 포인터는 초기 상태에서 NULL로 설정한다.

---

### 양방향 리스트를 활용한 덱 구현

```c
#include <stdio.h>
#include <stdlib.h>

// 노드 구조체
typedef struct Node {
    int data;
    struct Node* prev;
    struct Node* next;
} Node;

// 덱 구조체
typedef struct Deque {
    Node* front;
    Node* rear;
    int size;
} Deque;

// 덱 초기화
void initDeque(Deque* dq) {
    dq->front = NULL;
    dq->rear = NULL;
    dq->size = 0;
}

// 덱이 비었는지 확인
int is_empty(Deque* dq) {
    return dq->front == NULL;
}

// 덱 크기 반환
int get_size(Deque* dq) {
    return dq->size;
}

// front에 삽입
void insert_front(Deque* dq, int data) {
    Node* new_node = (Node*)malloc(sizeof(Node));
    new_node->data = data;
    new_node->prev = NULL;
    new_node->next = dq->front;

    if (is_empty(dq)) {
        dq->front = dq->rear = new_node;
    } else {
        dq->front->prev = new_node;
        dq->front = new_node;
    }
    dq->size++;
}

// rear에 삽입
void insert_rear(Deque* dq, int data) {
    Node* new_node = (Node*)malloc(sizeof(Node));
    new_node->data = data;
    new_node->next = NULL;
    new_node->prev = dq->rear;

    if (is_empty(dq)) {
        dq->front = dq->rear = new_node;
    } else {
        dq->rear->next = new_node;
        dq->rear = new_node;
    }
    dq->size++;
}

// front에서 삭제
void delete_front(Deque* dq) {
    if (is_empty(dq)) {
        printf("UnderFlow\n");
        return;
    }

    Node* temp = dq->front;
    dq->front = dq->front->next;
    if (dq->front) {
        dq->front->prev = NULL;
    } else {
        dq->rear = NULL;
    }
    free(temp);
    dq->size--;
}

// rear에서 삭제
void delete_rear(Deque* dq) {
    if (is_empty(dq)) {
        printf("UnderFlow\n");
        return;
    }

    Node* temp = dq->rear;
    dq->rear = dq->rear->prev;
    if (dq->rear) {
        dq->rear->next = NULL;
    } else {
        dq->front = NULL;
    }
    free(temp);
    dq->size--;
}

// front 값 반환
int get_front(Deque* dq) {
    return is_empty(dq) ? -1 : dq->front->data;
}

// rear 값 반환
int get_rear(Deque* dq) {
    return is_empty(dq) ? -1 : dq->rear->data;
}

// 덱 비우기
void clear(Deque* dq) {
    while (!is_empty(dq)) {
        delete_front(dq);
    }
}

// 테스트용 메인 함수
int main() {
    Deque dq;
    initDeque(&dq);

    insert_rear(&dq, 5);
    insert_rear(&dq, 10);
    printf("Rear: %d\n", get_rear(&dq));

    delete_rear(&dq);
    printf("New Rear: %d\n", get_rear(&dq));

    insert_front(&dq, 15);
    printf("Front: %d\n", get_front(&dq));
    printf("Size: %d\n", get_size(&dq));

    delete_front(&dq);
    printf("New Front: %d\n", get_front(&dq));

    clear(&dq);
    return 0;
}

```

```c
if self.front is None:  # 마지막 노드였던 경우
    self.rear = None

```

- 덱에서는 양쪽에서 삽입과 삭제가 발생하므로, front와 rear뿐만 아니라 각 노드의 prev와 next도 정확히 NULL로 갱신해야 한다.
- 헤드 노드의 prev는 반드시 NULL이어야 하며, rear도 연결이 끊긴 경우 NULL로 설정해야 한다.

## 마무리

Queue나 Deque는 삽입과 삭제가 빈번하게 발생하므로, 배열처럼 데이터를 매번 이동시키는 방식은 비효율적이다.
front와 rear 포인터를 사용하면 각 연산을 O(1) 시간 복잡도로 처리할 수 있다.
예를 들어, 큐에서 dequeue 시 front만 이동시키면 되므로, 배열의 shift 연산처럼 데이터를 당길 필요가 없다.
rear는 삽입 지점을 고정해 효율적인 삽입을 가능하게 한다.

연결 리스트를 이용한 큐와 덱 구현에서는 front와 rear 포인터 관리가 핵심이다.
이 포인터들이 잘못 관리되면 데이터 손실이나 무한 루프 같은 문제가 발생할 수 있으므로, 각 연산에서 포인터의 변화를 정확히 파악해야 한다.