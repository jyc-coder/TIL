## authentication이 뭐임?

참이라는 근거가 있는 무언가를 확인하거나 확증하는 행위
- 유저의 identification을 확인하는 절차
- 유저의 아이디와 비밀번호를 확인
- 인증을 위해서 먼저 유저의 아이디와 비번을 생성할수 있는 기능이 필요



메인 화면에서 로그인고 회원가입 기능 구현을 위해서 modal 을 생성한다.

mui modal을 사용해서 기본 모달을 구현한다.


```tsx
import * as React from 'react';
import Box from '@mui/material/Box';
import Button from '@mui/material/Button';
import Typography from '@mui/material/Typography';
import Modal from '@mui/material/Modal';

const style = {
  position: 'absolute' as 'absolute',
  top: '50%',
  left: '50%',
  transform: 'translate(-50%, -50%)',
  width: 400,
  bgcolor: 'background.paper',
  border: '2px solid #000',
  boxShadow: 24,
  p: 4,
};

export default function BasicModal() {
  const [open, setOpen] = React.useState(false);
  const handleOpen = () => setOpen(true);
  const handleClose = () => setOpen(false);

  return (
    <div>
      <Button onClick={handleOpen}>Open modal</Button>
      <Modal
        open={open}
        onClose={handleClose}
        aria-labelledby="modal-modal-title"
        aria-describedby="modal-modal-description"
      >
        <Box sx={style}>
          <Typography id="modal-modal-title" variant="h6" component="h2">
            Text in a modal
          </Typography>
          <Typography id="modal-modal-description" sx={{ mt: 2 }}>
            Duis mollis, est non commodo luctus, nisi erat porttitor ligula.
          </Typography>
        </Box>
      </Modal>
    </div>
  );
}
```

당연히 `npm install @mui/material`을 해서 mui를 설치해줘야 렌더링 될것이다. 


```tsx
'use client'
import { useState } from 'react'
import Box from '@mui/material/Box'
import Button from '@mui/material/Button'
import Typography from '@mui/material/Typography'
import Modal from '@mui/material/Modal'

const style = {
  position: 'absolute' as 'absolute',
  top: '50%',
  left: '50%',
  transform: 'translate(-50%, -50%)',
  width: 400,
  bgcolor: 'background.paper',
  border: '2px solid #000',
  boxShadow: 24,
  p: 4,
}

export default function LoginModal({ isSignin }: { isSignin: boolean }) {
  const [open, setOpen] = useState(false)
  const handleOpen = () => setOpen(true)
  const handleClose = () => setOpen(false)

  const renderContent = (signinContent: string, signupContent: string) => {
    return isSignin ? signinContent : signupContent
  }
  return (
    <div>
      <button
        className={`${renderContent('text-white bg-blue-400', '')} border p1 px-4 rounded mr-3 `}
        onClick={handleOpen}
      >
        {renderContent('Sign in', 'Sign up')}
      </button>

      <Modal
        open={open}
        onClose={handleClose}
        aria-labelledby="modal-modal-title"
        aria-describedby="modal-modal-description"
      >
        <Box sx={style}>
          <Typography id="modal-modal-title" variant="h6" component="h2">
            Text in a modal
          </Typography>
          <Typography id="modal-modal-description" sx={{ mt: 2 }}>
           {renderContent('로그인 모달!', '회원가입 모달!')}
          </Typography>
        </Box>
      </Modal>
    </div>
  )
}
```
위의 코드는 상위컴포넌트로부터 `isSignin`이라는 boolean값을 받아와서 값에 따라서 다른 스타일 속성의 버튼 디자인을 렌더링 하는 것이다. 

```tsx
import Link from 'next/link'
import LoginModal from './LoginModal'

export default function NavBar() {
  return (
    <nav className="flex justify-between p-2 bg-white">
      <Link href="/" className="text-2xl font-bold text-gray-700">
        {' '}
        OpenTable{' '}
      </Link>
      <div>
        <div className="flex">
          <LoginModal isSignin={true} />
          <LoginModal isSignin={false} />
        </div>
      </div>
    </nav>
  )
}

```

이런식으로 로그인, 회원가입 버튼을 렌더링 하면

![](https://velog.velcdn.com/images/jhs000123/post/f08ada02-6593-44a5-9fb8-76231b262e7c/image.png)

각각의 버튼을 클릭할때 모달 창이 나타난다.


## Implement Authentication

생성한 두 모달은 로그인, 회원가입의 용도로 사용할 것이다. 하지만 그전에 어떻게 auth endpoint에 http request 를 보내는지 알아보자.

만약 회원가입을 하는 경우라면
1. 사용자가 입력한 input 값이 유효성이 올바른지 확인
2. 입력한 account 값이 중복된 account인지 확인
3. 비밀번호를 hash
4. DB에 user 정보를 저장
5. JWT를 생성
6. JWT를 클라이언트에게 전달


### 0. Createing an Endpoint
위의 6가지를 진행하기 전에 먼저 `page/api/auth/signup.ts` 파일을 생성해서 signup을 할때 서버에게 request를 보내는 파일을 만들어보자.

```ts
import { NextApiRequest, NextApiResponse } from 'next'

// making sinup handler
export default async function handler(req: NextApiRequest, res: NextApiResponse) {
  res.status(200)
  res.json({ hi: 'welcome to hell' })
}

```

이런식으로 작성을 해놓고 메인 페이지에서 `api/auth/signup`을 입력하면 
![](https://velog.velcdn.com/images/jhs000123/post/9340ac9b-90d0-401d-8cf2-63432cb552db/image.png)

요런 식으로 json을 볼수 있다.



물론 postman으로 테스트를 해보면 get메소드로 요청하던, post 메소드로 요청하던 전부 같은 응답이 나오기 때문에 이렇게만 하면 좋은 방법은 아니다. 
따라서 req의 method에 따라 조건부로 요청을 보내게끔 작성하는 것이다.
```tsx
import { NextApiRequest, NextApiResponse } from 'next'

// making sinup handler
export default async function handler(req: NextApiRequest, res: NextApiResponse) {
  res.status(200)
  if (req.method === 'POST') {
    res.json({ hi: 'welcome to hell' })
  }
  res.json({ no: 'you are not post' })
}

```

### 1. 사용자가 입력한 input 값이 유효성이 올바른지 확인
postman 에서 입력한 body값을 request 데이터로 지정하고 request를 보낼때 유효한 데이터가 맞는지 아닌지 확인하는 과정을 구현한다.
이를 위해서 validator js 라는 라이브러리를 설치해서 구현하는데, 각각의 데이터 형식에 따른 유효성 검사 기능이 존재했다(진작 이거 쓸걸)

```ts
import { NextApiRequest, NextApiResponse } from 'next'
import validator from 'validator'
// making sinup handler
export default async function handler(req: NextApiRequest, res: NextApiResponse) {
  res.status(200)
  if (req.method === 'POST') {
    const { firstName, lastName, email, phone, city, password } = req.body
    const errors: string[] = []
    const validatationSchema = [
      {
        valid: validator.isLength(firstName, { min: 1, max: 20 }),
        errorMessage: 'First name is Invalid',
      },
      {
        valid: validator.isLength(lastName, { min: 1, max: 20 }),
        errorMessage: 'First name is Invalid',
      },
      {
        valid: validator.isEmail(email),
        errorMessage: 'Email is Invalid',
      },
      {
        valid: validator.isMobilePhone(phone),
        errorMessage: 'Phone is Invalid',
      },
      {
        valid: validator.isLength(city, { min: 1 }),
        errorMessage: 'City is Invalid',
      },
      {
        valid: validator.isStrongPassword(password),
        errorMessage: 'Password is Invalid',
      },
    ]

    validatationSchema.forEach((check) => {
      if (!check.valid) {
        errors.push(check.errorMessage)
      }
    })
    if (errors.length) {
      return res.status(400).json({ errorMessage: errors[0] })
    }
    res.json({ hi: 'body' })
  }
  res.json({ no: 'you are not post' })
}

```

위와 같이 `validationSchema`를 생성해서 각각의 데이터 항목마다 유효성 검사를 생성하여 `forEach`를 통해서 valid 값이 `false`일 경우 `errors`배열에 push를 해서 모든 오류를 담아놓게 된다. 지금같은 경우는 return 구문에서 error의 첫번째 항목만 가져오게 구현했으므로, 아마 한개만 나타날 것이다. 현재 테스트 요청 데이터는 아래와 같은데

```
{
    "firstName" : "Laith",
    "lastName" : "Harb",
    "email" : "lath@gmail.com",
    "phone" : "1112222222",
    "city" : "toronto",
    "password":"password"
}
```

이걸 request 데이터로 요청을 보내면
![](https://velog.velcdn.com/images/jhs000123/post/3ef6559e-3bf9-40ee-b059-c8add3aa8f50/image.png)

이렇게 비밀번호의 형식이 올바르지 않다고 오류가 나오게 된다.


### 2. 입력한 account 값이 중복된 account인지 확인

만약 중복된 이메일인지 확인하고 싶다면 이전에 작성한 prisma 스키마에서 USER 테이블에 email을 unique 속성을 부여한다

```json
model User {
  id         Int      @id @default(autoincrement())
  first_name String
  last_name  String
  city       String
  email      String @unique
  password   String
  phone      String
  review     Review[]
  created_at DateTime @default(now())
  updated_at DateTime @updatedAt
}

```
그리고 `signup.ts`에서 `prismaClient`를 통해서 사용자가 입력한 email과 같은 email을 가지고 있는 userdata를 가져왔을때 존재한다면 오류를 보내게 설정할수 있다

```ts
    const userWithEmail = await prisma.user.findUnique({
      where: {
        email,
      },
    })

    if (userWithEmail) {
      return res.status(400).json({ errorMessage: 'Email is associated with another account' })
    }
```

이렇게 해주면 만약 사용자가 데이터베이스에 존재하는 email을 입력해서 전송을 누르면 오류 메시지를 전송한다.

### 3. 비밀번호를 hash

bcrypt를 사용해서 비밀번호를 hashing 한다

```ts
  const hashedPassword = await bcrypt.hash(password, 10)

    res.status(200).json({ hi: hashedPassword })
```

bcrypt에서 지원하는 hash 메소드를 통해서 비밀번호를 해싱한다. 이때 2번째 파라미터는 `salt`로 비밀번호에다가 2번째 파라미터의 숫자만큼 내용이 추가된 상태에서 hashing이 진행된다!

![](https://velog.velcdn.com/images/jhs000123/post/be069b82-399a-4f12-851c-5629bbc8090a/image.png)


### 4. DB에 USER를 저장

prisma를 사용해서 간단하게 구현할 수 있다. 테이블을 선택해서 create메소드를 호출하면 된다.

```tsx
const user = await prisma.user.create({
      data: {
        first_name: firstName,
        last_name: lastName,
        email,
        phone,
        city,
        password: hashedPassword,
      },
    })

    res.status(200).json({ hi: user })
```
![](https://velog.velcdn.com/images/jhs000123/post/02693305-1ee7-4a24-b914-d8d37ae6ce9f/image.png)


### 5. JWT를 생성
- `JWT` : 인터넷 상에서 정보를 안전하게 전송하기 위해 사용되는 인증 및 권한 부여 프로토콜

헤더, 클레임, 서명 이렇게 3가지로 구성되어있다.

jwt는 주로 사용자 인증 및 인가에 사용되는데,로그인한 사용자에 대해 jwt를 발급하고, 클라이언트는 이를  사용하여 자원에 접근할 수 있다. 


jose 라이브러리를 사용해서 jwt를 생성하기로 한다.

```ts
    const alg = 'HS256'

    const secret = new TextEncoder().encode(process.env.JWT_SECRET)


  const token = await new jose.SignJWT({ email: user.email })
      .setProtectedHeader({ alg })
      .setExpirationTime('24h')
      .sign(secret)
```
SignJWT로 사용자가 입력한 email을 저장하고 hs256 알고리즘을 바탕으로 protectedHeader를 설정한 다음 유효기간까지 설정해준다.

여기서 secret은 .env에서 JWT_SECRET이름으로 선언한 데이터를 사용한다. 

이렇게 코드를 추가하고 res.json의 내용을 token으로 해서 제대로 발급되어있는지 확인해보면

![](https://velog.velcdn.com/images/jhs000123/post/5e09cefd-6e29-456b-bfd0-acdfb485ed72/image.png)

이렇게 토큰이 생성된 것을 확인할수 있다. 

https://jwt.io/ 이 페이지를 들어가보면 jwt의 기본적인 양식을 보여주는데 여기서 Verify Signature 항목에서 `your-256-bit-secret`대신 `.env`에 설정한 내용을 작성하면 verifed가 확인된다. 

![](https://velog.velcdn.com/images/jhs000123/post/399a2529-a936-4b4a-a862-9018a125d84c/image.png)



### 6. JWT를 클라이언트에게 전달

signup은 이제 진행된것 같으니 이제 sign in을 하면 JWT를 클라이언트에게 전달하는 것도 할수 있어야한다. 


로그인을 통해서 JWT를 클라이언트에게 전달하는 과정은
1. 사용자의 input의 유효성 검사
```ts
import { NextApiRequest, NextApiResponse } from 'next'
import validator from 'validator'

export default async function handler(req: NextApiRequest, res: NextApiResponse) {
  if (req.method === 'POST') {
    const errors: string[] = []
    const { email, password } = req.body

    const validationSchema = [
      {
        valid: validator.isEmail(email),
        errorMessage: 'Email is Invalid',
      },
      {
        valid: validator.isLength(password, { min: 1 }),
        errorMessage: 'Password is Invalid',
      },
    ]

    validationSchema.forEach((check) => {
      if (!check.valid) {
        errors.push(check.errorMessage)
      }
    })

    if (errors.length) {
      return res.status(400).json({ errorMessage: errors[0] })
    }
  }
  return res.status(404).json('Unkown endpoint')
}

```

2. 해당 input과 일치하는 user가 있는지 확인
```ts
    const userWithEmail = await prisma.user.findUnique({
      where: {
        email,
      },
    })

    if (!userWithEmail) {
      return res.status(401).json({ errorMessage: 'Email or Password is invalid' })
    }
```

3. 해싱된 비밀번호 확인
```ts
 const isMatch = await bcrypt.compare(password, userWithEmail.password)

    if (!isMatch) {
      return res.status(401).json({ errorMessage: 'Email or Password is invalid' })
    }
```

4. JWT 생성후 사용자에게 전달

```ts
   const alg = 'HS256'

    const secret = new TextEncoder().encode(process.env.JWT_SECRET)

    const token = await new jose.SignJWT({
      email: userWithEmail.email,
    })
      .setProtectedHeader({ alg })
      .setExpirationTime('24h')
      .sign(secret)

    return res.status(200).json({ your_token: token })
```


이제 postman에서 테스트 해보면 토큰값을 보내주는 것을 볼수 있다

![](https://velog.velcdn.com/images/jhs000123/post/989d53dc-d901-4058-9092-e5bee5825716/image.png)




## JWT로 유저 인식하기

만약 사용자가 로그인을 했을 때, 전달 받는 JWT로 어떻게 유저를 인식할수 있을까?

1. 헤더로부터 token값을 가져오기
사용자가 요청을 보낼때 헤더에 token 값을 집어넣어서 서버측에서 해당 token을 사용해야한다
```ts
 const bearerToken = req.headers['authorization'] as string
  // 토큰이 없다?
  if (!bearerToken) {
    res.status(401).json({ errorMessage: 'Unauthorized(no bearer token)' })
  }

  // 토큰에서 bearer를 분리한다.
  const token = bearerToken.split(' ')[1]

  // 분리한 토큰이 없다?
  if (!token) {
    res.status(401).json({ errorMessage: 'Unauthorized(no token)' })
  }
```

2. token 유효성 검사

토큰이 존재하는지, 전부 가져온 것인지, 유효한 토큰인지를 검사
```tsx
// 토큰을 검증한다.
  const secret = new TextEncoder().encode(process.env.JWT_SECRET)

  try {
    await jose.jwtVerify(token, secret)
  } catch (err) {
    res.status(401).json({ errorMessage: 'Unauthorized(invalid token)' })
  }
```

3. 토큰 복호화

유효성 검사를 통과했다면 토큰 복호화를 진행한다. 여기서는 jose 대신 jsonwebtoken를 사용한다 (서버사이드 렌더링 관련 이슈때문) jsonwebtoken는 복화를 용도로만 사용한다.

```ts
 // 토큰 복호화
  const payload = jwt.decode(token) as { email: string }

```


4. 데이터베이스로부터 사용자 정보 가져오기

```ts
// email 이 없으면?
  if (!payload.email) {
    res.status(401).json({ errorMessage: 'Unauthorized(invalid token)' })
  }

  const user = await prisma.user.findUnique({
    where: {
      email: payload.email,
    },
    // password를 제외한 모든 필드를 가져온다.
    select: {
      id: true,
      first_name: true,
      last_name: true,
      email: true,
      city: true,
      phone: true,
    },
  })

  return res.json({ user })
```

![](https://velog.velcdn.com/images/jhs000123/post/07c2550c-00af-4a8e-b7f5-327ce0c3eb02/image.png)

5. 완료!



## 미들 웨어의 필요성

이제 토큰가져오고 복호화해서 사용자에게 데이터를 전달하는 기능을 구현했지만, 이 모든 내용들이 단 하나의 엔드 포인트에 들어있다. 

만약에 여러개의 엔드 포인트가 존재한다면 이들 하나하나에 토큰을 가져오고 복호화하는 코드를 하나하나 작성하는것이 효율적이지 않을 뿐더러 보안상에서도 좋은 방법은 아니다.
따라서 token을 가지고 엔드 포인트쪽으로 오기 전에 복호화및 여러 프로세스를 진행해줄수 있는 미들웨어를 생성하는 것이 좋다.


next.js 에 존재하는 middleware를 사용한다.
루트 경로에 `middleware.ts`를 생성한다 이름은 반드시 middleware로 해줘야 next.js에서 인식한다.

```ts
import { NextRequest, NextResponse } from 'next/server'

// 미들웨어 생성
export async function middleware(req: NextRequest, res: NextResponse) {
  console.log('나는 미들웨어에요, 엔드포인트가 듣기 전에 제가 먼저 실행돼요. ')
}

```

이렇게 해주고 postman에서 test를 해주면 
![](https://velog.velcdn.com/images/jhs000123/post/05791277-f940-4aa6-aa9a-3a8adf59c0da/image.png)

서버 콘솔에 텍스트가 나타난다!

그럼 이 미들웨어에서 엔드포인트에 접근하기전 수행할 작업들을 작성해주면 된다.

```ts
import { NextRequest, NextResponse } from 'next/server'
import * as jose from 'jose'

// 미들웨어 생성
export async function middleware(req: NextRequest, res: NextResponse) {
  const bearerToken = req.headers.get('authorization') as string
  // 토큰이 없다?
  if (!bearerToken) {
    return new NextResponse(JSON.stringify({ errorMessage: 'Unauthorized request' }), {
      status: 401,
    })
  }

  // 토큰에서 bearer를 분리한다.
  const token = bearerToken.split(' ')[1]

  // 분리한 토큰이 없다?
  if (!token) {
    return new NextResponse(JSON.stringify({ errorMessage: 'Unauthorized request' }), {
      status: 401,
    })
  }

  // 토큰을 검증한다.
  const secret = new TextEncoder().encode(process.env.JWT_SECRET)

  try {
    await jose.jwtVerify(token, secret)
  } catch (err) {
    return new NextResponse(JSON.stringify({ errorMessage: 'Unauthorized request' }), {
      status: 401,
    })
  }
}
export const config = {
  matcher: ['/api/auth/me'],
}

```

1. 토큰 가져오기
2. 토큰 분리하기
3. 토큰 검증하기

이 3가지의 작업을 어떤 엔드포인트에서 호출해도 미리 작업해주는 것이다. 단! 만약 원하는 엔드포인트에서만 이 미들웨어가 작동하기를 원한다면 맨 마지막에 있는 `config`처럼 matcher 배열에 원하는 엔드포인트만 작성해주면 해당 url에서만 동작한다.
![](https://velog.velcdn.com/images/jhs000123/post/04cb513c-1bcf-4bc2-a83d-be64eef90cf2/image.png)


