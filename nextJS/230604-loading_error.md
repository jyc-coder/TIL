## loading state
서버사이드 렌더링에서는 클라이언트가 요청하면 서버측에서는 데이터베이트에서 데이터를 가져오고, html과 함께 클라이언트로 보내주는데, 여기서 0.2 ~ 2초 가량 소요된다.(물론 더 소요될수 있음)

그렇다면, 그동안에는 아무것도 보여줄수 없는데, 이를 대신할수 있는 무언가가 있다면 어떨까?

여기서 로딩 화면이 등장하는 것이다. next.js 13에서는 loading.tsx 파일을 만들어서 로딩 화면 컴포넌트들 생성하면 로딩중에서는 해당 컴포넌트를 보여줄수 있게된다.

app 경로에 loading.tsx 파일을 생성해서 

```jsx
import React from 'react'

export default function loading() {
  return <div className="h-screen bg-purple-400">loading...</div>
}

```
이런식으로 loading... 문구와 함께 보라색 배경화면을 보여주는 로딩 화면을 만든 다음 프로젝트를 실행해보자.

![](https://velog.velcdn.com/images/jhs000123/post/e4ba70ef-e925-4190-a438-c090603a1a0d/image.gif)


그냥 파일 추가만 한것 뿐인데도 실행을 하면 자동으로 로딩화면을 보여준다

그럼 만약에 메인 페이지에서 보여주는 12개의 카드에 스켈레톤 ui를 적용시키고 싶다면

```jsx
export default function loading() {
  return (
    <main>
      <Header />
      <div className="flex flex-wrap justify-center py-3 mt-10 px-36">
        {[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12].map((num) => (
          <div
            key={num}
            className="w-64 m-3 overflow-hidden border rounded cursor-pointer animate-pulse bg-slate-200 h-72"
          ></div>
        ))}
      </div>
    </main>
  )
}
```

이런식으로 카드 목록이 가진 스타일속성을 그대로 사용해서 위치를 동일하게 맞춰주고, 해당 div 에는 배경색깔을 회색으로 설정한 스켈레톤 ui를 보여주는 방식으로 만들 수 있다.
![](https://velog.velcdn.com/images/jhs000123/post/257084ee-5b41-4354-a252-9d3cfdcea400/image.gif)


물론 이렇게만 하면 될것 같지만 만약에 해당 식당중 하나를 선택해서 상세 페이지로 이동할때의 로딩 화면도 위와 동일하게 된다. 
`loading.tsx`파일을 루트 경로에 생성해서 로딩 컴포넌트를 제작했다면 그것이 이 프로젝트의 default 로딩 화면이 된다.

그렇다면, 클릭해서 들어간 상세페이지와 일치하지 않은 레이아웃의 스켈레톤 ui 대신 다른 로딩 화면을 원한다면 어떻게 할까?


상세페이지 경로에 `loading.tsx`파일을 따로 만들면 된다.
```jsx
import React from 'react'

export default function loading() {
  return (
    <main>
      <div className="overflow-hidden h-96 animate-pulse bg-slate-200">
        <div className={`bg-center h-full`} />
      </div>
      <div className="flex items-start justify-between w-2/3 m-auto 0 -mt-9">
        <div className="bg-white w-[70%] rounded p-3 shadow">
          <nav className="flex pb-2 border-b text-reg">
            <h4 className="mr-7">Overview</h4>
            <p className="mr-7">Menu</p>
          </nav>

          <div className="mt-4 border-b pb-6 animate-pulse bg-slate-200 w-[400px] h-16 rounded"></div>

          <div className="flex items-end animate-pulse">
            <div className="flex items-center mt-2 ratings">
              <div className="flex items-center w-56 bg-slate-200"></div>
              <p className="ml-3 text-reg"></p>
            </div>
            <div>
              <p className="ml-4 text-reg"> </p>
            </div>
          </div>
        </div>
      </div>
    </main>
  )
}

```
![](https://velog.velcdn.com/images/jhs000123/post/161c82a6-94e6-4405-a7d9-30cc7c901bb3/image.gif)

그럼 이제 이전의 로딩 화면이 아닌 위의 코드로 생성한 로딩 ui가 보여지게 된다.

## error state

error 페이지도 이와 비슷하게 만들 수 있다. 
원하는 페이지 경로에 `error.tsx`파일을 만들면 끝!

예를 들어서 링크에 `restaurant/말도안되는 식당이름` 이렇게 작성해서 에러 페이지를 보여주게 하려면, restaurant 경로에 `error.tsx`파일을 생성해서 오류 컴포넌트를 만들어주면 된다.

```jsx
'use client'
import Image from 'next/image'
import React from 'react'
import errorMascot from '../../public/icons/error.png'

export default function Error() {
  return (
    <div className="flex flex-col items-center justify-center h-screen bg-gray-200">
      <Image src={errorMascot} alt="error" className="w-56 mb-8" />
      <div className="bg-white rounded shadow p-9 py-14">
        <h3 className="text-3xl font-bold">에고! </h3>
        <p className="font-bold text-reg">여긴 그 식당이 없어요...</p>
        <p className="mt-6 text-sm font-light">Error Code: 400</p>
      </div>
    </div>
  )
}
```
error.tsx는 클라이언트 컴포넌트이므로 `'use client'`를 꼭 맨위에 작성해줘야한다.


![](https://velog.velcdn.com/images/jhs000123/post/502e34da-a7b7-4896-b167-eed8d9c988b6/image.png)


그런데 여기서 오류의 세부 내용을 보여주고 싶다면 Error 컴포넌트에 추가되어있는 props을 불러오면 되는데 next.js가 Error 컴포넌트에 자동적으로 주입하는 것으로 오류의 상세 정보를 불러올수 있다

`{ error }: { error: Error }` 이런식으로 Error 컴포넌트 props에 추가해주고 `{error.message}`를 확인해보면 

![](https://velog.velcdn.com/images/jhs000123/post/803ac841-ffb1-4264-be22-8bfcba329dc4/image.png)

이렇게 오류에 대한 설명이 나와있지만... 사용자가 이런 내용의 오류보다는 조금더 직관적인 오류 내용을 보여주는 것이 좋을 것 같다.
그렇다면, 오류가 발생하는 곳에서 throw Error 를 사용하는 구간에 설명을 추가해준다면?

![](https://velog.velcdn.com/images/jhs000123/post/4b0ded4b-72d0-41d4-97de-ec21dea4531e/image.png)
 위의 코드는 restaurant 카드를 클릭했을때 데이터베이스에서 해당 식당을 찾는 코드이며, 만약 존재하지 않을 경우에는 Error를 던지는 코드이다. 여기에서 지금 `식당 이름을 찾을 수 없습니다`와 같은 설명을 작성하고 다시 오류페이지로 가보면?
 
 
 ![](https://velog.velcdn.com/images/jhs000123/post/a7c5851f-a167-4a2d-92ba-89dfd5cf0539/image.png)

이렇게 원하는 상황에서의 오류 내용을 조절할수 있게된다!

 

