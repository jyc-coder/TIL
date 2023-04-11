이전시간에 만든 express + TypeORM + S3 로 구성한 백엔드를 react에서 글 작성 요청을 보내보자.


### 1) next 실행

> yarn create next-app (앱이름) --ts


### 2) 타입 설정

앞서 만들었던 모델 entity와 동일하게 Post 데이터에 대한 타입을 만들어준다.

`src/types/Post.ts`
```ts
export type Post = {
    id: number,
    title: string,
    body: string,
    img: string,
    createdAt: Date,
    updatedAt: Date
}
```

### 3) axios 설정

요청을 보내기 위해서 axios 를 설치하고, 관련 설정을 진행한다.

> yarn add axios

환경변수 형태로 백엔드 서버의 주소 명시

`.env`
```
NEXT_PUBLIC_SERVER_URL=http://localhost:3001
```

해당 서버로, form-data 형태로 요청을 보내도록 axios 인스턴스를 만들어준다.

`src/apis/instance.ts`
```ts
import axios from "axios";

export const instance = axios.create({
  baseURL: process.env.NEXT_PUBLIC_SERVER_URL,
  headers: {
    "Content-Type" : "multipart/form-data" // 파일을 보내기 위해서 이렇게 작성한다.
  }
})
```

next에서 이미지를 표시하고자 할때는 아래와 같이 config에 허용하고자 하는 도메인을 명시해줘야한다.

`next.config.js`

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: '**', // s3 서버 주소로 설정
        port: '',
        pathname: '**',
      },
    ],
  },
}

module.exports = nextConfig
```


### 4) pages 작성

페이지 컴포넌트 작성
- 글 리스팅 담당 POST
- 특정 글의 세부내용을 보여주는 PostDetail


`pages/posts/index.tsx`

```ts
import { instance } from '@/apis/instance'
import { Post } from '@/types/Post'
import Image from 'next/image'
import Link from 'next/link'
import React from 'react'

export const getServerSideProps = async () => {
  const response = await instance.get('/posts')
  const posts = response.data
  return {props: {posts}}
}

interface PostProps {
  posts: Post[]
}

function Post({posts}: PostProps) {
  return (
    <div>
      <Link href='/'>홈으로 가기</Link>
      {posts.map((post) => (
        <div key={post.id}>
          <h1>{post.title}</h1>
          <p>{post.body}</p>
          <div style={{ position: 'relative', height: '200px', width: '200px' }}>
            <Image src={post.img} alt={post.title} fill/>
          </div>
        </div>
      ))}
    </div>
  )
}

export default Post
```

`pages/posts/[id].tsx`
```tsx
import { instance } from '@/apis/instance'
import { GetServerSideProps } from 'next'
import React from 'react'
import Post from '.'
import Link from 'next/link'
import Image from 'next/image'

export const getServerSideProps: GetServerSideProps = async ({ params }) => {
  const response = await instance.get(`/posts/${params?.id}`)
  const post = response.data
  return {props : {post}}
}

interface PostDetailProps {
  post: Post
}

const PostDetail = ({ post }: PostDetailProps) => {
  return (
    <div>
      <Link href="/">
        홈으로 가기
      </Link>
     
      <h5>{post.id}</h5>
      <h4>{post.title}</h4>
      <p>{post.body}</p>
      <div style={{ position: 'relative', height: '200px', width: '200px' }}>
        <Image src={post.img} alt={post.title} fill/>
      </div>
     
    </div>
  )
}

export default PostDetail
```

### 5) 글 작성 요청 보내기 (+cors 설정)

이제 글 작성을 위한 PostCreate 페이지를 작성한다.

`pages/posts/new.tsx`

```tsx
import { instance } from '@/apis/instance'
import React, { useState } from 'react'



const PostCreate = () => {
  const [postInput, setPostInput] = useState({ title: '', body: '', img: new Blob() })
  const onSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault()
    const formData = new FormData()
    formData.append('title', postInput.title)
    formData.append('body', postInput.body)
    formData.append('img', postInput.img)
    instance.post('/posts', formData)
  }
  const onChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value, files } = e.target
    if (files) {
      setPostInput({...postInput, [name]: files[0]})
    } else {
      setPostInput({...postInput, [name]: value})
    }
  }
  return (
    <form onSubmit={onSubmit}>
      <input type="text" name="title" onChange={onChange} />
      <input type="text" name="body" onChange={onChange} />
      <input type="file" name="image" onChange={onChange} />
      <button type='submit'>등록</button>
    </form>
  )
}

export default PostCreate
```
CORS Policy 때문에 백엔드 관련 오류가 발생할 수 있는데 CORS Policy란?
> 같은 origin(도메인+포트)간의 요청만 허용하도록 하는 정책이다. 보안상으로는 매우 중요한 정책으로서 , 외부에서 크롤링하는 것등을 차단하고자 할때 슬수 있는데, 지금 다른 서버끼리 통신을 하고 있기 때문에, 서로 간에는 통신할 수 있도록 열어주는 설정을 해줘서 해당 에러를 해결할 수 있다.


백엔드로 돌아가서 cors 관련 설정을 진행한다.

> npm install cors
> npm install -D @types/cors

`app.ts`


```ts
// 코드 추가
app.use(cors({
  origin:true
}))
```

### 6) 글 수정 요청 


- 백엔드 

`src/controller/PostController.ts`
```tsx
 static updatePost = async (req: MulterS3request, res: Response) => {
    const { title, body } = req.body;
    const post = new Post();
    post.title = title;
    post.body = body;

    if (req.file) {
      const { location } = req.file
      post.img = location
    }

    const results = await myDataBase.getRepository(Post).update(Number(req.params.id), post)
    res.send(results)

  }
```

`src/router/post.ts

```ts
import {Router} from "express";
import {PostController} from "../controller/PostController";
import { upload } from "../uploadS3";

const routes = Router();

routes.post('', upload.single('img'), PostController.createPost);
routes.get('', PostController.getPosts);
routes.get('/:id', PostController.getPost);
routes.put('/:id', upload.single('img'), PostController.updatePost);

export default routes;
```

- 프론트 엔드 

`components/PostForm.tsx` (글 작성, 수정 페이지에서 공통으로 사용 가능)

```tsx
import { useRouter } from 'next/router';
import React, { useState } from 'react'

type PostFormProps = {
  submitHandler: (post: FormData) => void
  initialValue?: { title: string; body: string;  img: Blob}
}

function PostForm({ submitHandler, initialValue = { title: '', body: '', img: new Blob() } }: PostFormProps) {
  const [postInput, setPostInput] = useState(initialValue)
  const router = useRouter()
  const onSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault()
    const formData = new FormData()
    formData.append('title', postInput.title)
    formData.append('body', postInput.body)
    if (postInput.img.size) {
      formData.append('img', postInput.img)
    }
    submitHandler(formData)
   router.push('/posts')

  }

  const onChange = (e: React.ChangeEvent<HTMLInputElement>) => { 
    const { name, value, files } = e.target
    if (files) {
      setPostInput({...postInput, [name] : files[0]})
    } else {
      setPostInput({...postInput, [name] : value })
    }
  }
  return (
    <form onSubmit={onSubmit}>
      <input type="text" name='title' value={postInput.title} onChange={onChange} />
      <input type="text" name='body' value={postInput.body} onChange={onChange} />
      <input type="file" name='img'  onChange={onChange} />
      <button type='submit'>등록</button>
   </form>
  )
}

export default PostForm
```

`api/post.ts`

```ts
import { instance } from "./instance"

export const getPost = async (id: string) => {
  const response = await instance.get(`/posts/${id}`)
  return response.data
}
```

`pages/posts/[id]/edit.tsx`
```tsx
import { instance } from '@/apis/instance'
import { getPost } from '@/apis/post'
import PostForm from '@/components/PostForm'
import { useRouter } from 'next/router'
import React, { useEffect, useState } from 'react'

function PostEdit() {
  const router = useRouter()
  const { id } = router.query
  const [post, setPost] = useState({ title: '', body: '' })
  const fetchData = async (id: string) => {
    const data = await getPost(id)
    setPost(data)
  }
  useEffect(() => {
    if (router.isReady && id) {
      fetchData(id as string)
    }
  },[router.isReady, id])

  return (
    <>
      {post.title && (
        <PostForm
          submitHandler={(post) => instance.put(`/posts/${id}`, post)}
          initialValue={{ title: post.title, body: post.body , img: new Blob()}}
        />
      )}
    </>
  )
}

export default PostEdit
```
`pages/posts/new.tsx`
```tsx
import { instance } from '@/apis/instance'
import PostForm from '@/components/PostForm'
import React, { useState } from 'react'



const PostCreate = () => {
  
  
  return (
   <PostForm submitHandler={(post) => instance.post(`/posts`, post)}/>
  )
}

export default PostCreate
```

![](https://velog.velcdn.com/images/jhs000123/post/cd86d567-8087-453f-b56d-58a8bbe4af96/image.gif)

