
## JavaScript

`.splice()` : 대상 배열에 요소를 추가하거나 삭제하거나 교체한다. 대상배열의 원본이 변경되므로 사용에 주의할 것 

- 요소 추가  
```js
let list = [1,2,3,4,5]

list.splice(2,0,"추가된 요소") // 시작 인덱스, 제거 개수, 대채 요소내용 순서로 작성한다.


console.log(list) // [1,2,"추가된 요소",3,4,5]

```

- 요소 제거
```js
let list = [1,2,3,4,5]

list.splice(1,1) //1번 인덱스를 시작으로 하나 제거해줘

console.log(list) // [1,3,4,5]
```

- 요소 교체

```js
let list = [1,2,3,4,5]

list.splice(1,1,"이") //1번 인덱스 제거하고 맨 마지막에 적힌 내용으로 교체한다!

console.log(list) // [1,"이",3,4,5]
```

- 요소 추가 및 삭제
```js
let list = [1,2,3,4,5]

list.splice(0,0,"이","삼","사") // 0개 제거하고 0번 인덱스자리에 내용을 추가한다.

console.log(list)
```

`unshift()` : 새로운 요소를 대상 배열의 맨 앞에 추가하고 새로운 배열 길이를 반환한다. 대상 배열 원본이 변경된다.

```js
let list = [1,2,3,4,5]

list.unshift("하이")

console.log(list) //  ["하이",1,2,3,4,5]
```

`Array.from()` : 유사 배열을 실제 배열로 반환한다. 

```js

const arraylike = {
  0: 'ham',
  1: 'bur',
  2: 'ger',
  length: 3
}


console.log(arraylike.constructor === Array) // false
console.log(arraylike.constructor === Object) // true


Array.from(arraylike).forEach(item => console.log(item)) // "ham","bur","ger"

```
`Array.isArray()` : 배열 데이터인지 확인한다.
```js
const array =['ham','bur','ger']
const arraylike = {
  0: 'ham',
  1: 'bur',
  2: 'ger',
  length: 3
}

console.log(Array.isArray(array)) // true
console.log(Array.isArray(arraylike)) // false

```

#### Object

`Object.assign()` : 하나 이상의 출처 객체로부터 대상 객체로 속성을 복사하고 대상 객체를 반환한다.

```js
const target = { a: 1, b: 2 }
const source1 = { b: 3, c: 4 }
const source2 = { c: 5, d: 6 }
const result = Object.assign(target, source1, source2)

console.log(target) // { a: 1, b: 3, c: 5, d: 6 }
console.log(result) // { a: 1, b: 3, c: 5, d: 6 }
```

만약 원본 객체가 수정되는 것을 원하지 않으면 요소에 빈 객체를 추가해주면 된다.

```js
const target = { a: 1, b: 2 }
const source1 = { b: 3, c: 4 }
const source2 = { c: 5, d: 6 }
const result = Object.assign({}, target, source1, source2)

console.log(target) // { a: 1, b: 2 }
console.log(result) // { a: 1, b: 3, c: 5, d: 6 }
```

아니면 전개 연산자를 사용해도 된다.

```js
const target = { a: 1, b: 2 }
const source1 = { b: 3, c: 4 }
const source2 = { c: 5, d: 6 }
const result = {
  ...target,
  ...source1,
  ...source2
}

console.log(target) // { a: 1, b: 2 }
console.log(result) // { a: 1, b: 3, c: 5, d: 6 }
```

`Object.entries()` : 주어진 객체의 각 속성과 값으로 하나의 배열을 만들어서 요소로 추가한 2차원 배열을 반환한다.

```js
const jyc = {
  name: 'jyc',
  age: 28,
  favfood: 'hamburger',
  email: 'jyc4648@gmail.com'
}

console.log(Object.entries(jyc))
/* 
[
	["name", "jyc"],
    ["age","28"],
    ["favfood","hamburger"],
    ["email","jyc4648@gmail.com"]
    
]
}*/

```

`Object.keys()` : 주어진 객체의 속성 이름을 나열한 배열을 반환한다. 

```js
const jyc = {
  name: 'jyc',
  age: 28,
  favfood: 'hamburger',
  email: 'jyc4648@gmail.com'
}

console.log(Object.keys(jyc)) //["name","age","favfood","email"]
```
`Object.values()` : 주어진 객체의 속성 값을 나열한 배열을 반환한다.
```js
const jyc = {
  name: 'jyc',
  age: 28,
  favfood: 'hamburger',
  email: 'jyc4648@gmail.com'
}

console.log(Object.values(jyc)) //["jyc",28,"hamburger","jyc4648@gmail.com"]

```
`Object.freeze(), Object.isFrozen()` : 주어진 객체를 변경하지 못하게 동결시기고, 동결의 여부를 확인하는 메소드 

```js
const jyc = {
  name: 'jyc',
  age: 28,
  favfood: 'hamburger',
  email: 'jyc4648@gmail.com'
}

console.log(jyc) /* 
	{"name":"jyc"
    "age":"28"
    "favfood":"hamburger"
    "email":"jyc4648@gmail.com"}
    */

console.log(Object.isFrozen(jyc)) //false

Object.freeze(jyc) // 동결

jyc.age = 22

console.log(jyc) /* 
	{"name":"jyc"
    "age":"28"
    "favfood":"hamburger"
    "email":"jyc4648@gmail.com"}
    */ 
console.log(Object.isFrozen(jyc)) // true

```

`Object.seal(), Object.isSealed()`: 주어진 객체를 변경할 수 없도록 밀봉하거나, 밀봉 여부를 확인한다. 동결과 다른점은 밀봉후에도 속성값은 변경 가능하다. 
> 수정은 가능하지만 추가는 안됨

```js
const jyc = {
  name: 'jyc',
  age: 28,
  favfood: 'hamburger',
  email: 'jyc4648@gmail.com'
}

console.log(jyc) /* 
	{"name":"jyc"
    "age":"28"
    "favfood":"hamburger"
    "email":"jyc4648@gmail.com"}
    */

console.log(Object.isSealed(jyc)) //false

Object.seal(jyc) // 밀봉!

jyc.age = 22 //값이 수정되는것은 가능하다!

console.log(jyc) /* 
	{"name":"jyc"
    "age":"22"
    "favfood":"hamburger"
    "email":"jyc4648@gmail.com"}
    */ 
console.log(Object.isSealed(jyc)) // true

```

`Object.defineProperty()` : 주어진 객체에 속성을 추가하거나, 특성을 변경한다. 다른 사용자가 데이터를 가져오려고 할때 감시의 용도로 사용할 수 있다.

`속성:기본값` | 설명
:--:|:--:
`enumerable: false`	|속성의 열거 가능 여부
`configurable: false`	|속성의 수정(이미 존재할 때), 삭제 가능 여부
`writable: false`	|속성의 값 변경 가능 여부
`value: undefined`|	속성의 값
`get: undefined`|	속성의 Getter
`set: undefined`	|속성의 Setter


`Object.defineProperties()` : 주어진 객체에 여러 속성을 추가하거나 ,특성을 변경한다.

```js
const user = {}
Object.defineProperties(user, {
  name: {
    enumerable: true,
    value: 'jyc'
  },
  age: {
    enumerable: true,
    value: 28
  },
  email: {
    enumerable: true,
    configurable: true,
    writable: true,
    value: []
  },
  address: {
    value: '전남'
  }
})

console.log(user)
for (const key in user) {
  console.log(key)
}
// 'name'
// 'age'
// 'email'
```

#### Json
JSON(JavaScript Object Notation)는 데이터 전달을 위한 표준 데이터 포맷

- 문자,숫자, 불린,null,객체,배열만 사용
- 문자는 큰 따옴표만 사용
- 후행 쉼표 사용 불가
- `.json` 확장자 사용

`.stringify()` : JavaScript 데이터를 json 문자로 변환
```js
console.log(JSON.stringify('Hello world!')) // '"Hello world!"'
console.log(JSON.stringify(123)) // '123'
console.log(JSON.stringify(false)) // 'false'
console.log(JSON.stringify(null)) // 'null'
console.log(JSON.stringify({ name: 'Heropy', age: 85 })) // '{"name":"Heropy","age":85}'
// console.log(JSON.stringify({ name: 'Heropy', age: 85 }, null, 2))
console.log(JSON.stringify([1, 2, 3])) // '[1,2,3]'
```

`.parse()` : JSON 문자를 분석해서 javascript 데이터로 변환한다.
```js
console.log(JSON.parse('"Hello world!"')) // "Hello world!"
// console.log(JSON.parse('Hello world!'))
// console.log(JSON.parse("Hello world!"))
console.log(JSON.parse('123')) // 123
console.log(JSON.parse('false')) // false
console.log(JSON.parse('null')) // null
console.log(JSON.parse('{"name":"Heropy","age":85}')) // { name: 'Heropy', age: 85 }
console.log(JSON.parse('[1,2,3]')) // [1, 2, 3]
```


### 모듈 
이해 가능한, 보다 작은 단위로 나눠진것
자바스크립트에서 파악하면 모듈은 특정 데이터들의 집합이다. 
	
#### 내보내기(exports)

`기본 내보내기 (Default exports)`
- 이름 지정할 필요가 없다. 
- 모듈당 단 1번만 가능하다

`이름 내보내기 (Named exports)`
- 이름을 무조건 지어야한다
- 모듈당 n번 사용가능!

