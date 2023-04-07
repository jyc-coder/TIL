## Next.js 와 TypeScript

### 1) 정적 사이트 생성 등

타입스크립트 next앱을 만들기 위해서는 아래 명령어를 입력하면 된다. 

> yarn create next-app (앱이름) --ts

기존과 다른 점은 getStaticProps~~ 등을 지정해야한다는 것


`getStateProps`, `getStaticPaths`, `getServerSideProps` 마다 next에서 타입을 제공하고 있으니 , 해당 타입을 지정해서 사용하면 된다. 

`post/index.tsx`
```tsx
import { Post } from '@/types/Post'
import axios from 'axios'
import { GetStaticProps } from 'next'
import Link from 'next/link'
import React from 'react'

export const getStaticProps: GetStaticProps = async () => {
    const response = await axios.get('https://jsonplaceholder.typicode.com/posts')
    const posts = response.data
    return { props: { posts } }
}

type PostProps = {
    posts: Post[]
}

function Post({ posts }: PostProps) {
    return (
        <div>
            <Link href="/post">홈으로 가기</Link>
            {posts.map((post) => (
                <div key={post.id}>
                    <Link href={`/post/${post.id}`}>{post.title}</Link>
                    <p>{post.body}</p>
                </div>
            ))}
        </div>
    )
}

export default Post

```

`post/[id].tsx`

```tsx
import { Post } from '@/types/Post'
import axios from 'axios'
import { GetStaticPaths, GetStaticProps } from 'next'
import Link from 'next/link'
import React from 'react'

export const getStaticPaths: GetStaticPaths = async () => {
    const response = await axios.get(`https://jsonplaceholder.typicode.com/posts`)
    const posts = response.data
    const paths = posts.map((post: Post) => ({ params: { id: `${post.id}` } }))
    return { paths, fallback: false }
}

export const getStaticProps: GetStaticProps = async ({ params }) => {
    const response = await axios.get(`https://jsonplaceholder.typicode.com/posts/${params.id}`)
    const post = response.data
    return { props: { post } }
}

type PostDetailProps = {
    post: Post
}

function PostDetail({ post }: PostDetailProps) {
    return (
        <div>
            <h5>{post.id}</h5>
            <h4>{post.title}</h4>
            <p>{post.body}</p>
            <Link href="/post">뒤로가기</Link>
        </div>
    )
}

export default PostDetail

```

![](https://velog.velcdn.com/images/jhs000123/post/eaa15abe-7f9a-4cb2-a953-c83f28fd52cd/image.png)

