Express 와 TypeORM을 함께 활용한다.
AWS 의 RDS를 통해서 DB서버를 구축한다.

# Express 사용하기
### 1) Express란?
Node.js를 사용해서 REST 서버를 쉽게 구현할수 있도록 도와주는 프레임 워크

REST? 
Representaional State Transfer의 약자로, **특정한 데이터 자원을 이름으로 구분해서 해당 자원과 관련된 정보를 처리하는 것**을 뜻한다.

지금까지 우리가 실컷 이용해왔던 것이 REST 서버라고 생각하면 된다. 주소의 route 부분에 자원의 이름을 명시하고, 해당 route로 POST,GET,DELETE,PUT 등의 요청을 받고 이를 처리하는 서버를 바로 REST서버라고 한다. 


REST API 를 설계할 때는 몇가지 규칙이 존재한다.
- URI에는 자원의 이름을 명시 (/posts, /users등)
- 자원에 대한 행위는 HTTP Method로 표현 (/posts/delete 라는 api를 만드는 것이 아니라, /posts로 delete 요청을 보내도록 작성)
- URI 마지막 문자에는 /를 작성하지 않음 (example.com/posts/ 가 아니라, `example.com/posts`로 작성)
- 가독성을 위해서 `하이픈(-)`은 필요한 경우 사용하되, `밑줄(_)`은 사용하지 않음 



### 2) Express 설치 후 서버 시작하기

그럼 한번 사용해보자.

> npm init

express 모듈 설치

> nm instll express


`server.js`

```js
const express = require('express') //express 모듈을 import

const app = express()

// app.메서드 명 (주소, 실행될 함수수)
app.get('/', function (req, res) {
  res.send('Hello World');
});

// 3000번 포트로 서버를 열고, 서버가 실행되면 콘솔에 출력된다
app.listen(3000, function () {
  console.log("Express server has started on port 3000")
})
```

이제 `node server.js`를 입력해서 실행하고 해당 포트번호 페이지로 이동하면?

![](https://velog.velcdn.com/images/jhs000123/post/e832414d-25f2-49cb-bd1b-80de6efa1e73/image.png)

이렇게 Hello World 가 입력되어있다.


### 3) api 추가방법

api를 추가하고 싶으면

`app.메서드명(엔드포인트,함수)`형태로 추가로 명시해주면 된다.(여기서 메서드란 get,post,delete,put)등을 얘기한다.

```js
const express = require('express') //express 모듈을 import

const app = express()

// app.메서드 명 (주소, 실행될 함수수)
app.get('/', function (req, res) {
  res.send('Hello World');
});

app.get('/posts', function (req, res) {
  res.send('Post Response')
})

// 3000번 포트로 서버를 열고, 서버가 실행되면 콘솔에 출력된다
app.listen(3000, function () {
  console.log("Express server has started on port 3000")
})
```
![](https://velog.velcdn.com/images/jhs000123/post/998784fb-d521-488d-be10-215bba3d8739/image.png)



여기서 함수는 추가적으로 연달아 명시해줄수 있따.

`app.메서드명(엔드포인트,함수,함수)`
```js
const express = require('express') //express 모듈을 import

const app = express()

// app.메서드 명 (주소, 실행될 함수수)
app.get('/', function (req,res,next) {
  req.body = { id: 1 }
  next()
},
  function (req, res) {
    res.send(req.body);
  }
  );

app.get('/posts', function (req, res) {
  res.send('Post Response')
})

// 3000번 포트로 서버를 열고, 서버가 실행되면 콘솔에 출력된다
app.listen(3001, function () {
  console.log("Express server has started on port 3000")
})
```

![](https://velog.velcdn.com/images/jhs000123/post/8994c3f2-0e5c-4da9-8e92-231c21a174c0/image.png)

보통은 유저 인증과 관련된 미들웨어 함수 등이 먼저 실행되도록하고 ,해당 미들웨어를 통과한 후에 요청을 처리하는 방식으로 작성한다.

> 기본 기능은 이정도로 하고 이제 express를 데이터베이서와 함께 사용해보자!


## Typeorm 의 개념과 특징

### 1) 관계형 데이터베이스(RDB)

블로그를 만들려면 뭐가 필요할까?

이용자가 글을 쓸 수 있고, 이용자가 쓴 글이 계쏙해서 쌓이면서 이를 다른 사람이 볼수 있어야 블로그라고 할수 있을 것이다.

이를 위해서는 바로 `데이터베이스`가 피룡하다

어떤 이용자 혹은 게시글이 있는지를 데이터베이스라는 `정보저장소`를 통해서 확인하고, 사용자나 게시글을 해당 정보 저장소에 추가하는 형태로 작업이 이루어지게 된다.

오늘은 `관계형 데이터베이스`를 사용하는데 ,`rbd` 라고도 부른다.
즉, 데이터베이스인데, 안의 데이터들이 서로 관계를 맺으면서 이루어져있다는 뜻이다.

db 내부에서 특징 정보에 대해서 정보를 담는 공간을 `테이블` 이라고 한다. 
db 는 엑셀파일이고 ,테이블은 액셀 파일 내부의 시트라고 생각하면 된다.
엑셀에서 시트를 여러개 만들수 있듯, db에서 테이블도 여러개를 만들 수 있다. 

유저가 글을 쓰는 기능을 만든다고 생각하면, 
우선 `user 테이블`과 `post 테이블`이 각각 존재해야한다.

각각 테이블은 아래와 같이 생겼다고 생각해보면 
각각의 열을 `attribute 혹은 field`라고 말한다.

`post 테이블`

id|title|body|userId
:--:|:--:|:--:|:--:
1|제목1|내용1|1
2|제목2|내용2|2


`User 테이블`

id|name|password
:--:|:--:|:--:
1|탕수육|123
2|라조기|123

우리는 Post 테이블 내부의 `userId`를 보고, User 테이블로 가서 해당 유저의 이름을 알 수 있습니다.
id가 1인 글의 경우, userId 가 1이므로, User 테이블에서 id가 1인 사람을 찾으면 '탕수육'이라는 사람이 글을 썼다는 것을 알 수 있을 것이다

이렇게 테이블로 서로 연결짓는 필드를 `forein key` 라고 한다. (예시에서는 post 테이블의 userId 필드를 foreign key 라고 말한다.)


이렇게 id로 특정한 유저를 구분짓기 위해서는 id가 고유한 값(unique) 여야 한다. 다른 유저랑 id가 똑같다면 다른 유저와 구별을 할 수가 없게 되니, 누가 쓴 글인지 확인할 수 없게 될 것이다. 

그래서 이렇게 각 테이블마다 존재하는 unique한 id를 `primary key`라고 한다.

보통은 아래와 같이 연결 구조를 표현하는 diagram을 그려서 db에 대한 초기 설계를 진행하게 된다.


![](https://velog.velcdn.com/images/jhs000123/post/cf0144af-2499-4004-a773-21d3f6188c23/image.png)



![](https://velog.velcdn.com/images/jhs000123/post/c8aa41ac-6294-4ad6-b573-7c35e286bc2b/image.png)
그래서 그게 typeorm 이랑 뭔상관임?


### 2) Typeorm 이란?

관계형 데이터베이스에 대해서 데이터를 추가, 삭제등의 작업을 진행하기 위해서는 SQL언어를 사용해야한다. 따라서 보통은 백엔드를 배우기 전에 SQL언어를 공부하는 것이 좋다!


`ORM(Object-relational-mapping)`이란, 관계형 데이터베이스를 사용하는 것에 있어서, 객체 지향적인 코드로 데이터베이스를 사용할 수있도록 도와주는 프레임워크를 말한다.

백엔드마다 다양한 ORM 이 존재한느데, 우리가 오늘 배울 Typeorm 은 `Nodejs.Typescript` 환경에서 대표적으로 활용되는 ORM 이다.

### 3) Express + TypeScript 설정하기

새로운 폴더를 만들고, express를 설치해보자

>npm init

>npm install express

>npm install -D @types/express


`src/app.ts`

```ts
import * as express from "express"
import { Request, Response } from "express"

const app = express()

app.listen(3000, () => {
    console.log("Express server has started on port 3000")
})
```

타입스크립트로 작성했으니 이렇게 작성해서는 작동하지 않는다.
정확히 어떤 방식을 컴파일 할지를 설정해주고

`tsconfig.json`
```json
{
    "compilerOptions": {
        "lib": ["es5", "es6", "dom"],
        "target": "es6",
        "module": "commonjs",
        "moduleResolution": "node",
        "emitDecoratorMetadata": true,
        "experimentalDecorators": true,
        "outDir":"./dist",
    }
}
```

컴파일 명령어를 사용하기 위해서 typescript를 글로벌에 설치해둔다.

> npm install -g typescript

이제 명령어를 입력하면 컴파일이 시작된다!

> tsc

dist 폴더가 생성되어있을텐데, 해당경로의 app.js파일을 node 명령어로 호출하면

> node dist/app.js


그럼 이제 컴파일 후 바로 실행될수 있도록 scripts의 코드를 추가해주자

> "start" : "tsc && node dist/app.js"

이렇게 해주면 `npm start`만 해도 실행할수 있다.


하지만 파일을 변경할 때마다 npm start를 다시 해야한다.


> npm install -g ts-node nodemon

두가지 패키지를 추가로 설치해서, 파일벼녁ㅇ할때 자동으로 ts파일을 다시 컴파일 - 실행 하도록 설정한다.

scripts 부분을 아래와 같이수정한다.
```json
"start": "nodemon --exec ts-node src/app.ts",
"build": "tsc"
```

npm start를 하게 되면, 파일으 수정할 때마다 자동을 서버가 다시 실행된다.


### 4) Typeorm 설치 후 DB 연결하기(RDS 설정하기)

Typeorm 과 함께 RDB중 하나인 mysql을 사용한다.

필요한 라이브러리를 설치한다.

>npm install typeorm mysql reflect-metadata

이제 mysql 데이터베이스와 연결한다.
데이터베이스는 우리 컴퓨터에 설치해놓고 사용할수 있지만, 실제로 서비스를 할 때는 db전용 서버를 따로 분리해서 활용하게된다.

db서버를 분리해서 사용하는 이유

- **확장성** (하나의 서버 컴퓨터에 DB 를 설치해놓게 되면, 추후에 트래픽 관리 등을 위해서 서버를 확장할 때 DB 간의 싱크가 어긋나는 현상이 발생할 수 있다. DB 서버를 따로 하나를 두고, 백엔드 서버 컴퓨터를 여러 대 두어 활용하면 해당 문제를 해결할 수 있다.)
- **보안** (설령 백엔드 서버가 해킹당하더라도, DB 서버는 분리되어 있기 때문에 더욱 안전하게 데이터들을 보호할 있다.)

AWS의 RDS라는 서비스를 이용해서 DB서버를 구축하려고 한다.

rds를 검색하고 데이터베이스 생성을 선택한다.

템플릿은 프리티어, 퍼블릭 액세스는 yes로 설정한다. 현재 내 컴퓨터에서도 접근할수 있도록 하는 설정이다. 실제 Production 단계에서는 No로 설정해주면 된다.


모니터링 아래에 additional configuration을 누르고
db이름을 입력해준다.


마지막으로 생성을 눌러주면 

![](https://velog.velcdn.com/images/jhs000123/post/f9b4fa91-252a-48c0-ac89-a819eadd53d8/image.png)

이렇게 데이터베이스가 생성된다.
지금은 상태가 생성중이라고 되어있지만, 시간이 지나고 새로고침하면 사용가능으로 변경되어있다


이제 VPC 보안 그룹 아래의 `dafault`를 누른다.

![](https://velog.velcdn.com/images/jhs000123/post/4e457db5-5ee3-46dc-946e-472bed1af767/image.png)

인바운드 규칙 -> 인바운드 규칙편집 으로 들어간다.

![](https://velog.velcdn.com/images/jhs000123/post/dd6d56a1-0a9b-45d9-aec8-14026f516613/image.png)


규칙 추가를 누르고, mysql/aurora랑 mp IP로 설정해준다. (내 IP로 해당 DB 서버에 접근할수 있도록 설정해준 것이다. )

![](https://velog.velcdn.com/images/jhs000123/post/c9bb87d7-3f1f-4e7a-9a90-d0d549f66f32/image.png)

이렇게 해주고 우측 하단의 규칙 저장을 누르면 설정완료!

그럼 이제 연결해보자.

`src/db.ts`(host에는 endpoint주소를 작성해주고, username,password,database는 방금서정한 대로 작성한다)

```ts
import { DataSource } from "typeorm"

const myDataBase = new DataSource({
    type: "mysql",
    host: "db서버 주소",
    port: 3306,
    username: "유저이름",
    password: "비밀번호",
    database: "mydb", // db 이름
    entities: ["src/entity/*.ts"], // 모델의 경로
    logging: true, // 정확히 어떤 sql 쿼리가 실행됐는지 로그 출력
    synchronize: true, // 현재 entity 와 실제 데이터베이스 상 모델을 동기화
})
```

이제 app.ts로 가서 db 연결을 진행한다.
```ts
import * as express from "express"
import { Request, Response } from "express"
import { myDataBase } from "./db.ts"

// db 연결
myDataBase
    .initialize()
    .then(() => {
        console.log("DataBase has been initialized!")
    })
    .catch((err) => {
        console.error("Error during DataBase initialization:", err)
    })

const app = express()

app.listen(3000, () => {
    console.log("Express server has started on port 3000")
})
```

이제 실행해보면 
> npm start

![](https://velog.velcdn.com/images/jhs000123/post/4ea94376-dbf2-4101-a74e-183ee2863b5d/image.png)

잘 연결 되었다!

## Post 만들기

### 1) Post 모델 만들기 (entity)


모델을 만들때는 클래스를 활용하면 된다.

클래스 앞에 `@entity`와 같이 데코레이터를 명시하면, TypeORM이 해당 클래스를 하나의 데이터베이스 테이블로서 인식하게 된다.

title과 body를 가진 Post 테이블은 아래와 같이 작성할수 있다.
`entity/post.ts`
```ts
import { Column, CreateDateColumn, Entity, PrimaryGeneratedColumn, UpdateDateColumn } from "typeorm";

@Entity() // this class is database TABLE 
export class Post{
  @PrimaryGeneratedColumn() // Primary Key, 자동생성
  id: number

  @Column() // one of Table attribute
  title: string

  @Column() 
  body: string

  @CreateDateColumn() // date of created post
  createdAt: Date
  
  @UpdateDateColumn() // date of updated post
  updatedAt: Date
}
```

이렇게 두고 db에 연결하면, 자동으로 Post라는 이름의 테이블이 만들어진다.

### 2) Post CRUD 만들기

해당 Post 테이블에 내용을 CRUD 하는 기능을 구현해보자!

db에서 해당 테이블에 접근하기 위해서는 `(데이터베이스객체).getRepository(테이블명)`과 같이 작성하면 된다.

데이터베이스 객체의 이름은 `db.ts`에 작성한 
`myDataBase`이고 , 테이블 이름은 entity.ts에 Post라고 작성했다.

이를 활용해서 각 메서드 별로 코드를 작성한다.
- GET
테이블 내부의 모든 데이터를 가져오고 싶으면 find를 사용하면된다.

```ts
const posts = await myDataBase.getRepository(Post).find()
```

만약에 특정한 조건을 만족하는 데이터만 가져오고 싶으면 `findOne({where: 조건})`혹은 `findOneBy(조건)` 함수를 사용하면 된다.

엔드포인트 주소를 `posts/:id`와 같은 형태로 작성하면, 해당 id값은 `req.params.id`로 받아올수 있다!

```ts
const post = await myDataBase.getRepository(Post).findOneBy({ 
  id: Number(req.params.id) // 요청 파라미터에 있는 id 값을 가져와서 조건으로 명시
})
```

```ts
const post = await myDataBase.getRepository(Post).findOne({
  where: { // sql 상 where 절
    id: Number(req.params.id) // 요청 파라미터에 있는 id 값을 가져와서 조건으로 명시
  }
})
```

- POST

데이터를 추가하고 싶으면 insert함수를 사용하면된다.

```ts
cosnt result = await myDataBase.getRepository(Post).insert(Post객체)
```
여기서 Post객체는 직접 만들어줘야한다.

```ts
const post = newPost() 
post.title = req.body.title
post.body = req.body.body
```

- PUT
데이터를 수정하고 싶으면 update함수를 쓰면된다.

```ts
const result = await myDataBase.getRepository(Post).update(어떤 객체를 업데이트할 지 기준, 수정할 값이 담긴 객체)
```
ex) id가 일치하는 글을 찾고 해당 글의 내용을 2번째 인자로 명시한 객체대로 수정해준다.

```ts
const result = await myDataBase.getRepository(Post).update(Number(req.params.id), {title: '새 제목'})
```

이렇게 하면 제목 부분만 수정해준다.

- DELETE
데이터를 지우고 싶으면 delete 함수를 쓴다.

```ts
const result = await myDataBase.getRepository(Post).delete(삭제할데이터의id)
```

전체 코드
```ts
import * as express from "express"
import { Request, Response } from "express"
import { myDataBase } from "./db"
import { Post } from "./entity/post"


myDataBase.initialize().then(() => {
  console.log("DataBase has been initialized")
})
  .catch((err) => {
  console.error("Error during DataBase initialization", err)
})

const app = express()



app.get("/posts", async function (req: Request, res: Response) {
  const posts = await myDataBase.getRepository(Post).find()
  return res.send(posts)
})

app.get("/posts/:id", async function (req: Request, res: Response) {
  const post = await myDataBase.getRepository(Post).findOneBy({
    id: Number(req.params.id)
  })
  if (!post) {
    return res.status(404).send("No Content")
  }
  return res.send(post)
})


app.post("/posts", async function (req: Request, res: Response) {
  // 데이터 가져오기
  const { title, body } = req.body
  const post = new Post()
  // 해당 값대로 객체를 생성
  post.title = title
  post.body = body
  // insert로 쏙!
  const result = await myDataBase.getRepository(Post).insert(post)
  return res.send(result)
})


app.put("/posts/:id", async function (req: Request, res: Response) { 
  const result = await myDataBase.getRepository(Post).update(Number(req.params.id), req.body)
  return res.send(result)
})

app.delete("/posts/:id", async function (req: Request, res: Response) {
  const result = await myDataBase.getRepository(Post).delete(Number(req.params.id))
  return res.send(result)
})
app.listen(3001, () => {
    console.log("Express server has started on port 3000")
})
```

### 3)파일 분리하기 (route 와 controller)

app.ts에 모든 코드를 다 때려넣으니 파일이 너무 길어졌다.

따라서 역할에 따라서 분리해주도록 하자
- routes 폴더 -> 각 자원에 따른 route 파일을 만들고, 해당 route 파일 내부에 post,get에 따른 함수를 작성

- controllers 폴더 -> route에 들어갈 함수 (db에 접근하는 로직)을 따로 분리해서 작성


routes 의 경우 특정한 router 객체에 자원에 따른 엔드포인트를 작성해놓고, app.ts파일에서 해당 router 객체를 불러와서 사용하게 하면된다.


```ts
const router = express.Router();

router.get(엔드포인트, 함수)

export default router

```

**앱 파일 예시**
```ts
import myRouter from '라우터 경로'

app.use('my', myRouter) // 위 라우터에 작성한 엔드포인트 모두 my/ 로 접근할 수 있게 됨
```

Post Router는 이렇게 작성해볼수 있다.

`src/routes/posts.ts`
```ts
import * as express from "express"
import { Request, Response } from "express"
import { myDataBase } from "../db";
import { Post } from "../entity/post";

const router = express.Router()

router.get('/', async function (req: Request, res: Response) {
  const posts = await myDataBase.getRepository(Post).find()
  return res.send(posts)
})

router.get("/:id", async function (req: Request, res: Response) {
  const post = await myDataBase.getRepository(Post).findOneBy({
    id: Number(req.params.id),
  })
  if (!post) {
    return res.status(404).send('No Content')
  }
  return res.send(post)
})


router.post("/", async function (req: Request, res: Response) { 
  const { title, body } = req.body
  const post = new Post()
  post.title = title
  post.body = body
  const result = await myDataBase.getRepository(Post).insert(post)
  return res.send(result)
})

router.put("/:id", async function (req: Request, res: Response) {
  const result = await myDataBase.getRepository(Post).update(Number(req.params.id), req.body)
  return res.send(result)
})

router.delete("/:id", async function(req: Request, res: Response) {
  const result = await myDataBase.getRepository(Post).delete(Number(req.params.id))
  return res.send(result)
})

export default router

```

이제 이걸 app에서 불러와 사용해주면 된다
`src/app.ts`
```ts
import * as express from "express"
import { Request, Response } from "express"
import { myDataBase } from "./db"
import { Post } from "./entity/post"
import postRouter from "./routes/posts"


myDataBase.initialize().then(() => {
  console.log("DataBase has been initialized")
})
  .catch((err) => {
  console.error("Error during DataBase initialization", err)
})

const app = express()
app.use(express.json())

// Routes
app.use('/api/posts', postRouter)

app.listen(3001, () => {
    console.log("Express server has started on port 3000")
})
```

이제 /api/posts로 요청을 보내면 위 라우터에 작성한 엔드포인트로 연결되는 것이다.


이제 controller 까지 분리해보겠습니다.

controller는 특별히 새로운 개념이 활용되는 것이 아니라, 위 라우터의 함수 부분을 따로 분리하는 것 뿐입니다!

controller 는 클래스와 static 함수를 활용하는 형태로 작성한다.

`src/controllers/PostController.ts`

```ts
import { myDataBase } from "../db"
import { Post } from "../entity/post"
import { Request, Response } from "express"

export class PostController {
  static getPosts = async (req: Request, res: Response) => {
    const posts = await myDataBase.getRepository(Post).find()
    return res.send(posts)
  }
  static getPost = async (req: Request, res: Response) => {
    const post = await myDataBase.getRepository(Post).findOneBy({
      id: Number(req.params.id),
    })
    if (!post) {
      return res.status(404).send('No Content')
    }
    return res.send(post)
  }
  static createPost = async (req: Request, res: Response) => {
    const { title, body } = req.body
    const post = new Post()
    post.title = title
    post.body = body
    const result = await myDataBase.getRepository(Post).insert(post)
    return res.send(result)
  }

  static updatePost = async (req: Request, res: Response) => {
[O    const result = await myDataBase.getRepository(Post).update(Number(req.params.id), req.body)
    return res.send(result)
  }

  static deletePost = async (req: Request, res: Response) => {
    const result = await myDataBase.getRepository(Post).delete(Number(req.params.id))
    return res.send(result)
  }
}
```

함수 부분을 다 분리했으니, 라우터 파일은 아래처럼 단순하게 작성이 가능하다!
`src/routes/posts.ts`
```ts
import * as express from "express"
import { PostController } from "../controllers/PostController";

const router = express.Router()

router.get('/', PostController.getPosts)
router.get('/:id', PostController.getPost)
router.post('/', PostController.createPost)
router.put('/:id', PostController.updatePost)
router.delete('/:id', PostController.deletePost)



export default router

```

![](https://velog.velcdn.com/images/jhs000123/post/ac5ef0d5-3fd6-4b6b-b37f-ae3e31bdb9f2/image.jpg)

와! 이제 Post 데이터베이스의 작업이 끝났다!

이제 가도 돼요?

## Comment 만들기 (Foreign key)

### 1) Comment 모델 만들기
이제 게시글 마다 댓글을 달수 있도록, 댓글 모델을 작성해보자. 


**‘게시글 마다’** 댓글이 달려야 하고, 존재하지도 않는 게시글에 댓글이 달려서는 안되기 때문에 Foreign Key 로 댓글 모델과 게시글 모델을 연결시켜주도록 하겠습니다.

Foreign Key 에는 총 4가지 종류의 연결 관계가 존재합니다.

- **OneToOne → 게시글 하나 당 댓글 하나만 존재할 수 있는 경우**
- **OneToMany → 게시글 하나 당 댓글 여러개가 존재할 수 있는 경우**
- **ManyToOne → 게시글 여러개 당 댓글 하나만 존재할 수 있는 경우**
- **ManyToMany → 게시글 하나에 댓글이 여러개 존재할 수 있고, 댓글 하나가 여러 게시글에 존재할 수 있는 경우**

게시글 - 댓글의 관계는 게시글 입장에서 OneToMany 관계에 해당한다.
따라서 Post 모델 안에 OneToManu라는 데코레이터를 명시해서 댓글 어트리뷰트를 만들어주면 된다.

`src/entity/Post.ts`
```ts
import { Column, CreateDateColumn, Entity, OneToMany, PrimaryGeneratedColumn, UpdateDateColumn } from "typeorm";
import {Comment} from './comment'

@Entity() // this class is database TABLE 
export class Post{
  @PrimaryGeneratedColumn() // Primary Key, 자동생성
  id: number

  @Column() // one of Table attribute
  title: string

  @Column() 
  body: string

  @OneToMany(() => Comment, comment => comment.post) // (타입, 어트리뷰트) 명시
  comments : Comment[]

  @CreateDateColumn() // date of created post
  createdAt: Date

  @UpdateDateColumn() // date of updated post
  updatedAt: Date
}
```

이렇게 하면 Post 테이블에 Comment 테이블을 연결시켜주세 되는데, 아직 Comment 모델이 없으니까 모델을 만들어주자

댓글은 내용만 받도록 설계한다.
댓글 입장에서 Post와의 관계는 ManyToOne(댓글 여러개가 하나의 게시글에 존재할수 있음)이다.

`src/entity/Comment.ts`

```ts
import { Entity, Column, PrimaryGeneratedColumn, CreateDateColumn, UpdateDateColumn, ManyToOne } from "typeorm"
import { Post } from "./post";

@Entity()
export class Comment {
    @PrimaryGeneratedColumn()
    id: number

    @Column()
    body: string

    @ManyToOne(() => Post, post => post.comments)
    post: Post // 정확히 어떤 게시글에 달린 댓글인지

    @CreateDateColumn()
    createdAt: Date

    @UpdateDateColumn()
    updatedAt: Date;
}
```

### 2) Comment POST 추가

controller 부터 바로 작성해보자
`src/controllers/CommentController.ts`
```ts
import { myDataBase } from "../db"
import { Post } from "../entity/Post"
import { Request, Response } from "express"
import { Comment } from "../entity/Comment"

export class CommentController {
  static createComment = async (req: Request, res: Response) => {
    const { postId, body } = req.body
    const post = await myDataBase.getRepository(Post).findOneBy({
      id: postId,
    })
    const comment = new Comment()
    comment.body = body
    comment.post = post
    // 해당 객체대로 db에 추가
    const result = await myDataBase.getRepository(Comment).insert(comment)
    return res.send(result)
  }
}
```

이제 라우터를 작성해보자.

`routes/comments.ts`

```ts
import * as express from "express"
import { CommentController } from "../controllers/CommentController";

const router = express.Router();
// postId, body 2가지가 필요
router.post("/", CommentController.createComment)

export default router
```

이제 요 라우터를 앱에 등록해보자.

```ts
import * as express from "express"
import { myDataBase } from "./db"
import postRouter from "./routes/posts"
import commentRouter from "./routes/comments"


myDataBase.initialize().then(() => {
  console.log("DataBase has been initialized")
})
  .catch((err) => {
  console.error("Error during DataBase initialization", err)
})

const app = express()
app.use(express.json())

// Routes
app.use('/api/posts', postRouter)
app.use('/api/comments',commentRouter)

app.listen(3001, () => {
    console.log("Express server has started on port 3000")
})
```

요렇게 해주면 등록 요청을 할수 있게 된다!
댓글이 잘 등록되었는지 확인하려면 어떻게 할까?
게시글을 가져올때, 해단 게시글에 등록된 댓글도 표시하도록 Post Controller를 수정해보자.

find를 할때, 인자로 `relations:['다른 테이블과 연결된 어트리뷰트명']`을 명시해주면 된다.


`src/controllers/PostController.ts`
```ts
static getPosts = async (req: Request, res: Response) => {
    const posts = await myDataBase.getRepository(Post).find({ relations: ['comments'] })
    return res.send(posts)
}
```

이제 post 쪽으로 get요청을 보내면 댓글 정보까지 모두 표시되는 것을 확인할수 있다

글 하나를 가져오는 부분은 이렇게 수정하자.

```tsx
 static getPost = async (req: Request, res: Response) => {
    const post = await myDataBase.getRepository(Post).findOne({
      where: {
        id: Number(req.params.id),
      },
      relations:["comments"]
    })
    if (!post) {
      return res.status(404).send('No Content')
    }
    return res.send(post)
  }
```

수정과 제거는 Post와 비슷한 방식으로 해주면 된다!

```ts
import { Request, Response } from "express"
import { myDataBase } from "../db"
import { Comment } from "../entity/Comment"
import { Post } from "../entity/Post"

export class CommentController {
    static createComment = async (req: Request, res: Response) => {
        // 요청 데이터 값 가져오기
        const {postId, body} = req.body
        // 해당 게시글 번호로 게시글 찾기
        const post = await myDataBase.getRepository(Post).findOneBy({
            id: postId,
        })
        // 해당 값대로 Comment 객체 생성
        const comment = new Comment()
        comment.body = body
        comment.post = post // 연결할 게시글 지정
        // 해당 객체대로 db 에 추가
        const result = await myDataBase.getRepository(Comment).insert(comment)
        return res.send(result)
    }
    static getComments = async (req: Request, res: Response) => {
        const comments = await myDataBase.getRepository(Comment).find({ relations: ['post'] })
        return res.send(comments)
    }
    static getComment = async (req: Request, res: Response) => {
        const comment = await myDataBase.getRepository(Comment).findOne({
            where: {
                id: Number(req.params.id),
            },
            relations: ['post']
        })
        if (!comment) {
            return res.status(404).send('No Content')
        }
        return res.send(comment)
    }
    static updateComment = async (req: Request, res: Response) => {
        const result = await myDataBase.getRepository(Comment).update(Number(req.params.id), req.body)
        return res.send(result)
    }
    static deleteComment = async (req: Request, res: Response) => {
        const result = await myDataBase.getRepository(Comment).delete(Number(req.params.id))
        return res.send(result)
    }
}
```

라우터도 마찬가지

```ts
import * as express from "express"
import { CommentController } from "../controllers/CommentController";

const router = express.Router();
// postId, body 2가지가 필요
router.post("/", CommentController.createComment)
router.get("/", CommentController.getComments)
router.get("/:id", CommentController.getComment)
router.put("/:id", CommentController.updateComment)
router.delete("/:id", CommentController.deleteComment)


export default router
```

## Like 만들기 (Many-toMany)

### 1) User 모델 만들기


이제 좋아요 기능을 만들어보자.

좋아요를 할 주체가 필요한데, 이번에는 유저 인증 시스템을 구현하지 않고, Like 기능 구현을 위해서 User 모델만 만들도록 하자

`src/entity/User.ts`		
```ts
import { Entity, Column, PrimaryGeneratedColumn } from "typeorm"

@Entity()
export class User {
    @PrimaryGeneratedColumn()
    id: number

    @Column({ unique: true }) // 유저이름만 받도록 설계
    username: string
}
```

### 2) User GET/POST 추가

User 모델과 관련해서, get / post api를 구현해보자.

`src/controllers/UserController.ts`
```ts
import { myDataBase } from "../db"
import { User } from "../entity/User"
import { Request, Response } from "express"

export class UserController {
  static getUsers = async (req: Request, res: Response) => {
    const users = await myDataBase.getRepository(User).find()
    return res.send(users)
  }
  static createUser = async (req: Request, res: Response) => { 
    const { username } = req.body
    const user = new User()
    user.username = username
    const result = await myDataBase.getRepository(User).insert(user)
    return res.send(result)
  }

}
```

`src/routes/users.ts`
```ts
import * as express from "express"
import { UserController } from "../controllers/UserController";

const router = express.Router();

router.get("/", UserController.getUsers)
router.post("/", UserController.createUser)

export default router
```

`app.ts`
```ts
import * as express from "express"
import { myDataBase } from "./db"
import postsRouter from "./routes/posts"
import commentsRouter from "./routes/comments"
import usersRouter from "./routes/users"

// db 연결
myDataBase
    .initialize()
    .then(() => {
        console.log("DataBase has been initialized!")
    })
    .catch((err) => {
        console.error("Error during DataBase initialization:", err)
    })

const app = express()
app.use(express.json())

// Routes
app.use('/api/posts', postsRouter)
app.use('/api/comments', commentsRouter)
app.use('/api/users', usersRouter)

app.listen(3000, () => {
    console.log("Express server has started on port 3000")
})
```

이제 모델에 좋아요를 등록하기 위해서 그 주체로 사용할 user를 postman을 사용해서 미리 만들어주자

![](https://velog.velcdn.com/images/jhs000123/post/ea24f070-51e3-49c4-a2a6-8e50ccf95f04/image.png)


![](https://velog.velcdn.com/images/jhs000123/post/22b5a386-260e-4627-b69b-535227c05811/image.png)


### 3) Like 모델 만들기

이제 Many-toManu 관계의 모델을 설계해보자

유저들은 여러가지 게시글에 좋아요를 누를수 있고, 게시글 또한 여러가지 유저에 의해 좋아요가 될수 있다.

이러한 관계를 지정해 놓는 용도로 Like라는 모델을 만들어보자.
![](https://velog.velcdn.com/images/jhs000123/post/4e26036a-04b2-4645-b503-38c50e8492c4/image.png)

원래는 ManyToMany 데코레이터를 사용해서 아래보다 더 짧은 코드로 구현할수 있지만 더 쉬운 방법으로 구현해보자.


Like 입장에서는 게시글, 유저와의 관계 모두 ManyToOne 이다. (하나의 게시글이 여러 좋아요를 가질수 있고, 하나의 유저가 좋아요를 여러번 할수 있기 때문이다. )




`src/entity/Like`
```ts
import { Entity, Column, PrimaryGeneratedColumn, CreateDateColumn, UpdateDateColumn, ManyToOne } from "typeorm"
import { Post } from "./Post"
import { User } from "./User"


@Entity()
export class Like {
    @PrimaryGeneratedColumn()
    id: number

    @ManyToOne(() => Post, post => post.likes, { onDelete: 'CASCADE', onUpdate: 'CASCADE' })
    post: Post

    @ManyToOne(() => User, user => user.likes, { onDelete: 'CASCADE', onUpdate: 'CASCADE' })
    user: User
}
```

Post 모델도 Like 모델과 연결되도록 수정한다.
likes 라는 필드를 따로 만들어서 모델의 post 부분과 연결한다. (likes 필드에는 해당 게시글을 좋아한 유저가 누구인지가 들어가게 된다.)

`src/entity/Post.ts`

```ts
import { Column, CreateDateColumn, Entity, OneToMany, PrimaryGeneratedColumn, UpdateDateColumn } from "typeorm";
import { Comment } from "./Comment";
import { Like } from "./Like";
@Entity() // this class is database TABLE 
export class Post{
  @PrimaryGeneratedColumn() // Primary Key, 자동생성
  id: number

  @Column() // one of Table attribute
  title: string

  @Column() 
  body: string

  @OneToMany(() => Comment, comment => comment.post) // (타입, 어트리뷰트) 명시
  comments: Comment[]
  
  @OneToMany(() => Like, like => like.post)
    likes:Like[]

  @CreateDateColumn() // date of created post
  createdAt: Date

  @UpdateDateColumn() // date of updated post
  updatedAt: Date
}
```


User 모델도 Like와 연결되도록 수정한다.
likes 라는 필드를 따로 만들어서 Like 모델의 user부분과 연결한다. (likes 필드에는 해당 유저가 어떤 게시글을 좋아했는지가 들어간다.)

`src/entity/User.ts`

```ts
import { Column, Entity, OneToMany, PrimaryColumn, PrimaryGeneratedColumn } from "typeorm";
import { Like } from "./Like";

@Entity()
export class User{
  @PrimaryGeneratedColumn()
  id: number
  
  @Column({ unique: true }) // 유저이름만 받도록 설계
  username: string

  @OneToMany(() => Like, like => like.user)
  likes: Like[]
}
```

### 4) Like Post 추가
좋아요 요청에 대한 api는 따로 Like라는 router를 만들어도 되고, 기존의 post 라우터에 추가하는 방식으로 작성해도 된다.


좋아요 요청의 프로세스는 아래와 같다
- 요청한 사람이 해당 게시글을 이미 좋아했는지 확인
- 이미 좋아요 했다면 좋아요 테이블에서 해당 정보를 제거
- 좋아요 하지 않았다ㅕㄴ 좋아요 테이블에 좋아요를 추가

> 유저 인증이 구현되지 않았으므로, 요청한 사람이 누구인지는 요청 데이터에 포함된 유저 id값으로 식별하도록 한다.

`PostController.ts`
```ts

  static likePost = async (req: Request, res: Response) => {
    const isExist = await myDataBase.getRepository(Like).findOne({
      where: {
        post: { id: Number(req.params.id) },
        user: {id:req.body.id}, // {id: 유저아이디} 가 요청 시에 필요
      }
    })
    // 이미 좋아요를 누른게 아니라면?
    if (!isExist) {
      const post = await myDataBase.getRepository(Post).findOneBy({
        id:Number(req.params.id),
      })
      const user = await myDataBase.getRepository(User).findOneBy({
        id: req.body.id,
      })

      const like = new Like()
      like.post = post
      like.user = user 
      await myDataBase.getRepository(Like).insert(like)
    } else {
      // 좋아요가 이미 있네?
      await myDataBase.getRepository(Like).remove(isExist)
    }
    return res.send({message:'success'})
  }
```

`routes/posts.ts`
```ts
router.post('/:id/like',PostController.likePost)
```

get 요청 시에 좋아요와 유저정보도 받고싶으면 이렇게 controller를 수정하면 된다.

```ts
static getPosts = async (req: Request, res: Response) => {
    const posts = await myDataBase.getRepository(Post).find(
      {relations:["comments","likes" , "likes.user"]}
    )
    return res.send(posts)
  }
  static getPost = async (req: Request, res: Response) => {
    const post = await myDataBase.getRepository(Post).findOne({
      where: {
        id: Number(req.params.id),
      },
      relations:["comments","likes","likes.user"]
    })
    if (!post) {
      return res.status(404).send('No Content')
    }
    return res.send(post)
  }
```




