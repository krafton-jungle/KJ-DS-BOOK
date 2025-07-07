# 체이닝(Chaining) {#chaining}

체이닝은 각 해시 테이블의 **버킷(bucket)**에 **연결 리스트(Linked List)**를 사용하여 충돌이 발생한 요소들을 저장하는 방법이다. 즉, 동일한 인덱스로 해시되는 모든 키-값 쌍은 해당 인덱스의 연결 리스트에 추가된다.

체이닝의 작동 방식은 다음과 같다.

1. **해시 함수를 통해 키를 인덱스로 변환한다.**
2. **해당 인덱스에 해당하는 버킷을 찾는다.**
3. **버킷이 비어 있다면, 새로운 키-값 쌍을 저장한다.**
![충돌 없을 때](/assets/ch9_hash_table/hash_not_collision.svg)
4. **버킷에 이미 다른 요소가 있다면, 해당 버킷의 연결 리스트의 끝에 새로운 키-값 쌍을 추가한다.**
![충돌 있을 때](/assets/ch9_hash_table/hash_collision.svg)
![체이닝](/assets/ch9_hash_table/hash_chaining.svg)
5. **검색 시:** 해시 함수를 통해 인덱스를 찾고, 해당 버킷의 연결 리스트를 순회하여 원하는 키를 찾아낸다.

### **장점:**

- 해시 테이블의 **적재율**(삽입된 요소 수 / 버킷 수)이 1을 초과해도 사용할 수 있다.
- 삭제 연산이 다른 충돌 해결 기법에 비해 직관적이고 간단하다.
- 구현이 비교적 쉽다.

### **단점:**

- 연결 리스트를 사용하므로 추가적인 메모리 오버헤드가 발생한다.
- 최악의 경우, 모든 요소가 하나의 버킷에 집중되면 연결 리스트의 길이가 길어져 검색 시간이 O(N)으로 저하될 수 있다 (N은 테이블에 저장된 요소의 수).

### 시간, 공간 복잡도

전체 키의 갯수를 $$N$$, 전체 버킷의 갯수를 $$M$$ 이라 할 시.

| 연산              | 평균 시간복잡도  | 최악 시간복잡도 |
| --------------- | --------- | -------- |
| **탐색 (Search)** | $$O(1)$$     | $$O(N/M)$$   |
| **삽입 (Insert)** | $$O(1)$$     | $$O(N)$$   |
| **삭제 (Delete)** | $$O(1)$$     | $$O(N)$$   |

해쉬 함수를 통과해 버킷을 찾아가는데 $$O(1)$$, 버킷에 연결되어있는 링크드리스트를 탐색하는데 $$O(n)$$이 걸린다.

최악의 경우, 모든 값이 같은 버킷에 연결리스트로 연결되었을 때 탐색, 삽입, 삭제가 전부 $$O(n)$$ 만큼 걸리게 된다.
일반적으로 해쉬함수가 모든 키를 균등하게 잘 분배한다고 했을 시, $$ \text{전체 키의 갯수} / \text{버킷의 크기}$$ 만큼의 평균 시간복잡도가 걸린다.

공간복잡도는 버킷의 갯수 + 저장한 키의 갯수이므로 $$O(N+M)$$ 이다.

---

## **2. Python을 이용한 체이닝 구현 예시**

Python에서는 리스트(list)를 사용하여 연결 리스트의 역할을 대신할 수 있다. 각 버킷에 리스트를 할당하고, 충돌이 발생하면 해당 리스트에 `(키, 값)` 튜플을 추가한다.

**Python**

`class HashTable:
    def __init__(self, size=10):
        self.size = size
        self.table = [[] for _ in range(self.size)] # 각 버킷은 리스트 (체이닝)

    def _hash(self, key):
        """간단한 해시 함수"""
        return hash(key) % self.size

    def insert(self, key, value):
        """해시 테이블에 데이터 삽입"""
        index = self._hash(key)
        for i, (k, v) in enumerate(self.table[index]):
            if k == key:
                self.table[index][i] = (key, value) # 기존 키가 있으면 값 업데이트
                return
        self.table[index].append((key, value)) # 없으면 리스트에 추가

    def search(self, key):
        """해시 테이블에서 데이터 검색"""
        index = self._hash(key)
        for k, v in self.table[index]:
            if k == key:
                return v # 값을 찾으면 반환
        return None # 값을 찾지 못하면 None 반환

    def delete(self, key):
        """해시 테이블에서 데이터 삭제"""
        index = self._hash(key)
        for i, (k, v) in enumerate(self.table[index]):
            if k == key:
                del self.table[index][i] # 해당 요소를 리스트에서 삭제
                print(f"'{key}' 삭제 완료")
                return
        print(f"'{key}'를 찾을 수 없다.")

    def display(self):
        """해시 테이블 내용 출력 (디버깅용)"""
        for i, bucket in enumerate(self.table):
            print(f"Bucket {i}: {bucket}")

# 사용 예시
if __name__ == "__main__":
    ht = HashTable()

    ht.insert("apple", 10)
    ht.insert("banana", 20)
    ht.insert("grape", 30)
    ht.insert("orange", 40) # apple과 같은 인덱스에 충돌 발생 가능 (해시 함수에 따라)
    ht.insert("mango", 50)
    ht.insert("apple", 15) # apple 값 업데이트

    print("--- 해시 테이블 내용 ---")
    ht.display()

    # 검색 예시
    print("\n--- 검색 결과 ---")
    print(f"apple: {ht.search('apple')}")
    print(f"orange: {ht.search('orange')}")
    print(f"kiwi: {ht.search('kiwi')}")

    # 삭제 예시
    print("\n--- 삭제 작업 ---")
    ht.delete("banana")
    ht.delete("kiwi")

    print("\n--- 삭제 후 해시 테이블 내용 ---")
    ht.display()

    print("\n--- 삭제 후 검색 결과 ---")
    print(f"banana: {ht.search('banana')}")`

**Python 코드 설명:**

- `HashTable` 클래스는 `__init__` 메서드에서 `size`를 받아 해시 테이블의 크기를 설정하고, 각 버킷을 위한 빈 리스트를 `table`에 초기화한다.
- `_hash` 메서드는 Python의 내장 `hash()` 함수를 사용하고 테이블 크기로 모듈러 연산을 수행하여 인덱스를 생성한다.
- `insert` 함수는 해시된 인덱스에 해당하는 리스트를 순회하여 키가 이미 존재하는지 확인한다. 존재하면 값을 업데이트하고, 없으면 `(키, 값)` 튜플을 리스트에 추가한다.
- `search` 함수는 해시된 인덱스의 리스트를 순회하여 키를 찾고 해당 값을 반환한다.
- `delete` 함수는 해시된 인덱스의 리스트에서 해당 키를 찾아 삭제한다.

---

<!-- ## **2. C를 이용한 체이닝 구현 예시**

C 언어에서 체이닝을 구현하려면 구조체와 포인터를 사용하여 연결 리스트를 직접 만들어야 한다.

```c
#**include** <stdio.h>
#**include** <stdlib.h>
#**include** <string.h>
#**define** TABLE_SIZE 10 // 해시 테이블의 크기// 해시 테이블에 저장할 노드 구조체 (키-값 쌍)

typedef struct Node {
    char* key;
    int value;
    struct Node* next; // 다음 노드를 가리키는 포인터 (연결 리스트)
} Node;

// 해시 테이블 구조체
typedef struct HashTable {
    Node* buckets[TABLE_SIZE]; // 각 버킷은 Node 포인터의 배열로 연결 리스트의 시작을 가리킴
} HashTable;

// 해시 함수 (간단한 예시)
unsigned int hash(const char* key) {
    unsigned int hash_value = 0;
    for (int i = 0; key[i] != '\0'; i++) {
        hash_value = hash_value * 31 + key[i]; // 31은 소수
    }
    return hash_value % TABLE_SIZE;
}

// 새로운 노드 생성
Node* createNode(const char* key, int value) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    if (newNode == NULL) {
        fprintf(stderr, "메모리 할당 실패\n");
        exit(EXIT_FAILURE);
    }
    newNode->key = strdup(key); // 키 복사
    newNode->value = value;
    newNode->next = NULL;
    return newNode;
}

// 해시 테이블 초기화
void initHashTable(HashTable* ht) {
    for (int i = 0; i < TABLE_SIZE; i++) {
        ht->buckets[i] = NULL; // 모든 버킷을 NULL로 초기화
    }
}

// 해시 테이블에 데이터 삽입
void insert(HashTable* ht, const char* key, int value) {
    unsigned int index = hash(key);
    Node* newNode = createNode(key, value);

    // 해당 인덱스의 버킷이 비어있으면 새 노드를 바로 삽입
    if (ht->buckets[index] == NULL) {
        ht->buckets[index] = newNode;
    } else {
        // 충돌 발생: 연결 리스트의 끝에 추가
        Node* current = ht->buckets[index];
        while (current->next != NULL) {
            // 중복 키 처리 (기존 값 업데이트)
            if (strcmp(current->key, key) == 0) {
                current->value = value;
                free(newNode->key); // 새로 할당된 키 메모리 해제
                free(newNode); // 새로 할당된 노드 메모리 해제
                return;
            }
            current = current->next;
        }
        // 마지막 노드에서 중복 키 확인
        if (strcmp(current->key, key) == 0) {
            current->value = value;
            free(newNode->key);
            free(newNode);
            return;
        }
        current->next = newNode;
    }
}

// 해시 테이블에서 데이터 검색
int* search(HashTable* ht, const char* key) {
    unsigned int index = hash(key);
    Node* current = ht->buckets[index];

    while (current != NULL) {
        if (strcmp(current->key, key) == 0) {
            return &(current->value); // 값을 찾으면 해당 값의 주소 반환
        }
        current = current->next;
    }
    return NULL; // 값을 찾지 못하면 NULL 반환
}

// 해시 테이블에서 데이터 삭제
void delete(HashTable* ht, const char* key) {
    unsigned int index = hash(key);
    Node* current = ht->buckets[index];
    Node* prev = NULL;

    while (current != NULL) {
        if (strcmp(current->key, key) == 0) {
            if (prev == NULL) { // 첫 번째 노드인 경우
                ht->buckets[index] = current->next;
            } else {
                prev->next = current->next;
            }
            free(current->key);
            free(current);
            printf("'%s' 삭제 완료\n", key);
            return;
        }
        prev = current;
        current = current->next;
    }
    printf("'%s'를 찾을 수 없다.\n", key);
}

// 해시 테이블 메모리 해제
void freeHashTable(HashTable* ht) {
    for (int i = 0; i < TABLE_SIZE; i++) {
        Node* current = ht->buckets[i];
        while (current != NULL) {
            Node* temp = current;
            current = current->next;
            free(temp->key);
            free(temp);
        }
    }
}

int main() {
    HashTable ht;
    initHashTable(&ht);

    insert(&ht, "apple", 10);
    insert(&ht, "banana", 20);
    insert(&ht, "grape", 30);
    insert(&ht, "orange", 40); // apple과 같은 인덱스에 충돌 발생 가능 (해시 함수에 따라)
    insert(&ht, "mango", 50);

    // 검색 예시
    int* val = search(&ht, "apple");
    if (val != NULL) {
        printf("apple: %d\n", *val);
    } else {
        printf("apple Not Found\n");
    }

    val = search(&ht, "orange");
    if (val != NULL) {
        printf("orange: %d\n", *val);
    } else {
        printf("orange Not Found\n");
    }

    val = search(&ht, "kiwi");
    if (val != NULL) {
        printf("kiwi: %d\n", *val);
    } else {
        printf("kiwi Not Found\n");
    }

    // 삭제 예시
    delete(&ht, "banana");
    delete(&ht, "kiwi");

    val = search(&ht, "banana");
    if (val != NULL) {
        printf("banana: %d\n", *val);
    } else {
        printf("banana Not Found\n");
    }

    freeHashTable(&ht); // 메모리 해제
    return 0;
}
```

**C 코드 설명:**

- `Node` 구조체는 각 키-값 쌍과 다음 노드를 가리키는 `next` 포인터를 포함하여 연결 리스트의 노드를 구성한다.
- `HashTable` 구조체는 `Node` 포인터의 배열인 `buckets`를 가지고 있으며, 각 `buckets[i]`는 해당 인덱스의 연결 리스트의 헤드를 가리킨다.
- `hash` 함수는 간단한 문자열 해시 함수를 사용하여 키를 인덱스로 변환한다. 실제 응용에서는 더 정교한 해시 함수를 사용해야 한다.
- `insert` 함수는 해시된 인덱스에 해당하는 연결 리스트에 새로운 노드를 추가한다. 이미 키가 존재하면 값을 업데이트한다.
- `search` 함수는 해시된 인덱스의 연결 리스트를 순회하여 키를 찾고 해당 값의 포인터를 반환한다.
- `delete` 함수는 해시된 인덱스의 연결 리스트에서 해당 키를 찾아 삭제한다.
- `freeHashTable` 함수는 동적으로 할당된 모든 노드의 메모리를 해제하여 메모리 누수를 방지한다.

--- -->
