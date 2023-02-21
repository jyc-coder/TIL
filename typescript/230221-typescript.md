# Typescript

### Tuple

배열과 유사하지만 저해진 타입의 고정된 길이 배열을 표현한다.
```js
let tuple: [string, number]
tuple = ['a', 1]
tuple = ['a', 1, 2] // Error!
tuple = [1, 'a'] // Error!
```

튜플 타입의 배열을 사용할수 있다.

```js
let users: [number, string, boolean][]
// Or
// let users: Array<[number, string, boolean]>

users = [[1, 'Neo', true], [2, 'jyc', false], [3, 'Lewis', true]]
```

`.push()`나 `.splice()`등을 통해 값을 넣는 행위는 막을 수 없음
```js
let tuple: [string, number]
tuple = ['a', 1]
tuple = ['b', 2]
tuple.push(3)
console.log(tuple) // ['b', 2, 3]
tuple.push(true) // Error - TS2345: Argument of type 'true' is not assignable to parameter of type 'string | number'.
```

### Any

모든 타입을 의미한다.
따라서 일반적인 자바스크립트 변수와 동일하게 어떤 타입의 값도 할당이 가능하다.
외부 자원을 활용해 개발할 때 불가피하게 타입을 단언할 수 없는 경우 유용할수 있지만, 되도록이면 안쓰는 것이 좋다.

```js
let any: any = 123
any = 'Hello world'
any = {}
any = null
```

### Unknown

알수 없는 타입을 의미한다. 
Any와 같이 Unknown에는 어떤 타입의 값도 할당 할수 있지만, Unkown을 다른 타입에는 할당할수 없다.

```js
let a: any = 123
let u: unknown = 123

const boo1: boolean = a // 모든 타입(any)은 어디든 할당할 수 있습니다.
const any1: any = u // OK!
const num1: number = u // Error! - unknown은 any를 제외한 다른 타입에 할당할 수 없습니다.
const num2: number = u as number // 타입을 단언하면 할당할 수 있습니다.
```

### Object
기본적으로 `typeof`연산자가 `object`로 반환하는 모든 타입을 나타낸다.

```js
let obj: object = {}
let arr: object = []
let func: object = function () {}
let date: object = new Date()
// ...
```
타입 재사용하기 위해, `interface`나 `type`을 사용하는 것도 좋다

```js
interface User {
  name: string,
  age: number
}

const userA: User = {
  name: 'JYC',
  age: 123
}

const userB: User = {
  name: 'JYC',
  age: false, // Error
  email: 'jyc4648@gmail.com' // Error
}
```

### void

값을 반환하지 않는 함수에서 사용한다.
```js
function hello(msg: string): void {
  console.log(`Hello ${msg}`)
}
```

### Never 
절대 발생하지 않을 값을 나타내며, 어떠한 타입도 적용할수 없다.


```js
// Error! - 선언된 타입이 'void' 혹은 'any'가 아닌 함수는 return 문을 포함해야 합니다.(2355)
function hello(msg: string): undefined {
  console.log(`Hello ${msg}`)
}
```

### 유니언

2개이상의 타입을 허용하는 경우 이를 유니언 이라고한다.
`|`를 통해 타입을 구분하며 ()는 선택적으로 사용 가능

```js
let union: (string | number)
union = 'Hello type!'
union = 123
union = false // Error - TS2322: Type 'false' is not assignable to type 'string | number'.
```

### 인터섹션

`&`를 사용해서 2개 이상의 타입을 조합하는 경우, 이를 인터섹션이라고 한다. 인터섹션은 새로운 타입을 생성하지 안혹 기존의 타입들을 조합할수 있기 때문에 유용하다고 할수 있다.

```js
interface IUser {
  name: string,
  age: number
}
interface IValidation {
  isValid: boolean
}
 
```


### 함수(Function)
화살표 함수를 이용해 타입을 지정할수 있다.
인수의 타입과 반환 값의 타입을 입력한다.

```js
// myFunc는 2개의 숫자 타입 인수를 가지고, 숫자 타입을 반환하는 함수.
let myFunc: (arg1: number, arg2: number) => number
myFunc = function (x, y) {
  return x + y
}

// 인수가 없고, 반환도 없는 경우.
let yourFunc: () => void
yourFunc = function () {
  console.log('Hello world~')
}
```


## 타입 추론(Inference)
명시적으로 타입선언이 되어있지 않을때 얘가 알아서 타입을 추론해서 제공하는것을 말한다.

```js
let num = 12
num = 'Hello type!' // TS2322: Type '"Hello type!"' is not assignable to type 'number'.
```
num값을 숫자로 선언한 다음에 문자 데이터로 데이터를 변경하면 오류가 생기게 된다. 
숫자로 한다며, 갑자기 왜 문자가 나와?

타입스크립트가 타입을 추론하는 경우는 다음과 같다
- 초기화된 변수
- 기본값이 설정된 매개변수
- 반환 값이 있는 함수
```js

// 초기화된 변수 `num`
let num = 12

// 기본값이 지정된 매개 변수 `b` + 반환 값이 확실한 함수 `add`
function add(a: number, b = 2) {
  return a + b
}
```

## 타입 단언(Assertion)

타입스크립트가 타입추론을 통해 판단할수 있는 타입의 범주를 넘는경우, 더이상 추론하지 않도록 지시하는 것을 말한다.
프로그래머가 타입스크립트보다 타입에 대해 더 잘이해하고 있는 사오항에서 사용한다.

```js
// 타입 에러가 발생하는 코드!
const el = document.querySelector('.title')
el.textContent = 'Hello world!' // Error! - 'el' is possibly 'null'.(18047)
```
설렉터로 가져온 엘리먼트가 null일수도 있잖니

### non-null 단언 연산자
`!`단언 연산자로 Nullish(`null`|`undefined`)가 아님을 단언한다

```js
const el = document.querySelector('.title)
el!.textContent = 'hello world!' // ok
```
이 요소 확실하게 있단다 그냥 null 걱정하지 말고 해

### as 키워드 단언
`as` 키워드로 변수의 특정한 타입으로 단언한다.


```js
const el = document.querySelector('.title)
;(el as HTMLHeadingElement).textContent = 'Hello world!'
```
이 요소 확실하게 html엘리먼트 타입이란다. 걱정말고 해라

### 타입 가드
데이터가 참인 경우에만 동작하도록 작성한다.

```js
const el = document.querySelector('.title')
if (el) {
  el.textContent = 'Hello world!'
}
```
el이 참일때만 동작하니까 null이여도 신경안써!

### 할당 단언

```js
let num!: number
console.log(num) // OK!
```
할당은 나중에 할게 일단 해봐


## 타입 가드(Guards)
타입 가드를 제공하면 타입스크립트가 추론가능한 범위에서 타입을 보장할수 있음

```js
function log(val: string | number | boolean) {
  let res = 'Result => '

  // 가드 블록
  if (typeof val === 'number') {
    res += val.toFixed(2)
  }

  // 가드 블록
  if (typeof val === 'string') {
    res += val.toUpperCase()
  }

  console.log(res)
}

log(3.14159265358979) // 'Result => 3.14'
log('hello world') // 'Result => HELLO WORLD'
```
숫자 데이터면 소수점 2자리 남기고 나머지는 떼버리고, 문자데이터면 다 대문자로 바꿔버려!

`typeof`,`in` 그리고 `instanceof`연산자를 직접 사용하는 좀더 심플한 타입 가드를 사용할수도 있다.
```js
// `typeof` 입력한 값의 타입에 따른 동작내용 작성
function someFuncTypeof(val: string | number) {
  if (typeof val === 'number') {
    val.toFixed(2)
    isNaN(val)
  } else {
    val.split('')
    val.toUpperCase()
    val.length
  }
}

// `in` 입력한 값의 타입에서 해당 메소드가 존재하니? 
function someFuncIn(val: any) {
  if ('toFixed' in val) {
    val.toFixed(2)
    isNaN(val)
  } else if ('split' in val) {
    val.split('')
    val.toUpperCase()
    val.length
  }
}

// `instanceof` 인스턴스 값이 뭐니? 해당 값에 따른 동작 내용 작성
class Cat {
  meow() {}
}
class Dog {
  woof() {}
}
function sounds(ani: Cat | Dog) {
  if (ani instanceof Cat) {
    ani.meow()
  } else {
    ani.woof()
  }
}
```

## 인터페이스(interface)
인터페이스는 타입스크립트에서 객체를 정의하는 일종의 규칙이며 구조, 그리고 타입이다.
`interface`키워드와 함께 사용한다.
```js
interface User {
  name: string
  age: number
  isValid: boolean
}
```
### 선택 속성(Oprional properties)
`?`를 사용해서 선택 속성을 정의한다
`?`가 없는 다른 속성은 필수 속성이다.
```js
interface IUser {
  name: string,
  age: number,
  isValid?: boolean // 선택적 속성! 있어도 되고 없어도 되고
}
// `isValid`를 초기화하지 않아도 에러가 발생하지 않습니다.
const user: IUser = {
  name: 'Neo',
  age: 123
}
```

### 읽기 전용 속성(Readonly properties)
`readonly`키워드를 사용해 읽기 전용 속성을 정의한다.
```js
interface User {
  readonly name: string
  age: number
  isValid: boolean
}
const heropy: User = {
  name: 'Heropy',
  age: 85,
  isValid: true
}

heropy.age = 85 // Ok
heropy.name = 'Neo' // Error! - 읽기 전용 속성이므로 'name'에 할당할 수 없습니다.(2540)
```
쓰기는 되지 않으므로 재할당을 할수 없음

만약 모든 속성이 읽기 전용이라면, 유틸리티 타입 혹은 단언(Assertion)을 활용할 수 있다.

```js
interface User {
  readonly name: string,
  readonly age: number
}
```
아니면
```js
interface User {
  name: string,
  age: number
}
const user: Readonly<User> = {
  name: 'Neo',
  age: 36
}
```
아니면
```js
const user = {
  name: 'Neo',
  age: 36
} as const // 단언!
```
이런식으로 읽기 전용속성을 부여한다.

### 함수 타입

함수 타입을 인터페이스로 정의하는 경우, 호출 시그니처를 사용한다. 호출시그니처는 다음과 같이 함수의 매개변수와 반환타입을 지정한다.

```js
interface Name {
  (매개변수: 변수타입): 반환타입 // Call signature
}
```

### 클래스 타입

인터페이스로 클래스를 정의하는 경우, `implements`키워드를 사용한다.

```js
interface UserInterface {
  name: string
  getName(): string
}
class User implements UserInterface {
  public name
  constructor(name: string) {
    this.name = name
  }
  getName() {
    return this.name
  }
}

const neo = new User('Neo')
neo.getName() // Neo
```
