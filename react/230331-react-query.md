reqct query 의 활용방법에 대해 이해한다.

## react query


### 1) 특징

react-query 는 서버 데이터 요청/관리를 위한 라이브러리로서 ,최근 가장 많이 사용되는 라이브러리중 하나이다.

지금까지는 redux를 통해서 프론트엔드 자체적인 데이터와 함께, 서버에서 받아온 데이터도 모두 하나의 store 에 담고 관리를 진행했다.

그러다보니 redux로 관리하는 데이터에는 새로운 데이터가 추가되어있는데, 서버에는 아직 데이터가 추가되지 않았다거나 그 반대의 경우도 발생할 수 있다.

react-query 를 사용하면 해당 문제를 해결할 수 있고, 이러한 데이터의 동기화 문제와 별개로, 프론트 자체 데이터와 서버 데이터를 별개의 코드로 관리함으로써 개발하는 입장에서도 훨씬 명확한 코드를 작성할 수 있게 된다.


`react-query`에는 이런 기능이 포함되어있다.

- 서버 데이터 캐싱
- 데이터가 오래되면 자동으로 다시 요청
- 한번 GET 해온 데이터에 대해서 , POST/PUT 등으로 인해 데이터가 업데이트가 되면 자동으로 다시 요청
- 동일한 데이ㅣ터를 여러번 요청할 경우 한번만 요청
- 해당 화면에 focus할 경우 자동으로 데이터를 다시 요청
- 요청 실패시 자동으로 다시 요청


** rtk query vs react-query? **
rtk query는 도입된지 얼마 되지 않았으므로 미완성된 기능들이 존재한다. 
react-query는 그런부분까지 업데이트가 되어있다.



### 2) 설치


> yarn add react-query

> yarn add axios


우선 아래와 같이 QueryClientProvider 로 컴포넌트를 감싸주고 시작한다

```jsx
import { QueryClient, QueryClientProvider } from "react-query";

const queryClient = new QueryClient();

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <component />
    </QueryClientProvider>
  );
}

export default App;
```

이렇게 하면, 내부 컴포넌트에서 React-query 관련 함수들을 사용할 수 있을 뿐만 아니라 마치 context api 처럼 전역에서 캐싱된 쿼리 데이터를 사용할 수 있게 된다.

이렇게만 설정해도, react-query 를 쓸 준비는 끝난다.


### 3) dev-tools 활용

또 설치함? 

react query 는 자체적으로 dev tools 를 가지고 있다.

```jsx
import { QueryClient, QueryClientProvider } from 'react-query'
import { ReactQueryDevtools } from 'react-query/devtools'

const queryClient = new QueryClient()

function App() {
    return (
        <QueryClientProvider client={queryClient}>
            <ReactQueryDevtools initialIsOpen={true} />
            <컴포넌트 />
        </QueryClientProvider>
    )
}

export default App
```


### 4) router 설정하기

이제 react query 가 가진 hook 을 활용해보면서 각종 쿼리 옵션을 살펴볼 것이다. 옵션에 대해서 정확히 이해하기 위해 router 설정을 한다.

Post 페이지와 User 페이지를 만들어서 App에 사용할 것이다.

```jsx
import { QueryClient, QueryClientProvider } from 'react-query'
import { ReactQueryDevtools } from 'react-query/devtools'
import { BrowserRouter, Link, Route, Routes } from 'react-router-dom'
import Post from './pages/Post'
import User from './pages/User'

const queryClient = new QueryClient()

function App() {
    return (
        <QueryClientProvider client={queryClient}>
            <ReactQueryDevtools initialIsOpen={true} />
            <BrowserRouter>
                <nav>
                    <Link to="/post">Post</Link> | <Link to="/user">User</Link>
                </nav>
                <Routes>
                    <Route path="post" element={<Post />} />
                    <Route path="user" element={<User />} />
                </Routes>
            </BrowserRouter>
        </QueryClientProvider>
    )
}

export default App
```

## useQuery

### 1) useQuery로 GET 요청 보내기

react-query 는 GET 요청 시와 POST/PUT 요청 시 사용하는 Hook 이 구분되어 있다.

`useQuery` 는 서버에 GET 요청을 보내기 위한 함수이다.

아래처럼 사용할 수 있다. (이외에도 isFetching, isError, refetch 등이 반환됨)
```js
const { isLoading, data, error } = useQuery(쿼리 키값, axios 요청보내는 함수, {쿼리 옵션})
```
- 쿼리 키

쿼리 키값은 해당 요청에 대한 응답 데이터에 이름을 붙이는 것이다. 해당 쿼리 키값을 명시해놓으면, 아래 코드를 통해서 해당 응답 데이터를 어디서든 가져와서 사용할 수 있다.

```jsx
const queryClient = useQueryClient()
const data = queryClient.getQueryData(queryKey)
```
쿼리 키값은 아래처럼 일정한 컨벤션에 맞춰서 배열형태로 적는 것이 좋다. (다만 단순히 ‘todos’ 라고 적더라도, react query 가 자체적으로 [’todos’] 로 반환한다.)
```jsx
{
  ['todos', 'list', { filters: 'all' }],
  ['todos', 'list', { filters: 'done' }],
  ['todos', 'detail', 1],
  ['todos', 'detail', 2],
  ['todos', 1],
  ['todos', 2],
}
```


해당 함수가 실행 되면 무엇이 반환되는가?

- **isLoading : 로딩 중인지 여부 (처음 요청을 하는 경우)**
- **isFetching : 로딩 중인지 여부 (첫 요청 이후에 refetch 를 하는 경우)**
- **isError : 에러 여부**
- **isSuccess : 요청 성공 여부**
- **error : 에러 객체**
- **status : 현재 상태 (loading, error, success 가 들어감, is~~~ 대신 사용 가능)**
- **data : 응답 데이터**
- **refetch : 다시 요청을 하기 위한 함수 (이 함수를 실행하면 해당 요청을 다시 보낼 수 있음)**

이전에는 로딩 여부, 에러 여부에 대해서 직접 state 를 만들어서 관리를 해주어야 했지만, react-query 는 자동으로 해당 state 들을 만들어준다.

그럼 getPosts 요청을 보내보자
```jsx
const getPosts = async () => {
    const response = await axios.get('https://jsonplaceholder.typicode.com/posts')
    return response.data
}
const { isLoading, data: posts, error } = useQuery('posts', getPosts)
```
Post 컴포넌트 안에서 작성하면 끝이다! (쿼리 키 값은 posts로 지정했다.

`Post.jsx`
```jsx
import axios from 'axios'
import React from 'react'
import { useQuery } from 'react-query'

function Post() {
    const getPosts = async () => {
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts')
        return response.data
    }
    const { isLoading, data: posts, error } = useQuery('posts', getPosts)

    if (isLoading) return <div> Loading...</div>
    if (error) return <div>{error.message}</div>
    return (
        <div>
            {posts.map((post) => (
                <div key={post.id}>
                    <h3>{post.title}</h3>
                    <p>{post.body}</p>
                </div>
            ))}
        </div>
    )
}

export default Post

```

![](https://velog.velcdn.com/images/jhs000123/post/12bf44bd-93c0-4f24-ac22-f7b1d24be6d1/image.png)


### 2) 쿼리 옵션 (데이터 캐싱) 및 refetch

- staleTiem(데이터의 fresh 상태 유지기간)

재요청을 보내지 않아도 되는 데이터 상채(아직 오래되지 않은 상태)를 `fresh 상태`라고 하며 다시 재요청을 보내야 하는 데이터 상태를 `stale 상태` 라고 말한다

쿼리 옵션에서 staleTime 을 지정해주면 fresh 상태로 유지되는 시간을 지정할수 있다.

```jsx
// staleTime을 10초로 설정
const { isLoading, data: posts, error } = useQuery('posts', getPosts, { staleTime: 10000 })
```

react-query 는 유저가 화면에 focus 하는 순간 요청을 다시 보내는데, fresh 상태일 때는 요청을 다시 보내지 않는 것을 확인할 수 있다.

- cacheTime

쿼리 요청을 보낸 페이지를 보다가, 다른 페이지를 보게 될 경우 처음의 쿼리는 `inactive 상태`가 됩니다. 이 순간부터 얼마동안 데이터를 보관하고 있을 지 선택하는 것이 바로 cacheTime 이다.

```js
// cacheTime을 10초로 설정
const { isLoading, data: posts, error } = useQuery('posts', getPosts, { cacheTime: 10000 })
```

- refetchOnWindowFocus

해당 윈도우에 focus 했을 때 다시 요청을 보낼지 설정한다.
기본값은 true 라서 항상 윈도우에 focus 할때마다 요청을 다시 보낸다(stale 상태라면)

- refetch
말 그대로 다시 요청을 보내는 것이다.

```jsx
const { isLoading, data: posts, error, refetch } = useQuery('posts', getPosts, {})
```
refetching을 하는 과정에서 로딩 화면을 보여주고 싶다면 isFetching을 추가해서 로딩을 보여주면 된다.


- enabled
해당 쿼리 요청을 활성화할지 여부를 정하기 위한 옵션이다.
```jsx
const { isLoading, data: posts, error } = useQuery('posts', getPosts, { enabled: isPrepared })
```

이 기능을 사용해서 원하는 타이밍에 요청을 보낼수 있다.

```jsx
import axios from 'axios'
import React, { useState } from 'react'
import { useQuery } from 'react-query'

function Post() {
    const getPosts = async () => {
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts')
        return response.data
    }
    const [isPrepared, setIsPrepared] = useState(false)
    const { isLoading, data: posts, error } = useQuery('posts', getPosts, { enabled: isPrepared })
    if (isLoading) return <div> Loading...</div>
    if (error) return <div>{error.message}</div>
    return (
        <div>
            <button onClick={() => setIsPrepared(!isPrepared)}>요청보내기</button>
            {posts &&
                posts.map((post) => (
                    <div key={post.id}>
                        <h3>{post.title}</h3>
                        <p>{post.body}</p>
                    </div>
                ))}
        </div>
    )
}

export default Post

```

이런식으로 state를 생성해서 버튼을 클릭할때 `isPrepared`를 변경해서 query의 호출을 실행하게 설정할수 있다.
![](https://velog.velcdn.com/images/jhs000123/post/0275eb80-583b-4ca7-8ded-8a295cc21d31/image.gif)

posts가 비어있다가, 버튼을 누르니 그제서야 요청이 가고 채워지는 것을 확인할수 있다. 


### 3) 요청후 처리 (success, error, settled, select)

react-query 에서는 요청의 성공, 실패 등의 여부에 따라서 특정한 코드를 실행하도록 설정해줄수 있다.
필요한 경우에 따라, 아래와 같은 옵션을 명시하면 된다.


1. onSuccess(요청 성공하면..?)

```jsx
const { isLoading, data: posts, error } = useQuery('posts', getPosts, {
    onSuccess: (data) => {
        console.log('데이터 요청 성공', data)
    }
})
```

2. onError(요청 실패 하면...?)
```jsx
const { isLoading, data: posts, error } = useQuery('posts', getPosts, {
    onError: (error) => {
        console.log('데이터 요청 실패', error)
    }
})
```

3. onSettled(성공, 실패 여부와 상관없이 요청 완료시 실행)

```jsx
const { isLoading, data: posts, error } = useQuery('posts', getPosts, {
    onSettled: (data, error) => {
        console.log('데이터 요청 완료')
    }
})
```

4. select(데이터 받은후 가공처리 가능!)

```jsx
const { isLoading, data: posts, error } = useQuery('posts', getPosts, {
    select: (posts) => {
        return posts.filter(post => post.id === 1)
    }
})
```

### 4) 인자를 받아서 요청보내기

id 번호를 토대로 특정 게시글 하나를 받아오는 요청을 보내고, 해당 게시글을 표시하는 작업을 진행해보자.

먼저 api 요청 코드를 다른 폴더에 집어넣고 import 해서 사용하려고 한다.
`api/services/Posts.js`
```jsx
import axios from 'axios'

export const getPosts = async () => {
    const response = await axios.get('https://jsonplaceholder.typicode.com/posts')
    return response.data
}

export const getPost = async (id) => {
    const response = await axios.get(`https://jsonplaceholder.typicode.com/posts/${id}`)
    return response.data
}
```
id 를 받아서 axios 요청을 보내는 getPost 함수를 여기다가 미리 만들어둔다

(이제 axios 관련 함수들은 모두 여기에 작성한다고 생각하면 된다)

특정 게시글하나를 보고싶다면 그에대한 useQuery는?
```jsx
const { isLoading, data, error } = useQuery(['posts', 글 아이디], () => getPost(글 아이디))
```
이렇게 작성할수 있다.
```jsx
import axios from 'axios'
import React, { useState } from 'react'
import { useQuery } from 'react-query'
import { getPost } from '../apis/services/posts'

function Post() {
    const { isLoading, error, data: post } = useQuery(['posts', 2], () => getPost(2))

    if (isLoading) return <div> Loading...</div>
    if (error) return <div>{error.message}</div>
    return (
        <div>
            <div key={post.id}>
                <h3>{post.title}</h3>
                <p>{post.body}</p>
            </div>
        </div>
    )
}

export default Post

```

이렇게 작성해서 실행해보면
![](https://velog.velcdn.com/images/jhs000123/post/db01aa8c-ad60-42a5-95d3-ec8671877866/image.png)

tool을 보면 ['posts', 2] 라는 쿼리 키값이 존재하는 것이 보인다!

### 5) 병렬 처리

useQuery 는 아래처럼 여러 개를 작성해도 병렬적으로, 동시에 요청을 보낸다.

```jsx
const {data: users} = useQuery('users', getUsers)
const {data: posts} = useQuery('posts', getPosts)
```

그런데 이렇게 작성할 경우, 각각에 대해서 isLoading 과 error 을 처리해주어야 한다.

이를 쉽게 해결하는 방법은 useQueries 를 쓰는 것이다!
```jsx
 const results = useQueries([
        {
            queryKey: 'user',
            queryFn: getUsers,
        },
        {
            queryKey: 'posts',
            queryFn: getPosts,
        },
    ])
```

이렇게 요청하면 어떻게 데이터가 올까?
![](https://velog.velcdn.com/images/jhs000123/post/e74f097c-bfc4-4f5e-b176-441e5fa4110b/image.png)

이런 식으로 각각의 요청에 따른 status가 배열형태로 출력된다.

따라서 만약 여러개의 요청을 통해서 로딩 여부를 판단하기 위해서는 

```jsx
 if (results.some((result) => result.isLoading)) return <>로딩중...</>
```
이런 식으로 some 을 사용해서 하나라도 true 일 경우에는 계속 로딩 화면을 표시해주면 된다!

## useMutation

useMutation 은 post, put, delete 등의 method 로 서버의 데이터에 변화를 일으키고자 할 때 쓰는 Hook이다.

기본 형태
```jsx
const { mutate, isLoading, error } = useMutation(axios 요청 함수);
```

useMutation 은 useQuery 와 마찬가지로 isLoading, isError 등을 반환하지만, 쿼리 키값을 명시하지 않는다는 점에서 차이가 있다.

그리고 위처럼 작성했다고 해서 바로 요청이 보내지는 것이 아니라, `mutate(보낼 데이터)`  와 같이 실행해야만 해당 요청이 보내진다는 점에서도 차이가 있다. 

`App` 에서 글쓰기 링크를 추가하여 클릭하면 작성 페이지로 넘어가게 설정했다.
```jsx
  <BrowserRouter>
                <nav>
                    <Link to="/post">Post</Link> | <Link to="/user">User</Link> | <Link to="/post/new">글작성</Link>
                </nav>
                <Routes>
                    <Route path="post" element={<Post />} />
                    <Route path="post/new" element={<PostCreate />} />
                    <Route path="user" element={<User />} />
                </Routes>
            </BrowserRouter>
```

`PostCreate`
```jsx
import React, { useState } from 'react'
import { useMutation } from 'react-query'
import { createPost } from '../apis/services/posts'

function PostCreate() {
    const { mutate, isLoading, error } = useMutation(createPost)

    const [post, setPost] = useState({ title: '', body: '' })
    const onChange = (e) => {
        const { name, value } = e.target
        setPost({ ...post, [name]: value })
    }

    return (
        <div>
            <input name="title" value={post.title} onChange={onChange}></input>
            <input name="body" value={post.body} onChange={onChange}></input>
            <button type="button" onClick={() => mutate(post)}>
                글쓰기
            </button>
        </div>
    )
}

export default PostCreate
```

![](https://velog.velcdn.com/images/jhs000123/post/9feee162-f4e6-4de9-8d98-c6059b1441cf/image.gif)



### 2) 요청 후 처리

useQuery와 마찬가지로 요청의 성공, 실패 등의 여부에 따라서 특정한 코드를 실행하도록 설정할수 있다.

1. onMutate(mutate 시 실행)
```jsx
const { mutate, isLoading, error } = useMutation(createPost, {
    onMutate: (variables) => {
        console.log('요청 시 내가 명시한 값', variables)
        return {id: 1}
    }
})
```

2. onSuccess (성공하면...?)
```jsx
  onSuccess: (data, variables, context) => {
        console.log('요청 성공', data)
        console.log('요청 시 내가 명시한 값', variables)
        console.log('onmutate 에서 리턴한 값', context)
    }
```

3. onError (실패하면....?)
```jsx
  onError: (error, variables, context) => {
        console.log('요청 실패', error)
        console.log('요청 시 내가 명시한 값', variables)
        console.log('onmutate 에서 리턴한 값', context)
    }
```

4. onSettled (완료하면..?)

```jsx
 onSettled: (data, error, variables, context) => {
        console.log('요청 완료')
        console.log('요청 시 내가 명시한 값', variables)
        console.log('onmutate 에서 리턴한 값', context)
    }
```

### 업데이트(put,post) 및 자동Get(invalidateQueries)

- invalidateQueries
`invalidateQueries` 는 특정한 쿼리 키에 대해서 현재 받아온 데이터가 유효하지 않으니, 다시 해당 쿼리 키를 가진 useQuery 를 실행하라는 함수입니다.

아래와 같은 코드를 통해서 invalidateQueries 함수를 사용할 수 있다.

```jsx
const queryClient = useQueryClient();

queryClient.invalidateQueries("posts");
```
이 invalidateQueries 와 함께 onSuccess 를 활용하면, 글 작성 혹은 수정이 성공했을 때 자동으로 해당 글을 다시 받아오도록 코드를 작성할 수 있습니다.

post 요청이라면 아래와 같이 작성할 수 있습니다.
```jsx
// query client 를 가져옴
const queryClient = useQueryClient();

const { mutate } = useMutation(createPost, {
  onSuccess: () => {
    // createPost 가 성공하면 posts 라는 쿼리 키를 가진 useQuery 함수를 다시 실행합니다.
    queryClient.invalidateQueries("posts");
  }
});
```
자동으로 다시 요청을 보내기 위해서는 , posts 라는 쿼리 키를 가진 데이터가 표시되고 있어야 한다. (inactive 상태거나 아직 최초의 요청을 보내지도 않았다면 자동으로 요청을 보내지 않음)

(useQuery 와 useMutation 을 하나의 페이지에서 쓰도록 구성해보시면, 자동으로 요청을 보내는 것을 확인하실 수 있다.)


- setQueryData
글을 수정하는 요청이라면, useQuery 로 값 받아오고 → mutate 해서 수정한 값 put 하고 → setQueryData 로 저장된 캐시 데이터를 현재 수정된 값으로 변경하는 방식으로 진행이 된다.

```jsx
const { status, data, error } = useQuery(["posts", 글 번호], () => getPost(글 번호));

const queryClient = useQueryClient();

const { mutate } = useMutation(updatePost, {
  onSuccess: (data) => {
    // 요청 성공시 위 useQuery의 data가 자동으로 현재 data로 수정됨
    queryClient.setQueryData(["posts", 글 번호], data);
  }
});

mutate({title: '제목', body: '내용'});
```

## pagination

### 1) 구현하기

api를 사용해서 10개씩만 데이터를 보내주는 요청 코드를 작성한다.
```jsx
export const getPostsByPage = async (page) => {
    const response = await axios.get(`https://jsonplaceholder.typicode.com/posts?_limit=10&_page=${page}`)
    return response.data
}
```



```jsx
import axios from 'axios'
import React, { useState } from 'react'
import { useMutation, useQuery, useQueryClient } from 'react-query'
import { deletePost, getPosts, getPostsByPage } from '../apis/services/posts'

function Post() {
    const [pageNum, setPageNum] = useState(1)
    const queryClient = useQueryClient()
    const { isLoading, error, data: posts } = useQuery(['posts', { page: pageNum }], () => getPostsByPage(pageNum), {})
    const { mutate } = useMutation(deletePost, {
        onSuccess: () => {
            queryClient.invalidateQueries('posts')
        },
    })

    if (isLoading) return <div> Loading...</div>
    if (error) return <div>{error.message}</div>
    return (
        <div>
            {posts &&
                posts.map((post) => {
                    return (
                        <div key={post.id}>
                            <h3>{post.title}</h3>
                            <p>{post.body}</p>
                            <button onClick={() => mutate(post.id)}>삭제</button>
                        </div>
                    )
                })}
            <button onClick={() => setPageNum(pageNum - 1)}> 이전 페이지</button>
            <button onClick={() => setPageNum(pageNum - 1)}> 다음 페이지</button>
        </div>
    )
}

export default Post

```

### 2) useInfiniteQuery 활용하기

react query 는 데이터를 누적하면서 불러올 수 있도록 `useInfiniteQuery`라는 함수를 지원하고 있다.

무한 스크롤 구현을 위한 함수이지만, 지금은 간단하게 활용해보자.

```jsx
const { data, fetchNextPage } = useInfiniteQuery({
    queryKey: 'posts',
    queryFn: (pageParam 을 인자로 받는 axios 함수),
    getNextPageParam: (lastPage, pages) => {
        return (다음 pageParam)
    },
})
```

- pageParams의 역할

`useInfiniteQuery` 에 명시하는 쿼리 함수는 `pageParam` 이라는 인자를 무조건 받아야 합니다.

왼쪽의 `fetchNextPage` 라는 함수를 실행하면, 자동으로 위에 명시한 `getNextPageParam` 함수가 실행되면서 그 리턴값이 다시 쿼리 함수의 `pageParam` 으로 들어가게 된다.


- getNextParam 

즉, `getNextPageParam` 은 다음 페이지로 넘어가기 위한 파라미터를 리턴할 수 있도록 작성해줘야 한다

`getNextPageParam` 에서는 `lastPage (바로 이전 페이지의 데이터들)` 및 `pages (지금까지 받아온 모든 페이지의 데이터들)` 에 접근할 수 있다.

그러면 이전 페이지의 데이터들을 토대로 다음 페이지 번호가 무엇인지 찾고, 그 다음 페이지 번호를 리턴하는 방식으로 작성해주면 된다.

만약 false 를 리턴하면, 다음 페이지로 넘어가지 않는다. 


- 왼쪽 data에는 뭐가 들어감?
이전에는 데이터 그 자체가 들어갔지만, 지금은 pages 라는 키에 페이지 별 데이터가 들어가는 형태로 구성이 된다. 그래서 데이터에 접근하려면 data.pates를 map함수를 통해서 접근해줘야한다. 

그럼 한번 써보자


getPostsByPage를 아래와 같이 변경한다.
```jsx
export const getPostsByPage = async ({pageParam={start: 0, limit: 10}}) => {
    const response = await axios.get(`https://jsonplaceholder.typicode.com/posts?_start=${pageParam.start}&_limit=${pageParam.limit}`)
    return response.data
}
```

**초기값을 반드시 명시해야한다**

pageParam을 비구조할당을 통해서 받도록 작성한다.

```jsx
import axios from 'axios'
import React, { useState } from 'react'
import { useInfiniteQuery, useMutation, useQuery, useQueryClient } from 'react-query'
import { deletePost, getPosts, getPostsByPage } from '../apis/services/posts'

function Post() {
    const [pageNum, setPageNum] = useState(1)
    const queryClient = useQueryClient()
    /*  const { isLoading, error, data: posts } = useQuery(['posts', { page: pageNum }], () => getPostsByPage(pageNum), {}) */
    const {
        data: posts,
        fetchNextPage,
        isLoading,
    } = useInfiniteQuery({
        queryKey: 'posts',
        queryFn: getPostsByPage,
        getNextPageParam: (lastPage, pages) => {
            const lastPost = lastPage[lastPage.length - 1]
            return lastPost.id < 100 ? { start: lastPost.id, limit: 10 } : false
        },
    })
    const { mutate } = useMutation(deletePost, {
        onSuccess: () => {
            queryClient.invalidateQueries('posts')
        },
    })

    if (isLoading) return <div> Loading...</div>

    return (
        <div>
            {posts.pages &&
                posts.pages.map((posts) => {
                    return posts.map((post) => {
                        return (
                            <div key={post.id}>
                                <h3>{post.title}</h3>
                                <p>{post.body}</p>
                                <button onClick={() => mutate(post.id)}>삭제</button>
                            </div>
                        )
                    })
                })}
            {/*   <button onClick={() => setPageNum(pageNum - 1)}> 이전 페이지</button>
            <button onClick={() => setPageNum(pageNum - 1)}> 다음 페이지</button> */}
            <button onClick={() => fetchNextPage()}>더 가져와!!!</button>
        </div>
    )
}

export default Post

```

`더 가져와!!!` 버튼을 누르게 되면 `getchNextPage`가 호출되고, 그러면 `getNextPageParam`함수가 실행되면서 그에대한 리턴값이 다시 쿼리 함수의 `pageParam`으로 들어가게된다.


![](https://velog.velcdn.com/images/jhs000123/post/1ded4e45-0c92-4940-b4c2-82b3c845132e/image.gif)

