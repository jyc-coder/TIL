## JavaScript

### 구조 분해 할당(Destructuring assignment)

배열이나 객체의 구조에 맞게 바로 개별 변수에 값을 할당하는 방법


#### 배열 구조 분해 (Array destructuring)

기본 형태

```js
const arr = [2,4,6]
const [a,b,c] = arr

console.log(arr)
// 2 4 6

```

선언과 분리 

```js
const arr2 = [1,3,5]
let d, e, f
[d, e, f] = arr2

console.log (d,e,f) // 1 3 5

```

기본값 

```js
const arr = [, , 3]
const [a=8, b,c] = arr

console.log(a, b, c) // 8 undefined 3

```

변수 값 교환

```js
let a = 1;
let b = 2;
;[b,a] = [a,b]

console.log(a,b) // 2 1
```
선연을 역순으로 작성한 듯한 느낌으로 작성하면 변수 값이 교환된다.

반환 값 무시
```js
const arr = [2,4,5]

const [,, c] = arr
console.log(c)
```

나머지 할당
```js
const arr = [2, 4, 6]
const [a, ...rest] =arr

console.log(a, rest) // 2 [4,6]
```
나머지는 배열의 형태로 출력된다.


#### 객체 구조분해(Object destructuring)

기본
```js
const obj = {a:2,b:4,c:6}
const {a,b,c} = obj

console.log(a,b,c) // 2, 4, 6
```

선언과 분리

```js
const obj = {a:2, b:3, c:7}
let a,b,c
({a, b, c} =obj)

console.log(a, b, c) // 2 3 7
```

기본값
```js
const obj = {c : 8}
const { a = 0, b, c} = obj


console.log(a,b,c) // 0 undefined 8
```

변수명 변경

```js
const obj = {a : 2 , b: 3, c : 7}
const {a:x, b:y, c:z} = obj
console.log(x,y,z) // 2 3 7
```
변수의 이름을 변동하되 value값은 변동하지 않는다.

기본값 + 변수명 변경
```js
const obj = { c : 7 }
const { a: x=0, b:y, c:z} = obj

console.log(x,y,z) // 0 undefined 7
```
나머지 할당

```js
const obj = {a: 2, b:3, c:7}
const { a, ...rest} = obj

console.log(a, rest) // 2 {b:3, c:7}
```

## 2차 과제 관련 내용

영화검색 api를 사용하여 데이터를 가져오고 렌더링을 통해 영화 정보를 사용자에게 보여주는 영화 검색 사이트를 제작해야하는 과제를 간단하게 어떤 식으로 진행하는지, 그리고 api를 통해 데이터를 받아오는 과정을 배움
```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <script defer type="text/javascript" src="main.js"></script>
  <title>Document</title>
</head>
<body>

  <input type="text" />
  <button class="search">어찌됐던 검색하기</button>
  <ul class="movies">
     
  </ul>

  <button class="view-more">더보기...</button>
</body>
</html>
```

`index.js`
```js
const inputEl = document.querySelector('input')
const buttonEl = document.querySelector('.search')
const viewMoreEl = document.querySelector('.view-more')
const moviesEl = document.querySelector('.movies')


let searchText = '';
let pageNumber = 1;
inputEl.addEventListener('input', function () {
  searchText = inputEl.value
})

inputEl.addEventListener('keydown', function (e)  {
  if (e.key === 'Enter' && !e.isComposing) {
    getMovies()
  }
})

buttonEl.addEventListener('click', function () {
  getMovies(true)
})

viewMoreEl.addEventListener('click', function () {

  getMovies(false)

})



let movies = []
async function getMovies(first) {
  console.log('searchText:', searchText)
  const res = await fetch(`https://omdbapi.com/?apikey=보안&s=${searchText}&page=${pageNumber}`)
  pageNumber++
  const json = await res.json()
  const result = json
  movies = json.Search
  console.log(result)
  renderMovies(first)
}

function renderMovies(isFirst) {
  const movieEls = movies.map(function(movie) {
    const liEl = document.createElement('li')
    const titleEl = document.createElement('h2')
    const posterEl = document.createElement('img')

    titleEl.textContent = movie.Title
    posterEl.src = movie.Poster
    liEl.append(titleEl, posterEl)
    return liEl
  })
  if (isFirst) {
    moviesEl.innerHTML=''
  }
  moviesEl.append(...movieEls)
  console.log(movieEls)
}


```
`어찌됐건 검색하기` 버튼을 클릭하면 `getMovies(boolean)`이 호출되고, 함수 내부에 있는 `renderMovies`또한 호출되면서 `createElemnt`를 통해 만들어진 dom 안에 데이터를 집어넣음으로서 렌더링이 완료된다.

- `getMovies`의 파라미터를 boolean으로 한 이유는 검색 버튼을 눌렀을 때, 처음 새로운 검색어로 실행하는 검색인지 아닌지를 판단하기 위한 지표역할이다.

- true 인경우는 처음 검색하는 것이므로 이전의 결과물을 지우고 렌더링을 실시

- falase 인경우는 처음 검색하는 것이 아니기 때문에 이전 결과를 그대로 남겨놓고 추가적으로 렌더링을 진행함 이때 1페이지당 10개가 렌더링 되며 페이지의 값 `pageNumber`값을 더해주는 프로세스가 있어야 새로운 데이터가 렌더링된다.


- 전반적으로 `index.js`에서 만들어진 메서드 `getMovies()`는 독립적으로 진행되는 것이 아닌, 또다른 메서드 `renderMoves`를 호출하기 때문에 나중에 유지보수를 할때 힘들어질수 있기 때문에, 메서드를 작성할 때에는 각각의 메서드가 독립적으로 동작할수 있게 설계하는 것이 바람직하다고함.





