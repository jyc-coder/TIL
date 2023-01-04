
## Javascript

### 연산자

- 부정

참과 거짓의 반댓값을 불린(Boolean)데이터로 반환한다.

```js
console.log(!true) // false
```


- 비교

두개의 피연산자들의 값을 비교하여 불린 데이터로 반환한다.
```js
// 일치
console.log(180 === 170) // false

// 불일치
console.log(180 !== 170) // true

// 큼
console.log(170 > 180) // false

// 크거나 같음
console.log(170 >= 180) // false

// 작음
console.log(170 < 180) // true

// 작거나 같음
console.log(170 <= 180) // true
```

물론 `==`연산자가 존재하지만 사용하면 의도치 않은 오류가 발생할수 있기 때문에 그냥 `===`를 사용하자.

- 논리

AND|OR
:--:|:--:|
모두 참|하나 이상 참
&& | \|\|
가장 먼저 만나는 참 데이터를 반환함| 마지막의 거짓 데이터를 반환함


- Nullisth 병합

or 연산자(||) 는 왼쪽에서부터 거짓을 만나면 다음으로 넘어가지만,`0`이나 `''`값을 유효값으로 사용한다면 원치 않는 결과가 생긴다.

이때 `??`연산자를 사용해서 Nullish(null 과 undafined를 만났을 때만 다음으로 넘어가기 때문에 유용하다.

```js
// OR 연산자
const num1 = n || 6
console.log(num1) // 6

// Nullish 병합 연산자
const num2 = n ?? 6
console.log(num2) // 0
```


### 삼항연산자

```
조건 ? 참일때 실행되는 내용 : 거짓이면 여기가 실행됨
```
```js
const a = 2

console.log(a > 20 ? 'true' : 'false')
// a 가 20보다 크면 true, 아니면 false를 콘솔로 출력


function getAlert(message) {
  return message ? message : '메시지가 존재하지 않습니다!'
}

console.log(getAlert('안녕하세요~')) // '안녕하세요~'
console.log(getAlert()) // '메시지가 존재하지 않습니다!'
```
아무것도 안넣었는데 왜 뒤에것이 나옴?

아무것도 입력을 하지 않으면 암시적으로 자바스크립트가 매개변수에 전달한 인수를 `undefined`라고 지정하기 때문에 조건문에서 `falsey` 로 파악을 하기 때문에 `:`뒤의 값이 나타나는 것이다.


### 전개 연산자 

배열이나 객체의 아이템이나 속성을 전개한다.
과자 봉지를 열어서 과자를 끄집어 내는 느낌

`배열`
```js
const a = [1, 2, 3]
const b = [4, 5, 6]
const c = a.concat(b) // b 과자봉지를 뜯어서 과자를 꺼낸 다음 a 과자봉지에 과자를 집어넣는다.
const d = [...a, ...b]

console.log(c) // [1, 2, 3, 4, 5, 6]
console.log(d) // [1, 2, 3, 4, 5, 6]
```

`객체`
객체는 박스로된 과자를 뜯어서 소포장된 과자를 옮기는 느낌으로 생각하면 이해가 쉽다.
```js
const a = { x: 1, y: 2 }
const b = { y: 3, z: 4 }
const c = Object.assign({}, a, b) 
const d = { ...a, ...b }

console.log(c) // { x: 1, y: 3, z: 4 }
console.log(d) // { x: 1, y: 3, z: 4 }
```

`인수 전개`
```js
const a = [1, 2, 3]
function fn(x, y, z) {
  console.log(x, y, z)
}
fn(...a) //과자를 꺼내서 다른 그릇에 집어넣는다
// 1 2 3
```


`나머지 매개변수` -> ...rest
과자를 꺼낼만큼 꺼내고 나머지는 과자봉지에 넣고 다시 포장한다는 느낌

```js
const a = [1, 2, 3, 4, 5, 6]
function fn(x, y, ...rest) {
  console.log(x, y, rest)
}
fn(...a)
// 1 2 [3, 4, 5, 6]
```

#### 구조 분해 할당

배열이나 객체의 구조에 맞게 바로 개별 변수에 값을 할당하는 방법 
제품을 다시 분해해서 재조립하는 느낌

```js
const arr = [1, 2, 3]
const [a, b, c] = arr

console.log(a, b, c) 
// 1 2 3

//b만 뽑아내고 싶은데?
const [, b] =arr // 첫번째 건너뛰고 2번째 항목의 값을 집어넣는다
console.log(b) // 2
```

`선언과 분리` 
변수를 지정해서 배열의 위치에 따라 그에 해당하는 값을 가지게 된다.
```js
const arr = [1, 2, 3]
let a, b, c
[a, b, c] = arr

console.log(a, b, c) 
// 1 2 3
```

`기본값` 

```js
const arr = [, , 3]
const [a = 0, b, c] = arr
// a의 값이 없으면 0으로 지정하자라는 내용이 있기 때문에 a가 0으로 출력됨

console.log(a, b, c)
// 0 undefined 3
```

`변수 값 교환`
```js
let a = 1
let b = 2
;[b, a] = [a, b] // 상대방이 앉았던 자리에 가서 앉으면 의자가 바뀐다.

console.log(a, b) 
// 2 1
```

`반환 값 무시`
```js
const arr = [1, 2, 3]
const [,, c] = arr // 변수가 c밖에 없으니 해당 위치의 값을 c만 가져오게 되는 것

console.log(c) 
// 3
```

`나머지 할당`
```js
const arr = [1, 2, 3]
const [a, ...rest] = arr 
// 배열과 비슷하다. 과자를 꺼내서 나머지는 다시 포장
console.log(a, rest) 
// 1 [2, 3]
```

#### 구조 분해

`기본`
```js
const obj = { a: 1, b: 2, c: 3 }
const { a, b, c } = obj

// a,b,c 같이 간단한 변수로 객채의 값을 불러오고 싶다면 사용하는 방법이다. 
console.log(a, b, c) 
// 1 2 3
```

`선언과 분리`

```js
const obj = { a: 1, b: 2, c: 3 }
let a, b, c
({ a, b, c } = obj) // 객체를 분리할때는 소괄호를 바깥에 작성해줘야 오류가 발생하지 않는다.

console.log(a, b, c)
// 1 2 3
```

`기본값`
```js
const obj = { c: 3 }
const { a = 0, b, c } = obj

// 배열과 비슷하다. obj에 존재하지 않지만 a의 기본값을 0으로 정의했기 때문에 0이 출력됨

console.log(a, b, c) 
// 0 undefined 3
```
`변수명 변경`
```js
const obj = { a: 1, b: 2, c: 3 }
const { a: x, b: y, c: z } = obj
// 각각의 속성에 해당하는 값을 따로 선언한 변수에 집어넣는다.
console.log(x, y, z) 
// 1 2 3
```

`기본값 + 변수명 변경`

```js
const obj = { c: 3 }
const { a: x = 0, b: y, c: z } = obj
// a 대신에 x라는 이름으로 변경하고 기 기본값을 0으로 교체할거야

console.log(x, y, z) 
// 0 undefined 3
```

`나머지 할당`
```js
const obj = { a: 1, b: 2, c: 3 }
const { a, ...rest } = obj
// 배열과 비슷한 방식으로 과자 하나 담고 나머지는 객체로 포장!
console.log(a, rest) 
// 1 { b: 2, c: 3 }
```

#### 선택적 체이닝

점 표기법에 `?`를 추가해서 `?.`로 사용하면 객체 속성이 없는 경우도 에러 없이 접근이 가능하다

```js
let user = null

console.log(user) // null 에러발생
console.log(user?.name) // undefined
            
```

```js
const userA = {
  name: 'jyc',
  age: 28,
  address: {
    country: 'Korea',
    city: 'hwasoon'
  }
}
const userB = {
  name: 'jys',
  age: 32
}

function getCity(user) {
  return user.address?.city || '집이 없어'
}
// 객체에 없는 속성을 찾으려고 할때 선택적 체이닝 덕분에 
//undefined가 나오게 되서 논리연산자에 따라 `집이없어`라는 문구가 나오게 되는 것이다.
  
console.log(getCity(userA)) // 'Seoul'
console.log(getCity(userB)) // '집이 없어'
```

### 함수

#### 함수 데이터와 함수 호출 

```js
function hello() {
  return 'Hello~'
} // 함수 선언!
console.log(hello) // ƒ hello() { return 'Hello~' } 
// 함수 내용 자체가 출력 
console.log(hello()) // 'Hello~' 
// 함수를 불러오는것 
```

#### 선언과 표현

`함수 선언문` 

1. function 키워드로 시작하는가?
2. 함수 이름을 지정했는가?


`함수 표현식`

변수에 함수를 할당하는 것

```
const hello = function() {
  console.log('Hello~')
}
```

기명 함수 선언과 동시에 변수에 할당하면 표현식이 된다. -> function 키워드로 시작하지 않으면 함수 표현식이라고 생각하면 편함

#### 호이스팅
함수 선언부가 유효범위 최상단으로 끌어올려지는 현상을 말합니다.

```js
hello1()
hello2() // Error!
// 함수 표현식은 호이스팅이 되지 않기 때문이다.

// 함수 선언문
function hello1() {
  console.log('Hello~1')
}

// 함수 표현식
const hello2 = function() {
  console.log('Hello~2')
}
```

 ' |함수 선언문|함수 표현식
:--:|:--:|:--:|
호이스팅 여부| 가능| 불가능 


위에서 호출하던 아래서 호출하던 똑같은거 아님?
> 함수 선언문이 수십줄이라고 생각해보면 위의 내용들을 전부 내린 뒤에 함수 호출을 보게되면 가독성이 떨어진다. 과자 이름보다 과자 성분표가 먼저 나타나게되면 소비자 입장에서는 가독성이 떨어지게 되는 느낌이랑 비슷하다.



