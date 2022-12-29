## javaScript 자료형


- 심볼

변경 불가 데이터
유일한 식별자를 만들어 데이터를 보호하는 용도로 사용할수 있다.
심볼에 추가하는 설명은 단순 디버깅을 위한 용도일 뿐, 심볼 값과는 관계가 없다.


```java

// Symbol('설명')
const sKey = Symbol('Hello!')
const user = {
  key: '흔한 정보!',
  [sKey]: '위험한 정보!'
}

console.log(user.key) // '흔한 정보!'
console.log(user['key']) // '흔한 정보!'
console.log(user[sKey]) // '위험한 정보!'
console.log(user[Symbol('Hello!')]) // undefined
```

- BigInt

길이 제한이 없는 정수
숫자 데이터가 안정적으로 표시할수 있는 최대치 보다 큰 정수를 표현할때 사용한다.




※ 형변환
```js
const a = 11n // BigInt
const b = 22 // 숫자

// 숫자 => Bigint
console.log(a + BigInt(b)) // 33n

// Bigint => 숫자
console.log(Number(a) + b) // 33
```


### 참조형

- 배열

배열(array)은 같은 타입의 변수들로 이루어진 유한 집합

```js
const fruits = ['apple','banana','watermelon']

// 리터럴 
fruits = ['apple', 'Banana', 'cherry]
          
 // 배열의 아이뎀 인덱싱
 console.log(fruits[0]) // 'apple'
 
 // 배열의 길이
 console.log(fruits.length) // 3

// 첫번째 아이템 인덱싱
console.log(fruits[0]) // 'apple'

// 마지막 아이템 인덱싱

console.log(fruits[fruits.length -1]) //'cherry'
console.log(fruits.at(-1)) //'cherry'

```

- 객체

`Key:Value(속성:값)` 형태로 더 복잡한 데이터 구조를 표시할때 사용한다.

```js
const user = {
  name: 'jyc',
  age: '27',
  email: 'jyc4648@gmail.com',
}

// 다른 방법
function User() {
  this.name = 'jyc'
  this.age = '27
}
user = new User()


console.log(user) // {name: 'jyc', age: '27', email: 'jyc4648@gmail.com'}
```

※ 점 표기법과 대괄호 표기법
객체의 멤버를 2가지 방법을 통해 접근할수 있다.

```js
const user = {
  name: 'jyc',
  age: '27',
  email: 'jyc4648@gmail.com',
}

// 점 표기법
console.log(user.name) // 'jyc'
console.log(user.age) // '27'

// 대괄표 표기법

console.log(user['name']) // 'jyc'
console.log(user['age'] // '27'

```
※ 속성 지우기

```js
const user = {
  name: 'jyc',
  age: '27'
}

delete user.age

console.log(user) // {name: 'jyc'}

```

속성의 이름은 고유하므로 중복된 속성이 존재한다면, 마지막에 작성된 속성으로 값이 할당된다.

```js
const user = {
	name: 'jyc'
    age: 85, // X
    age : 27
}
```

점 표기법과 대괄호 표기법은 체이닝으로 작성이 가능함
```js
const userA = {
  name: 'jyc',
  age: 28,
  email: 'jyc4648@gmail.com',
}

const userB = {
  name: 'jys',
  age: 32,
  brother: userA,
}

console.log(userB.brother.name) // 'jyc'

console.log(userB['brother']['name']) // 'jyc'

```

- 함수 

자바스크립트에서 함수는 1급 개체로, 하나의 값으로 변수나 인수 혹은 반환이 가능하다

```js
function hi() {
  return 'how are you?'
}

console.log(hi()) // 'how are you?'

```

### 형변환

동등 연산자(==)는 두 피연산자의 값이 서로 같으면 참(true)을 반환하다. 이때 두 피연산자의 타입이 서로 다르다면 비교를 위해 강제로 형태의 변환이 일어난다.

반대로 일치 연산자(===)는 타입의 변환없이 두 피연산자의 타입까지 같은가의 여부를 따지게 된다.


```
const a = 1
const c = '1'

console.log(a === c) // false 
console.log(a == c) // true
```



