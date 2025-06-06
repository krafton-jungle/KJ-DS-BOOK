# 목차

1. [원형 큐란](#원형-큐란)
2. [공백과 포화 상태 구분](#공백과-포화-상태-구분)
   - [1. 한 칸 비워두기 방식](#1-한-칸-비워두기-방식)
   - [2. 카운터 변수 사용 방식](#2-카운터-변수-사용-방식)
   - [3. 플래그 비트 사용 방식](#3-플래그-비트-사용-방식)
   - [방식별 비교](#방식별-비교)
3. [원형 큐의 주요 연산](#원형-큐의-주요-연산)
   - [삽입 연산 (enqueue)](#삽입-연산-enqueue)
   - [삭제 연산 (dequeue)](#삭제-연산-dequeue)
4. [원형 큐의 구현](#원형-큐의-구현)
   - [한 칸 비워두기 방식 구현](#한-칸-비워두기-방식-구현)
   - [카운터 변수 방식 구현](#카운터-변수-방식-구현)
   - [플래그 비트 방식 구현](#플래그-비트-방식-구현)


---

# 원형 큐란

**원형 큐(Circular Queue)**는 선형 큐의 메모리 비효율성 문제를 해결하기 위해 고안된 큐 자료구조이다. 배열의 끝과 시작을 논리적으로 연결하여 원형 구조를 만들어, 모든 메모리 공간을 순환적으로 활용할 수 있다.

**선형 큐**에서는 삽입과 삭제가 반복되면서 front와 rear 포인터가 배열의 한쪽 끝으로만 계속 이동한다. 
이로 인해 배열의 앞부분에 빈 공간이 생겨도 포인터들이 되돌아가지 않아 해당 공간을 재사용할 수 없다.

반면 **원형 큐**에서는 front와 rear 포인터가 배열의 끝에 도달하면 다시 배열의 시작 부분으로 돌아가며 
순환적으로 이동한다. 즉, 포인터들이 원형으로 회전하면서 배열의 모든 공간을 반복적으로 활용할 수 있다. 원형 큐의 포인터 이동은 마치 시계의 시침이 12시를 지나 1시로 이동하는 것과 같다. 
배열의 인덱스가 0, 1, 2, ..., n-1의 순서로 배치되어 있을 때, 포인터는 이 순서를 따라 
순환하며 n-1 다음에는 다시 0으로 돌아간다.

삽입 연산에서는 rear 다음 위치에 새 원소를 추가하고, 삭제 연산에서는 front 위치의 원소를 제거한 후 front를 다음 위치로 이동시킨다.

선형 큐는 앞서 설명한 것처럼 공간 낭비 문제가 발생한다. 이는 배열에 실제로는 빈 공간이 있음에도 불구하고 rear 포인터가 배열의 끝에 도달했을 때 더 이상 삽입할 수 없는 상황을 의미한다.

![legend.png](https://velog.velcdn.com/images/plan6062/post/319f248c-2d3d-4ec6-9df3-a99542940bfa/image.png)
![Linear_queue_space_waste.png](https://velog.velcdn.com/images/plan6062/post/cf3b93b1-f239-43e6-98fe-4622c70df1e8/image.png)



예를 들어, 크기가 5인 배열에서 5개의 원소를 모두 삽입한 후 3개를 삭제하면, 실제로는 앞쪽에 3개의 공간이 비어있지만 rear 포인터가 배열의 끝에 있어 새로운 원소를 삽입할 수 없다.

원형 큐는 **모듈로 연산(Modulo Operation), `(index + 1) % maxSize)`** 을 사용하여 이 문제를 해결한다. 배열의 크기를 n이라 할 때, 포인터가 n-1을 넘어가면 다시 0부터 시작하도록 하여 순환 구조를 구현한다. 이를 통해 rear 포인터가 배열의 끝에 도달해도 앞부분에 빈 공간이 있다면 계속해서 삽입 연산을 수행할 수 있다.

![Circular_queue.png](https://velog.velcdn.com/images/plan6062/post/8030ac13-994c-4d45-8526-260b66a2c110/image.png)



# 공백과 포화 상태 구분

원형 큐에서는 큐의 상태를 정확히 파악하는 것이 중요하다.

**공백 상태(Empty State)**: 큐에 저장된 원소가 하나도 없는 상태이다. 이 상태에서는 삭제 연산을 수행할 수 없다.

**포화 상태(Full State)**: 큐가 최대 용량에 도달하여 더 이상 새로운 원소를 삽입할 수 없는 상태이다. 이 상태에서는 삽입 연산을 수행할 수 없다.

원형 큐에서 가장 까다로운 문제는 이 두 상태를 구분하는 것이다. 초기 상태와 모든 공간이 사용된 후의 상태에서 모두 `front == rear` 조건을 만족할 수 있기 때문에 추가적인 방법이 필요하다.


## 상태 구분 방법

## 1. 한 칸 비워두기 방식

가장 널리 사용되는 방법으로, 배열의 한 칸을 항상 비워둔다. 즉, 최대 n-1개의 원소만 저장할 수 있도록 제한한다.

**공백 조건**: `front == rear`
**포화 조건**: `(rear + 1) % maxSize == front`

![Empty_slot_method.png](https://velog.velcdn.com/images/plan6062/post/d9a4eefd-f7e6-445b-b6ae-09d49129e219/image.png)


이 방식은 구현이 간단하고 직관적이지만, 배열의 한 칸을 사용하지 못하므로 공간 효율성이 약간 떨어진다.

## 2. 카운터 변수 사용 방식

큐에 저장된 원소의 개수를 별도의 변수로 관리하는 방법이다.

**공백 조건**: `count == 0`
**포화 조건**: `count == maxSize`

![Counter_variable_method.png](https://velog.velcdn.com/images/plan6062/post/7260333e-5f78-4b06-b426-12673f35decf/image.png)


이 방식은 배열의 모든 공간을 활용할 수 있고 현재 큐의 크기를 즉시 알 수 있지만, 추가적인 변수가 필요하고 모든 연산에서 카운터를 갱신해야 한다.

## 3. 플래그 비트 사용 방식

마지막 연산이 삽입인지 삭제인지를 기록하는 플래그를 사용하는 방법이다.

**공백 조건**: `front == rear && last_op == DELETE`
**포화 조건**: `front == rear && last_op == INSERT`

이 방식은 추가 공간을 최소화하면서 모든 배열 공간을 활용할 수 있지만, 플래그 관리가 복잡하다.

![Flag_bit_method.png](https://velog.velcdn.com/images/plan6062/post/cd204382-31bf-4919-a23a-017e9572858f/image.png)


## 방식별 비교

| 방식 | 공간 효율성 | 구현 복잡도 | 추가 메모리 | 장점 |
|------|-------------|-------------|-------------|------|
| 한 칸 비워두기 | n-1개 저장 | 낮음 | 없음 | 구현 간단 |
| 카운터 변수 | n개 저장 | 중간 | int 1개 | 크기 정보 제공 |
| 플래그 비트 | n개 저장 | 높음 | 1bit | 메모리 최소화 |

# 원형 큐의 주요 연산

원형 큐에서 지원하는 주요 연산들은 삽입, 삭제, 상태 확인 연산으로 구분된다.

## 삽입 연산 (enqueue)

새로운 원소를 큐의 rear 다음 위치에 삽입하는 연산이다.

**연산 과정**:
1. 포화 상태 검사
2. `rear = (rear + 1) % maxSize`로 이동
3. 새로운 원소를 rear 위치에 저장

![Circular_queue_insertion.png](https://velog.velcdn.com/images/plan6062/post/d835ec62-df96-4e29-9e4f-4ed6c788b2c3/image.png)




```
enqueue(item):
    if isFull():
        error "Queue is full"
    else:
        rear = (rear + 1) % maxSize
        queue[rear] = item
        // 크기 정보 갱신 (방식에 따라)
```

## 삭제 연산 (dequeue)

큐의 front 위치에 있는 원소를 제거하고 반환하는 연산이다.

**연산 과정**:
1. 공백 상태 검사
2. `front = (front + 1) % maxSize`로 이동
3. front 위치의 원소 반환

![Circular_queue_deletion.png](https://velog.velcdn.com/images/plan6062/post/45cc9ebe-01b2-4669-86a3-aeb3b045eaf4/image.png)




```
dequeue():
    if isEmpty():
        error "Queue is empty"
    else:
        front = (front + 1) % maxSize
        item = queue[front]
        // 크기 정보 갱신 (방식에 따라)
        return item
```



# 원형 큐의 구현

## 한 칸 비워두기 방식 구현

``` c
// C언어 - 한 칸 비워두기 방식 (Empty Slot Method)

#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

typedef struct {
    int maxsize;
    int *queue;
    int front;
    int rear;
} CircularQueue;

CircularQueue* createCircularQueue(int maxsize) {
    CircularQueue* cq = (CircularQueue*)malloc(sizeof(CircularQueue));
    cq->maxsize = maxsize;
    cq->queue = (int*)calloc(maxsize, sizeof(int));
    cq->front = 0;
    cq->rear = 0;
    return cq;
}

bool isEmpty(CircularQueue* cq) {
    return cq->front == cq->rear;
}

bool isFull(CircularQueue* cq) {
    return (cq->rear + 1) % cq->maxsize == cq->front;
}

void enqueue(CircularQueue* cq, int item) {
    if (isFull(cq)) {
        printf("Queue is full\n");
        return;
    }
    cq->rear = (cq->rear + 1) % cq->maxsize;
    cq->queue[cq->rear] = item;
}

int dequeue(CircularQueue* cq) {
    if (isEmpty(cq)) {
        printf("Queue is empty\n");
        return -1;
    }
    cq->front = (cq->front + 1) % cq->maxsize;
    return cq->queue[cq->front];
}

int peek(CircularQueue* cq) {
    if (isEmpty(cq)) {
        printf("Queue is empty\n");
        return -1;
    }
    return cq->queue[(cq->front + 1) % cq->maxsize];
}

int size(CircularQueue* cq) {
    return (cq->rear - cq->front + cq->maxsize) % cq->maxsize;
}

void destroyCircularQueue(CircularQueue* cq) {
    free(cq->queue);
    free(cq);
}
```

## 카운터 변수 방식 구현

```c
// C언어 - 카운터 변수 방식 (Counter Variable Method)

#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

typedef struct {
    int maxsize;
    int *queue;
    int front;
    int rear;
    int count;
} CircularQueueWithCounter;

CircularQueueWithCounter* createCircularQueueWithCounter(int maxsize) {
    CircularQueueWithCounter* cq = (CircularQueueWithCounter*)malloc(sizeof(CircularQueueWithCounter));
    cq->maxsize = maxsize;
    cq->queue = (int*)calloc(maxsize, sizeof(int));
    cq->front = 0;
    cq->rear = -1;
    cq->count = 0;
    return cq;
}

bool isEmpty(CircularQueueWithCounter* cq) {
    return cq->count == 0;
}

bool isFull(CircularQueueWithCounter* cq) {
    return cq->count == cq->maxsize;
}

void enqueue(CircularQueueWithCounter* cq, int item) {
    if (isFull(cq)) {
        printf("Queue is full\n");
        return;
    }
    cq->rear = (cq->rear + 1) % cq->maxsize;
    cq->queue[cq->rear] = item;
    cq->count++;
}

int dequeue(CircularQueueWithCounter* cq) {
    if (isEmpty(cq)) {
        printf("Queue is empty\n");
        return -1;
    }
    int item = cq->queue[cq->front];
    cq->front = (cq->front + 1) % cq->maxsize;
    cq->count--;
    return item;
}

int peek(CircularQueueWithCounter* cq) {
    if (isEmpty(cq)) {
        printf("Queue is empty\n");
        return -1;
    }
    return cq->queue[cq->front];
}

int size(CircularQueueWithCounter* cq) {
    return cq->count;
}

void destroyCircularQueueWithCounter(CircularQueueWithCounter* cq) {
    free(cq->queue);
    free(cq);
}
```

## 플래그 비트 방식 구현

```c
// C언어 - 플래그 비트 방식 (Flag Bit Method)

#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

typedef enum {
    DELETE,
    INSERT
} LastOperation;

typedef struct {
    int maxsize;
    int *queue;
    int front;
    int rear;
    LastOperation last_op;
} CircularQueueWithFlag;

CircularQueueWithFlag* createCircularQueueWithFlag(int maxsize) {
    CircularQueueWithFlag* cq = (CircularQueueWithFlag*)malloc(sizeof(CircularQueueWithFlag));
    cq->maxsize = maxsize;
    cq->queue = (int*)calloc(maxsize, sizeof(int));
    cq->front = 0;
    cq->rear = 0;
    cq->last_op = DELETE;
    return cq;
}

bool isEmpty(CircularQueueWithFlag* cq) {
    return cq->front == cq->rear && cq->last_op == DELETE;
}

bool isFull(CircularQueueWithFlag* cq) {
    return cq->front == cq->rear && cq->last_op == INSERT;
}

void enqueue(CircularQueueWithFlag* cq, int item) {
    if (isFull(cq)) {
        printf("Queue is full\n");
        return;
    }
    cq->rear = (cq->rear + 1) % cq->maxsize;
    cq->queue[cq->rear] = item;
    cq->last_op = INSERT;
}

int dequeue(CircularQueueWithFlag* cq) {
    if (isEmpty(cq)) {
        printf("Queue is empty\n");
        return -1;
    }
    cq->front = (cq->front + 1) % cq->maxsize;
    int item = cq->queue[cq->front];
    cq->last_op = DELETE;
    return item;
}

int peek(CircularQueueWithFlag* cq) {
    if (isEmpty(cq)) {
        printf("Queue is empty\n");
        return -1;
    }
    return cq->queue[(cq->front + 1) % cq->maxsize];
}

int size(CircularQueueWithFlag* cq) {
    if (isEmpty(cq)) {
        return 0;
    } else if (isFull(cq)) {
        return cq->maxsize;
    } else {
        return (cq->rear - cq->front + cq->maxsize) % cq->maxsize;
    }
}

void destroyCircularQueueWithFlag(CircularQueueWithFlag* cq) {
    free(cq->queue);
    free(cq);
}
```

