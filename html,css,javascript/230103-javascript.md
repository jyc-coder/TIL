
## Javascript 

### 자료형 확인
```js
const data = {
  string: '123',
  number: 123,
  boolean: true,
  null: null,
  undefined: undefined,
  symbol: Symbol('Hello'),
  bigint: 123n,
  array: [],
  object: {},
  function: function () {}
}


function checkType(d) {
  return Object.prototype.toString.call(d).slice(8, -1)
}
console.log(checkType(data.string) === 'String')
console.log(checkType(data.string)) // 'String'
console.log(checkType(data.number)) // 'Number'
console.log(checkType(data.boolean)) // 'Boolean'
console.log(checkType(data.null)) // 'Null'
console.log(checkType(data.undefined)) // 'Undefined'
console.log(checkType(data.symbol)) // 'Symbol'
console.log(checkType(data.bigint)) // 'BigInt'
console.log(checkType(data.array)) // 'Array'
console.log(checkType(data.object)) // 'Object'
console.log(checkType(data.function)) // 'Function'

```


### 변수

const 변수 
- 중괄호 유효범위
- 값을 재할당 할 수 없다
- 중복 선얼을 할 수 없다.
- 호이스팅이 되지 않는다
	- 호이스팅? 
    코드가 실행하기 전 변수선언/함수선언이 해당 스코프의 최상단으로 끌어 올려진것 같은 현상을 말한다.
    내가 작성한 글도 있으니 확인해보자
    링크: https://velog.io/@jhs000123/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-var%EC%99%80-%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85
- 전역에서 선언한다고 해도 전역 객체의 속성으로 등록되지 않는다.


let 변수

- 중괄호 유효범위
- 재할당이 가능하다
- 중복선언은 불가능하다
- 호이스팅이 되지 않는다.
- 전역에서 선언한다고 해도 전역 객체의 속성으로 등록 되지 않는다.


### 구문

- if

`if`, `else` 키워드를 사용해서 구문을 작성한다.

```js
if(option){
	//option의 값이 참이면 실행되는 내용
}

if(option){
  // option의 값이 참이면 실행되는 내용
} else{
  // option의 값이 거짓일때 실행될 코드 
}

if(option){
  // option의 값이 참이면 실행되는 내용
} else if(option 2){
  // option 2 의 값이 참 일때 실행될 코드 
} else if(option 3){
  // option 3 의 값이 참 일때 실행될 코드 
}
```

- switch
`switch`,`case`,`break`,`default`키워드를 써서 구문을 작성한다. 

```js
switch(option){
  case 값1:
    // option의 값이 '값1'이면 실행되는 구문
    break // 동작했다면 여기서 종료!
  case 값2:
    // option의 값이 '값2'이면 실행되는 구문 
    break
  case 값3:
    // option의 값이 '값3'이면 실행되는 구문
    break
  default:
    // option의 값이 세가지 모두 해당되지 않으면 실행되는 구문
```

- for 

```js
for(시작 조건; 종료 조건; 증감){
	// code
}

for( let i = 0; i < 10; i += 1) {
	console.log(i)
}
// 0 1 2 .... 9

```

- break

전체 반복을 종료함
```js
for (let i = 0; i < 10; i += 1) {
  if (i > 5) {
    break
  }
  console.log(i)
}
// 0
// 1
// 2
// 3
// 4
// 5
```

- continue

```js
for (let i = 0; i < 10; i += 1) {
  if (i % 2 === 0) {
    continue
  }
  console.log(i)
}
// 1
// 3
// 5
// 7
// 9
```

현재 반복을 종료하고 다음 반복으로 넘어감


- for of  
반복 가능한 객체를 반복한다 (array,Map,Set,String,TypedArray)
```js
  for (v of strlist){
        answer.push(v.length)
    }
```

- for in
객체 데이터를 반복 객체안의 key 갯수만큼 반복

```js
const user = {
  name: 'jyc',
  age: 28,
  isValid: true,
  email: 'jyc4648@gmail.com'
}

for (const key in user) {
  console.log(key, user[key])
}
// name jyc
// age 28
// isValid true
// email jyc4648@gmail.com
```


- while
option이 참인동안에 반복된다

주의 - 잘못 사용하면 무한 루프에 빠져서 컴퓨터 크래쉬 될수 있음

```js
while(option){
	//
}
```

### 연산자 

- 산술

기호 | 뜻
:--:|:--:|
+| 더하기
- | 빼기
* | 곱하기
/ | 몫
% | 나머지

- 할당

`const a = 2` : 'const 형태로 선언한 a라는 변수에 2라는 값을 `할당`하거라'
`=` 


- 증감

```
let hamburger = 3

console.log(hamburger++) // 3?? 먼저 값출력하고 난 다음에 더해
console.log(hamburger) // 4


console.log(++hamburger) // 4?? 먼저 더한다음에 출력해
console.log(hamburger) // 4
```

`hamburger++`과 `++hamburger`는 달라요
