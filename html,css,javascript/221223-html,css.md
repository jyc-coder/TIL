## flex


하나의 플렉스 아이템이 자신의 컨테이너가 차지하는 공간에 맞추기 위해 크기를 키우거나 줄이는 방법을 설정하는 속성

※ flex container : flex 속성을 가진 부모요소
※ flex items : flex 속성을 가진 부모 아래의 자식요소


- flex items의 경우 block 속성처럼 최대한으로 늘어나는 것이 아니라 최소한으로 줄어들려고 한다!

1차원 레이아웃 구조를 만드는용도로 사용하는 css 기술!!




### flex, inline flex

flex : Block 특성의 Flex Container를 정의	
inline-flex : Inline 특성의 Flex Container를 정의	


### flex-flow
단축 속성으로 Flex Items의 주 축(main-axis)을 설정하고 Items의 여러 줄 묶음(줄 바꿈)도 설정합니다.


`flex-dirction` 과 `flex-wrap`을 동시에 사용하고 싶을때 이 속성하나로 정의가 가능함




### justify-content
메인 축 방향 정렬 

- lex-start (기본값)
아이템들을 시작점으로 정렬.

- flex-end
아이템들을 끝점으로 정렬.


- center
아이템들을 가운데로 정렬.

- space-between
아이템들의 “사이(between)”에 균일한 간격 생성.

- space-around
아이템들의 “둘레(around)”에 균일한 간격을 생성.

- space-evenly
아이템들의 사이와 양 끝에 균일한 간격을 생성.


### align-content

교차 축의 정렬 방법을 설정

주의할 점은 flex-wrap 속성을 통해 Items가 여러 줄(2줄 이상)이고 여백이 있을 경우만 사용이 가능하다.

기본 값은 `strech` 교차 축을 채우기 위해 Items를 늘린다.

Content를 기준으로 정렬하는 것!


### align-items

Items가 한줄일 때 사용이 가능

여기도 기본값은 `strech`

주의할 점은 Items가 `flex-wrap`을 통해 여러 줄(2줄 이상)일 경우에는 align-content 속성이 우선하게된다!


### flex-items

- order : flex item의 순서를 설정
- flex-grow : Flex Item의 증가 너비 비율을 설정
- flex-shrink : Flex Item의 감소 너비 비율을 설정
- flex-basis : Flex Item의 (공간 배분 전) 기본 너비 설정



※ flex-grow는 콘텐츠(문자 텍스트)사이의 양 옆의 공간에 따른 여백의 비율을 결정하므로 서로의 콘텐츠의 길이가 다른 상황에서는 원하는 만큼의 비율로 나누어 지지 않을수 있다.

이럴때에는 `flex-basis`의 값으 0으로 변경하면 원하는 만큼의 비율로 나타나는 모습을 볼수 있다.

### flex

flex-grow, flex-shrink, flex-basis의 단축 속성

flex : 1

오 그럼 이건 flex-grow가 1 flex-shrink 1 flex-basis는 auto겠네?

아니다. flex:1 로 지정하면 flex-basis는 0으로 되어버린다.


### align-self

필요에 의해 일부 Item만 정렬 방법을 변경하려고 할 경우 align-self를 사용할 수 있다.


## css 코딩 관련 조언

- 부모 요소에서 width 혹은 height를 관리하는 것이 수월하다.
  부모 요소에서 너비를 정하면 자식요소에도 적용될수 있기 때문

- `position:fixed`는 무적이 아니다. 
부모요소에서 scale()을 써버리면 위치가 바뀌게 된다.
만약 `position:fiexed`를 사용했는데 원하는 위치가 보이지 않으면 scale()과같은 요소가 주변에 있는지 확인해보자.


- `position`을 사용할때는 부모 혹은 그 상위 요소에 `position`이 존재하는지를 확인해야한다. 참고로 `position`의 기본값은 staticdlek.

- `position:sticky` 를 사용할때는 top,bottom,left,right 요소중에 하나를 무조건 작성해야한다.


## 말줄임표 만들기
- 요소의 너비가 존재해야한다.
- `white-space: nowrap`을 사용해서 지정된 너비이상의 텍스트가 삐져나오게 만듦
- `overflow:hidden`을 사용해서 삐져나온 부분을 보이지 않게 설정
- `text-overflow: ellipsis`를 사용해서 ...을 추가함


## contain,cover

contain은 이미지를 부모 요소중 좁은 너비에 맞추고, cover는 이미지를 부모요소중 넓은 너비에 맞춘다.

## 마진 중복(margin collapse)

만약 3개의 아이템을 작성하고 각각 margin 값을 20px씩 하면 위 아래같의 여백은 40px로 늘어날까?

!codepen[jyc-coder/embed/ExpjRYV?default-tab=html%2Cresult]


아래의 결과를 보면 그게 아니다! 

- 형제요소의 마진이 위 아래가 만나면 서로 중복되는 현상이 발생한다!

그런데 재미있는건,


!codepen[jyc-coder/embed/abjOKNZ?default-tab=html%2Cresult]
- 이렇게 좌우로 만나는 경우는 중복현상이 발생하지 않는다!



다음 경우는 좀 다르다.

!codepen[jyc-coder/embed/vYaOrGW?default-tab=html%2Cresult]


- 부모요소의 top 와 자식요소의 margin-top으로 인한 여백이 부딪히면, 중첩이 발생해서 위쪽이 붕 뜨게 된다.


해결 방법은 부모요소에 `border`를 추가해서 서로 부딪치지 않게만 해주면 위쪽의 여백이 생기지 않게 된다!









