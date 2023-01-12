## JavaScript

### 모듈

`as`를 사용해서 데이터의 이름 변경이 가능하다. 

```js
const hello = 'hello world'
const b = 123
const c = ['A', 'B', 'C']
export default hello
export { b as num, c as arr }
```
```js
import hello, { num, arr } from './hello.js'

console.log(hello, num, arr)
// hello world 123 ['A', 'B', 'C']

```

#### 가져오기(Imports)

> import 기본데이터, {이름데이터1, 이름데이터2} from '경로'

```js
import defData from './myModule.js'
import defData, { str, num, arr, fn } from './myModule.js'
import { str, num, arr, fn } from './myModule.js'
import { 
  str as myStr, 
  num as myNum, 
  arr as myArr, 
  fn as myFn 
} from './myModule.js'
import * as myName from './myModule.js'
```
import 옆에 중괄호가 있으면 기본 가져오기, 중괄호가 있으면 이름 가져오기

`동적모듈 가져오기` : import  함수를 통해 동적으로 모듈을 가져올 수 있다.  이때 `import` 함수는 `promise`객체를 반환한다.


`가져온 다음 바로 내보내기` : 가져온 모듈을 바로 내보낼수 있다. `import` 대신에 export 키워드를 사용한다. 

### 비동기

동기? 순차적으로 코드가 실행
비동기? 순차적으로 코드가 실행되지 않는다

동기 예

```js
console.log(1)
console.log(2)
console.log(3)
// 1
// 2
// 3
```

비동기 예시 : 
```js
console.log("햄버거 주세요")
setTimeout(() => console.log("아참 감자튀김 빼구요"), 1000)
console.log("메뉴 나왔습니다.") 
//햄버거 주세요
//메뉴 나왔습니다.
//아참 감자튀김 빼구요... 이미 늦었다. 메뉴가 나와버렸다.
```

#### 콜백 패턴 

```js
function a(callback) {
  setTimeout(() => {
    console.log('A')
    callback()
  })
}

function b() {
  console.log('B')
}

a(() => {
  b()
})
//A
//B


```

setTimeout 같은 비동기 함수를 사용하면 당연히 나중에 나타나겠지만, 그럼에도 불구하고 비동기 함수가 호출된 다음에 다음으로 넘어가게 하고 싶다면 이렇게 작성한다. 

인수로 함수 데이터를 받아와서 해당 함수(b)가 호출되기 전에 (a)가 먼저 호출되고 그다음에 (b)가 호출됨!


```js
function add(a, b, callback) {
  setTimeout(() => {
    const result = a + b
    callback(result)
  }, 1000)
}

add(2, 7, (result) => {
  console.log(result) // 1초 뒤에 9가 나타남
})


```
계산하는데 시간이 좀 걸리는데 한 1초뒤에 나오는 결과값 뽑아주세요~


이거랑 비슷한건 이전에 봤었다. 바로 addEventListener!


```js

const h1El = document.querySelector('h1')
h1El.addEventListener('click', (event) => {
  console.log(event)
})
```
시간이 얼마나 걸리는지는 모르겠는데 `h1` dom 클릭하면 해당 이벤트에대한 내용을 뽑아주세요~


그런데 이런 비동기에대한 내용을 콜백 패턴으로만 작성하면 한두개정도는 괜찮을지 몰라도 프로세스가 길어지면

```js
const a = callback => {
  setTimeout(() => {
    console.log(1)
    callback()
  }, 1000)
}
const b = callback => {
  setTimeout(() => {
    console.log(2)
    callback()
  }, 1000)
}
const c = callback => {
  setTimeout(() => {
    console.log(3)
    callback()
  }, 1000)
}
const d = callback => {
  setTimeout(() => {
    console.log(4)
    callback()
  }, 1000)
}
// ...

a(() => {
  b(() => {
    c(() => {
      d(() => {
        // ...
      })
    })
  })
})
```
이렇게  지옥이 펼쳐진다.

다른 방법은 없을까?

#### Promise

```js
const add = (a, b) => {
  return new Promise((resolve) => {
    setTimeout(() => {
      const result = a + b
      resolve(result)
    }, 1000)
  })
}

const res = await add(2, 7)
console.log(res) // 1초 뒤에 9

```
에이 뭐야 아까랑 비슷하잖아?

```js
const a = () => {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log(1)
      resolve()
    }, 1000)
  })
}
const b = () => {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log(2)
      resolve()
    }, 1000)
  })
}
const c = () => {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log(3)
      resolve()
    }, 1000)
  })
}
const d = () => console.log(4)



a()
	.then(b)
	.then(c)
	.then(d)
```
이래도 차이가 없어보이는가?


```js
a(() => {
  b(() => {
    c(() => {
      d(() => {
        // ...
      })
    })
  })
})

------------------------
a()
	.then(b)
	.then(c)
	.then(d)
```
이렇게 Promise로 구현된 비동기 함수는 Promise 객체를 반환하며, 이로 구현된 비동기 함수를 호출하는 측에서 후속 처리 메서드(then)을 통해 비동기 처리 결과를 처리하게 된다.

#### async/await
```js
const a = () => {
  return new Promise(resolve => {
    setTimeout(() => {
      console.log(1)
      resolve()
    }, 1000)
  })
}
const b = () => console.log(2)

// a().then(() => b())

const wrap = async () => {
  await a()
  b()
}
wrap()
```

이렇게 async와 await를 사용해서 작성도 가능하다. 


#### 이미지 로딩 

```js
function loadImage(src) {
  return new Promise((resolve) => {
    const imgEl = document.createElement('img')
    imgEl.src = src
    imgEl.addEventListener('load', () => {
      resolve(imgEl)
    })
  })
}
```
resolve안에 imgEl을 넣었기 때문에 다른 곳에서 imgEl을 사용할수 있게된다. 
```js
import { loadImage } from './loadImage.js'

const src = 'https://picsum.photos/2000'
const imgEl = await loadImage(src)
document.body.innerHTML = ''
document.body.append(imgEl)
// document.body.innerHTML = `<img src="${src}" />`
console.log('Loaded!')
```
위의 내용은 `laodImage`를 모듈화 해서 다른 곳에서 불러와 사용하는 모습이다. `imgEl`이 loadImage 비동기 함수로 인해 발생한 결과물이기 때문에 이를 사용해서 렌더링을 하는 모습이다.
#### fetch()

첫번째 인자로 URL, 두번째 인자로 옵션 객체를 받고, Promise 타입의 객체를 반환한다. 

```js
const res = await fetch('https://omdbapi.com/?apikey=7035c60c&s=frozen')

const json = await res.json
console.log(json)

```
이런 타입의 내용은 과제를 진행해보면서 봤었다. 


