# Hash Map
Key를 사용해서 매핑된 Value를 빠르게 찾을 수 있는 자료구조

## 내부 구조
Array를 기반으로 만들고 key를 hash function에 넣어서 Array의 어떤 위치에 저장할지 결정

## ArrayList와의 비교
사람 이름으로 나이를 찾는다고 할 때,
ArrayList: [('David', 33), ('Alex', 35), ('Steve', 61), ('Nicole', 59)]
Steve의 나이를 찾으려면 처음부터 하나씩 순회해서 찾아야함 -> O(n)

HashMap: hash function을 통해 해당 이름(key)가 어느 index로 들어가는지 구하게 되므로
ageMap.get('Steve')를 하면 hash function으로 index 2에 있다는 것을 구해 내부 Array의 internalArray[2]로 접근 -> O(1)

HashMap은 결국 Array 기반으로 만들어졌기 때문에 index를 알면 빠르게 값을 조회할 수 있다.

## Hash Function
값을 넣었을 때, 숫자 값으로 만들어주는 함수
해당 숫자를 Array 크기에 맞게 index로 구하게 된다 -> Array 길이가 10 -> hash number % 10

저장: key를 hash Function으로 숫자로 변경 -> Array index로 변환 -> 해당 위치에 저장 O(1)
조회: key를 hash Function으로 숫자로 변경 -> Array index로 변환 -> 해당 위치의 key를 확인 후 정보 조회 O(1)
삭제: key를 hash Function으로 숫자로 변경 -> Array index로 변환 -> 해당 위치에서 key를 가진 정보 삭제 O(1)

이렇게 했을 때 index가 같은 값이 나오면 어떻게 하지?라는 질문이 생길 수 있는데 이런 것을 충돌(Collision)이라고 한다.

## Collision 해결 방법
Collision은 key를 index로 변환했을 때 같은 값이 나왔을 때를 의미하는데 이를 해결하는 방법이 몇가지 있다
Java는 기본적으로 Chaining을 사용.

### Chaining
한 인덱스에 하나만 저장하는게 아니라 같은 index를 가진 값들을 연결해서(chaining) 저장하는 것

하나의 index 2에 다 수의 value가 들어갈 수 있다는 의미인데 이렇게 되면
Array index로 변환하는 것까지는 같으나 해당 index에 연결된 데이터들을 순회해서 값을 찾아야 하기 때문에 충돌이 적을 수록 HashMap의 성능이 좋아진다
O(1) ~ O(n)

### Open Addressing
이미 변환된 index 자리에 다른 데이터가 있다면, 다른 비어있는 index를 찾아서 해당 자리에 값을 저장하는 것을 의미

빈칸을 찾는 방식 또한 여러가지가 존재
Linear Probing: 다음 칸 확인
Quadratic Probing: 1, 4, 9... 떨어진 곳을 확인
Double Hashing: 다른 Hash Function을 한번 더 사용

## Bucket (Hash 결과의 최종 도착지인 저장 공간)
하나의 바구니의 개념으로 Hash Map 내부의 Array의 각 index에 해당하는 칸을 bucket이라 함
bucket 안에는 데이터가 없을 수도 있고 충돌로 인한 다수가 있을 수도 있다

## Load Factor
Hash Mamp에 데이터가 얼마나 찼는지를 나타내는 비율
load factor = 저장된 데이터 개수 / bucket 개수
ㅉ
보통 load factor가 0.75 이상이 되면 충돌이 많아지기 때문에 사이즈를 늘리게 된다.
ㅉㄴ
## Resizing, Rehashing
load factor가 높아져서 Hash Map 내부의 Array 크기를 늘리는 것을 Resizing이라 한다.
Resizing을 하면 바로 따라오는게 Rehasing인데 Array 크기가 달라졌기 때문에 index를 구하는 계산도 달라지기 때문이다.
따라서 Rehashing은 기존 데이터를 새로 만들어진 Array크기에 맞게 다시 index를 계산해서 넣는 작업을 의미한다.

## 시간 복잡도
보통 HashMap의 시간 복잡도는 평균적으로 O(1)이나, Collision으로 인해 하나의 bucket에 데이터가 많이 쌓이게 되면 복잡도가 늘어나게 된다.
해당 index에 있는 bucket은 linkedList처럼 chaining되어 있기 때문에 최악의 경우 O(n)까지 늘어날 수 있음 (하나의 bucket의 모든 데이터가 들어갔을 때)

(Java) 충돌이 너무 심해지면 bucket 내부 구조가 LinkedList에서 Tree구조로 변경되기도 함 -> 최악은 피함

## key 비교
Hash Map은 bucket이 있기 때문에 index비교 -> key 조회로 진행

HashMap에서 같은 값이 되려면 hashCode도 동일해야하지만, hashCode가 같다고 같은 값은 아니다 (Collision 때문)

## 장단점
장점: key값을 통해 value를 빠르게 조회 가능하다
삽입, 조회, 삭제가 평균 O(1)
데이터 검색이 많은 경우 적합하다

단점: 순서 보장이 되지 않는다
충돌이 많으면 성능이 떨어진다 최대 O(n)
hashCode/equals 구현이 중요 -> 버그 생길 수 있음
Array/List보다 메모리를 더 많이 사용 -> hash function?

순서 보장되지 않는 단점을 해결하기 위해 LinkedHashMap을 사용 가능하고 정렬된 것을 원하면 TreeMap을 사용하면 된다.




