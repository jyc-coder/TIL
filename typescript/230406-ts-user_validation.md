이전에 사용했던 서버 파일을 사용해서 유저인증을 구현한다.

## Typescript 로 유저 인증 구현



### 1) 백엔드 API

https://github.com/jyc-coder/typescriptServer
해당 리포지토리에서 server를 참고할 것 


백엔드의 db는 아래와 같다
![](https://velog.velcdn.com/images/jhs000123/post/f02d44df-1f2e-4acd-9ab3-e02bb498a7ae/image.png)

API 명세서는 다음과 같다.
https://www.notion.so/6d89c07b18794207835de1a6cebfa591?v=f551d89a29f14debb9966992478dbb8c


### 2) api 설정하기 (react-query)(apis, interfaces,utils)

react-query와 react-cookie 설치

> yarn add react-query react-cookie


- 쿠기 util 함수 작성

쿠키의 설정, 제거 등을 위해서 `react-cookie`모듈을 활용한다.

`utils/cookies.ts`

```tsx
import { Cookies } from 'react-cookie'
import { Cookie, CookieSetOptions } from 'universal-cookie'

const cookies = new Cookies()

export const getCookie = (name: string) => {
    try {
        return cookies.get(name)
    } catch (error) {
        console.error(error)
    }
}

export const setCookie = (name: string, value: Cookies, option?: CookieSetOptions) => {
    try {
        cookies.set(name, value, { ...option })
    } catch (error) {
        console.error(error)
    }
}

export const removeCookie = (name: string, option?: CookieSetOptions) => {
    try {
        cookies.remove(name, { ...option })
    } catch (error) {
        console.error(error)
    }
}

```
이제 컴포넌트에서 해당 함수들을 가져와서 사용할 것이다.

- axios 설정하기

우선 우리 서버 URL을 환경 변수로 작성할 것이다. 

`.env`
```
VITE_SERVER_URL = 'http://localhost:3000/'
```

`apis/axios.ts`
```tsx
import axios, { AxiosError, AxiosRequestConfig } from 'axios'
import { getCookie } from '../utils/cookies'

const getAxiosInstance = () => {
    const config: AxiosRequestConfig = {
        baseURL: import.meta.env.VITE_SERVER_URL,
        headers: {
            'Content-Type': 'application/json',
        },
        withCredentials: true,
    }

    const instance = axios.create(config) // instance: AxiosInstance로 타입 추론
    instance.defaults.timeout = 3000
    instance.interceptors.request.use(
        (request) => {
            const token = getCookie('accessToken')
            if (token) request.headers['Authorization'] = `Bearer ${token}`
            return request
        },
        (error: AxiosError) => {
            console.log(error)
            return Promise.reject(error)
        },
    )
    return instance
}

export const axiosInstance = getAxiosInstance()

```

로그인, 회원가입이 구현된 상태라면, 요청을 보낼 때 토큰값도 함께 보내주어야 한다. (그렇게 해야만 백엔드에서 인증 여부를 확인할수 있다.)

그렇기 때문에 instance를 만들면서, 인터셉터 형태로 요청을 보낼 때마다 쿠키에 저장된 토큰을 가져와서 보내도록 작성해준다. (또한 withCredentials: true로 설정해서 백엔드에서 쿠키를 설정할수 있도록 해준다.)
앞으로는 위에서 작성한 `axiosInstance`를 활용해서 요청을 보낼 예정이다. 

- 요청 별 데이터 타입 및 쿼리 함수 작성하기 
요청에 필요한 데이터는 무엇인지, 제대로 응답이 왔다면 어떤 형태의 데이터가 오는지에 대해서 미리 타입을 만들어놓을 것이다. 

보통은 요청별 Request, Response 마다 타입을 지정해놓게 되는데, 지금같은 경우에는 REsponse 데이터롸 프론트에서 사용할 Post 자체의 데이터 타입이 일치하기 때문에 아래처럼 만들어준다. 

(혹시나 프론트엔드에서 사용하는 데이터와, 리스폰스로 오는 데이터가 다르다면 Response타입을 따로 만들어주면된다. )

`interfaces/post.ts`
[O
```tsx
export interface PostRequest {
    title: string
    body: string
}

export interface Post extends PostRequest {
    id: number
    userId: number
}

```

`interfaces/auth.ts`

```tsx
export interface LoginRequest {
    username: string
    password: string
}

export interface RegisterRequest extends LoginRequest {
    email: string
}

export interface UserPayload {
    id: number
    username: string
    email: string
}

export interface AuthResponse extends UserPayload{
  iat: number
  exp: number
  accessToken : string
}

//프론트에서 따로 관리하고자 하는 유저 데이터 타입이 있다면, 타입을 추가해주시면 됩니다.
```

`react-query`에서 사용할 쿼리 함수를 만들텐데, 이번에는 정확히 어떤 응답 데이터가 오는지를 쿼리 함수에서 지정한다. 

이렇게 미리 응답 데이터의 타입을 지정해놓으면, 타입 추론이 잘 작동하기 때문에 굳이 useQuery를 사용할 때 타입을 지정하지 않아도 된다.

`apis/services/post.ts`

```tsx
import { Post, PostRequest } from '../../interfaces/post'
import { axiosInstace } from '../axios'

export const getPosts = async () => {
  try {
    const { data } = await axiosInstace.get<Post[]>('/api/posts')
    return data
  } catch (error) {
    console.log(error)
  }
}

export const createPost = async (post: PostRequest) => {
  try {
    const { data } = await axiosInstace.post<Post>('/api/posts', post)
    return data
  } catch (error) {
    console.log(error)
  }
}
```

`api/services/auth.ts`
```tsx
import { AuthResponse, LoginRequest, RegisterRequest, UserPayload } from '../../interfaces/auth'
import { axiosInstance } from '../axios'

export const login = async (user: LoginRequest) => {
    try {
        const { data } = await axiosInstance.post<AuthResponse>('/api/auth/login', user)
        return data
    } catch (error) {
        console.log(error)
    }
}

export const register = async (user: RegisterRequest) => {
    try {
        const { data } = await axiosInstance.post<AuthResponse>('/api/auth/register', user)
        return data
    } catch (error) {
        console.log(error)
    }
}


export const verify = async () => {
  try {
    const { data } = await axiosInstance.get<UserPayload>('/api/auth/verify')
    return data
  } catch (error) {
    console.log(error)
  }
}
```

이렇게 해주고 app.tsx에 react query 세팅을 해준다.

`app.tsx`
```tsx
import './App.css'
import { QueryClientProvider, QueryClient } from 'react-query'

function App() {
    const queryClient = new QueryClient({
        defaultOptions: {
            queries: {
                refetchOnWindowFocus: false,
            },
        },
    })
    return <QueryClientProvider client={queryClient}></QueryClientProvider>
}

export default App

```

이제 컴포넌트를 만들어보자.

### 3)compoennt 생성하기
작은 단위로 컴포넌트를 생성하고, 해당 컴포넌트를 이용해서 페이지를 구성해보자.

- login -> loginform 컴포넌트
- posts -> postform, postlist 컴포넌트

- login 컴포넌트
`login/LoginForm/index.jsx`

```tsx
import React, { useState } from 'react'
import { UseMutateFunction } from 'react-query'
import { AxiosError } from 'axios'
import { LoginRequest, AuthResponse } from '../../../interfaces/auth'
interface LoginFormProps {
    mutate: UseMutateFunction<AuthResponse, AxiosError, LoginRequest>
}
function LoginForm({ mutate }: LoginFormProps) {
    const [userInput, setUserInput] = useState<LoginRequest>({ username: '', password: '' })
    const onChange = (e: React.ChangeEvent<HTMLInputElement>) => {
        const { name, value } = e.target
        setUserInput({ ...userInput, [name]: value })
    }
    const onSubmit = (e: React.FormEvent<HTMLFormElement>) => {
        e.preventDefault()
        mutate(userInput)
    }
    return (
        <form onSubmit={onSubmit}>
            <input type="text" name="username" value={userInput.username} onChange={onChange} />
            <input type="password" name="password" value={userInput.password} onChange={onChange} />
            <button type="submit">로그인</button>
        </form>
    )
}

export default LoginForm

```

- post 컴포넌트
`posts/PostList/index.js`
```tsx
import React from 'react'
import { Post } from '../../../interfaces/post'

interface PostListProps {
    posts: Post[]
}

function PostList({ posts }: PostListProps) {
    return (
        <>
            {posts.map((post) => (
                <div key={post.id}>
                    <h2>{post.title}</h2>
                    <p>{post.body}</p>
                    <p>작성자: {post.userId}</p>
                </div>
            ))}
        </>
    )
}

export default PostList

```

`post/PostForm/index.tsx`

```tsx
import React, { useState } from 'react'
import { Post, PostRequest } from '../../../interfaces/post'
import { AxiosError } from 'axios'
import { UseMutateFunction } from 'react-query'

interface PostFormProps {
    mutate: UseMutateFunction<Post, AxiosError, PostRequest>
}

function PostForm({ mutate }: PostFormProps) {
    const [userInput, setUserInput] = useState<PostRequest>({ title: '', body: '' })
    const onChange = (e: React.ChangeEvent<HTMLInputElement>) => {
        const { name, value } = e.target
        setUserInput({ ...userInput, [name]: value })
    }
    const onSubmit = (e: React.FormEvent<HTMLFormElement>) => {
        e.preventDefault()
        mutate(userInput)
    }

    return (
        <form onSubmit={onSubmit}>
            <input type="text" name="title" value={userInput.title} onChange={onChange} />
            <input type="text" name="body" value={userInput.body} onChange={onChange} />
            <button type="submit">작성하기</button>
        </form>
    )
}

export default PostForm

```


### 4) page 설정하기 (pages, routes)


> yarn add react-router-dom
yarn add @types/react-router-dom

page 와 route는 아래와 같이 구성한다

- /login -> 로그인 페이지
- /posts -> 글 리스트
- /posts/new -> 글 작성

`pages/LoginPage.tsx`
```tsx
import React from 'react'
import LoginForm from '../components/LoginForm'
import { useMutation } from 'react-query'
import { login } from '../apis/services/auth'
import { AxiosError } from 'axios'
import { setCookie } from '../utils/cookies'
import { useNavigate } from 'react-router-dom'

function LoginPage() {
  const navigate = useNavigate()
  const { mutate } = useMutation(login, {
    onSuccess: (data) => {
      setCookie('accessToken', data.accessToken, { path: '/', maxAge: data.exp - data.iat })
      navigate('/posts') // 로그인 성공 시 이동
    },
    onError: (err: AxiosError) => {
      console.log(err)
    },
  }) // 에러 타입 추론할 수 있도록 설정

  return <LoginForm mutate={mutate} />
}

export default LoginPage
```

`pages/PostsPage.tsx`

```tsx
import React from 'react'
import { useQuery } from 'react-query'
import { getPosts } from '../apis/services/post'
import PostList from '../components/PostList'

function PostsPage() {
  const { data: posts, isLoading, error } = useQuery('posts', getPosts)

  if (isLoading) return <>로딩 중</>
  if (error || !posts) return <>에러 발생</>
  return <PostList posts={posts} />
}

export default PostsPage
```

`pages/PostCreatePage.tsx`

```tsx
import React from 'react'
import PostForm from '../components/PostForm'
import { useMutation } from 'react-query'
import { createPost } from '../apis/services/post'
import { AxiosError } from 'axios'
import { useNavigate } from 'react-router-dom'

function PostCreatePage() {
  const navigate = useNavigate()
  const { mutate } = useMutation(createPost, {
    onSuccess: () => {
      navigate('/posts') // 글 작성 성공 시 이동
    },
    onError: (err: AxiosError) => {
      console.log(err)
    },
  })

  return <PostForm mutate={mutate} />
}

export default PostCreatePage
```

`routes/Router.tsx`
```tsx
import React from 'react'
import { BrowserRouter, Route, Routes } from 'react-router-dom'
import LoginPage from '../pages/LoginPage'
import PostCreatePage from '../pages/PostCreatePage'
import PostsPage from '../pages/PostsPage'

function Router() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/login" element={<LoginPage />} />
        <Route path="/posts" element={<PostsPage />} />
        <Route path="/posts/new" element={<PostCreatePage />} />
      </Routes>
    </BrowserRouter>
  )
}

export default Router
```

현재는 모든 route 가 로그인을 안해도 접근이 가능하지만, 이제 로그인 여부를 검증하는 로직을 추가하려고 한다. 
`app.tsx`
```tsx
import { QueryClient, QueryClientProvider } from 'react-query'
import Router from './routes/Router'

const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      refetchOnWindowFocus: false,
    },
  },
})

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <Router />
    </QueryClientProvider>
  )
}

export default App
```


### 5) token 검증 관련 추가 설정(hooks)

`토큰 검증`은 매우 중요하다. 단순히 토큰이 있다고 해서 특정한 페이지에 접근이 가능하도록 한다면, 보안상의 문제가 생길 수 있다.
그렇기 때문에 현재 유저의 토큰이 정말 유효한 토큰인지(우리가 발급해준 토큰이 맞는지, 유효기간이 지난건 아닌지) 등을 점검하고, 혹시나 유효하지 않다면 접근하지 못하도록 하는 로직이 필요하다.

토큰 검증을 위한 훅(verifyToken)을 만들어서 사용해보자.

`hooks/verifyToken.tsx`

```tsx
import React, { useState } from 'react'
import { useQuery } from 'react-query'
import { verify } from '../apis/services/auth'
import { getCookie } from '../utils/cookies'

type authType = 'PENDING' | 'SUCCESS' | 'FAILED'

function verifyToken() {
  const [isAuthenticated, setIsAuthenticated] = useState<authType>('PENDING')
  const verifyResult = useQuery(['auth', 'verify'], verify, {
    onSuccess: () => {
      setIsAuthenticated('SUCCESS')
    },
    onError: () => {
      setIsAuthenticated('FAILED')
    },
    retry: 0, // 실패했더라도 다시 요청하지 않음
    enabled: !!getCookie('accessToken'), // 토큰이 있을 때만 verify
  }) // 쿠키가 없으면 undefined 라서, boolean 타입으로 변환하기 위해 !! 사용

  return isAuthenticated // 인증 여부 값을 리턴해서 사용
}

export default verifyToken
```

이제 가져온 `isAuthenticated`값을 가지고, 페이지 접근 가능 여부를 판단하는 로직을 `ProtectedRouter`에 작성한다.

`router/ProtectedRouter.tsx`
```tsx

import React, { useEffect } from 'react'
import verifyToken from '../hooks/verifyToken'
import { getCookie } from '../utils/cookies'
import { Outlet, useNavigate } from 'react-router-dom'

export default function ProtectedRouter() {
    const isAuthenticated = verifyToken()
    const token = getCookie('accessToken')
    const navigate = useNavigate()

    useEffect(() => {
        if (!token || isAuthenticated === 'FAILED') {
            //토큰이 없거나, 인증 실패 시 로그인 페이지로
            alert('로그인이 필요합니다')
            navigate('/login')
        }
    }, [isAuthenticated])

    return <Outlet />
}

```

이제 이 라우터로 감싸는 페이지는 항상 토큰 및 isAuthenticated를 토대로 유효한 토큰이 있는 경우에만 접근할수 있다.
`routes/Router.tsx`
```tsx
import { BrowserRouter, Route, Routes } from 'react-router-dom'
import LoginPage from '../pages/LoginPage'
import PostCreatePage from '../pages/PostCreatePage'
import PostsPage from '../pages/PostsPage'
import ProtectedRouter from './ProtectedRouter'

function Router() {
    return (
        <BrowserRouter>
            <Routes>
                <Route path="/login" element={<LoginPage />} />
                <Route path="/posts" element={<PostsPage />} />
                <Route element={<ProtectedRouter />}>
                    <Route path="/post/new" element={<PostCreatePage />} />
                </Route>
            </Routes>
        </BrowserRouter>
    )
}

export default Router

```

이제 PostCreate는 로그인을 해서 유효한 토큰이 쿠키에 있는 경우에만 접근 가능하다.


### 6) token refresh 설정

만약 access token 이 만료되어서 없고, refresh token 만 쿠키에 존재하는 상황이라면?
그때는 자동으로 refresh 요청을 보내고 access token을 재발급 받도록 해줘야한다. 


우선 refresh 쿼리 함수를 작성한다.

`apis/services/auth.ts`

```tsx
import { useState } from 'react'
import { useQuery } from 'react-query'
import { refresh, verify } from '../apis/services/auth'
import { getCookie, setCookie } from '../utils/cookies'

type authType = 'PENDING' | 'SUCCESS' | 'FAILED'

function verifyToken() {
    const [isAuthenticated, setIsAuthenticated] = useState<authType>('PENDING')
    const verifyResult = useQuery(['auth', 'verify'], verify, {
        onSuccess: () => {
            setIsAuthenticated('SUCCESS')
        },
        onError: () => {
            setIsAuthenticated('FAILED')
        },
        retry: 0,
        enabled: !!getCookie('accessToken'), // 토큰이 있을 때만 verify
    }) // 쿠키가 없으면 undefined라서, boolean 타입으로 변환하기 위해서 !!를 사용한다.

    // 액세스 토큰이 없을 때만
    // 가능하다면, refresh token의 존재 여부를 쿠키 혹은 redux 상태로 관리하면서 함께 써주는 것이 좋다.
    const refreshResult = useQuery(['auth', 'refresth'], refresh, {
        onSuccess: (data) => {
            setCookie('accessToken', data.accessToken, { path: '/', maxAge: data.exp - data.iat })
            setIsAuthenticated('SUCCESS')
        },
        onError: () => {
            setIsAuthenticated('FAILED')
        },
        retry: 0,
        enabled: !getCookie('accessToken'),
    })
    return isAuthenticated
}

export default verifyToken

```

`ProtectedRouter`쪽에서, 토큰이 없으면 바로 로그인을 넘기던 로직은 제외한다.

`routes/ProtectedRouter.tsx`
```tsx
import React, { useEffect } from 'react'
import verifyToken from '../hooks/verifyToken'
import { Outlet, useNavigate } from 'react-router-dom'

function ProtectedRouter() {
  const isAuthenticated = verifyToken()
  const navigate = useNavigate()

  useEffect(() => {
    if (isAuthenticated === 'FAILED') {
      // 이제 토큰이 있든 없든 isAuthenticated 는 값이 set 되므로 token 은 조건문에서 제외
      // 인증 실패 시 로그인 페이지로
      alert('로그인 해주세요')
      navigate('/login')
    }
  }, [isAuthenticated])

  return <Outlet />
}

export default ProtectedRouter
```

이제 다 됐다고 생각했겠지만 아직 문제가 있다. 
라우터를 아래처럼 수정해보자.

```tsx
import React from 'react'
import { BrowserRouter, Route, Routes } from 'react-router-dom'
import LoginPage from '../pages/LoginPage'
import PostCreatePage from '../pages/PostCreatePage'
import PostsPage from '../pages/PostsPage'
import ProtectedRouter from './ProtectedRouter'
import { Link } from 'react-router-dom'

function Router() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/posts">포스트</Link> | <Link to="/posts/new">새 글</Link>
      </nav>
      <Routes>
        <Route path="/login" element={<LoginPage />} />
        <Route element={<ProtectedRouter />}>
          <Route path="/posts" element={<PostsPage />} />
          <Route path="/posts/new" element={<PostCreatePage />} />
        </Route>
      </Routes>
    </BrowserRouter>
  )
}

export default Router
```

1. 지금은 react query 쪽에서 accessToken의 존재 여부만을 바라보고 있어서, accessToken 이 수정되면 그 수정된 값으로 다시 요청을 보내지 않는다.

2. 토큰이 없는데도, verifyToken 로직이 작동하기 전까지 페이지가 잠깐 보인다.

3. 인증이 실패했는데도, 이전에 받아온 서버 데이터 캐시가 남아있어서 해당 데이터가 잠깐 보인다.


1번 문제는 verify 쿼리의 쿼리 키를 변경하는 것으로 해결이 가능하다, 엑세스 토큰 값에 따라서 결과가 얼마든지 달라질수 있는 쿼리이기 때문에, 당연히 쿼리 키에 엑세스 토큰 값도 포함시켜줘야 하는 것이다. 
`verifyToken`
```tsx
import React, { useState } from 'react'
import { useQuery } from 'react-query'
import { refresh, verify } from '../apis/services/auth'
import { getCookie, setCookie } from '../utils/cookies'

type authType = 'PENDING' | 'SUCCESS' | 'FAILED'

function verifyToken() {
  const [isAuthenticated, setIsAuthenticated] = useState<authType>('PENDING')
  const token = getCookie('accessToken')
  // 엑세스 토큰이 있을 때만
  const verifyResult = useQuery(['auth', 'verify', token], verify, {
    onSuccess: () => {
      setIsAuthenticated('SUCCESS')
    },
    onError: () => {
      setIsAuthenticated('FAILED')
    },
    retry: 0, // 실패했더라도 다시 요청하지 않음
    enabled: !!token, // 엑세스 토큰이 있을 때만 verify
  })
  // 엑세스 토큰이 없을 때만
  // 가능하다면, refresh token 의 존재 여부를 쿠키 혹은 redux 상태로 관리하면서 함께 써주는게 좋음
  const refreshResult = useQuery(['auth', 'refresh'], refresh, {
    onSuccess: (data) => {
      setCookie('accessToken', data.accessToken, { path: '/', maxAge: data.exp - data.iat })
      setIsAuthenticated('SUCCESS')
    },
    onError: () => {
      setIsAuthenticated('FAILED')
    },
    retry: 0, // 실패했더라도 다시 요청하지 않음
    enabled: !token, // 엑세스 토큰이 없을 때만 refresh
  })

  return isAuthenticated // 인증 여부 값을 리턴해서 사용
}

export default verifyToken
```

2번 문제는 router 에서 조건부 렌더링을 해주는 것으로 해결할수 있다.

엑세스 토큰이 없으면 무조건 컴포넌트를 숨기되, 혹시나 토큰이 리프레쉬 될수 있으므로 인증이 되는 순간 다시 해당 페이지를 보여주면 된다.

토큰이 있다면 일단 컴포넌트를 보여주지만, 유효하지 않은 토큰일 경우 내부컴포넌트에서 데이터 요청을 보내더라도 백엔드에서 데이터를 응답해주지 않을 것이기에 민감한 데이터가 표시될 일은 없다.

(다만 만약 백엔드에서 요청을 거부하지 않고 데이터를 내려준다면 잠깐 표시될수 있다.)
`ProtectedRouter`
```tsx
import React, { useEffect } from 'react'
import verifyToken from '../hooks/verifyToken'
import { getCookie } from '../utils/cookies'
import { Outlet, useNavigate } from 'react-router-dom'

export default function ProtectedRouter() {
    const navigate = useNavigate()
    const token = getCookie('accessToken')
    const isAuthenticated = verifyToken()

    useEffect(() => {
        if (isAuthenticated === 'FAILED') {
            //토큰이 없거나, 인증 실패 시 로그인 페이지로
            alert('로그인이 필요합니다')
            navigate('/login')
        }
    }, [isAuthenticated])

    // 토큰이 없다면 verifyToken 이 작동할 때까지 잠시 컴포넌트를 숨김
    // 토큰이 있으면 verifyToken 작동 전이라도 일단 컴포넌트를 보여주되
    // 만약 그게 잘못된 녀석이라면, 백엔드에 요청을 못보내기에 컨텐츠를 볼수는 없다.
    if (!token && isAuthenticated !== 'SUCCESS') return <>로딩 중</>
    return <Outlet />
}

```

마지막으로 3번은 queryClient의 캐시를 삭제하는 것으로 해결할수 있다.

```tsx
import React, { useEffect } from 'react'
import verifyToken from '../hooks/verifyToken'
import { getCookie } from '../utils/cookies'
import { Outlet, useNavigate } from 'react-router-dom'
import { useQueryClient } from 'react-query'

export default function ProtectedRouter() {
    const queryClient = useQueryClient()
    const navigate = useNavigate()
    const token = getCookie('accessToken')
    const isAuthenticated = verifyToken()

    useEffect(() => {
        if (isAuthenticated === 'FAILED') {
            queryClient.clear() // 캐시 삭제
            // 유효하지 않은 access token 이라면 removeCookie 로 삭제 필요
            // 다만 refresh token (httponly cookie) 삭제는 서버쪽에서 해야한다.

            // 이제 토큰이 있든 없든 isAuthenticated 는 값이 set 되므로 token 은 조건문에 제외
            alert('로그인이 필요합니다')
            navigate('/login')
        }
    }, [isAuthenticated])

    // 토큰이 없다면 verifyToken 이 작동할 때까지 잠시 컴포넌트를 숨김
    // 토큰이 있으면 verifyToken 작동 전이라도 일단 컴포넌트를 보여주되
    // 만약 그게 잘못된 녀석이라면, 백엔드에 요청을 못보내기에 컨텐츠를 볼수는 없다.
    if (!token && isAuthenticated !== 'SUCCESS') return <>로딩 중</>
    return <Outlet />
}

```



