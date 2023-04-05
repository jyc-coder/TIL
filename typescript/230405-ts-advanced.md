## Reduc Toolkit 과 TypeScript 

### 1) 카운터 만들어보기 (slice 활용)

redux toolkit 을 설치 

> yarn add react-redux @reduxjs/tookit

logger도 설치할거면 redux logger도 설치해도 됨

> yarn add redux-logger
> yarn add @types/redux-logger --dev

사용방법이 크게 다른건 없다

state에 대한 타입을 지정하고, action에 대한 타입을 (payload 에 대한 타입과 함께) 지정하면 된다

`store/slice/counterSlice.ts`

```tsx
import { useSelector } from 'react-redux'
import { RootState } from '../../modules'
import { createSlice, PayloadAction } from '@reduxjs/toolkit'
import { useDispatch } from 'react-redux'

type CounterState = {
    count: number
}

type CounterPayload = {
    diff: number
}

const initialState: CounterState = {
    count: 0,
}

export const counterSlice = createSlice({
    name: 'counter',
    initialState,
    reducers: {
        increase: (state: CounterState) => {
            state.count = state.count + 1
        },
        decrease: (state: CounterState) => {
            state.count = state.count - 1
        },
        //PayloadAction 타입 명시
        increaseByDiff: (state: CounterState, action: PayloadAction<CounterPayload>) => {
            state.count = state.count + action.payload.diff
        },
        decreaseByDiff: (state: CounterState, action: PayloadAction<CounterPayload>) => {
            state.count = state.count - action.payload.diff
        },
    },
})

export const { increase, decrease, increaseByDiff, decreaseByDiff } = counterSlice.actions

// 커스텀 훅 형태로 만들어주기 (Hook 폴더로 따로 빼도된다)

export function useCounter() {
    const count = useSelector((state: RootState) => state.counter.count)
    const dispatch = useDispatch()

    return {
        count,
        dispatch,
    }
}

export default counterSlice.reducer

```


`store/index.ts`
```tsx
import { createLogger } from 'redux-logger'
import counterReducer from './slices/counterSlice'
import { combineReducers, configureStore } from '@reduxjs/toolkit'

const logger = createLogger()

const rootReducer = combineReducers({
    counter: counterReducer,
})

// RootState 타입 생성

export type RootState = ReturnType<typeof rootReducer>

export const store = configureStore({
    reducer: rootReducer,
    middleware: (getDefaultMiddeleware) => getDefaultMiddeleware().concat(logger),
})


```
`component/Counter.tsx`
```tsx
import { decrease, decreaseByDiff, increase, increaseByDiff, useCounter } from '../store/slices/counterSlice'

function Counter() {
    // const count = useSelector((state: RootState) => state.counter.count)
    // const dispatch = useDispatch()
    const { count, dispatch } = useCounter()

    const onIncreate = () => {
        dispatch(increase())
    }
    const onDecreate = () => {
        dispatch(decrease())
    }
    const onIncreaseByDiff = (diff: number) => {
        dispatch(increaseByDiff({ diff }))
    }
    const onDecreaseByDiff = (diff: number) => {
        dispatch(decreaseByDiff({ diff }))
    }

    return (
        <div>
            <p>{count}</p>
            <button onClick={onIncreate}>증가</button>
            <button onClick={onDecreate}>감소</button>
            <button onClick={() => onIncreaseByDiff(2)}>+ 2 </button>
            <button onClick={() => onDecreaseByDiff(2)}>- 2</button>
        </div>
    )
}

export default Counter
```

### 2) createAsyncThunk 활용하기

> yarn add axios

limit값을 인자로 받아서, 아래 주소로 요청을 보내고 응답 데이터를 컴포넌트에 표시하도록 구현해보자!

```
https://jsonplaceholder.typicode.com/posts?_limit=${limit}
```

타입스크립트로 작성 시에 기존과 다른 점은 요청 성공 시 리턴값의 타입, 요청 실패 시 리턴 값의 타입, 우리가 관리할 state 의 타입을 각각 만들어줘야한다.

store/slices/postSlice.ts

```tsx
import { createSlice, PayloadAction } from '@reduxjs/toolkit'
// Post타입은 따로 type폴더를 만들어서 작성해두는 것을 추천

import { createAsyncThunk } from '@reduxjs/toolkit'
import axios from 'axios'

type Post = {
    userId: number
    id: number
    title: string
    body: string
}

// State의 타입

type PostState = {
    entities: Post[]
    loading: boolean
}

// 요청 실패 시 에러 타입

type PostError = {
    message: string
}

const initialState: PostState = {
    entities: [],
    loading: false,
}

//createAsyncThunk<요청성공시 리턴값 타입, 함수에 필요한 인자와 타입, 요청 실패시 리턴값 타입>
// 다이아몬드 연산자의 3번째 값에는 요청 실패시의 타입 외에도 다양한 타입을 명시할수 있다

export const getPosts = createAsyncThunk<Post[], number, { rejectValue: PostError }>('posts/getPosts', async (limit: number, thunkApi) => {
    try {
        const response = await axios.get(`https://jsonplaceholder.typicode.com/posts?_limit=${limit}`)
        return response.data
    } catch (error: any) {
        return thunkApi.rejectWithValue(error)
    }
})

export const postSlice = createSlice({
    name: 'posts',
    initialState,
    reducers: {},
    extraReducers: {
        [getPosts.pending.type]: (state) => {
            state.loading = true
        },
        [getPosts.fulfilled.type]: (state, action: PayloadAction<Post[]>) => {
            state.loading = false
            state.entities = action.payload
        },
        [getPosts.rejected.type]: (state) => {
            state.loading = false
        },
    },
})

export default postSlice.reducer

```

`store/slices/postSlice`
```tsx
import { createLogger } from 'redux-logger'
import counterReducer from './slices/counterSlice'
import postReducer from './slices/postSlice'
import { combineReducers, configureStore } from '@reduxjs/toolkit'
import { TypedUseSelectorHook, useDispatch } from 'react-redux'
import { useSelector } from 'react-redux'

const logger = createLogger()

const rootReducer = combineReducers({
    counter: counterReducer,
    posts: postReducer,
})

// RootState 타입 생성

export type RootState = ReturnType<typeof rootReducer>

// thunk 사용 시에는 dispatch 타입을 명시해줘야함 (명시하지 않으면 thunk 함수를 받지 못함)
export type AppDispatch = typeof store.dispatch

export const store = configureStore({
    reducer: rootReducer,
    middleware: (getDefaultMiddeleware) => getDefaultMiddeleware().concat(logger),
})

// 미리 useDispatch, useSelector 에 대해서 타입을 지정해놓고 사용하기 위해 작성

export const useAppDispatch: () => AppDispatch = useDispatch
export const useAppSelector : TypedUseSelectorHook<RootState> = useSelector
```

`components/PostList.tsx`
```tsx
import { useEffect } from 'react'
import { useAppDispatch, useAppSelector } from '../store'
import { getPosts } from '../store/slices/postSlice'

function PostList() {
    const dispatch = useAppDispatch()
    const { entities: posts, loading } = useAppSelector((state) => state.posts)

   useEffect(() => {
        dispatch(getPosts(10))
    }, [])

    if (loading) return <p>Loading...</p>

    return (
        <div>
            {posts.map((post) => (
                <p key={post.id}>{post.title}</p>
            ))}
        </div>
    )
}

export default PostList

```

`app.tsx`
```tsx
import { Provider } from 'react-redux'
import Counter from './components/Counter'

import { CounterProvider } from './contexts/CounterContext'
import { store } from './store'
import PostList from './components/PostList'

function App() {
    return (
        <Provider store={store}>
            <Counter />
            <PostList />
        </Provider>
    )
}

export default App

```

![](https://velog.velcdn.com/images/jhs000123/post/f7241cad-afb8-4e03-9112-0e8cc0d1659a/image.png)

### 3) RTK 활용하기

이번에는 방금 구현한 것과 동일한 요청을 RTK QUERY를 사용해서 진행해보자

builder.query 부분에 리턴하는 데이터 타입을 명시하는 것만 추가해주면 된다.

`api/postApi.ts`

```tsx
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react'

type Post = {
    userId: number
    id: number
    title: string
    body: string
}

export const postApi = createApi({
    reducerPath: 'postApi',
    baseQuery: fetchBaseQuery({
        baseUrl: 'https://jsonplaceholder.typicode.com/',
    }),
    endpoints: (builder) => ({
        getPosts: builder.query<Post[], number>({
            query: (limit) => `posts?_limit=${limit}`,
        }),
    }),
})

export const { useGetPostsQuery } = postApi


```

`store/index.ts`

```tsx
import { createLogger } from 'redux-logger'
import { configureStore } from '@reduxjs/toolkit'
import { postApi } from '../api/postApi'

const logger = createLogger()

// thunk 사용 시에는 dispatch 타입을 명시해줘야함 (명시하지 않으면 thunk 함수를 받지 못함)
export type AppDispatch = typeof store.dispatch

export const store = configureStore({
    reducer: {
        [postApi.reducerPath]: postApi.reducer,
    },
    middleware: (getDefaultMiddeleware) => getDefaultMiddeleware().concat(logger, postApi.middleware),
})

```
`components/PostList.tsx`
```tsx

import { useGetPostsQuery } from '../api/postApi'

function PostList() {
    const { data: posts, isLoading, isError } = useGetPostsQuery(10)

    if (isLoading) return <p>Loading...</p>

    if (isError || !posts) return <p>Error</p>
    return (
        <div>
            {posts.map((post) => (
                <p key={post.id}>{post.title}</p>
            ))}
        </div>
    )
}

export default PostList

```


## React Query 와 Typescript 

### 1) useQuery 사용하기

react-query 설치

> yarn add react-query axios


useQuery는 제네릭 형태로 총 4가지 타입을 명시할수 있다.

```
useQuery<리턴데이터의 타입, 에러 타입, select 사용 시 select 한 값의 타입, 쿼리 키의 타입>()
```

우선 가장 기본적으로 리턴 데이터의 타입과 에러타입만 명시해서 사용해보자.

`api/services/Post.ts`

```tsx
import axios from 'axios'

export const getPosts = async () => {
  const response = await axios.get('https://jsonplaceholder.typicode.com/posts')
  return response.data
}

```


`types/Post.ts`
```tsx
export type Post = {
    id: number
    userId: number
    title: string
    body: string
}

```

`components/PostList.tsx` (useQuery사용시 제네릭 형태로 타입을 명시, 렌더링시 posts 가 null일 가능성도 있으므로 옵셔널 체이닝을 작성한다.

```tsx
import { AxiosError } from 'axios'
import { useQuery } from 'react-query'
import { getPosts } from '../api/services/Post'
import { Post } from '../types/Post'

function PostList() {
    const { isLoading, data: posts, error } = useQuery<Post[], AxiosError>('posts', getPosts)

    if (isLoading) return <div>Loading...</div>
    if (error) return <div>{error.message}</div>
    return (
        <div>
            {posts?.map((post) => (
                <div key={post.id}>
                    <h1>{post.title}</h1>
                    <p>{post.body}</p>
                </div>
            ))}
        </div>
    )
}

export default PostList
```

`app.tsx`

```tsx
import PostList from './components/PostList'
import {QueryClient, QueryClientProvider} from 'react-query'

const queryClient = new QueryClient()

function App() {

  return (
    <QueryClientProvider client={queryClient}>
      <PostList/>
    </QueryClientProvider>
  )
}

export default App
```

만약 select도 사용하고, 쿼리 키에 대한 타입도 명시하고 싶으면?
```tsx
const { isLoading, data: posts, error } = useQuery<Post[], AxiosError, number, [string]>(['posts'], getPosts, { select: posts => posts.length })
```

이렇게 작성하면 된다.


### 2) useMutation 사용하기

`useMutation`의 경우에도 제네릭 형태로 4가지 타입을 명시할수 있다.
```tsx
useMutation<응답의 타입, 에러 타입, 인자 타입, onMutate 에서 리턴하는 값 (context) 의 타입>
```
`api/service/Post.ts`

```jsx
import axios from 'axios'
import { Post } from '../../types/Post'

export const getPosts = async () => {
    const response = await axios.get('https://jsonplaceholder.typicode.com/posts')
    return response.data
}

export const createPost = async (post: Post) => {
    const response = await axios.post(`https://jsonplaceholder.typicode.com/posts`, post)
    return response.data
}
```

`types/Post.ts`
```tsx
export type Post = {
    id?: number
    userId?: number
    title: string
    body: string
}
// 지금 서버는 응답이 그냥 Post 와 동일하지만, 혹시나 다르다면 정의해주어야 함
export type PostResponse = {
    id: number,
    userId?: number,
    body: string,
    title: string
}
```
`components/PostForm.tsx`
```tsx
import { AxiosError } from "axios";
import React, { useState } from "react";
import { useMutation } from "react-query";
import { createPost } from "../api/services/Post";
import { Post, PostResponse } from "../types/Post";

function PostForm() {
    const { mutate, isLoading, error } = useMutation<PostResponse, AxiosError, Post>(createPost)

    const [post, setPost] = useState({ title: '', body: '' })
    const onChange = (e: React.ChangeEvent<HTMLInputElement>) => {
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

export default PostForm;
```

![](https://velog.velcdn.com/images/jhs000123/post/5fe9d441-2aa5-421a-9f66-a967d2fd8c29/image.png)

