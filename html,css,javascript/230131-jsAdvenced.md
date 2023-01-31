# javascript

## 클로저

함수가 선언될 때의 유효범위를 기억하고 있다가 함수가 외부에서 호출될때 그 유효범위의 특정 변수를 참조할수 있는 개념을 말한다.

```js
function createCount() {
  let a = 0
  return function () {
    return a += 1
  }
}

const count = createCount()

console.log(count())  // 1
console.log(count())  // 2
console.log(count())  // 3
```

함수가 리턴으로 또다른 함수를 반환할때 함수 바깥에 선언된 변수의 값이 누적되는 것을 뜻한다.


그래서 이거 어디에 쓸수 있을까?
```js
const h1El = document.querySelector('h1')
const h2El = document.querySelector('h2')

// 별도의 상태 관리가 필요!
let h1IsRed = false
let h2IsRed = false

h1El.addEventListener('click', event => {
  h1IsRed = !h1IsRed
  h1El.style.color = h1IsRed ? 'red' : 'black'
})
h2El.addEventListener('click', event => {
  h2IsRed = !h2IsRed
  h2El.style.color = h2IsRed ? 'red' : 'black'
})
```
위의 코드는 클로저를 사용하지 않은 상태이며 엘리먼트를 클릭했을때 특정 변수의 값에 따라서 엘리먼트 스타일 속성을 지정하는 코드이다.

```js
const h1El = document.querySelector('h1')
const h2El = document.querySelector('h2')

// 하나의 함수로 정리!
const createToggleHandler = () => {
  let isRed = false
  return event => {
    isRed = !isRed
    event.target.style.color = isRed ? 'red' : 'black'
  }
}
h1El.addEventListener('click', createToggleHandler())
h2El.addEventListener('click', createToggleHandler())
```
클로저를 사용해서  여러가지 엘리먼트의 속성을 관리할때 하나하나 addEventListener 메소드를 작성할 필요없이 하나만 작성해서 관리가 가능해진다.

### 잘못된 클로저 사용

프로젝트를 제작하면서 결과값을 테스트 하기 위해 console.log를 작성하지만 이것을 지우지 않고 놔두면 동작할 때마다 메모리가 사용되는 현상이 일어나기 때문에 서비스 빌드를 할때는 콘솔 로그를 지워주는 것이 좋다


## 메모리 누수


메모리 누수(Memory Leak)란, 더 이상 필요하지 않은 데이터가 해제되지 못하고 메모리를 계속 차지되는 현상


### 분리된 노드 참조
```html
<button>Remove!</button>

<div class="parent">
  <div class="child">1</div>
  <div class="child">2</div>
</div>
```


```js
const btn = document.querySelector('button')
const parent = document.querySelector('.parent')

btn.addEventListener('click', () => {
  console.log(parent) // <div class="parent">...</div>
  parent.remove()
})

```
이런식으로 작성하면 parent 가 제거된 상황에도 계속 console.log가 나타나게 된다.

이를 해결하기 위해서는 
```js
const btn = document.querySelector('button')

btn.addEventListener('click', () => {
  const parent = document.querySelector('.parent')
  console.log(parent) // <div class="parent">...</div>
  parent && parent.remove()
})

```
이벤트 리스너 안에서 쿼리 설렉터를 추가해주는 것이다. 따라서 이벤트가 동작할 때마다 해당 엘리먼트를 찾아주는 것!


## 콜 스택, 테스크 큐, 이벤트 루프

- Heap: 데이터가 할당되는 메모리, 수동 할당/해제는 불가, 가비지 콜렉터(GC)가 관리
- Queue: 대기 행렬, 줄을 서서 기다리다
- FIFO(First In First Out): 선입선출, 먼저 들어온 데이터가 먼저 나감
- LIFO(Last In First Out): 후입선출, 마지막에 들어온 데이터가 먼저 나감



### 테스크 큐
 종류
 - 마이크로테스크(Microtask Queue) - Promise, queueMicrotask() 등
- 랜더(Render Queue) - requestAnimationFrame()
- 메크로테스크(Macrotask Queue 혹은 Task Queue) - fetch(), Ajax, DOM Events 등


순서 : micro > Render > Macro


## 리플로우 & 리페인트

리플로우 : 브라우저 화면에 무엇인가 출력하기 위해, 크기나 위치 등을 계산하는 과정
리페인트 : 리플로우 이후 화면에 실제 출력하는 과정을

노드의 크기, 여백, 위치 등 주변 노드에 영향을 주는 레이아웃 속성이 변경되면,
브라우저는 모든 노드를 리플로우하고 영향을 받은 모든 화면 영역을 다시 리페인트합니다.

### 브라우저 렌더링 과정

HTML 파싱
DOM 트리 생성(DOMContentLoaded 이벤트)
CSS 파싱
CSSOM 트리 생성
DOM-CSSOM 결합
렌더 트리 생성
레이아웃 계산
렌더링(load 이벤트)


※ 애니메이션으로 엘리먼트 동작을 제어할때 리플로우가 최대한 많이 동작하지 않게끔 설정하는 마인드를 갖자!

리플로우가 많이 되는 경우가 뭔데? 라고 물어본다면 마우스를 올렸을때 엘리먼트가 이동하게 되는 효과를 부여하는 과정에서 `margin`값을 변경하게 작성한다면 다음 위치를 계산해야하는 리플로우가 계속 발생하게 되는 것이다. 이를 줄이기 위해서는 `transform`을 사용해서 다음 위치를 파악할수 있게 해서 리플로우를 줄여주는 것이 브라우저 성능에 도움을 준다.



