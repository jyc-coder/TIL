## javascript 강의 (이벤트 루프)

링크 : https://www.youtube.com/watch?v=8aGhZQkoFbQ

### 자바스크립트는 뭐에요?

싱글 스레드 논 블록킹 비동기 동적 언어입니다.....?

콜스택, 이벤트 푸르, 콜백 큐 , 이런저런 api를 보유했죠....


### 자바스크립트 런타임

![](https://velog.velcdn.com/images/jhs000123/post/a254d484-7ff3-4d44-b3fd-6efdb4cc17c7/image.png)

위의 그림은 자바스크립트의 런타임을 간단하게 그림으로 나타낸 것이다.

- heap: 메모리 할당 변수와 객체에 대한 메모리 할당

- call stack : 코드가 실행될 때 쌓이는 호출 스택

그런데 v8 프로젝트를 클로닝해서 코드 베이스를 들여다 보면, setTimeout, DOM, HTTP 요청을 관리하는 코드를 찾아볼수가 없다...

#### 엥 너 비동기 언어라면서?

위의 그림에서 좀더 디테일한 내용을 보여주면

![](https://velog.velcdn.com/images/jhs000123/post/d11e66d5-8441-4d4c-b50e-9fb5591f13be/image.png)
(이미지 출처 : https://baeharam.github.io/posts/javascript/jshow-javascript-works/)


이렇게 생겼다. V8런타임과 브라우저가 제공해주는 웹 API가 존재한다.


### call stack
> one thread == one call stack == one thing at a time

자바스크립트는 싱글 스레드 프로그래밍 언어인데, 이것은 한번에 하나의 싱글 콜 스택만을 가지고 있다는 뜻이다.
`하나의 프로그램은 동시에 하나의 코드만 실행할수 있다`

데이터 스트럭처로 실행되는 순서를 기억하고 있다. 따라서 함수를 실행하려면 스택에 해당하는 함수를 집어넣게 되고, 함수에서 리턴이 일어나면 스택의 맨위에서 해당 함수를 꺼내서 사용하게 된다.

#### 뭔소리여...

그냥 한번 눈으로 보자.
```js

function multiply(a,b){
	return a * b;
}

function square(n){
	return multiply( n,n);
}

function pringSquare(n){
	var squared = square(n);
  	console.log(squared); // 16
}
```

숫자를 입력하면 그에 대한 제곰을 해서 값을 출력하는 내용이다.

실행하게 되면 진행되는 과정은 아래와 같다. 

![](https://velog.velcdn.com/images/jhs000123/post/042f633b-f89f-424c-9f44-cffdc79f9e76/image.gif)

main() -> printSquare(n)호출 -> square(n) -> multiply(a,b)

main() <- console.log(sqaured) <- printSquare(n)리턴  <- square(n)리턴 <- multiply(a,b)리턴

이렇게 차근차근 하나씩 스택에 쌓이고 해당 함수의 내용을 실행하는 과정을 진행해 나가면서, 리턴값을 보낼때는 다시 스택에서 하나씩 제거하는 모습을 볼수 있다.


만약 같은 함수를 여러번 호출하게 되면 어떻게 될까?
```js
function fu() {
  return fu()
}

fu()
```
![](https://velog.velcdn.com/images/jhs000123/post/65d3e5f8-1c05-438c-9a28-b463bef73ba1/image.png)

함수를 호추라는 횟수가 정도를 넘어가게 되면 위와같은 오류를 보여주면서 종료시켜버린다.


### 블로킹(blocking)

정확한 정의는 존재하지 않지만, 간단하게 말하면 느리게 동작하는 코드이다.

console.log는 느린게 아니지만 while 루프안에서 수억번 돌아가면 느릴것이다.

네트워크 요청, 이미지 프로세싱 같이 느린 동작이 스택에 남아있는것을 보통 블로킹이라고 말한다.



```js
var foo = $.getSync('.//foo.com');
var bar = $.getSync('.//bar.com');
var qux = $.getSync('//qux.com');

console.log(foo);
console.log(bar);
console.log(qux);
```

예를 들어서 이런 jquery 구문을 실행한다고 가정해보면 어떻게 될까? 3개의 console.log 모두 동기적으로 실행된다고 가정했을때 하나의 console.log가 끝날때까지 아무런 동작을 할수 없게 된다. 바꿔 말하면 콜스택에 어떤 것들이 남아있으면, 네트워크 요청이 콜스택을 막아버려서 브라우저는 다른일을 할수가 없다.

이런 현상을 막기 위해서 사용하는 것이 비동기 콜백이다.

#### 비동기 콜백
어던 코드를 실행하면 결국 콜백을 받고 나중에 실행한다는 뜻

```js
console.log('hi)
setTimeout(function() {
  console.log('there');
},5000)

console.log('jyc')

```
해당 코드를 실행하게 되면 

![](https://velog.velcdn.com/images/jhs000123/post/ba70146e-5e47-459c-8b22-5c862ec8c925/image.gif)

이렇게 위아래의 console.log가 먼저 실행되고 5초뒤에 there가 나타난다. `there`를 출력하는 내용은 콜스택에 추가되지 않고 있다가. 5초뒤에 스택에 추가되고 결과를 출력해준다.


#### 엥 왜지?

여기에서 이벤트 루프와 동시성이 나설 차례가 되는 것이다.

### 이벤트 루프 & 동시성

자바스크립트는 하나에 한가지 일만 한다... 이게 정말 맞는말일까?
그럼 다른 사이트들은 로딩하면서 다른 동작도 하는데.. 순엉터리 아냐?

자바스크립트는 다른 코드를 실행시키는 동한 Ajax 요청을 실행할수 없다. setTimeout도 마찬가지이고.
근데 우리가 이걸 동시에 할수 있는 이유는 바로 
> `브라우저는 단순 런타임 이상`을 의미하기 때문이다.

처음에도 언급했지만 자바스크립트 런타임은 한번에 하나만 할수 있다고 설명했다. 하지만 브라우저가 `Web API`와 같은 것들을 제공한다. 이들은 자바스크립트에서 호출할 수 있는 스레드를 효과적으로 지원하게 된다.
아까도 말했지만 자바스크립트 엔진 내부에는 비동기적 코드가 없다고 설명했었는데, `setTimeout`이 바로 그 `web API`중에 하나였던 것!

아래의 화면을 보자.


![](https://velog.velcdn.com/images/jhs000123/post/e7044e44-2b04-4954-8c34-d7fb92da4d22/image.gif)


console.log(hi)가 콜스택에서 빠져나가 출력된다음 `setTimeout`콜에 넘어가게 되는데 `setTimeout`은 브라우저에서 제공하는 API로, setTimeout이 콜스택으로 들어가면 브라우저는 타이버를 실행시키고 카운트 다운을 시작한다.

![](https://velog.velcdn.com/images/jhs000123/post/5c10afb6-3556-40ae-bcad-9e8e66dec655/image.gif)

그리고 이것이 `setTimeout` 호출 자체가 완료되었다는 의미이고, 콜스택에서 함수를 지울수 있게 된다. 이제 바로 다음에 작성된 console.log를 실행하게 된다.

자 이제 남은건 webapis에서 신나게 돌아가고있는 timer인데 만약 시간이 경과하게 되면 어떻게 될까?
![](https://velog.velcdn.com/images/jhs000123/post/1d967f43-0e01-4dd0-9f7e-0e46901ee0fa/image.gif)

시간이 경과하게 되면 태스크 큐로 넘어가게 된다. 이제 여기서 드는 의문은, '저기있는 이벤트 루프는 뭐하는 녀석이지?' 일텐데 이벤트 루프의 역할은 `콜 스택과 태스크 큐를 주시하는 것`이다. 

- 스택이 비어있으면, 큐의 첫번째 콜백을 스택에 쌓아서 효과적으로 실행할 수 있게 해준다
- 현재 상황에서 스택이 비어있고, 태스크 큐에는 콜백이 하나가 있으니 스택으로 옮겨주게 되는 것이다.

![](https://velog.velcdn.com/images/jhs000123/post/6154741c-f88a-4f1c-88e1-1b2df6a1dc1d/image.gif)


이제 마지막으로는 cb 함수에 작성된 내용이 실행되는 것으로 마무리가 되는 것이다. 


> - 비동기 함수가 실행되면 (setTimeout, ajax Request, 따위)는 web API이기 때문에 스택에서 바로 전달되어서 작업을 마친뒤 태스크 큐로 넘긴다.
- 이벤트 루프는 콜스택과 태스크 큐를 주시하고 있다가 스택이 비었고, 태스크큐에 콜백이 존재하므로 스택으로 옮겨준다!

더 간단하게 말하면

> 비동기 함수가 스택에 추가되면 webAPI로 떠넘기고 다음 내용을 받는다.

라고 생각하면 이해가 편할 것이다.


#### setTime 함수의 진실

setTimeout 함수는 정말 지정한 시간이 지나면 실행될까?
아래의 코드는 어떻게 동작할까? 정말로 1초뒤에 주르륵 'hi'가 나타날까?


``` js
setTimeout(function timeout() {
    console.log('hi')
}, 1000);

setTimeout(function timeout() {
    console.log('hi')
}, 1000);

setTimeout(function timeout() {
    console.log('hi')
}, 1000);

setTimeout(function timeout() {
    console.log('hi')
}, 1000);

```

실상은 그렇지 않다.



![](https://velog.velcdn.com/images/jhs000123/post/6782c297-9e73-4f7a-a6f2-77e8bc7628ec/image.gif)

시간이 다 지났음에도 불구하고 큐에서 대기하고 있다가 전부 다 콜백 큐로 이동하고 난 다음 하나씩 차례로 진행하는 모습니다. 
이를 통해서 timeout이 실제로 정해진 시간과는 달리 제대로 작동하지 않을 수도 있고 다만 최소의 시간만을 지정할수 있다는 것을 알수 있다. (실제로 코드를 작성해서 실행해보면 거의 동시에 출력되는 것이라고 생각 될 
만큼 빠르게 나온다.)
위의 이미지는 자바스크립트의 비동기 콜백이 어떤 과정을 통해서 출력되는지를 보여주기위한 프로그램이다.


링크: http://latentflip.com/loupe/?code=c2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coJ2hpJykKfSwgMTAwMCk7CgpzZXRUaW1lb3V0KGZ1bmN0aW9uIHRpbWVvdXQoKSB7CiAgICBjb25zb2xlLmxvZygnaGknKQp9LCAxMDAwKTsKCnNldFRpbWVvdXQoZnVuY3Rpb24gdGltZW91dCgpIHsKICAgIGNvbnNvbGUubG9nKCdoaScpCn0sIDEwMDApOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coJ2hpJykKfSwgMTAwMCk7Cg%3D%3D!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D

코드를 작성하고 실행하면 어떤 과정을 거치는지 보여준다.



### 렌더링

브라우저는 우리가 자바스크립트로 하는 어떤 무언가로 인해서 제약을 받게된다.
브라우저는 기본적으로 화면을 매 16.6 밀리초, 즉 1초에 60프레임을 repaint 하는데, 우리가 자바스크립트로 실행하는 코드들로 인해서 혹은 다른 이유들로 제약을 받게된다. 그래서 아까 블로킹 섹션에서도 얘기 했지만 스택에 코드가 존재하면 렌더링을 못하게 된다. 렌더도 하나의 콜백처럼 행동하기 때문인데, 여기서 다른 점은 우리가 발생시킨 콜백에 비해서 더 높은 우선순위를 가진다.

매 16.6 밀리초마다 큐에 렌더가 들어가고 스택이 깨끗한 것을 확인한 후에 렌더링을 진행한다. 녹색으로 깜빡이고 있는것이 렌더링 콜백이라고 생각하자.
![](https://velog.velcdn.com/images/jhs000123/post/8fa20863-dd67-4c27-b8cd-dbc7d5aa972f/image.gif)


만약 우리가 비동기식으로 코딩을 짜지 않은 방식으로 실행을 한다면, 계속 콜스택에 코드가 머무르게 되므로 렌더링이 더뎌지게 된다. 스택에 표시된 `delay()` 우리가 원치 않게 작성한 동기적 코드라고 생각하자. 예를들면 다른 서버에서 데이터를 요청하고 계속 기다리는 요청 메서드로 봐도 좋다.

![](https://velog.velcdn.com/images/jhs000123/post/07a777f2-d586-4056-bf24-07bda6ba1308/image.gif)


만약 이런 현상을 줄이기 위해서 비동기 콜백을 사용한다면
![](https://velog.velcdn.com/images/jhs000123/post/3832b56a-01a6-43d3-b76b-72a17801847a/image.gif)
반복으로 동작하는 console.log를 setTimeout 콜백을 사용해서 webAPI로 돌린 다음 태스크 큐로 전부 전송한 다음에 하나씩 동작을 진행하면서 렌더링이 방해받는 시간을 줄이는 것이다. 물론 delay()는 똑같은 횟수로 동작하지만, 작은 시간을 벌어주게 되면서 그 시간에 하나씩 동작이 완료되는 것이다. 

이를 통해서 이벤트 루프를 막는 행위를 줄이라는 이유가 설명된다. 스택에 눌러앉게 되는 코드를 작성해서 브라우저가 할일을 못하게 되는 것이고, 이를 막기위한 유동적인 UI를 만들어야 한다는 것이다.



해당 강의를 통해서 자바스크립트가 무엇인지, 비동기적 콜백이 어떤 과정을 거쳐서 실행되는 것인지 조금 더 이해할수 있게 되어서 좋았다.

