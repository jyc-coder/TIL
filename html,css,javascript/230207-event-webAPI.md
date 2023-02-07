### 이벤트 위임(Delegation)
```html
<div class='parent'>
  <div class='child'>1</div>
  <div class='child'>2</div>
  <div class='child'>3</div>
  <div class='child'>4</div>
</div>
```
```js
const parentEl = document.querySelector('.parent')
const childEls = document.querySelectorAll('.child')

// 모든 대상 요소에 이벤트 등록!
childEls.forEach(el => {
  el.addEventListener('click', event => {
    console.log(event.target.textContent)
  })
})

// 조상 요소에 이벤트 위임!
parentEl.addEventListener('click', event => {
  const childEl = event.target.closest('.child')
  if (childEl) {
    console.log(childEl.textContent)  
  }
})
```

2번째 이벤트리스너의 경우 child 클래스로부터 가장 가까운 곳을 선택하고, 그것이 childEl이라면 해당 엘리면트의 text 값을 출력한다. 

첫번재는 forEach로 4번 생성하지만 2번째는 하나의 이벤트 리스너를 사용해서 여러개의 엘리먼트를 관리할수 있다. 

### 커스텀 이벤트
```js
대상.dispatchEvent(이벤트)
```
```js
const child1 = document.querySelector('.child:nth-child(1)')
const child2 = document.querySelector('.child:nth-child(2)')

child1.addEventListener('click', event => {
  // 강제로 이벤트 발생!
  child2.dispatchEvent(new Event('click'))
  child2.dispatchEvent(new Event('wheel'))
  child2.dispatchEvent(new Event('keydown'))
})
child2.addEventListener('click', event => {
  console.log('Child2 Click!')
})
child2.addEventListener('wheel', event => {
  console.log('Child2 Wheel!')
})
child2.addEventListener('keydown', event => {
  console.log('Child2 Keydown!')
})
```

`customEvent` 생성자의 detail옵셩르 사용해서, `event.detail` 속성으로 데이터를 전달한다.
```js
const child1 = document.querySelector('.child:nth-child(1)')
const child2 = document.querySelector('.child:nth-child(2)')

child1.addEventListener('hello-world', event => {
  console.log('커스텀 이벤트 발생!')
  console.log(event.detail) // ?!
})

child2.addEventListener('click', event => {
  // 일반 이벤트!
  child1.dispatchEvent(new Event('hello-world', {
    detail: 123 // undefined
  }))
  // 커스텀 이벤트!
  child1.dispatchEvent(new CustomEvent('hello-world', {
    detail: 123 // 123
  }))
})
```

### 이벤트 종류

#### UI Events
이벤트 | 설명
:--:|:--:|
DOMContentLoaded| DOM트리를 완성했을때
load| DOM트리를 완성후 기타 자원로드가 완료되었을때
abort|로드가 중단되었을때
unload|페이지가 닫힐때
error|오류가 발생하거나 리로스가 존재하지 않는경우
resize|크기를 조절할때
scroll|스크롤할때
select|텍스트를 선택했을 때
fullscreenchange|정체화면 모드나 일반 모드로 변경될 때

#### Mouse Events

이벤트 | 설명
:--:|:--:|
click|클릭했을 때
dbclick|더블 클릭했을 때
mousedwon | 버튼을 누를 때
mouseup | 버튼을 뗄때
mouseenter | 포인터가 요소로 들어갈 때
mouseleave | 포인터가 요소에서 나올때
mousemove | 포인터가 움직일 때
contextmenu | 우클릭했을 때
wheel | 휠버튼이 회전할 때



## Web APIs

### console

#### .log(), .warn(), .error(), .dir()
콘솔에 메시지나 객체출력

- log: 일반 메시지
- warn: 경고 메시지
- error: 에러 메시지
- dir: 속성을 볼 수 있는 객체를 출력

#### .count(), .countReset()
콘솔에 메소드 호출의 누적 횟수를 출력하거나 초기화한다.

```js
console.count('a') // 'a: 1'
console.count('a') // 'a: 2'
console.count('b') // 'b: 1'
console.count('a') // 'a: 3'
console.log('Reset a!') // 'Reset a!'
console.countReset('a')
console.count('a') // 'a: 1'
console.count('b') // 'b: 2'

console.count() // 'default: 1'
console.count() // 'default: 2'
console.count('default') // 'default: 3'
console.count() // 'default: 4'

```

#### .time(), timeEnd()
콘솔에 타이머가 시작해서 종료되기까지의 시간(ms)을 출력한다.

```js
console.time('햄버거 주문');

for (let i = 0; i < 10000; i += 1) {
  console.log(i)
}

console.timeEnd('햄버거 주문') // '햄버거 주문: 170.137...ms'
```

#### .trace()
메소드 호출 스택(call stack)을 추적해서 출력한다.

```js
function a() {
  function b() {
    function c() {
      console.log('Log!')
      console.trace('Trace!')
    }
    c()
  }
  b()
}
a()

// Log!
// Trace!
// c
// b
// a
// (anonymous)
```

#### .clear()
콘솔 기록된 메시지 삭제


#### 서식문자 치환
`%s` - 문자로 적용
`%o` - 객체로 적용
`%c` - CSS를 적용
```js
const a = 'The brown fox'
const b = 3
const c = {
  f : 'fox',
  d : 'dog'
}
console.log('%s jumps over the lazy dog %s times.', a, b)
console.log('%o is Object!', c)
console.log(
  '%cThe brown fox %cjumps over %cthe lazy dog.',
  'color: brown; font-family: serif; font-size: 20px;',
  '',
  'font-size: 18px; color: #FFF; background-color: green; border-radius: 4px;'
)
```

### Cookie

- 도메인 단위로 저장
- 모든 웹 요청에 포함돼 서버로 전송
- 표준안 기준, 사이트당 최대 20개 및 4kb로 제한한다
- 영구저장 불가능

옵션

- `domain` : 유효 도메인 설정 - `domain=localhost`
- `path` : 유효 경로 설정 - `path=/;`
- `expires` : 만료 날짜 설정
- `max-age` : 만료 타이머 설정
- `secure` : HTTPS 보안 프로토콜만 전송 - `secure`;
- `samesite` : 크로스 사이트 요청 처리 여부(none, lax, strict)- `samesite=none`

### Storage

- 도메인 단위로 저장
- 서버로 전송되지 않음
- 5mb 제한
- 세션 혹은 영구 저장가능

#### 종류
sessionStorage: 브라우저 세션이 유지되는 동안에만 데이터 저장
localStorage: 따로 제거하지 않으면 영구적으로 데이터 저장

#### 메소드

- .getItem(): 데이터 조회
- .setItem(): 데이터 추가
- .removeItem(): 데이터 제거
- .clear(): 스토리지 초기화


### Location
현재 페이지 정보를 반환하거나 제어한다.

#### 속성
- .href: 전체 URL 주소
- .protocol: 프로토콜 - 'http:' or 'https:'
- .hostname: 도메인 이름 - 'google.com'
- .pathname: 도메인 이후 경로 - '/a/b/c'
- .host: 포트 번호를 포함한 도메인 이름 - 'google.com:5500'
- .port: 포트 번호 - '5500'
- .hash: 해시 정보(페이지의 ID) - '' or '#abc'

#### 메소드

- 페이지가 항상 새로고침된다.

- .assign(주소): 해당 '주소'로 페이지 이동
- .replace(주소): 해당 '주소'로 페이지 이동, 현재 페이지 히스토리를 제거
- .reload(강력): 페이지 새로고침, true 인수는 '강력' 새로고침


### History

브라우저 히스토리 정보를 반환하거나 제어합니다.

#### 속성
- .length: 등록된 히스토리 개수
- .scrollRestoration: 스크롤 위치 복원 여부 - 'auto' / 'manual'
- .state: 현재 히스토리에 등록된 데이터(상태)

#### 메소드
모든 브라우저(Safari 제외)는 .pushState()와 .replaceState()의 '제목' 옵션을 무시한다.
.pushState()와 .replaceState() 호출은 popstate 이벤트가 발생하지 않는다!

- .back(): 뒤로 가기
- .forward(): 앞으로 가기
- .go(위치): 현재 페이지 기준 특정 히스토리 '위치'로 이동 - .go(-2)
- .pushState(상태, 제목, 주소): 히스토리에 상태 및 주소를 추가
- .replaceState(상태, 제목, 주소): 현재 히스토리의 상태 및 주소를 교체
