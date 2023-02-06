### E.style 
요소의 style 속성의 css 속성 값을 얻거나 지정한다.
```js
const el = document.querySelector('.child')

// 개별 지정!
el.style.width = '100px'
el.style.fontSize = '20px'
el.style.backgroundColor = 'green'
el.style.position = 'absolute'

// 한 번에 지정!
Object.assign(el.style, {
  width: '100px',
  fontSize: '20px',
  backgroundColor: 'green',
  position: 'absolute'
})

console.log(el.style) // CSSStyleDeclaration { ... }
console.log(el.style.width) // '100px'
console.log(el.style.fontSize) // '20px'
console.log(el.style.backgroundColor) // 'green'
console.log(el.style.position) // 'absolute'
```
### E.getAttribute() / E.setAttribute()
요소에서 특정 속성 값을 얻거나 지정합니다.

```js
const el = document.querySelector('.child')

console.log(el.hasAttribute('class')) // true

el.removeAttribute('class')
console.log(el.hasAttribute('class')) // false

console.log(el) // <div>1</div>
```

### 크기와 좌표
```html
<div class="parent">
  <div class="child">1</div>
  <div class="child">2</div>
</div>
```
```js

body {
  height: 1000px;
  padding: 500px 0;
}
.parent {
  width: 300px;
  height: 200px;
  margin-top: 1000px;
  padding: 20px;
  overflow: auto;
  border: 10px solid;
}
.child {
  height: 100px;
  margin-top: 100px;
  border: 10px solid red;
}
```


### window.innerWidth / window.innerHeight
현재 화면의 크기를 얻는다.

```js
console.log(window.innerWidth) // 1920
console.log(window.innerHeight) // 926
```

### window.scrollX / window.scrollY
페이지의 좌상단 기준, 현재 화면(Viewport)의 수평 혹은 수직 스크롤 위치를 얻는다
```js
console.log(window.scrollX, window.scrollY)
```

### window.scrollTo() / E.scrollTo()
지정된 좌표로 대상(화면, 스크롤 요소)을 스크롤한다.
```js
대상.scrollTo(X좌표, Y좌표)
대상.scrollTo({
  left: X좌표,  
  top: Y좌표,
  behavior: 'smooth' 
})
```

### E.clientWidth / E.clientHeight

테두리 선(border)을 제외한 요소의 크기를 얻는다

```js
const parentEl = document.querySelector('.parent')
const childEl = document.querySelector('.child')

console.log(parentEl.clientWidth, parentEl.clientHeight) // 325, 240 // 스크롤바 제외 너비
console.log(childEl.clientWidth, childEl.clientHeight) // 265, 100
```

### E.offsetWidth / E.offsetHeight
테두리 선(border)을 포함한 요소의 크기를 얻는다
```js
const parentEl = document.querySelector('.parent')
const childEl = document.querySelector('.child')

console.log(parentEl.offsetWidth, parentEl.offsetHeight) // 360, 260
console.log(childEl.offsetWidth, childEl.offsetHeight) // 285, 120

```

### E.scrollWidth / E.scrollHeight

요소의 테두리 선을 제외한 스크롤 여영ㄱ까지의 크기를 얻는다.

```js
const parentEl = document.querySelector('.parent')
const childEl = document.querySelector('.child')

console.log(parentEl.scrollWidth, parentEl.scrollHeight) // 325, 480
console.log(childEl.scrollWidth, childEl.scrollHeight) // 265, 100
```

### E.scrollLeft / E.scrollTop
스크롤 요소의 좌상단 기준, 현재 스크롤 요소의 수평 혹은 수직 스크롤 위치를 얻는다.


```js
window.parentEl = document.querySelector('.parent')

// 스크롤 요소에서 실제 스크롤한 후 콘솔에 따로 출력!
console.log(parentEl.scrollLeft, parentEl.scrollTop)
```

### E.offsetLeft / E.offsetTop
페이지의 좌상단 기준, 요소의 위치를 얻는다.

```js
const parentEl = document.querySelector('.parent')
const childEl = document.querySelector('.child')

console.log(parentEl.offsetLeft, parentEl.offsetTop) // 0, 1500
console.log(childEl.offsetLeft, childEl.offsetTop) // 30, 1630
```


### E.getBoundingClientRect()

테두리 선(border)을 포함한 요소의 크기와 화면에서의 상대 위치 정보를 얻는다.

```js


const parentEl = document.querySelector('.parent')
const childEl = document.querySelector('.child')

console.log(parentEl.getBoundingClientRect())
console.log(childEl.getBoundingClientRect())
```


## Events

기본 구조

```html
<div class='parent'>
  <div class='child'>
    <a href="https://naver.com" target="_blank">
      naver!
    </a>
  </div>
</div>
```
```css
.parent {
  width: 300px;
  height: 200px;
  padding: 20px;
  border: 10px solid;
  background-color: red;
  overflow: auto;
}
.child {
  width: 200px;
  height: 1000px;
  border: 10px solid;
  background-color: orange;
  font-size: 40px;
}
```
### .addEventListener()
대상에 이벤트 청취를 등록한다. 대상에 지정한 이벤트가 발생했을 때 지정한 삼수가 호출된다.
```js
const parentEl = document.querySelector('.parent')
const childEl = document.querySelector('.child')

parentEl.addEventListener('click', () => {
  console.log('Parent!')
})
childEl.addEventListener('click', () => {
  console.log('Child!')
})
```

### .removeEventListener()
대상에 등록했던 이벤트 청취(Listen)를 제거한다.


```js
const parentEl = document.querySelector('.parent')
const childEl = document.querySelector('.child')

const handler = () => {
  console.log('Parent!')
}

parentEl.addEventListener('click', handler)
childEl.addEventListener('click', () => {
  parentEl.removeEventListener('click', handler)
})
```

### 이벤트 객체

대상에서 발생한 이벤트 정보를 담고 있다.

### .target
이벤트가 발생한 요소입니다.

### .currentTarget
이벤트 청취가 등록된 요소입니다.

```js
const parentEl = document.querySelector('.parent')

parentEl.addEventListener('click', event => {
  console.log(event.target, event.currentTarget)
})
```

![](https://velog.velcdn.com/images/jhs000123/post/3925e61f-3c31-45fa-ac41-1cbfa275f563/image.png)

주황색이 child고 빨간색이 parent이다. 현재 parentEl에 이벤트 리스너를 추가했다. child를 클릭했다고 가정했을때 발생한 요소는 child이지만 이벤트 청취가 등록된 요소는 parent 인 것이다. 


### 이벤트 제어

### 기본 동작 방지

```js
// 마우스 휠의 스크롤 동작 방지!
const parentEl = document.querySelector('.parent')
parentEl.addEventListener('wheel', event => {
  event.preventDefault()
})

// <a> 태그에서 페이지 이동 방지!
const anchorEl = document.querySelector('a')
anchorEl.addEventListener('click', event => {
  event.preventDefault()
})
```

### 이벤트 전파(버블) 정지

```js
const parentEl = document.querySelector('.parent')
const childEl = document.querySelector('.child')
const anchorEl = document.querySelector('a')

window.addEventListener('click', event => {
  console.log('Window!')
})
document.body.addEventListener('click', event => {
  console.log('Body!')
})
parentEl.addEventListener('click', event => {
  console.log('Parent!')
  event.stopPropagation() // 버블링 정지!
})
childEl.addEventListener('click', event => {
  console.log('Child!')
})
anchorEl.addEventListener('click', event => {
  console.log('Anchor!')
})
```
