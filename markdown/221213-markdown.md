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

