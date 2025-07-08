# 콜 스택 (Call Stack)

> **요약** : 콜 스택은 프로그램이 **함수를 호출**하고 **반환**하는 과정을 추적하기 위해 사용하는 **LIFO(후입선출)** 구조의 메모리 영역이다. 각 호출마다 **스택 프레임(stack frame, activation record)**이 쌓이고 함수가 종료되면 해당 프레임이 제거된다.

---

## 목차
1. [콜 스택이란?](#콜-스택이란)
2. [스택 프레임의 구성](#스택-프레임의-구성)
3. [함수 호출 시퀀스](#함수-호출-시퀀스)
4. [예제 1 – 순차적 함수 호출](#예제-1--순차적-함수-호출)
5. [예제 2 – 재귀 호출](#예제-2--재귀-호출)
6. [스택 오버플로](#스택-오버플로)
7. [디버깅 / 분석 팁](#디버깅--분석-팁)
8. [참고 자료](#참고-자료)

---

## 콜 스택이란?
- **정의** : 실행 중인 프로그램의 **함수 호출 내역과 지역 컨텍스트**를 저장하는 메모리 영역.
- **동작 방식** : 후입선출(LIFO);
  - 가장 마지막에 호출된 함수가 가장 먼저 반환된다.
- **주요 목적** :
  1. 함수 인수와 지역 변수 보존
  2. 반환 주소(다음 실행 지점) 저장
  3. 레지스터(프레임 포인터 등) 보존

## 스택 프레임의 구성
| 항목 | 설명 |
|------|------|
| Return Address | 함수 종료 후 복귀할 코드 주소 |
| Saved FP / BP  | 이전 함수의 프레임 포인터(옵션) |
| Function Args  | 호출 시 전달된 값·참조 |
| Local Vars     | 함수 내부에서 선언된 변수 |
| Padding        | 구조체 정렬·ABI 요구사항 충족용 |

> **레지스터**
> - **SP(Stack Pointer)** : 스택의 최상단을 가리킴.
> - **FP/EBP/RBP** (x86)·**FP/X29** (ARM64) : 현재 프레임의 기준 위치.

## 함수 호출 시퀀스
```text
Caller                         Callee
------                         ------
(1) 인수 푸시/레지스터 세팅  →
(2) CALL 명령 (return addr 푸시)
(3)                   ← 스택 프레임 생성
(4)                   ← 로컬 변수 공간 확보
...
(5) 함수 본문 실행
(6) 로컬 변수 정리
(7)                   → 스택 프레임 제거
(8) RET (pop & jump)  → 이전 위치로 복귀
```

## 예제 1 – 순차적 함수 호출
```c
void D() { float  d = 40.5f; }
void C() { double c = 30.5;  printf("C\n"); }
void B() { int    b = 20;    C(); printf("B\n"); }
void A() { int    a = 10;    B(); printf("A\n"); }
int main() {
    A();   // A → B → C → (return) → B → A
    D();   // 마지막에 D
    return 0;
}
```
실행 순서
1. `main()` 프레임 생성
2. `A()` 호출 → 프레임 푸시
3. `B()` 호출 → 프레임 푸시
4. `C()` 호출 → 프레임 푸시
5. `C()` 반환 → 프레임 팝
6. `B()` 계속 → `printf("B")` 후 반환
7. `A()` 계속 → `printf("A")` 후 반환
8. `D()` 호출 → 새 프레임 푸시 후 반환
9. `main()` 종료 → 프로그램 종료

## 예제 2 – 재귀 호출
```c
void f(int n) {
    printf("PUSH %d\n", n);
    if (n > 1) { f(n-1); f(n-1); }
    printf("POP  %d\n", n);
}
```
`f(3)` 호출 시 스택 동작 (축약)
```
PUSH 3
  PUSH 2
    PUSH 1
    POP  1
    PUSH 1
    POP  1
  POP  2
  PUSH 2
    ... (대칭)
  POP  2
POP  3
```
- **깊이**: 최대 `n` (여기선 3)
- **총 프레임 수**: 완전 이진 트리이므로 `2^(n)−1 = 7`

### 스택 오버플로 위험
과도한 재귀 깊이 → 스택 공간 고갈 → **StackOverflow / Segmentation Fault**


## 스택 오버플로
- **원인** : 무한 재귀, 거대한 로컬 배열 할당, 스택 크기 제한(윈도우: 1 MB 기본, 리눅스: 8 MB 기본 등).
- **증상** : `Segmentation fault (core dumped)` 또는 `stack overflow` 예외.
- **해결** :
  - 재귀 → 반복문 변환
  - 로컬 대용량 배열 → 힙(동적) 할당
  - 컴파일러 옵션으로 스택 크기 확대 (`-Wl,--stack=...`, `ulimit -s unlimited` 등)

## 디버깅 / 분석 팁
| 환경 | 스택 확인 방법 |
|------|---------------|
| **GDB/Lldb** | `bt` (backtrace), `frame n`, `info locals` |
| **Visual Studio** | Call Stack 창, Step Out(F10) |
| **Python** | 예외 시 Traceback, `traceback.print_stack()` |
| **Chrome DevTools** | Sources → Call stack 패널 |


## 참고 자료
- ["stack-frame-layout-on-x86" – Eli Bendersky 블로그](https://eli.thegreenplace.net/2011/09/06/stack-frame-layout-on-x86-64#id9)
- [CPython bytecode evaluation loop (ceval.c)](https://github.com/python/cpython/blob/main/Python/ceval.c)
- [GeeksforGeeks: Function Call Stack in C](https://www.geeksforgeeks.org/function-call-stack-in-c/)