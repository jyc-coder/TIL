## javascript 시작

### 자바스크립트 런타임

자바스크립트가 구동되는 환경을 말한다. 이러한 자바스크립트 런타임의 종류로는 웹 브라우저(크롬, 파이어폭스, 익스플로러 등)프로그램과 Node.js 라는 프로그램이 있다.


### 자료형

#### 원시형

- 문자

```js
const str1 = "apple"
const str2 = 'pie'

// 템플릿 리터럴(보간 기능)
const str3 = `good ${str2}`
consoel.log(stre) // `Good World`

```

따옴표 세종류
- 큰따옴표: ""
- 작은따옴표: ''
- 백틱(그레이브): ``

※ 리터럴 : 기호를 통해서 데이터를 만드는 것!


- 숫자
숫자는 정수 및 부동소수점 숫자를 나타낸다.

```js
const n1 = 123
const n2 = 12.345665
```

※NAN? 
(Not-A-Number)숫자가 아닌 숫자를 나타낸다

```js
const str= 'hello me!!'
console.log(Number(str)) //NaN
```


- 불린

불린(boolean)은 `true`와 `false` 두가지 값인 논리 데이터이다


```js
let a = true
let b = false
```

- Null

존재하지 않는(nothing), 비어있는(empty), 알수없는(unknown)값을 명시적으로 나타냄

```js
let age= null
```

- Undefined
'값이 할당되지 않은 상태'를나타낼 때 사용한다.
변수는 선언했으나, 값을 할당하지 않았다면 해당변수에 `undefined`가 자동으로 할당된다.

```js
let age // age 만 해놓고 값이 뭔데?
alert(age) // undefined 
```


Null 은 명시적, undefined는 암시적인 개념이라고 생각하면 된다.



