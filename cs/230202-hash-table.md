## 해시테이블 

- 연관배열구조 자료구조입니다.
- 키를 이용하여 값을 알 수 있다.

### 장점
- 빠른 데이터 읽기, 삽입, 삭제 = 0(1)
- 해쉬 함수를 통하혀 공간을 호율적으로 사용

### 단점
- 메모리를 많이 차지하고 충돌 위험이 존재한다


### 해시
단방향 암호화 기법으로 해시함수를 이요해서 고정된 길이의 암호화된 문자열로 바꿔버리는 것

### 해시값
매핑 후 데이터의 값

### 해시 함수
- 키를 해시(hash)로 바꿔주는 역할을 한다. 여러 키를 분할하기 위해 키를 해시값으로 매칭시키는 역할을 한다.
- 다양한 길이를 가지고 있는 키를 일정한 길이를 가지는 해시로 바꿔서 저장소를 효율적으로 운영할수 있도록 한다.


### 해시 충돌

서로 다른 키가 같은 해시가 되는 경우를 해시 충돌(hash collision)이라고 한다.

- 해시 함수를 이용해서 키값을 구했는데 동일시 되는 것이 두개가 있으면 충돌이 일어나게 된다.
이때 같은 인덱스에 저장하고 데이터는 연결리스트 방식으로 저장된다.


### 충돌 해결 방법 

- 체이닝 : 충돌이 발생하면 연결 리스트에 추가하는 방식
- 개방 주소법 : 충돌이 발생하면 다른 버킷에 데이터를 삽입하는 방식
	- 선형 탐색 : 충돌시 바로 다음 버킷에 저장
    - 제곱 탐색 : 충돌시 제곱만큼 건너뛴 버킷에 데이터를 저장
    - 이중 해시 : 해시 충돌시 다른 해시함수를 한번더 적용한 결과를 저장
    

## 셋

- 데이터의 중복을 허용하지 않는 자료구조

특징
- 해시 테이블을 사용한다 해서 hash set이라고 부른다.
- 자료 접근시 인덱스 값은 사용하지 않고 키만 사용한다
- 인덱스가 존재하지 않음


장점

- 충돌이 날 경우가 없다
- 빠른 속도로 검색이 가능하다


## Object vs Map

구분 | Object | Map
:--:|:--:|:--:|
키의 타입 | string,symbol|모든 값
크기 | 수동 추적 | 내부 프로퍼티
사용 경우| 적용해야하는 로직이 있는 경우 | 실행히 키를 모르고 모든키와 값들이 동일한 경우


- Object는 size가 존재하지않지만 Map은 size를 통해서 크기를 얻을수 있다.
- 각 개별요소에 대해 적용해야하는 로직이 있을 경우에는 Object를 사용해야하고, Object.defineProperty 메서드를 사용해야한다.
- Map의 경우는 이터러블 객체가 반환되어 for ..of 함수를 사용할수 있고, 배열 스프레드 오퍼레이터와, 배열 비구조화 할당이 사용가능하다.


## Set vs Array

구분 | Set | Array
:--:|:--:|:--:|
중복허용 | O | X
인덱스 참조|불가능|가능

- length 프로퍼티에 대해 수정할 수 있으며, Array.prototype.length는 Setter를 가지고 있고, 이를 수정해서 요소의 개수와 배열의 길이가 다른 희소배열이 생긴다.

- Set 생성자를 가지고 생성한 이터러블 객체는 getter 만을 가지고 있어서 읽기 전용이다.


