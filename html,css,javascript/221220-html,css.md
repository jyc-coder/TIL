# TIL

## 부모, 자식, 상위요소

html의 요소들은 모두 하나 이상의 다른 요소를 포함할 수 있는데,  이 경우, 포함하는 요소를 부모요소, 포함되는 요소를 자식요소라고 한다.


부모요소 : 상위레벨
하위요소 : 하위레벨
상위(조상)요소 : 부모의 상위레벨


## 클래스 
요소의 이름을 지정

전역 속성: 어디든지 작성이 가능한 요소 (id,class)

## 컴포넌트를 제작할때는 끝부분에 빈 태그를 붙여야한다.


## input
focus 가능 요소 : 메인화면에서 `tab`키를 누르면 focus가 된다.

contenteditable을 태그에 추가하면 메인 화면에서 내용을 수정할 수 있게 된다.

그리고 `tabindex`를 0으로 부여하면 focus가 안되는 div에서도 focus가 가능해진다.


## data-XXX

html 의 태그에 data-xxx ="내용"을 추가하면 html에 javascript 에 사용할 데이터를 잠시 보관하는 용도로 사용한다.

## inline? block? 왜 나눔?

화면에는 글자와 레이아웃이 존재한다. 

- `inline`은 글자를 취급하기 위한 요소 

- `block`은 전후 줄바꿈이 들어가 다른 엘리먼트들을 다른 줄로 밀어내고 독립적으로 한 줄을 차지함

한줄로 나타내고 싶은데 block 요소를 사용해버리면 한줄이 넘어가버리니까 원하는 상황에 알맞게 사용하기 위해서 나눈것 

## 기본값?

css 의 요소는 저마다 기본값이 존재한다.

width의 기본값은 100%가 아니라 auto이다. (최대한으로 늘어나려고 함)
height의 기본값도 마찬가지이다.  (최소한으로 줄어들려고 함)

> 블록 요소
- 가로너비 -> 최대로 늘어남
- 세로너비 -> 최대로 줄어듦<br/>
인라인 요소
- 가로너비 => 최대로 줄어듦
- 세로너비 => 최대로 줄어듦


css 공부할때 기본값을 외워라
무조건

## 시맨틱 태그 왜씀?

웹 접근성 때문에

사이트를 만들었을때 모든 사람이 사이트를 이용할 수 있어야한다. 만약 헤더를 서로 다른 이름으로 각각 정의해버린다면, 다른 사람들이 이해하기 힘든 경우가 생길 수 있다. 




## 선택자 
.container ul li:nth-child(4)

항상 제일 오른 쪽에 있는 선택자로 선택된다. 따라서 오른쪽 부터 조건을 읽어라

li의 4번째 항목이고 ul에 속해있으며 container 클래스에 있어야 하는 조건을 만족하는 요소


## 가상 클래스 vs 가상 요소

> 가상 클래스: 실제로 존재하는 요소에 특정 이벤트나 환경에 맞춰 가상으로 클래스를 주는 것 
(ex:  `:active`,`:checked`) inline 요소<br/>
가상 요소: 실제로 존재하지 않는 가상의 요소를 만들어 스타일을 주는 것
(ex: `::before`) block 요소




## 상속

문자와 관련된 속성은 상속됨
font-weight
font-size
font-family
text-align


## 웹 폰트

sans-serif -> 고딕
serif -> 바탕
carsive -> 필기체
mono space -> 고정너비체

웹 폰트의 조건 - 경량화 (1mb이하) 
더 빠른 로딩 속도를 위해서


- line-height: x

글자 크기 기준으로 x배 한 줄높이를 부여한다.

- white-space: nowrap;

 스페이스와 탭, 줄바꿈, 자동줄바꿈을 어떻게 처리할지 정하는 속성 
 
 출처 : https://www.codingfactory.net/10597


- word-break: keep-all
한글의 경우 영어와는 다른게 단어를 간격으로 줄바꿈이 되지 않는데, 위의 속성을 사용하면 한글의 단어를 단위크기와 여백의 크기를 비교해서 줄바꿈이 이루어진다.



# inherit

부모가 가진 값을 자식이 그대로 가져가는 속성(강제 상속)


```html
<div class="container">
      <div class="item"></div>
    </div>
```

```css
.container {
  width: 200px;
  height: 100px;
  border: 4px solid black;
  background-color: orange;
}

.container:hover {
  background-color: green;
}

.item {
  width: 300px;
  height: 50px;
  background-color: inherit;
  border: 4px solid red;
}
```

item에 마우스를 올려도 부모와 같이 `green`으로 변하게 된다.


!codepen[jyc-coder/embed/jOpOgyQ?default-tab=html%2Cresult]


## 검증은 순수한 프로젝트에서 해라

이런거 저런거 코드가 많이 적힌 프로젝트에서의 검증은 확실하지 않을 가능성이 농후하다.
빈 프로젝트를 따로 만들어서 해당 증명 사실을 검증해야한다.

항상 너무 신뢰를 하지 마라.



## 우선 순위


선택자의 종류

전체(0점), 태그(1점), 클래스(10점), 아이디(100점)

왼쪽에서 오른쪽 순으로 순위 중요도가 높아진다! 

```html
<div class="container">
      <div class="item">123</div>
    </div>
```

```css
.container .item {
  color: blue;
}

.item {
  color: red;
}

```

이런 상황에서 item 클래스의 텍스트는 파랑색으로 된다.

- 점수가 같다고 해도 아이디 선택자라면 아이디 선택자가 우선순위가 높다!

- 점수가 같다고 해도 나중에 작성한 것이 우선순위가 높아진다!

- html에 직접적으로 `style`태그로 추가한 경우는 1000점으로 인식되고, id 10개를 갖다 붙여도 무조건적으로 인식됨


