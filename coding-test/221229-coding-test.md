## 코딩 테스트 9일차 문제 

### 조사한 자료
#### 정규식

> 정규 표현식, 또는 정규식은 문자열에서 특정 문자 조합을 찾기위한 패턴
객체로서 `exec()` `test()` 메서드 사용 가능 `String`의 `match()`, `matchAll()`, `replace()`, `replaceAll()`, `search()`, `split()` 메서드와 함께 사용 가능하다.

  패턴 작성 방법 
  
  - 단순 패턴 사용하기 - 문자열을 있는 그대로 탐색할 때 사용한다. ex) /abc/ -> 정확한 순서로 `abc`가 있는 조합을 찾는다.
  
  - 특수 문자 사용하기
  	- 어서션 : 줄이나 단어의 시작과 끝을 나타내는 경계와, 일치가 가능한 방법을 나타내는 패턴(전방탐색, 후방탐색, 조건 표현식)
    ```js
    const text = 'A quick fox';

	const regexpLastWord = /\w+$/;
	console.log(text.match(regexpLastWord));
	// expected output: Array ["fox"]
    ```
    -문자 클래스 (en-US) : 글자와 숫자처럼 다른 유형의 문자를 구분한다.
    ```js
    const chessStory = 'He played the King in a8 and she moved her Queen in c2.';
	const regexpCoordinates = /\w\d/g;
	console.log(chessStory.match(regexpCoordinates));
	// expected output: Array [ 'a8', 'c2']

    ```
    - 그룹과 범위 : 표현 문자의 그룹과 범위를 나타낸다.
    
    - 수량자 : 일치할 문자나 표현이 반복되어야할 횟수를 나타낸다.
    ```
    const ghostSpeak = 'booh boooooooh';
	const regexpSpooky = /bo{3,}h/;
	console.log(ghostSpeak.match(regexpSpooky));
	// expected output: Array ["boooooooh"]
    ```
    
    - 유니코드 속성 이스케이프 : 대/소문자, 수학기호, 문장부호처럼, 유니코드 문자 속성에 따라 문자를 구분한다. 
    ```js
    const sentence = 'A ticket to 大阪 costs ¥2000 👌.';

	const regexpEmojiPresentation = /\p{Emoji_Presentation}/gu;
	console.log(sentence.match(regexpEmojiPresentation));
	// expected output: Array ["👌"]
    ```
  
정규식 메타문자 :  표현식에서 사용되는 기호를 Meta문자라고  표현한다. 

![](https://velog.velcdn.com/images/jhs000123/post/4ce9964b-4e86-4efa-bac7-d19ec1e62051/image.png)

![](https://velog.velcdn.com/images/jhs000123/post/7d444756-6c12-4d74-9bd3-2c744cb46d9d/image.png)

플래그 문자 : 정규표현식을 사용할 때 Flag 라는 것이 존재하는데 Flag를 사용하지 않으면 문자열에 대해서 검색을 한번만 처리하고 종료하게 된다.

![](https://velog.velcdn.com/images/jhs000123/post/cda584b2-d283-439b-ae18-76fb7cc23ecf/image.png)

출처: https://hamait.tistory.com/342


#### reduce() 배열의 내장 객체

> arr.reduce(callback(accumulator, currentValue [, currentIndex[, array]]) [, initialValue])

>배열.reduce((누적값, 현잿값, 인덱스, 요소) => { return 결과 }, 초깃값);


배열(arr)의 각 element들에 대해서 파라미터로 입력받은 callback 함수를 실행하여, 하나의 리턴값을 반환하는 함수로 callback 함수는 배열(arr)의 모든 element를 대상으로 한번씩 호출된다.
다음 element에 대한 callback함수 실행시 파라미터로 입력되어 활용되며, 배열의 모든 element들에 대해 callback함수 실행이 완료되면 마지막 element의 callback 함수의 리턴값을 리턴한다.

이 메서드를 사용해서 배열의 합계를 구할수 있다.


```js
const arr = [1, 2, 3];

const result = arr.reduce(function add(sum, currValue) {
  return sum + currValue;
}, 0);

document.writeln(result); // 6
```

reduce메서드 내용을 더 간략하게 줄일수 있다.

```js
const result = arr.reduce((acc,cur,i) => {
  return acc+cur;
}, 0);
```

출처 : https://hianna.tistory.com/408

#### Number() 

문자열을 숫자로 변환하는 함수. 
프로그래머스 코딩테스트에서 문자와 숫자가 결합된 데이터에서 숫자만 추려내어 총 합계를 구하는 문제가 있었는데, 배열의 항목들을 문자에서 숫자로 바꾸는 방법을 검색하던 중에 찾게 되었다.

>Number( object )
- object: 문자열 또는 문자열을 값으로 하는 변수를 입력한다.


#### set()

배열에서 중복을 제거하는 방법중 하나이다. 
> new Set(arr)

위의 식을 입력하면 arr에 있는 데이터가 추가된 Set 객체가 생성되며, 중복은 허용되지 않기 때문에 하나씩만 요소가 추가된다. 객체에서 배열로 바꾸고 싶으면 스프레드 연산자를 쓰면 된다.

```js
const arr = ['A', 'B', 'C', 'A', 'B'];

const set = new Set(arr);
const newArr = [...set];
console.log(newArr) // [ 'A', 'B', 'C' ]
```
