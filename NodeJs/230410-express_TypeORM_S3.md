## 파일 업로드 구현하기 

### 1) 익스프레스 시작하기


폴더 생성, express 설치

> npm init

> npm install express

> npm install -D @types/express


기본 세팅

`src/app.ts`
```ts
import * as express from "express"

const app = express()

app.listen(3000, () => {
  console.log("Express server has started on port 3000")
})


```


`tsconfig.json`

```json
{
  "compilerOptions": {
    "lib" : ["es5","es6","dom"], // 컴파일에 포함될 라이브러리 파일 목록
    "target": "es6", // 사용할 특정 ECMAScript 버전 설정
    "module":"CommonJS", // 모듈을 위한 코드 생성 설정
    "moduleResolution": "node", // 모듈 해석 방식 설정
    "emitDecoratorMetadata": true, // 데코레이터를 위한 티입 메타데이터를 내보내는 것에 대한 실험적 지원
    "experimentalDecorators": true, // ES7 의 데코레이터에 대한 실험적 지원 여부
    "outDir" : "./dist"
  }
}
```

package.json(스크립트 부분)
```json
"start": "nodemon --exec ts-node src/app.ts",
"build": "tsc"
```


`src/upload.ts`

```ts
import { Request } from "express"
import multer = require("multer");


interface DestinationCallback {
  (error: Error | null, destination : string) : void
}

interface FileNameCallback {
  (error: Error | null, filename : string) : void
}


export const upload = multer({
  storage: multer.diskStorage({
    destination: function (req: Request, file: Express.Multer.File, cb: DestinationCallback) {
      cb(null, './uploads');
    },
    filename: function (req: Request, file: Express.Multer.File, cb: FileNameCallback) {
      cb(null, Date.now() + "-" + file.originalname);
    }
  })
})


```


### 2 업로드 api 구현하기

이제 앞서 작성했던 upload 객체를 가져와서, `upload.single('img')`라고 ㅎ면 요청으로 들어온 img 라는 키를 가진 파일을 지정한 경로에 업로드하게된다.

src/app.ts 에 아래와 같이 `/upload` api를 작성한다. 
`app.ts`
```ts
import { upload } from './util/upload';
import * as express from "express"
import { Request, Response } from "express"

const app = express()
app.use(express.json())
app.use(express.urlencoded({
  extended: true
}))

app.post('/upload', upload.single('img'), (req: Request, res: Response) => {
  res.json(req.file)
})

app.listen(3000, () => {
  console.log("Express server has started on port 3000")
})

```


### 3 postman 사용

/upload 쪽으로 formdata에 img파일을 포함시켜 보냈더니 어디에 업로드가 되었는지 잘 표시된다!

![](https://velog.velcdn.com/images/jhs000123/post/3420bda8-4a30-4f7b-afdf-b4e28e67fd03/image.gif)


## S3 활용하기

### s3 버킷 설정
지금은 업로드한 파일이 서버 컴퓨터에 저장되는데,
서버컴퓨터에 파일을 바로 저장하는 것은 바람직하지 않으며, 별도로 파일의 공급과 저장을 담당하는 스토리지 서버를 구성한다면

- 파일 공급을 별도의 서버에서 담당하여 트래픽을 분산
- 서버 컴퓨터의 스토리지를 증가시키는 것에 비해 매우 저렴


파일 스토리지 서버로 aws의 s3를 사용해보자

s3를 검색해서 들어간 다음 버켓 생성을 클릭해서 버켓 만들기 화면으로 넘어간다

![](https://velog.velcdn.com/images/jhs000123/post/b9fddbbd-0bc4-4694-92aa-05a89424f663/image.png)
[O

![](https://velog.velcdn.com/images/jhs000123/post/c07daec5-d3b3-47dd-89e3-c32510a7a0c5/image.png)


![](https://velog.velcdn.com/images/jhs000123/post/a4d58844-11ec-4919-8e69-b66588b31585/image.png)

원한다면 버킷에 있는 파일 별로 누구나 볼수 있게 설정할수도 있고, 특정한 서버에서만 볼수 있게 설정할수 있는데, 이를 정하기 위한 정책을 acl 이라고 한다.

위처럼 설정하면 (acl을 통한)파일에 대한 퍼블릭 엑세스를 허용하게 된다.



### 2) s3 액세스 키 발금

해당 s3 버킷에 있는 파일을 읽는 것은 서비스 유저들 누구나 할 수 있어야겠지만, 파일을 업로드하는 것은 접근 권한이 있는 우리 백엔드에서만 할수 있어야한다.

이 접근 권한이 있는지 판별하는 과정에서 암호화된 키를 사용하게 되는데, 이 키를 발급해서 백엔드에 등록해주는 작업을 해보자

키를 발급할때는, 정확히 어떤 권한을 부여할 것인지를 정할 수 있따.

aws 상에서 iam에 접속한다.

![](https://velog.velcdn.com/images/jhs000123/post/c45abba5-ef07-45f5-8d84-1ea449ab3d55/image.png)

여기서 사용자 아래의 숫자를 클릭해서

![](https://velog.velcdn.com/images/jhs000123/post/b5826b3d-e69d-42c0-9e20-8d91015c15ef/image.png)

사용자 추가를 클릭한다.

유저 이름을 입력하고  직접 정책연결을 체크한 다음
`AmazonS3FullAccess`를 검색하여 체크한다. 이때 대문자를 신경쓰면서 검색어를 입력해야 원하는 정책이 나타난다!
![](https://velog.velcdn.com/images/jhs000123/post/44e1aec3-6b5b-451a-ab24-53e1770e72a9/image.png)

`사용자 생성`을 클릭해서 유저를 생성하고 유저를 클릭한다.

그렇게 되면 보안 자격 증명이라는 메뉴가 보이는데, 해당 메뉴를 클릭하고, 그 아래에서 access keys 부분의 create access key 버튼을 눌러준다! (키를 발급하기 위한 절차이다.)

![](https://velog.velcdn.com/images/jhs000123/post/b9d668e5-747c-4958-ae0b-0d11cdb9dfc2/image.png)

![](https://velog.velcdn.com/images/jhs000123/post/2df21ee9-f2ee-40ac-8cb6-7adb1cd071e9/image.png)

키 발급 시에는 CLI 부분을 클릭해서 발급한다.

발급이 완료되면 2가지 key가 보이는데, 우측 하단에 `.csv 파일 다운로드` 를 클릭해서 키를 저장한다.


### 3) multerS3 설정하기

`multerS3`는 multer를 기반으로, 파일 업로드 시에 바로 s3에 업로드해주는 모듈이다.
우선 설치해보자!


> npm install multer-s3 aws-sdk
> npm install -D @types/multer-s3

`src/uploadS3.ts`
```ts
import { Request } from "express";
import multer from "multer";
import multerS3 from "multer-s3";
import {S3Client} from '@aws-sdk/client-s3'

interface KeyCallback {
    (error: any, key?: string): void
}

const s3 = new S3Client({ // aws-sdk 가 제공하는 s3 접속 클라이언트 객체를 만들고,
    region: "ap-northeast-2",
    credentials:{
        accessKeyId:'엑세스 키 아이디', // 방금 발급받은 키를 입력해주세요.
        secretAccessKey:'시크릿 엑세스 키' // 방금 발급받은 키를 입력해주세요.
    }
})
export const upload = multer({
    storage: multerS3({
        s3: s3,
        bucket: '버킷 이름', // 방금 생성한 버킷 이름을 입력해주세요.
        acl: 'public-read', // 업로드된 파일은 누구나 읽을 수 있게 설정
        contentType: multerS3.AUTO_CONTENT_TYPE,
        key: function (req: Request, file: Express.Multer.File, cb: KeyCallback) {
            cb(null, Date.now() + "-" + file.originalname);
        }
    })
});
```

### 4) dotenv 설정

그런데 액세스 키와 같은 민감한 정보는 환경변수 형태로 등록해주는것이 좋다!
이를 위해서 `dotenv`를 설치해보자!

> npm install dotenv


`.env`

```
AWS_ACCESS_KEY_ID=엑세스키아이디
AWS_SECRET_ACCESS_KEY=시크릿엑세스키
```

`uploadS3.ts`

```ts
import { Request } from "express";
import multer from "multer";
import multerS3 from "multer-s3";
import {S3Client} from '@aws-sdk/client-s3'
import dotenv from "dotenv"
dotenv.config()

interface KeyCallback {
    (error: any, key?: string): void
}

const s3 = new S3Client({ 
    region: "ap-northeast-2",
    credentials:{
        accessKeyId:process.env.AWS_ACCESS_KEY_ID,
        secretAccessKey:process.env.AWS_SECRET_ACCESS_KEY
    }
})
export const upload = multer({
    storage: multerS3({
        s3: s3,
        bucket: 'express-s3test',
        acl: 'public-read',
        contentType: multerS3.AUTO_CONTENT_TYPE,
        key: function (req: Request, file: Express.Multer.File, cb: KeyCallback) {
            cb(null, Date.now() + "-" + file.originalname);
        }
    })
});
```

### 5) 업로드 api 구현하기

multerS3로 변경되었지만, 사용하는 방법 자체는 동일하다. upload객체만 uploadS3 파일에서 가져오도록 변경해주면 된다.

`src/app.ts`

```ts
import * as express from "express"
import { Request, Response } from "express"
import { upload } from "./uploadS3"

const app = express()
app.use(express.json())
app.use(express.urlencoded({
  extended: true
}))

app.post('/upload', upload.single('img'), (req: Request, res: Response) => {
  res.json(req.file)
})

app.listen(3000, () => {
  console.log("Express server has started on port 3000")
})
```

### 6) postman 사용
전과 동일하게 upload api로 요청을 보내보자!

![](https://velog.velcdn.com/images/jhs000123/post/82e956a7-3663-469c-be75-4165d81b8dd2/image.png)

응답이 잘 오는 것을 확인할수 있다. 여기서 location에 포함된 것이 바로 s3에 업로드된 파일 주소이다. (해당 주소로 들어가보면 파일을 볼수 있다.)

![](https://velog.velcdn.com/images/jhs000123/post/bcb77cb8-129e-4bd7-a034-f4f13e1ee289/image.png)

와!


이제 aws 상으로 s3 버킷에 들어가보면 해당 파일이 업로드 되어있는 것을 확인해볼수 있다!

![](https://velog.velcdn.com/images/jhs000123/post/1df978a5-535b-4e07-a6ec-b179d7c7ade4/image.png)


## DB 함께 활용하기

### 1) db 설정

이제 지난 번에 진행했던 rds랑 콜라보를 해보자

먼저 필요 모듈 설치

> npm install typeorm mysql reflect-metadata

`src/db.ts` 내부값들은 지난 시간처럼 직접 채우면 된다!

```ts
import { DataSource } from "typeorm"

const myDataBase = new DataSource({
    type: "mysql",
    host: "db서버 주소",
    port: 3306,
    username: "유저이름",
    password: "비밀번호",
    database: "mydb",
    entities: ["src/entity/*.ts"],
    logging: true,
    synchronize: true,
})
```


### 2) entity 작성

이미지도 함께 작성할수 있는 post 모델을 설계해보자
제목,내용과 함께 이미지 파일의 s3 주소를 저장하도록 작성해보자.
(db에 파일 그 자체를 저장하는 것이 아니라, s3 주소만 저장하는 것이다.)
`src/entity/post.ts`
```ts
import { Column, CreateDateColumn, Entity, PrimaryGeneratedColumn, UpdateDateColumn } from "typeorm";

@Entity()
export class Post {
  @PrimaryGeneratedColumn()
  id: number

  @Column()
  title: string 

  @Column()
  body: string 

  @Column()
  img: string

  @CreateDateColumn()
  createdAt: Date

  @UpdateDateColumn()
  updatedAt: Date;
}
```

### 3) controller 작성

post controller 부분은 post,get 요청과 관련한 로직을 다루도록 작성한다.
get 요청은 단순히 db에서 특정한 글을 찾아오도록 작성하면되지만, post요청은 조금 다르게 작성해야한다.

글작성 요청은 앞서 작성해놓은 ipload 객체와 함께, 다음과 같은 절차로 진해오딘다.

- 요청에 포함된 파일을 upload.single 함수를 통해서 s3에 업로드(업로드 시 ,해당 파일의 s3주소를 req 객체에 포함시켜준다)

- req 객체에 포함된 s3 주소와 함께, req 객체의 body에 포함된 글을 db에 저장

즉., upload.single 함수 실행 -> createPost 함수 실행의 순서로 이루어진다는 것이다. 
`src/controller/postController.ts`

```ts
import {Request, Response} from 'express'
import { Post } from '../entity/post'
import { myDataBase } from '../db'

interface MulterS3request extends Request {
  file : Express.MulterS3.File
}



export class PostController {
  static createPost = async (req: MulterS3request, res: Response) => {
    const { title, body } = req.body
    const { location } = req.file //s3 주소 가져오기

    const post = new Post();
    post.title = title;
    post.body = body;
    post.img = location // s3 주소를 지정
    const result = await myDataBase.getRepository(Post).save(post);

    res.status(201).send(result)
  }

  static getPosts = async (req: Request, res: Response) => {
    const results = await myDataBase.getRepository(Post).find();
    res.send(results)
  }
  static getPost = async (req: Request, res: Response) => {
    const results = await myDataBase.getRepository(Post).findOneBy({
      id:Number(req.params.id)
    })
    res.send(results)

  }
}
```


### 4) router 작성

router 에서는 post요청시에 upload.single을 거치도록 작성해야한다.

`src/router/posts.ts`

```ts
import { Router } from "express";
import { PostController } from "../controller/postController";
import { upload } from "../uploadS3";

const routes = Router()

routes.post('', upload.single('img'), PostController.createPost)
routes.get('', PostController.getPosts);
routes.get('/:id', PostController.getPost);

export default routes;
```

이제 app 에 등록만 해주면 끝이다!

```ts
import express from "express"
import { myDataBase } from "./db"
import PostRouter from "./router/post"

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
app.use(express.urlencoded())

app.use('/posts', PostRouter) // posts 로 경로 설정

app.listen(3001, () => {
    console.log("Express server has started on port 3001")
})
```

### 5) postman 사용

파일,제목,내용을 모두 포함시켜서 form-data 형태로 요청을 보냈더니 제대로 작동하는 것을 확인할수 있다. 

