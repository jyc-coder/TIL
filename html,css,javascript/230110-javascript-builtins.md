## Javascript

### Array

`.length` 배열의 길이(숫자)를 반환한다.

`.at()` : 대상 배열을 인덱싱한다. 음수값을 사용하면 뒤에서부터 인덱싱 하게 된다. 


`.concat()` :  대상 배열과 주어진 배열을 병합해 새로운 배열을 반환한다. 


`.every()` : 대상 배열의 모든 요소가 콜백 테스트를 통과(참 반환) 하는지 확인한다. 테스트가 실패하게 된다면 콜백은 실행되지 않고 `false`를 반환한다. 

`.filter()` : 대상 배열에서 콜백 테스트를 통과하는 모든 요소를 새로운 배열을 만들어서 반환한다.

`.find()` : 대상 배열에서 콜백 테스트를 통과하는 첫번째 요소를 반환한다.

`.findIndex()` : 대상 배열에서 콜백 테스트를 통과하는 첫번째 요소의 인덱스를 반환한다. 최초로 테스트가 통과된다면, 이후 콜백은 실행되지 않는다. 모든 테스트 실패시 `-1`을 반환

`.flat()` : 대상 배열의 모든 하위 배열네 지정한 깊이까지 이어붙인 새로운 배열을 반환한다.


`.forEach()`: 대상 배열의 길이만큼 주어진 콜백을 실행함

`.includes()` : 대상 배열이 특정 요소를 포함하고 있는지 

`.join()` : 대상 배열의 모든 요소를 구분자로 연결한 문자를 반환한다.

`.map()` : 대상 배열의 길이만큼 주어진 콜백을 실행하고, 콜백의 반환 값을 모아서 새로운 배열을 반환한다. 


`.pop()` : 대상 배열에서 마지막 요소를 제거하고 그 요소를 반환한다.

`.push()` : 대상 배열의 마지막에 하나 이상의 요소를 추가하고 ,배열의 새로운 길이를 반환한다. 

`.reduce()`: 대상 배열의 길이만큼 주어진 콜백을 실행하고 ,마짐가에 호출되는 콜백의 반환값을 반환한다. 


`.reverse()` : 대상 배열의 순서를 반전한다.


`.shift()` : 대상 배열에서 첫번째 요소를 제거하고 ,제거된 요소를 반환한다.

`.slice()` : 대상 배열의 일부를 추출해서 새로운 배열을 반환한다. 두번째 인수 직전까지 추출하고, 두번째 인수를 생략할 경우 대상 배열의 끝까지 추출한다.

`.some()` : 대상 배열의 어떤 요소라도 콜백 테스트를 통과하는지 확인한다.

`.sort()` : 대상 배열을 콜백의 반환값에 따라 정렬한다.
	콜백의 반환값 : 
    	- 음수: `a`를 낮은 순서로 정렬
        - `0` : tnstj qusrud djqtdma
        - 양수 : `b`를 낮은 순서로 정렬
   ```js
const numbers = [8, 99, 13, 22, 34, 43, 9, 12]

numbers.sort()
console.log(numbers) // [12,13,22,34,43,8,9,99]

numbers.sort((a, b) => a - b)
console.log(numbers) // [8,9,12,13,22,34,43,99]

numbers.sort((a, b) => b - a)
console.log(numbers) // [99,43,34,22,13,12,9,8]
   ```
   ```js
const users = [
  { name: 'Neo', age: 85 },
  { name: 'Amy', age: 22 },
  { name: 'Lewis', age: 11 }
]

users.sort((a, b) => a.age - b.age)
console.log(users) // [ Lewis객체, Amy객체, Neo객체 ]

// users.sort((a, b) => b.age - a.age)
// console.log(users) // [ Neo객체, Amy객체, Lewis객체 ]
   
   ```
   
`.unshift()` 새로운 요소를 대상 배열의 맨앞에 추가하고 새로운 배열의 길이를 반환한다. 이때 배열의 원본이 변경된다.

`Array.from()` : 유사 배열을 실제 배열로 반환한다.

```js
const arraylike = {
  0: 'A',
  1: 'B',
  2: 'C',
  length: 3
}
// 현재 객체 데이터

console.log(arraylike.constructor === Array) // false
console.log(arraylike.constructor === Object) // true

// arraylike.forEach(item => console.log(item)) // Error!
Array.from(arraylike).forEach(item => console.log(item))
// 'A'
// 'B'
// 'C'
```

`Array.isArray()` : 배열 데이터인지 확인한다.

```js
const array = ['A', 'B', 'C']
const arraylike = {
  0: 'A',
  1: 'B',
  2: 'C',
  length: 3
}

console.log(Array.isArray(array)) // true
console.log(Array.isArray(arraylike)) // false

```


