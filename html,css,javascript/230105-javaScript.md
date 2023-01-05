## Javascript

### 함수


#### 반환 및 종료

`return` 키워드로 함수 안에서 밖으로 데이터를 반환한다. 

```js
function hamburger(){
return '햄버거'
}

console.log(hamburger()) // '햄버거'
```

`return`은 함수를 종료시키기 때문에 그 이하 명령은 실행되지 않는다.

```js
function hamburger(){
	return '햄버거' //
  console.log('감자튀김') // 동작하지 않는다.
}
```

`return` 키워드를 사용하지 않거나 반환데이터를 쓰지 않으면, `undefined`가 반환된다.


```js
function hamburger(){
}

function cheeseburger(){
	return
}

console.log(hamburger()) // undefined
console.log(cheeseburger()) // undefined
```

#### 매개변수 패턴
`기본값`
```js
function hamburger(a, b = 'burger') {
  return a + b
}

console.log(hamburger('cheese')) // cheeseburger
```
`구조 분해 할당`

```js
const user = {
  name: 'jyc',
  age: 28,
  // email: 'jyc4648@gmail.com'
}

function getName({ name }) {
  return name
}
function getEmail({ email = '이메일이 없습니다.' }) {
  return email
}
// 객체 구조분해 할당으로 user의 name 값과 email 값을 리턴함


console.log(getName(user)) // 'jyc'
console.log(getEmail(user)) // '이메일이 없습니다.'
```
```js
const fruits = ['Apple', 'Banana', 'Cherry']
const numbers = [1, 2, 3, 4, 5, 6, 7]

function getSecondItem([,b]) {
  return b
}
// 배열 구조분해를 통해서 2번째 요소를 리턴해라

console.log(getSecondItem(fruits)) // 'Banana'
console.log(getSecondItem(numbers)) // 2
```

`나머지 매개변수`
```js
function sum(...rest) {
  console.log(rest) // [1, 2, 3, 4, 5, 6, 7, 8]
  return rest.reduce((acc, cur) => acc + cur)
}
// 입력한 숫자들을 전부 배열로 변경해서 reduce 메서드가 호출됨

const res = sum(1, 2, 3, 4, 5, 6, 7, 8)
console.log(res) // 36
```
`arguments 객체`

```js
function sum() {
  let res = 0
  for (const item of arguments) {
    res += item
  }
  return res
}

const res = sum(1, 2, 3, 4, 5, 6, 7, 8)
console.log(res) // 36
```

엥 나는 arguments 선언 안했는데? 

arguments는 함수에 전달된 인수에 해당하는 Array 형태의 객체이다. 

#### 화살표 함수

화살표 함수는 기본적으로 익명으로 표현식이다. 

나름대로 제한점이 존재한다

- this나 super에 대한 바인딩이 없고, methods 로 사용될 수 없음.
- new.target키워드가 없음.
- 일반적으로 스코프를 지정할 때 사용하는 call, apply, bind methods를 이용할 수 없음.
- 생성자(Constructor)로 사용할 수 없음.
- yield를 화살표 함수 내부에서 사용할 수 없음.

```js

//함수 표현
const sum = function(a,b){
  return a + b
}


// 화살표 함수
const sum = (a,b) => a+b
```

```js

const a = () => {} // 매개변수가 없을때 
const b = x => {} // 매개변수가 1개만있으면 소괄호 제거 가능
const c = (x,y) => {} // 매개변수가 없거나 2개 이사은 소괄호 생략안됨
const d = x => {return x * x} // 중괄호가 있으면 무조건 return문구가 들어있어야함
const e = x = > x*x // 함수가 return 으로 시작된다면 {}, return 생략 가능함
const f = x => { // 함수가 return 부터 시작하지 않으면 중괄호 생략 안됨
	console.log(x*x)
  return x * x
}

const g = () => {return {a:1}}
const h =() => ({a:1}) // 객체 데이터는 {}기호를 사용하기 때문에 소괄호로 묶어야함

const i = () => {return[1,2,3]}
const j = () => [1,2,3] // 둘다 똑같음
```

#### 즉시 실행함수(IIFE)
함수 정의와 동시에 바로 실행하는 문법

```js
const a = "치즈버거"

// 함수 정의

const giveMeBurger = () => {
	console.log(a)
}

giveMeBurger() // '치즈버거'


// 함수를 정의함과 동시에 바로 실행
(function() {
	console.log('치즈버거')
})


// 사용 가능한 패턴 
;(() => {})() // (F)()
;(function () {})() // (F)()
;(function () { })() // (F())
;!(function () {})() // !F()
;+(function () {})() // +F()

```

#### 콜백(callback)
함수의 인수로 사용되는 함수
ex) addEventListener

```js
const a = (callback) => {
  callback()
  console.log('A')
}
const b = () => {
  console.log('B')
}

a(b) // 여기있는 b가 콜백함수

```

```js
function sum(a, b, callback) {
  // 1초 후 실행
  setTimeout(() => {
    callback(a + b)
  }, 1000)
}
sum(3, 7, function(value) {
  console.log(value) // 10
})

//sum이라는 함수의 인수로 다른 함수를 사용했다. 
```

```js
//loadImage
<div class="container">
  <h1>Loading...</h1>
</div>

const loadImage = (url, cb) => {
  const imgEl = document.createElement('img')
  imgEl.src = url
  imgEl.addEventListener('load', () => {
    setTimeout(() => {
      cb(imgEl)
    }, 1000)
  })
}

const containerEl = document.querySelector('.container')
loadImage('https://www.gstatic.com/webp/gallery/4.jpg', imgEl => {
  containerEl.innerHTML = ''
  containerEl.append(imgEl)
})

```
#### 재귀
하나의 함수에서 자기 자신을 다시 호출해서 실행하는 방법
```js
const userA = { name: 'A', parent: null }
const userB = { name: 'B', parent: userA }
const userC = { name: 'C', parent: userB }
const userD = { name: 'D', parent: userC }

const getRootUser = user => {
  if (user.parent) // parent 값이 있니? {
    return getRootUser(user.parent) // 있으면 user.parent값을 인수로 다시 함수를 호출한다.
  }
  return user
}

console.log(getRootUser(userD)) // userA가 출력된다. 

```

#### 호출 스케줄링

일정한 시간이 지난 후에 원하는 함수를 호출하는 것
```js
const hello = () => {
  console.log('Hello~')
}

setInterval(hello,3000) // 3초마다 hello 호출
const btnEl = document.querySelector('h1')

btnEl.addEventListener('click', () => {
console.log('Clear!')
})
```
