## 과제 구현 

### back to top 버튼 구현
```js
const flexBoxEl = document.querySelector('.flex-box')
const topBtnEl = document.querySelector('.topbtn')
const targetEl = document.querySelector('.search')
const rootElement = document.documentElement
// 검색 결과를 둘러보기위해 스크롤 해서 검색 엘리먼트가 보이지 않는 순간
// 하단에 맨 위로 이동하게 해주는 버튼이 나타남
function moveTop(entries, observer) {
  entries.forEach((entry) => {
    if (entry.isIntersecting) {
      flexBoxEl.style.opacity = '0'
    } else {
      flexBoxEl.style.opacity = '1'
    }
  })
}
// 검색창 엘리먼트를 감시하는 IntersectionObserver 생성
const topObserver = new IntersectionObserver(moveTop)
topObserver.observe(targetEl)

// 버튼을 클릭하게 되면 맨위로 스크롤 된다.
function scrollToTop() {
  // 맨 위로 스크롤
  rootElement.scrollTo({
    top: 0,
    behavior: 'smooth',
  })
}

// 버튼 엘리먼트에 이벤트 추가
topBtnEl.addEventListener('click', scrollToTop)
```
`IntersectionObserver`를 사용해서 검색창입력 엘리먼트를 target으로 지정했고, 뷰포트에 보이는 동안에는 버튼을 보이지 않게 하고 스크롤을 내려서 사라지는 순간 버튼이 나타나게 설정함

### 왜 배경화면의 색이 전체 화면으로 적용되지 않지?

`body`내부에 잇는 `viewpoint` 섹션에 색을 적용 시켰는데 내부의 엘리먼트 만큼만 칠해져 있어서 왜이러나 싶었는데,
그냥 `body` 에다가 스타일 속성을 부여하니까 해결됨


### 왜 li 엘리먼트 클릭했는데 동작을 안하지?

여러개의 li 가 렌더링 되었기 때문에 li 엘리먼트를 선택에서 `for each` 문법을 사용해서 작성했더니 동작함

```js
  const modalEl = document.querySelector('.modal')
  const list = document.querySelectorAll('.result__list__item')
  list.forEach((e) => {
    e.addEventListener('click', function () {
      modalEl.style.display = 'flex'
    })
  })
```

