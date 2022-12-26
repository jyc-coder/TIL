
## transition

바뀌기 전 상태에 `transition-duration`을 부여해야한다.


`transition-property` : 효과 적용되는 부분.
기본값 : all
`transition-duration` : 효과 지속 시간

`transition-delat` : 효과 지연 시간 


## animation


`@keyframes`를 정의해서 자신이 직접 애니메이션을 만들수 있음

`animation-name` : 사용할 애니메이션 선택
`animation-duration`: 애니메이션 1회 동작 시간
`animation-iteration-count` : 애니메이션 반복 횟수
` animation-timing-function` : 애니메이션 동작 효과 지정


!codepen[jyc-coder/embed/KKBdxOG?default-tab=html%2Cresult]

위의 codepen은 애니메이션 반복 횟수를 `infinite`로 설정하고 `border-top`만 색상을 투명하게 해서 계속 빙글빙글 도는 애니메이션을 표현한 것이다.


animation 단축 속성은 순서를 잘 지킬 것

animation: loading 1s 1.2s infinite linear;

1s가 `duration` 1.2s가 `delay`이다.


`animation-fill-mode`: 애니메이션이 끝난후 위치 변경 방식

>  none: 아무것도 지정되지 않은 상태
forwards: 애미네이션이 키프레임의 100%에 도달했을 때 마지막 키프레임을 유지
backwards: 애니메이션의 스타일을 애니메이션이 실제로 시작되기 전에 미리 적용함.
both: forwards, backwards를 둘다 적용함



`animation-direction`: 애니메이션이 진행하는 방향

> normal : 기본값. 애니메이션
reverse: 애니메이션 역방향 재생
alternate: 애니메이션 정방향 재생 후 역방향 재생 (반드시 애니메이션 반복 횟수가 2회 이상은 되어야함)
initial: 이 속성의 기본값으로 설정
inherit: 부모 요소 속성을 상속한다.


## @media 
출력 장치의 여러 특징 가운데 일부를 참조하여 CSS 코드를 분기 처리함으로써 하나의 HTML 소스가 여러가지 뷰를 갖도록 구현할 수 있는 명세

!codepen[jyc-coder/embed/KKBdGYP?default-tab=html%2Cresult]

너비가 450px보다 작아지면 `item`의 너비가 200px로 줄어들고 배경화면 색깔이 변경된다.

### 왜 @media로 var()를 사용해서 선언이 안되는거지?

@media 옆에 max-width 대신에 따로 선언한 var변수를 대입하려고하는데 적용되지 않는다. 이유를 찾아봤는데
>
 it comes down to what the :root element actually means: the root element of the HTML document. But conceptually, media queries aren't attached to HTML elements at all. These declarations are processed while your CSS is being parsed, so it won't know to look to the :root and pull in the variable values.
 
 해석해보면
  
  > 루트 요소가 실제로 의미하는 바에 달려 있습니다 : HTML 문서의 루트 요소. 그러나 개념적으로 미디어 쿼리는 HTML 요소에 전혀 첨부되지 않습니다. 이 선언은 CSS가 구문 분석되는 동안 처리되므로 : 루트와 변수 값을 당기는 것을 알지 못합니다.
 
 출처 : https://bholmes.dev/blog/alternative-to-css-variable-media-queries/

css에 선언된 변수는 html의 멤버의 일원이 아닌 상황이므로 직접적으로 불러와서 사용이 적용되지 않는다고 말하는 것 같다. 

이를 해결하기위한 플러그인이 있다고 한다. 
https://github.com/csstools/postcss-custom-media

`@custom-media --small-viewport (max-width: 30em);`이런 방식으로 선언을 할수있게 해주는 플러그인이라고 하니 나중에 한번 기회가 있다면 사용해봐야겠다.



## Grid 잠깐 실습
!codepen[jyc-coder/embed/XWBmyLE?default-tab=html%2Cresult]

