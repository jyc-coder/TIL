# typescript

## 인터페이스

### 인덱싱 가능 타입
수많은 속성을 가지거나 단언할수 없는 임의의 속성이 포함되는 구조에서는 인덱스 시그니처를 사용할 수 있다.

```js
interface INAME {
  [인덱서이름: 타입]: 반환타입 // Index signature
}
```
인덱스 시그니처와 명시적으로 정의된 속성을 함께 사용할 수 있다.

```js
interface User {
  [key: string]: string | number
  name: string,
  age: number
}
const user: User = {
  name: 'Neo',
  age: 123,
  email: 'thesecon@gmail.com',
  isAdult: true // Error! - 'boolean' 형식은 'string | number' 형식에 할당할 수 없습니다.(2322)
}
```

만약 인덱스 시그니처가 없으면, 정의되지않은 새로운 속성을 사용할수 없다.
```js
interface User {
  // [key: string]: unknown
  name: string
  age: number
}
const jyc: User = {
  name: 'jyc',
  age: 28
}
console.log(heropy['name'])
console.log(heropy['age'])
jyc['name'] = 'jyc'
jyc['age'] = 28
console.log(heropy['isValid']) // Error!
jyc['isValid'] = true // Error! 
```

#### 타입 인덱싱
```js
interface User {
  name: string,
  age: number
}

const a: User['name'] = 123 // Error! - name은 타입이 string 이잖아! 숫자는 안돼!
```


### 인터페이스 확장

인터페이스도 클래스처럼 `extends`키워드를 활용해 상속할 수 있습니다.

```js
interface Animal {
  name: string
}
interface Cat extends Animal {
  meow(): string
}

class Cat implements Cat {
  constructor(public name: string) {}
  meow() {
    return 'MEOW~'
  }
}
const cat = new Cat('Luxy')
```
Cat 인터페이스를 사용한 Cat 클래스에는 name 속성을 입력해야한다.

## 타입 별칭
`type` 키워드를 사용해서 새로운 타입 조합을 만들수 있다.
하나 이상의 타입을 조합해서 이름을 부여하며, 정확히는 조합한 각 타입들을 참조하는 별칭을 만드는 것.

```js
type MyType = string
type YourType = string | number | boolean
type User = {
  name: string,
  age: number,
  isValid: boolean
} | [string, number, boolean]
```

## 제네릭
제네릭은 재사용을 목적으로 함수나 클래스의 선언 시점이 아닌, 사용시점에 타입을 선언할수 있는 방법이다.
다음 예제에서 매개변수는 Number 타입만 허용하기 때문에, String 타입 인수의 함수 호출은 에러가 발생한다.


```js
function toArray(a: number, b: number): number[] {
  return [a, b]
}
toArray(1, 2)
toArray('1', '2') // Error - TS2345: Argument of type '"1"' is not assignable to parameter of type 'number'.
```

근데 나는 문자로 받고싶은데?
제네릭을 사용하면, 다양한 타입을 인수로 사용할수 있게된다.
`T`는 타입변수라고 하며, 사용자가 제공한 타입으로 변환될 식별자로 다른 이름으로 변경할수 있다.
매개변수와 비슷한 개념이다.
```js
function toArray<T>(a: T, b: T): T[] {
  return [a, b]
}

.
.
.

toArray<number>(1,2) // 숫자로 해야지
toArray<string>('1','2') // 이번에는 문자로
toArray<string | number>(1, '2')
toArray<number>(1, '2') // 숫자라며?
```

타입 추론을 활용해, 사용 지점에 인수 타입을 제공하지 않을 수도 있다.

```js
function toArray<T>(a: T, b: T): T[] {
  return [a, b]
}

toArray(1, 2)
toArray('1', '2')
toArray(1, '2') // Error!
```

### 제약조건
인터페이스나 타입 별칭에서 제네릭을 작성할수 있다.
`extends` 키워드를 사용하는 제약 조건을 추가할수 있다.

```js
interface MyType<T extends string | number> {
  name: string,
  value: T
}

const dataA: MyType<string> = {
  name: 'Data A',
  value: 'Hello world'
}
const dataB: MyType<number> = {
  name: 'Data B',
  value: 1234
}
const dataC: MyType<boolean> = { // Error! - 'boolean' 형식이 'string | number' 제약 조건을 만족하지 않습니다.(2344)
  name: 'Data C',
  value: true
}
const dataD: MyType<number[]> = { // Error! - 'number[]' 형식이 'string | number' 제약 조건을 만족하지 않습니다.(2344)
  name: 'Data D',
  value: [1, 2, 3, 4]
}
```

## 함수

### 명시적 this

`this` 의 타입을 명시적으로 다음과 같이 첫번째 가짜 매개변수로 선언이 가능함

```js

const cat = {
  name: 'Lucy',
  age: 3
}

function hello(message: string) {
  console.log(`Hello ${this.name}, ${message}`)
}
hello.call(cat, 'You are pretty awesome!')
```
자바스크립트에서는 이렇게 작성하면 문제 없지만, 타입스크립트의 경우에는 this가 뭔지 모르기 때문에 오류가 생긴다.

```js
interface ICat {
  name: string
  age: number
}
const cat: ICat = {
  name: 'Lucy',
  age: 3
}

function hello(this: ICat, message: string) {
  console.log(`Hello ${this.name}, ${message}`)
}
hello.call(cat, 'You are pretty awesome!')
```
따라서 이렇게 this가 무엇인지를 명시해줌으로써 this의 타입을 알게 해주어야한다.


### 오버로드 
타입스크립트의 함수 오버로드는 이름은 같지만 매개변수나 반환 타입이 다른 여러 함수를 가질수 있는 것을 말한다.
함수 오버로드를 통해 다양한 구조의 함수를 생성하고 관리할수 있다.
단! 함수 선언부와 구현부의 매개변수 개수가 일치해야한다.

```js
function add(a: string, b: string): string
function add(a: number, b: number): number
function add(a: any, b: any) {
  return a + b
}
add('hello ', 'world~') // 'hello world'
add(1, 2) // 3
add('hello ', 2) // Error! - 이 호출과 일치하는 오버로드가 없습니다.(2769)
add(1, 'world~') // Error! - 이 호출과 일치하는 오버로드가 없습니다.(2769)
```


## 클래스

클래스 속성은 클래스 바디에 별도로 타입을 선언해야한다
> 클래스 바디는 클래스 이름 뒤에 작성하는 중괄호 `{}` 영역


```js
class Animal {
  public name: string
  constructor(name: string) {
    this.name = name
  }
}
class Cat extends Animal {
  getName() {
    return `Cat name is ${this.name}.`
  }
}
const cat: Cat = new Cat('Lucy')
console.log(cat.getName()) // Cat name is Lucy.
```

### 클래스 수식어

클래스 멤버에서 사용할수 있는 접근 제어자들이 존재한다.
접근 제어자는 클래스, 메서드 및 기타 멤버의 접근 가능성을 설정하는 객체 지향 언어의 키워드이다.

접근 제어자 | 의미 | 범위
---|---|---
`public` | 어디서나 자유롭게 접근 가능(생략 가능) | 속성, 메소드
`protected` | 나와 파생된 후손 클래스 내에서 접근 가능 | 속성, 메소드
`private` | 내 클래스에서만 접근 가능 | 속성, 메소드



## 모듈

### 내보내기와 가져오기

타입스크립트는 일반적인 변수나 함수, 클래스 뿐만 아니라 다음과 같이 인터페이스나 타입별칭도 모듈에서 내보낼수 있다.

```js
// 인터페이스 내보내기
export interface User {
  name: string,
  age: number
}

// 타입 별칭 내보내기
export type MyType = string | number

// 선언한 모듈(myTypes.ts) 가져오기
import { User, MyType } from './myTypes'

const user: User = {
  name: 'jyc
  age: 28
}

const something: MyType = true // Error - TS2322: Type 'true' is not assignable to type 'MyType'.
```
타입스크립트는 CommonJS/AMD/UMD 방식의 모듈을 위해 export = ABC, import ABC = require('abc')의 문법도 제공한다.
컴파일옵션에서 `esModuleInterop: true`를 제공하면, 단순히 ESM 기본 내보내기 방식으로 사용할수 있다.


### 외부 모듈의 타입선언

자바스크립트 처럼 import를 사용하면 에러가 발생한다. 타입스크립트 컴파일러가 확인할수 있는 모듈의 타입 선언이 없기 때문이다.

`typs/모듈.d.ts`
```js
declare module '모듈이름'{
	export = '내보내는 모듈 이름'
}
  
```
