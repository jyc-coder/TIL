# 제목(Header)


# 제목 1
## 제목 2
### 제목 3
#### 제목 4
##### 제목 5
###### 제목 6

# 문장(Paragraph)

동해물과 백두산이 마르고 닳도록
하느님이 보우하사 우리나라 만세

# 줄바꿈 (Line Breaks)

동해물과 백두산이 마르고 닳도록  
하느님이 보우하사 우리나라 만세  
무궁화 삼천리 화려강산  
대한 사람 대한으로 길이 보전하세


# 강조 (Emphasis)

`_이텔릭_`

_이텔릭_


`**두껍게**`

**두껍게**

`**_이텔릭 + 두껍게_**`

**_이텔릭 + 두껍게_**

`~~취소선~~`

~~취소선~~

`<u>밑줄</u>`

<u>밑줄</u>


# 목록 (List)

```
1. 순서가 필요한 목록
1. 순서가 필요한 목록
1. 순서가 필요한 목록
```

1. 순서가 필요한 목록
1. 순서가 필요한 목록
1. 순서가 필요한 목록

1만 넣었는데 저절로 숫자가 추가됨

가운데를 지운다고 하더라도 저절로 숫자가 정렬됨

![](https://velog.velcdn.com/images/jhs000123/post/efce6c21-9937-4a28-b182-8192dd03a42f/image.gif)


4번 스페이스바를 누르면 sub 메뉴로 변경됨

```
1. 순서가 필요한 목록
1. 순서가 필요한 목록
1. 순서가 필요한 목록
    1. 순서가 필요한 목록
    1. 순서가 필요한 목록
1. 순서가 필요한 목록
```

1. 순서가 필요한 목록
1. 순서가 필요한 목록
1. 순서가 필요한 목록
    1. 순서가 필요한 목록
    1. 순서가 필요한 목록
1. 순서가 필요한 목록


`- 순서가 필요하지 않은 목록`

- 순서가 필요하지 않은 목록
- 순서가 필요하지 않은 목록
- 순서가 필요하지 않은 목록
    - 순서가 필요하지 않은 목록
    - 순서가 필요하지 않은 목록
- 순서가 필요하지 않은 목록


# 링크(Links)

html에서는 a태그를 사용한다.
```
<a href="https://google.com">GOOGLE<a/>
```

`[GOOGLE](https://google.com)`
[GOOGLE](https://google.com)


링크에 마우스를 올리면 세부내용이 나타나게 하고싶을 때

```
<a href="https://naver.com" title="NAVER로 이동!">NAVER<a/>
```
[NAVER](https://naver.com "NAVER로 이동!")


클릭하여 새창에 나타나게 하는 마크다운 설정은 없으므로 a태그로 작성한다.
```
<a href="https://naver.com" title="NAVER로 이동!" target="_blankj">NAVER<a/>
```

# 이미지(Image)
  ```
  ![이미지 이름](이미지 링크)
  ```

  예시
  ```
   ![cookie](https://images.pexels.com/photos/230325/pexels-photo-230325.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1)
  ```

  ![cookie](https://images.pexels.com/photos/230325/pexels-photo-230325.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1)


  이미지를 클릭하면 작성된 링크로 이동하게 만들고 싶을 때

  ```
  [![이미지 이름](이미지 링크)](이동 링크)
  ```

  예시
  ```
[![cookie](https://images.pexels.com/photos/230325/pexels-photo-230325.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1)](https://velog.io/@jhs000123)
  ```

  [![cookie](https://images.pexels.com/photos/230325/pexels-photo-230325.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1)](https://velog.io/@jhs000123)


# 인용문(BlockQuote)

```
> 남의 말이나 글에서 직접 또는 간접으로 따온 문장.
> (네이버 국어사전)
```

> 남의 말이나 글에서 직접 또는 간접으로 따온 문장.
> (네이버 국어사전)

중첩 기능이 존재함

```
>인용문을 작성하세요!
>> 중첩된 인용문
>>> 중중첩된 인용문 1
>>> 중중첩된 인용문 2
>>> 중중첩된 인용문 3
```

>인용문을 작성하세요!
>> 중첩된 인용문
>>> 중중첩된 인용문 1
>>> 중중첩된 인용문 2
>>> 중중첩된 인용문 3


# 인라인(inline) 코드 강조
```
CSS에서 `background` 혹은
`background-image` 속성으로 요소에 배경
이미지를 삽입할수 있습니다.
```

CSS에서 `background`혹은
`background-image` 속성으로 요소에 배경
이미지를 삽입할수 있습니다.


# 블록(block) 코드 강조

![](https://velog.velcdn.com/images/jhs000123/post/7e0fc5a6-487d-4435-8a35-2cf304ec1340/image.png)


```html
<a href="https://naver.com" title="NAVER로 이동!" target="_blankj">NAVER<a/>
```


![](https://velog.velcdn.com/images/jhs000123/post/e62bf5e5-4eec-4d5e-a749-554a77b92851/image.png)


```css
.list > li {
	position: absolute;
    top: 40px;
}
```



![](https://velog.velcdn.com/images/jhs000123/post/20f99f67-c0fc-4cdd-a06b-8f55fe7a7f19/image.png)

```javascript
function func() {
	var a = 'AAA';
  	return a;
}
```

![](https://velog.velcdn.com/images/jhs000123/post/27c423c6-2171-4f31-bc19-5b4ac175fc08/image.png)


```bash
$ git commit -m 'Study Markdown'
```


개발언어가 아닐 경우 paintext라고 태그를 붙여서 작성하면 아래와같이 나타난다.

![](https://velog.velcdn.com/images/jhs000123/post/6a79f449-8c07-4724-a1fa-cbd4a7a2048a/image.png)


```plaintext
동해물과 백두산이 마르고 닳도록
하느님이 보우하사 우리나라만세
```

# 표(Table)

position 속성

```
값 | 의미 | 기본값
--|--|-- (표의 머리와 몸통을 구분하는 선 항목수만큼 "--"를 추가하는 방식이다)
static | 기준 없음 | O
relative | 요소 자신 | X
absolute | 위치 상 부모요소 | X
fixed | 뷰포트 | x
```

값 | 의미 | 기본값
--|--|--
static | 기준 없음 | O
relative | 요소 자신 | X
absolute | 위치 상 부모요소 | X
fixed | 뷰포트 | x


정렬하기 

기본값은 왼쪽 정렬이다.

가운데 :  원하는 열에 해당하는 "--"양옆에 ":"을 작성한다
오른쪽 :  원하는 열에 해당하는 "--"우측에 ":"을 작성한다

```
값 | 의미 | 기본값
--|:--:|--: 
static | 기준 없음 | O
relative | 요소 자신 | X
absolute | 위치 상 부모요소 | X
fixed | 뷰포트 | x
```

값 | 의미 | 기본값
--|:--:|--:
static | 기준 없음 | O
relative | 요소 자신 | X
absolute | 위치 상 부모요소 | X
fixed | 뷰포트 | x

# 원시 HTML(Raw HTML)

줄바꿈을 하고싶으면 스페이스바 2번을 입력하면 되지만, 이것이 적용되지 않는 환경이 존재한다.
따라서 이때는 html태그를 추가하면 동일한 효과를 얻을 수 있다
```
동해물과 <u>백두산</u>이 마르고 닳도록 <br/>하느님이 보우하사 우리나라 만세
```

동해물과 <u>백두산</u>이 마르고 닳도록 <br/>하느님이 보우하사 우리나라 만세

밑줄 같은 경우는 `span`태그를 사용하는것이 좋다.

```
동해물과 <span style="text-decoration: underline;">백두산</span>이 마르고 닳도록 <br/>하느님이 보우하사 우리나라 만세
```

동해물과 <span style="text-decoration: underline;">백두산</span>이 마르고 닳도록 <br/>하느님이 보우하사 우리나라 만세


위의 링크 섹션에서 클릭하면 새로운 창에서 나타나게 설정하고 싶을 때, 
```
<a href="https://naver.com" title="NAVER로 이동!" target="_blank">NAVER<a/>
```

혹은 이미지의 너비를 수정하고 싶을 때,
```
<img width=200 src="https://images.pexels.com/photos/230325/pexels-photo-230325.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1" alt="cookie"/>
```
<img width=200 src="https://images.pexels.com/photos/230325/pexels-photo-230325.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1"/>


원시적인 HTML태그를 작성해서 원하는 기능을 구현한다.

# 수평선(Horizontal Rule)
아래의 세 기호중 하나를 연속으로 세번 작성하면 밑줄이 생성된다.

```
---
```
---

```
***
```
***

```
___
```
___


예시(사이트 링크와 이미지 사이의 수평선)

[NAVER](https://naver.com "NAVER로 이동!")

---

<img width=200 src="https://images.pexels.com/photos/230325/pexels-photo-230325.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1"/>


