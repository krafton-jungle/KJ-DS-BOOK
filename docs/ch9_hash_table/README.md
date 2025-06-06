# Hash Table

많은 애플리케이션에서 데이터의 빈번한 삽입, 삭제, 검색이 일어나고 있으며 이를 처리하기 위한 자료구조가 필요하다. 이때 해시테이블은 이를 구현하기 위해 효과적인 자료구조이다.

해시테이블은 키-값 쌍으로 데이터를 저장하는 자료구조이며, 해시 함수를 통해 키를 해시값으로 변환하여 이 함수의 출력값을 인덱스로 사용한다. 이를 통해 자료가 저장되어 있는 배열을 일일이 탐색하지 않고 해당 인덱스로 직접 접근할 수 있어 뛰어난 성능을 제공한다.

해시 테이블에서 요소를 검색하는 것이 연결 리스트에서 요소를 검색하는 것만큼 오래 걸릴 수 있다.(최악의 경우 시간복잡도 O(n)). 그러나 실제로 해싱은 매우 뛰어난 성능을 보이며, 요소를 검색하는 평균 시간복잡도는 O(1)이다.
Python의 내장 딕셔너리 또한 해시테이블로 구현되어 있다.

해시 테이블의 동작 원리를 확인하기에 앞서 우선 해시함수의 동작을 살펴본 뒤, 해시테이블의 동작 과정과 마지막으로 해시 충돌 문제를 어떻게 해결할 것인지에 관해 확인하겠다.

## 주요 키워드
| 항목 | 설명 |
| --- | --- |
| [해시 함수 (Hash Function)](/docs/ch9_hash_table/what_hash.md) |  |
| [해시 테이블 (Hash Table)](/docs/ch9_hash_table/hash_table.md) | 해시맵과 해시테이블의 차이와 해시테이블의 정의설명  |
| [적재율 (Load Factor)](/docs/ch9_hash_table/hash_table.md#load_factor) |  |
| [해시 충돌 (Hash Collision)](/docs/ch9_hash_table/hash_collision.md) |  |
| [개방주소법 (Open Addressing)](/docs/ch9_hash_table/open_addressing.md) |  |
| [체이닝 (Chaining)](/docs/ch9_hash_table/chaining.md#chaining) | 연결 리스트로 해시 충돌을 해소하는 방법 |