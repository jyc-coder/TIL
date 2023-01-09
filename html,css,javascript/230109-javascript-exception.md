## javascript

### 표준 내장객체 
많이 알고 있을수록 좋음!

#### String

- `.length` : 문자의 길이를 반환

- `.includes()` : 대상문자에 주어진 문자가 포함되어있는지 확인

- `.indexOf()` : 대상문자에서 주어진 문자와 일치하는 첫번째 인덱스를 반환. 만약 일치하는 문자가 없다면 -1을 반환한다.

- `.match()` : 대상 문자에서 주어진 정규식(RegExp)과 일치하는 배열을 반환한다.

- `.padStart()` : 대상 문자의 길이가 지정된 길이보다 작으면, 주어진 문자를 사용해서 지정된 길이가 될때까지 `앞에` 붙여 새로운 문자를 반환한다.

- `padEnd()` : 대상 문자의 길이가 지정된 길이보다 작으면 주어진 문자를 사용해서 지정된 길이가 될 때까지 앞에 붙여 새로운 문자를 반환한다.

- `replace()` : 대상 문자에서 패텅(문자,정규식)과 일치하는 부분을 교체한 새로운 문자를 반환한다.

- `.search()` : 대상 문자에서 정규식과 일치하는 첫번째 인덱스를 반환한다.

- `.slice()` : 대상 문자의 일부를 추출해 새로운 문자를 반환

- `split()` : 대상 문자를 주어진 구분자로 나눠서 배열로 반환한다.


- `startWith()` : 대상 문자가 주어진 문자로 시작하는지 여부를 반환한다.

- `toLowerCase()` - 대상 문자를 영어 소문자로 변환해서 새로운 문자로 반환

- `.toUpperCase()` - 대상 문자를 영어 대문자로 변환해서 새로운 문자로 반환

#### 숫자

- `toFixed()` : 숫자를 지정된 고정 소수점 까지 표현하는 문자로 변환


- `toLocaleString()` : 숫자를 현지 언어형식의 문자로 반환


- `Number.isInteger()` : 숫자가 정수인지 확인한다.


- `Number.isNaN() 또는 isNaN()` : 주어진 값이 `NaN`인지 확인. `isNaN()`보다 더 엄격한 버전으로 `Number.isNaN()`사용을 추천한다.


- `Number.parseInt() 또는 parseInt()` : 주어진 값(숫자, 문자)를 파싱해서 특정 진수의 정수로 반환한다.

- `Number.parseFloat() 또는 parseFloat()` : 주어진 값을 파싱해서 부동소수점 실수로 반환한다.


#### Math


- `Math.abs()` : 주어진 숫자의 절댓값을 반환한다.

- `Math.ceil()` : 주어진 숫자를 올림해 정수를 반환합니다.

- `Math.floor()` : 주어진 숫자를 내림해 정수를 반환

- `Math.round()` : 주어진 숫자를 반올림해 정수를 반환한다.

- `Math.max()` : 주어진 숫자중에서 가장 큰 숫자를 반환

- `Math.min()` : 주어진 숫자중에서 가장 큰 숫자를 반환

- `Math.pow()` : 주어진 숫자의 거듭제곱한 값을 반환한다.

- `Math.random()` : 0 이상 1미만의 난수를 반환한다.

#### Date
`new Date()`를 통해 반환되는 인스턴스를 '타임스탬프' 라고 한다.


- `getFullYear(), setFullYear()`:
날짜 인스턴스의 연도를 반환하거나 지정한다.

- `getMonth(), setMonth()`: 날짜 인스턴스의 연도를 반환하거나 지정한다.

- `getDate(), setDate()`: 날짜 인스턴스의 '일'을 반환하거나 지정한다.

- `.getHours(), .setHours()` : 날짜 인스턴스의 '시간' 을 반환하거나 지정한다.


- `.getMinutes(), .setMinutes()` : 날짜 인스턴스의 '분'을 반환하거나 지정

- `.getSeconds(), .setSeconds()` : 날짜 인스턴스의 '초'를 반환하거나 지정한다. 


- `.getDay()` : 날짜 인스턴스의 '요일'을 반환한다.

-`.getTime(), setTime()` : 유닉스 타임으로부터 날짜 인스턴스의 경과한 시간을 '밀리초'로 반환하거나 지정한다.
유닉스타임 : 1970.01.01 00:00:00 시간의 의미한다.

- `toUTCString()` : 날짜 인스턴스의 협정 세계시(UTC)를 반환한다. 협정 세계시(UTC) 혹은 그리니치 평균시(GMT)는 영국 런던 기점의 기준시이다. 참고로 한국은 UTC기준보다 9시간 빠르다.


-`toISOString()` : 날짜 인스턴스의 협정 세계시(UTC)를 'ISO 8601' 포맷으로 반환합니다. 'ISO 8601'는 날짜와 시간을 표현하는 국제 표준 규격이다. 

- `Date.now()` : 유닉스 타임(UNIX Time)으로부터 메소드가 호출될 때의 시간을 밀리초로 반환한다.

## javascript 과제

### 무한스크롤 예외처리 : N/A 이미지
- 이미지의 url 이 없어서 N/A로 img src에 들어가니 오류가 발생해서, 더미 이미지를 하나 생성하고 src의 내용에 N/A가 존재하는 경우에만 해당 더미 이미지를 사용하게 설정했다.

```js
 posterEl.src =
      movie.Poster.indexOf('N/A') === -1
        ? movie.Poster
        : 'https://dummyimage.com/300x445/000/3039b0.jpg&text=not+found'

```
### 무한스크롤 예외처리 : undefined listItem

listItem의 type이 undefined가 되는 상황을 고려하지 않고 코드를 작성하니, undefined 오류가 계속 콘솔창에 나타나서, undefined가 나오지 않을때에만 `unobserve`를 실행하게 설정을 변경했다.
```js
  // undefined 예외처리
    if (listItem.typeOf !== undefined) {
      lastItemObserver.unobserve(lastItem.target)
    }
```
 

### 문제 : 무한 스크롤 예외처리

무한스크롤의 기능은 잘 동작하는데 결과물이 모두 나오고 스크롤을 계속 움직이면 콘솔창에 오류가 계속 나타나는 현상이 나타남

![](https://velog.velcdn.com/images/jhs000123/post/b159b75b-d47e-49b1-8c81-25233fa435de/image.gif)

#### 무슨 오류인가?

![](https://velog.velcdn.com/images/jhs000123/post/a76cc636-a24e-4077-a801-78be34185169/image.png)

현재 영화 api에서 데이터를 가져오는 메서드인 getMovies가 계속 호출되는데, 검색 결과는 이미 다 렌더링 된 상태에서 최대 페이지를 넘어선 수치로 `fetch`를 시도하기 때문에 `undefined`를 읽어올수 없게되고, 그로 인해서 오류가 발생하게 됨.

분명히 최대 페이지를 넘어서면 json 값의 Response의 값이 false가 되기 때문에 해당 boolean 값이 변경되는 순간 `getMovies`가 실행되지 않을 거라고 생각했는데...

```js
const isExist = Boolean(json.Response)

...


 if (isExist) {
      getMovies(false)
    }
```

강사님께 한번 질문을 드리고 답변을 들은 다음 접근 방식을 변경했다. 최대 페이지 이상의 pageNum 상태에서 메소드를 호출하면 false가 나타나긴 하지만, Response의 값이 false 인 경우는 그것만이 해당되지 않을 수 있다는 점을 강조해주셨고, 데이터를 가져오는 과정에서도 충분이 false가 나타날수 있다는 것을 보여주셨다. 


#### 어떻게 해결 했는가?

영화를 검색하면 결과물의 갯수가 보이는데 (`totalResults`) 한번 렌더링 되는 갯수는 10개 즉, 최대로 렌더링 될수 있는 페이지는 `totalResults`를 10으로 나눈값에서 반올림한 값이다,(10개가 아니어도 렌더링은 되기 때문에) 

따라서 렌더링 될수 있는 최대의 페이지수를 정의했다. 

```js
// 총 영화 편수
  const totalResult = parseInt(json.totalResults)
  // 렌더링 될수 있는 페이지의 수
  const pageLimit = Math.round(totalResult / 10)
```

만약에 `pageNum` 이 `pageLimit`보다 같거나 작을 동안에만 `getMovies`가 실행되게 코드를 수정했다. 

```js
if (pageLimit >= pageNum) {
      getMovies(false)
    }
```

![](https://velog.velcdn.com/images/jhs000123/post/ed841a6e-5a65-4b93-ab45-6162493e579c/image.gif)

성공이다! 오류가 나타나지 않게 되었다.

