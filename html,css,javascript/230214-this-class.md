```js
const timer = {
  title: 'TIMER!',
  timeout() {
    console.log(this.title)
    setTimeout(function () {
      console.log(this.title)
    }, 1000)
  }
}

timer.timeout()
```

일반 함수식은 호출된 위치를 기준으로 this 가 정해지기 때문에 위와 같이 위치를 다르게 해서 this.의 값을 변경하는 것이다


### apply,call,bind

- call
다른 객체의 메서드를 빌려온다.

- apply
call()구문과 거의 유사하지만 call()은 함수에 전달될 인수 리스트를 받는데 비해, apply()는 인수들의 단일배열을 받는다는 차이점이 있다.

- bind
새로운 함수를 생성한다. 받게되는 첫인자의 value는 `this`키워드를 설정하고 이어지는 인자들은 바인드된 함수의 인수에 제공된다.

```js
const jyc = {
  name: 'Jyc'
}
const amy = {
  name: 'Amy',
  getName(age) {
    return `${this.name} is ${age}`
  }
}

// call
console.log(
  amy.getName.call(jyc, 28)
)
// apply
console.log(
  amy.getName.apply(jyc, [28])
)
// bind
const jycGetName = amy.getName.bind(jyc)
console.log(jycGetName(85))
```

### Function 클래스

`Function` 클래스로 함수를 생성할수도 있다.
```js
const sum = new Function('a', 'b', `
  console.log(a)
  console.log(b)
  return a + b
`)
console.log(sum)
console.log(sum(1, 2))
```

## Class

### prototype
자바스크립트의 모든 객체는 프로토타입(prototype)이라는 객체를 가지고 있다.

모든 객체는 그들의 프로토타입으로부터 프로퍼티와 메소드를 상속받는다.

이처럼 자바스크립트의 모든 객체는 최소한 하나 이상의 다른 객체로부터 상속을 받으며, 이때 상속되는 정보를 제공하는 객체를 프로토타입이라고 한다.

```js
const fruits = new Array('Apple', 'Banana', 'Cherry')
// const fruits = ['Apple', 'Banana', 'Cherry']

console.log(fruits)
console.log(fruits.length) // 3
console.log(fruits.includes('Banana')) // true
console.log(fruits.includes('Orange')) // false

Array.prototype.jyc = function () {
  console.log(this)
  return this.map(item => item[0])
}

const newF = fruits.jyc()
console.log(newF) // ["A","B","C"]
```

```js
const jyc = {
  firstName: 'yeongchan',
  lastName: 'jeong',
  getFullName: function () {
    return `${this.firstName} ${this.lastName}`
  }
}
const neo = {
  firstName: 'Neo',
  lastName: 'Anderson'
}
console.log(heropy.getFullName()) // yeongchan jeong
console.log(heropy.getFullName.call(neo)) // neo anderson
```
