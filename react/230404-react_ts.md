# react/ts

## TypeScript

### 1) 타입스크립트 개념 및 설정

타입스크립트는 기본적인 문법은 자바스크립트와 비슷하지만, 기존의 자바스크립트와 다르게 타입을 지정하면서 변수/함수를 선언한다는 특징을 가지고 있다.

타입을 지정하면,

1. 타입의 차이에 따른 서비스의 오작동 방지
2. 개발 과정에서의 정확한 코드 작성 (타입에 맞춰서 작성하지 않으면 에러가 생긴다)
3. 개발자 간에 각 함수/변수의 정확한 목적 전달

일단 해보자

> yarn careate vite (앱이름) --template react-ts


자바스크립트 때와는 다르게, 아래와 같이 tsconfig.json 파일이 생겨있는데,

이 파일 안에서, 타입스크립트의 사용과 관련한 규칙 설정이 가능하다.

이 외에도 리턴값의 타입이 정해져있지 않다면, any 라는 타입을 사용할 수도 있다. (그렇지만 그럴거면 타입스크립트를 쓰는 이유가 없지 않음?)

기본 설정 내용이 어떤 내용인지 한번 확인해보자.

```json
{
  "compilerOptions": {
    "target": "ESNext", // 어떤 버전의 Js 문법으로 컴파일할지 결정
    "useDefineForClassFields": true, // define property 를 활용해서 class field 를 정의
    "lib": ["DOM", "DOM.Iterable", "ESNext"], // 포함될 라이브러리 목록 (비유를 하자면, 정확히 어떤 자바스크립트 내장 문법/기능들을 포함시킬지를 지정)
    "allowJs": false, // js 파일을 ts 에서 import해와서 쓸 수 있는지
    "skipLibCheck": true, // library 파일에 대해서 타입 체크 스킵
    "esModuleInterop": false, // 모듈 import 방식 변환에 대한 설정
    "allowSyntheticDefaultImports": true, // import 호환 설정
    "strict": true, // strict 모드 활성화
    "forceConsistentCasingInFileNames": true, // 파일 이름에 대한 대소문자 구별을 강제
    "module": "ESNext", // 어떤 import 문법을 사용할지
    "moduleResolution": "Node", // 모듈 해석 전략을 결정
    "resolveJsonModule": true, // 확장자가 json 인 모듈의 import 를 허용
    "isolatedModules": true, // 각 파일을 모듈로 분리 생성
    "noEmit": true, // 컴파일러가 js 등 출력 파일을 만들어 내지 않도록 설정
    "jsx": "react-jsx" // tsx 파일을 jsx 로 컴파일하는 방식 지정
  },
  "include": ["src"],
  "references": [{ "path": "./tsconfig.node.json" }]
}
```
다양한 설정이 많지만, 우선은 기본 설정대로 해보자.

### 2) 타입 지정하기!
src 폴더 안에, prc.ts를 만들어서, 타입 스크립트와 관련한 간단한 문법연습을 진행해보자.

**컴포넌트를 만드는게 아니라, 정말로 그냥 ts파일을 만들어서 연습을 진행한다 **

```tsx
const hellowWorld: string = '뭘봐 세상아'
console.log(hellowWorld)

export {}

```
여기서 `: string` 부분이 바로 타입을 지정하는 부분이다.

기존에는 타입 지정없이 변수를 선언했지만, 타입스크립트에서는 이와 같이 타입을 지정하면서 변수를 선언한다.

만약 해당 변수에 할당값이 string 이 아니라면, 에러가 나타난다.

![](https://velog.velcdn.com/images/jhs000123/post/f3a9a4f7-139f-4223-bdd2-f98eb25eb9ed/image.png)

타입스크립트는 작성할 때만 타입을 지정하는 것이고, tsc라는 컴파일 명령어를 실행하면, 자동으로 우리가 아는 그 자바스크립트 언어로 변하게된다!( 즉, 기반은 자바스크립트이고, 작성하는 방식에서의 차이만 존재한다.


**[변수의 타입 지정]**
지정할수 있는 타입은 아래와 같다

```tsx
const strVar: string = 'hello world'
const numVar: number = 123
const boolVar: boolean = true
const numArray: number[] = [1, 2, 3] // 여기에 문자를 push 할 경우 (numArray.push('1')) 에러 발생
const strArray: string[] = ['hello', 'world'] // 여기에 숫자를 push 할 경우 (strArray.push(1)) 에러 발생
```

단순히 타입을 하나만 지정하는 것이 아니라, 아래처럼 두 가지 이상을 지정할 수도 있다.

```tsx
let stringOrUndefined: string | undefined = undefined; // 1 할당 시 에러 발생
let numberOrNull: number | null = null;
```


**[함수의 타입 지정]**

함수를 선언할 때는, 함수의 들어가는 파라미터의 타입과 함수가 리턴하는 값의 타입을 각각 지정해줘야한다.

기본 형태는 이렇다
```jsx

function 함수명(파라미터: 파라미터의 타입) : 리턴값의 타입
		{
		return 리턴 값
	}
```

예를 들어서 더하기 함수를 만들어본다고 해보자.

```tsx

function sum(a: number, b: number): number {
  return a + b
}

```
만약 리턴하는 값이 문자가 된다면 당연히 에러를 표시해줄 것이다.

만약 아무것도 리턴하지 않는 경우에는 void 라고 타입을 명시해주거나, 리턴부분에는 아무타입도 명시하지 않는 방식으로 작성하면 된다.


```tsx
function sum(x: number, y: number) {
    console.log('no result')
}
```


### 3) 타입 선언하기(type,interface)

**[interface를 통해서 객체의 타입 지정하기]**

interface 는 클래스/객체에 대해서 타입을 지정하고자 할때 쓰는 문법이다.
미리 특정한 객체의 내부 값들의 타입을 정의해놓고, 이를 불러와서 사용하는 것이다.
예들 들어서, 우리가 앞서 진행했던 Post데이터 객체에 대해서 타입을 지정해보자.

```json
interface Post {
    title: string;
    body: string;
    id: number;
}

const post: Post = {
    title: '제목',
    body: '내용',
    id: 1
};

```

만약에 여기서 title/body/id중에 하나라도 없으면 에러가 발생하고, 하나라도 타입이 다르다면 에러가 발생한다.

근데 예를 들어서 id같은 경우에는 없어도 될것 같다면 어떻게 하면 좋을까?

```json
interface Post {
    title: string;
    body: string;
    id?: number;
}

const post: Post = {
    title: '제목',
    body: '내용',
};
```
없어도 될것 같은 키값에다가 ?를 표시하면 된다.

만약에 작성자가 있는 게시글이 있고, 따로 작성자가 없는 게시글이 있다고 가정해보자. 작성자가 있는 게시글을 위해서 userpost interface를 지정해보자
```json
interface UserPost {
    title: string;
    body: string;
[O    id?: number;
    author: string;
}

const userPost: UserPost = {
    title: '제목',
    body: '내용',
    author: '작성자'
};
```

이미 Post 라는 인터페이스를 만들었는데 너무 같은 내용의 코드가 많은 것 같다.
이걸 더 줄일수 있을까?

```tsx
interface UserPost extends Post {
    author: string;
}

const userPost: UserPost = {
    title: '제목',
    body: '내용',
    author: '작성자'
};
```
extends 를 사용하면 이렇게 더 줄일수 있다!

만약에 객체 안에 함수가 이는 형태라면? 아래와 같이 작성해줄수 있다. 

```tsx
interface Post {
    title: string
    body: string
    id: number
    sayHello: (name: string) => string
}

const post: Post = {
    title: '제목',
    body: '내용',
    id: 1,
    sayHello: (name) => {
        return name
    },
}
```

**[type 을 통해서 객체의 타입 지정하기]**

type도 interface와 마찬가지로 객체의 타입을 지정하기위한 문법이다.

```tsx
type Post = {
    title: string;
    body: string;
    id?: number;
}

const post: Post = {
    title: '제목',
    body: '내용',
};

// & 를 통해서 두 개의 다른 타입을 합칠 수 있습니다!
type UserPost = Post & {
    author: string;
}

const userPost: UserPost = {
    title: '제목',
    body: '내용',
    author: '작성자'
};
```

아래처럼 작성해서, Post가 여러개 잇는 배열 형태의 타입을 만들 수도 있따.

```tsx
type Posts = Post[]
```

만약 객체 안에 함수가 있는 형태라면? 아래와 같이 작성해줄수 있다.

```tsx
type Post = {
    title: string;
    body: string;
    id?: number;
    sayHello: (name: string) => string;
}

const post: Post = {
    title: '제목',
    body: '내용',
    sayHello: (name) => {
        return name
    }
};
```

![](https://velog.velcdn.com/images/jhs000123/post/993fb26b-8dbc-4c62-ac4b-216c66dfd139/image.png)

<div align="center">둘다 똑같은데 그럼 interface랑 type 중에 뭘쓰라고?</div>

> 클래스와 관련된 타입 -> interface 사용
그냥 객체의 타입 -> type 사용 

라고는 하는데 보통 프로젝트 진행할때 컨벤션 정하면서 둘 중에 정확히 어떤 것을 쓸지 합의하게 되는 것같다. 즉 팀원과의 합의 하에 일관성을 유지할 수 있도록만 사용하면 된다는 뜻

타입스크립트 개발팀에서는 interface를 권장하고 있습니다만은, 실무에서 꼭 interface를 고집하기보다는 하나의 컨벤션으로 취급하는 경우가 많은 것 같다


### 4) Generics 활용하기
코드를 작성하다보면, 매개변수와 반환값의 타입을 정확히 지정할수 없는 경우도 발생한다. 특정한 함수가 숫자나 문자 둘중에 어떤 것도 리턴할수 있으면 어떻게 해야할까?

예를 들어서, 단순하게 유저한테 인풋값을 받아서 그 값을 출력하는 함수를 만든다고 생각해보자

```jsx
function GetInput(userInput) {
    return userInput;
}
```
자바스크립트면 이렇게 작성하고 끝이다

타입스크립트에서는 유저가 숫자를 입력할때 ,문자를 입력하는 경우에 대해서 나눠 작성해야하는 문제가 발생한다.

```tsx
function GetNum(userInput: number): number {
    return userInput;
}

function GetStr(userInput: string): string {
    return userInput;
}
```

그렇다고 any 타입을 써서 아래처럼 작성하면, 타입 스크립트를 쓰는 의미가 없다.

이런 경우를 대비해서 타입스크립트에는 Generics 라는 것이 있다!
`Generics`란 함수를 실행할때, 개발자가 `함수의 매개변수와 리턴값의 타입을 지정하면서 실행` 할수 있도록 해준다.

`Generics`는 다이아몬드 연산자를 활용해서 `<T>`라고 작성해주면 된다. 꼭 T라고 해야할 필요는 없지만 Type이라는 의미를 표현하기 위해서 그 약자로 T라고 작성하는 것이 관습이다.
    
```tsx
  function GetInput<T>(userInput: T): T {
    return userInput;
}
``` 
    
이제 함수를 정의했으니, 사용할 때는 저 T에 우리가 원하는 타입을 명시해주면 된다.
  
```tsx
const numInput = GetInput<number>(123);
const strInput = GetInput<string>('hello');
```

Generics 를 활용하면, 조금 더 개발자의 자유도를 높이면서도 타입 지정은 확실하게 할 수 있다는 점에서 큰 장점이 있다. 

## React 기본

### 1) props 활용해보기

원칙적으로 리액트의 Functional Component를 만들때에는 , React.FC라고 하는 타입을 지정해줘야한다. 그리고, props와 관련해서, 사용자가 어떤 타입을 props 로 내려줄지 모르기 때문에, Generics 형태로 React.FC 가 만들어져있다. 그래서! 그 props의 타입에 대해서는 Generics 형태로 지정해줘야한다!


```tsx
import React from 'react'

type MyProps = {
    name: string
    age: number
}

const PropsTest: React.FC<MyProps> = ({ name, age }) => (
    <div>
        Hello,{name}. Bye {age}.
    </div>
)

export default PropsTest

```


```tsx
import { useState } from 'react'
import PropsTest from './components/PropsTest'

function App() {
    const [count, setCount] = useState(0)

    return (
        <div>
            <PropsTest name="마라탕" age={20} />
        </div>
    )
}

export default App

```
![](https://velog.velcdn.com/images/jhs000123/post/deba488d-5f7a-4e45-aaf3-727be94b7bff/image.png)

그네 React.FC 라고 하는 방식은 잘 사용되지 않는다.

- 사용하지 않는 것이 단순성과 일관성을 유지할수 있기 때문 ( 자바스크립트로 작성할 때와 크게 다르지 않은 형태로 컴포넌트를 작성할수 있다.)

- React.FC는 defaultprops 를 사용하지 못하도록 만든다.

- React.FC는 defaultprops를 사용하지 못하도록 만든다.

그래서 props를 명시할때는 아래처럼 사용하는 것이 좋다.

```tsx
import React from 'react'

type MyProps = {
    name: string
    age: number
}

function PropsTest({ name, age }: MyProps) {
    return (
        <div>
            <h1>PropsTest</h1>
            <p>이름: {name}</p>
            <p>나이: {age}</p>
        </div>
    )
}

export default PropsTest

```

화살표 함수로 작성해도 된다.

```tsx
import React from 'react';

type MyProps = {
  name: string;
  age: number;
};

const PropsTest = ({ name, age }: MyProps) => {
	return (
    <div>Hello, {name}. Bye {age}.</div>
	);
};

export default PropsTest;
```

### 2) 카운터 만들기

```jsx
import React, { useState } from 'react'

function Counter() {
    const [count, setCount] = useState<number>(0)
    const onIncreate = () => setCount((prevState) => prevState + 1)
    const onDecreate = () => setCount((prevState) => prevState - 1)

    return (
        <div>
            <p>{count}</p>
            <button onClick={onIncreate}>증가</button>
            <button onClick={onDecreate}>감소</button>
        </div>
    )
}

export default Counter
```
 useState 를 사용하는 과정이 조금 다른데, useState 에 명시하는 초기값과 반환값의 타입은 얼마든지 달라질 수 있으니, 거기에 맞춰서 generics 형태로 useState 함수가 선언되어 있다. 그래서 우리는 `useState<number>`와 같은 형태로 사용해서, 타입을 지정하면서 useState 를 쓰면 되는 것이다!
  
  
사실 typescript 는 자동으로 타입을 유추하는 능력을 가지고 있다. 그래서 useState를 사용할 때 Generics를 사용하지 않아도 자동으로 타입을 지정해준다.
  
  > useState 사용시 Generics는 , 상태가 null일수도 있고 아닐 수도 있을 때 등 해당 상태가 여러 타입일수 있을 때 명시해주면 좋다
  

### 3) 인풋 받아보기 (onChange)
  
  ```tsx
  import React, { useState } from 'react'

function UserInput() {
    const [userInput, setUserInput] = useState({
        title: '',
        body: '',
    })
    const { title, body } = userInput
    const onChange = (e: React.ChangeEvent<HTMLInputElement>) => {
        const { name, value } = e.target
        setUserInput({ ...userInput, [name]: value })
    }
    return (
        <div>
            <input type="text" name="title" value={title} onChange={onChange} />
            <input type="text" name="body" value={body} onChange={onChange} />
        </div>
    )
}

export default UserInput

  ```
  이전에는 그냥 e만 하면 되지만, 이제는 `e: React.ChangeEvent<HTMLInputElement>` 라고 작성해야한다.
  
  정확히 htmlinputelement 타입을 받는 이벤트라고 말해주는 것이다.
  
  
## React 심화
  
  ### 1) useReducer
  
  useReducer를 사용할 때는 state의 타입, action객체의 타입을 명시해주는 부분이 추가된다.
  만약 dispatch 시에 함께 넘기는 data 가 있다면, 해당 data의 타입 또한 명시해줘야한다.

  `reducer/counterReducer.ts`
```tsx
  
  type Data = { // 전달하는 데이터가 객체 형태라면 타입 생성 필요
    diff: number
}
type State = number // state 의 타입을 명시 (객체가 아니므로, 굳이 타입을 만들지 않고 reducer 에 바로 number 라고 작성해도 무방)
type Action = // 아래처럼 action 객체에 포함되는 type 과 data 의 타입을 | 로 차례차례 명시
    | { type: 'INCREASE' } 
    | { type: 'DECREASE' }
    | { type: 'INCREASE_BY_DIFF'; data: Data }
    | { type: 'DECREASE_BY_DIFF'; data: Data }

// state, action 각각의 타입과, 리턴하는 값의 타입을 명시 (리턴값이 객체가 아니라 원시형 자료라면 그냥 해당 자료형을 바로 작성해줘도 무방)
export function counterReducer(state: State, action: Action): State {
    switch (action.type) {
        case 'INCREASE':
            return state + 1
        case 'DECREASE':
            return state - 1
        case 'INCREASE_BY_DIFF':
            return state + action.data.diff // Data 타입에 diff 가 있어야만 작성 가능
        case 'DECREASE_BY_DIFF':
            return state - action.data.diff
        default:
            throw new Error('Unhandled action')
    }
}
  ```
  
  `components/Counter.tsx`
  ```tsx
  import React, { useReducer } from 'react'
import { counterReducer } from '../reducers/counterReducer'

function Counter() {
    const [count, dispatch] = useReducer(counterReducer, 0)
    const onIncrease = () => dispatch({ type: 'INCREASE' })
    const onDecrease = () => dispatch({ type: 'DECREASE' })
    const onIncreaseByDiff = () => dispatch({ type: 'INCREASE_BY_DIFF', data: { diff: 2 } })
    const onDecreaseByDiff = () => dispatch({ type: 'DECREASE_BY_DIFF', data: { diff: 2 } })

    return (
        <div>
            <p>{count}</p>
            <button onClick={onDecrease}>-1</button>
            <button onClick={onIncrease}>+1</button>
            <button onClick={onDecreaseByDiff}>-2</button>
            <button onClick={onIncreaseByDiff}>+2</button>
        </div>
    )
}

export default Counter
  ```
  
  
  ### 2) useContext
  
  useContext는 기존과 다르게,
  - State를 위한 Context 하나 (우리가 쓰는 리듀서의 State 타입을 generic으로 지정)
  - Dispatch를 위한 Context 하나 (우리가 쓰는 리듀서의 Action을 활용하여 만든 Dispatch타입을 generic으로 지정)
  
  바로 Context를 분리해서 작성해줄텐데, 그에 앞서서 reducer에 작성한 타입들을 export 해준다. (export 해야 해당 타입을 가져와서 그에 맞는 Context를 작성해줄수 있기 때문)
  
  `reducers/counterReducer.ts`
  
```tsx
type Data = {
    //전달하려는 데이터가 객체 형태라면 타입생성 필요함
    diff: number
}

export type CounterState = number // state의 타입을 명시 (객체가 아니니까 굳이 타입을 만들지 않고 reducer에 바로 number라고 작성해도 무방)

export type CounterAction = { type: 'INCREASE' } | { type: 'DECREASE' } | { type: 'INCREASE_BY_DIFF'; data: Data } | { type: 'DECREASE_BY_DIFF'; data: Data }

// state, action 각각의 타입과 ,리턴하는 값의 타입을 명시 (리턴값이 객체가 아니라 원시형 자료라면 그냥 해당 자료형을 바로 작성해줘도 무방)
export function counterReducer(state: CounterState, action: CounterAction): CounterState {
    switch (action.type) {
        case 'INCREASE':
            return state + 1
        case 'DECREASE':
            return state - 1
        case 'INCREASE_BY_DIFF':
            return state + action.data.diff
        case 'DECREASE_BY_DIFF':
            return state - action.data.diff
        default:
            throw new Error('Unhandled action')
    }
}
  ```
  
  `contexts/CounterContext.tsx`(state Context, dispatch Context 각각 생성)
  
  ```tsx
  import { Dispatch, createContext, useReducer } from 'react'
import { CounterAction, CounterState, counterReducer } from '../reducers/counterReducer'

// children을 props 로 받을 수 있도록 props 타입을 작성
type CounterProviderProps = {
    children: React.ReactNode
}

// 작성한 카운터 액션 객체를 받는 Dispatch함수 타입

type CounterDispatch = Dispatch<CounterAction>

//  작성한 카운터 state을 받는 context를 생성
export const CounterStateContext = createContext<CounterState | null>(null)

// 작성한 카운터 dispatch를 받는 context를 생성
export const CounterDispatchContext = createContext<CounterDispatch | null>(null)

export function CounterProvider({ children }: CounterProviderProps) {
    const [state, dispatch] = useReducer(counterReducer, 0)
    return (
        <CounterStateContext.Provider value={state}>
            <CounterDispatchContext.Provider value={dispatch}>{children}</CounterDispatchContext.Provider>
        </CounterStateContext.Provider>
    )
}

  ```

  `aap.tsx` (위에 작성한 CounterProvider 사용)
  
  ```tsx
  import PropsTest from './components/PropsTest'
import Counter from './components/Counter'
import UserInput from './components/UserInput'
import { CounterProvider } from './contexts/CounterContext'

function App() {
    return (
        <CounterProvider>
            <Counter />
        </CounterProvider>
    )
}

export default App

  ```
  이제 Counter 에서 useContext를 통해서 사용할텐데, CounterContext를 수정한다. 
  `contexts/CounterContext.jsx` 에 아래의 내용을 추가한다.
  ```tsx


export function useCounterState() {
    const state = useContext(CounterStateContext)
   
}

export function useCounterDispatch() {
  const dispatch = useContext(CounterDispatchContext);
 
  return dispatch;
}
  ```
  
  `components/Counter.tsx`
  ```tsx
  import { useCounterDispatch, useCounterState } from '../contexts/CounterContext'

function Counter() {
    const count = useCounterState()
    const dispatch = useCounterDispatch()

    const onIncreate = () => {
        dispatch({ type: 'INCREASE' })
    }
    const onDecreate = () => {
        dispatch({ type: 'DECREASE' })
    }
    const onIncreaseByDiff = () => {
        dispatch({ type: 'INCREASE_BY_DIFF', data: { diff: 2 } })
    }
    const onDecreaseByDiff = () => {
        dispatch({ type: 'DECREASE_BY_DIFF', data: { diff: 2 } })
    }

    return (
        <div>
            <p>{count}</p>
            <button onClick={onIncreate}>증가</button>
            <button onClick={onDecreate}>감소</button>
            <button onClick={onIncreaseByDiff}>+ 2 </button>
            <button onClick={onDecreaseByDiff}>- 2</button>
        </div>
    )
}

export default Counter
  ```
  
  ### 커스텀 훅을 만들지 않고 작성하는 방법(초기값 지정해주기)
  createContext를 할 때 초기값을 null로 설정했기 때문에, null 체킹을 위해서 커스텀 훅을 만들어서 사용했다.
  
  만약 이전처럼 컴포넌트에서 바로 useContext를 사용하고 싶다면, 아래처럼 초기값을 지정해주면 된다. 
  
  `CounterContext.tsx`
  ```tsx
  //  작성한 카운터 state을 받는 context를 생성
export const CounterStateContext = createContext<CounterState>(0)

// 작성한 카운터 dispatch를 받는 context를 생성
export const CounterDispatchContext = createContext<CounterDispatch>(() => null)
  ```
  
  `Counter.tsx`
  ```tsx
  import React, { useContext } from 'react'
import { CounterStateContext, CounterDispatchContext } from '../contexts/CounterContext'

function Counter() {
    const count = useContext(CounterStateContext)
    const dispatch = useContext(CounterDispatchContext)
    const onIncrease = () => dispatch({ type: 'INCREASE' })
    const onDecrease = () => dispatch({ type: 'DECREASE' })
    const onIncreaseByDiff = () => dispatch({ type: 'INCREASE_BY_DIFF', data: { diff: 2 } })
    const onDecreaseByDiff = () => dispatch({ type: 'DECREASE_BY_DIFF', data: { diff: 2 } })

    return (
        <div>
            <p>{count}</p>
            <button onClick={onDecrease}>-1</button>
            <button onClick={onIncrease}>+1</button>
            <button onClick={onDecreaseByDiff}>-2</button>
            <button onClick={onIncreaseByDiff}>+2</button>
        </div>
    )
}

export default Counter
  ```
  
  ## Reduc와 Typescript
  
  ### 1) Redux 설치와 TypesScript추가 개념
  
  기존과 다른 점이 있다면 `@tyles/react-redux`라는 타입스크립트 지원 리덕스 라이브러리를 추가로 설치해줘야 한다는 것이다 (@types가 존재하는지는 npm에 검색해보면 알수 있다.)
  > yarn add redux react-redux
  > yarn add @types/react-redux
  
  
  - as const
  특정 값에 대해서 그 값 자체를 타입으로 지정하고 싶을 때 사용하는 문법이다
  
```tsx
  
const str = 'increase'
const strAsConst = 'increase' as const

const obj = {
    str,
}
const objAsConst = {
    strAsConst,
}

export {}
  ```
  
  그렇게 하고 각각의 객체 위에 마우스를 대보면 어떤 타입을 추론하고 있는지를 확인할수 있다.
  
  ![](https://velog.velcdn.com/images/jhs000123/post/28fc345f-6466-41fd-89e3-2d5febc78750/image.png)

  ![](https://velog.velcdn.com/images/jhs000123/post/a0207831-b151-41de-841a-fde6eab610c4/image.png)

`as const`를 쓴 경우, string 이 아니라 해당 문자열 그 자체로 타입이 지정된 것을 확인할수 있다.
  
  - ReturnType
  `ReturnType` 은 특정한 타입에 대해서 return 값의 타입만 가져오는 문법이다.
 
 
 
```tsx

type str = ReturnType<() => string> // type str = string
type obj = ReturnType<() => object> // type obj = object

const user = () => {
    return {
        name: 'Kim',
        age: 20,
    }
}
// typeof user -> () => {name: string, age: number}
// type user -> {name: string, age: number}
type user = ReturnType<typeof user>


```

ReturnType 뒤의 다이아몬드 연산자에는 타입을 명시해주면 된다.

그러면 명시된 타입의 return값 부분의 타입만을 가져오게 된다.

as const 와 returntype을 동시에 써보면 아래와 같이 쓸수 있다.

```tsx

const KIM = 'Kim' as const

const user = () => {
    return {
        name: KIM,
        age: 20,
    }
}
// typeof user -> () => {name: 'Kim', age: number}
// type user -> {name: 'Kim', age: number}
type user = ReturnType<typeof user>
```

name 부분의 타입이 이제 string이 아니라, asconst를 작성해준 그 값 자체로 지정된 것을 확인할수 있다.


### 2) ducks 패턴으로 카운터 만들기

이전에 만든 카운터를 redux를 활용해서 작성해보자.
`modules/counter.ts`
```tsx

// 액션 타입

const INCREASE = 'counter/INCREASE' as const
const DECREASE = 'counter/DECREASE' as const
const INCREASE_BY_DIFF = 'counter/INCREASE_BY_DIFF' as const
const DECREASE_BY_DIFF = 'counter/DECREASE_BY_DIFF' as const

// 액션 객체 생성 함수 (대부분 기본과 동일하짐나, 인자 부분에 타입 명시가 필요하다.)

export function increase() {
    return {
        type: INCREASE,
    }
}

export function decrease() {
    return {
        type: DECREASE,
    }
}

export function increaseByDiff(diff: number) {
    return {
        type: INCREASE_BY_DIFF,
        payload: { diff },
    }
}

export function decreaseByDiff(diff: number) {
    return {
        type: DECREASE_BY_DIFF,
        payload: { diff },
    }
}

//action 객체들의 타입을 선언
// increase 등의 액션 객체 생성 함수가 리턴하는 값의 타입으로 지정
type CounterAction = ReturnType<typeof increase> | ReturnType<typeof decrease> | ReturnType<typeof increaseByDiff> | ReturnType<typeof decreaseByDiff>

// state 의 타입을 선언

type CounterState = {
    count: number
}

//초기값 (앞서 만든 state 타입 지정 필요)

const initialState: CounterState = {
    count: 0,
}

// 리듀서 작성 (앞서 만든 state, action 타입 지정, 리턴 타입도 state 타입으로 지정 )

export default function counter(state: CounterState = initialState, action: CounterAction): CounterState {
    switch (action.type) {
        case INCREASE:
            return { count: state.count + 1 }
        case DECREASE:
            return { count: state.count - 1 }
        case INCREASE_BY_DIFF:
            return { count: state.count + action.payload.diff }
        case DECREASE_BY_DIFF:
            return { count: state.count - action.payload.diff }
        default:
            return state
    }
}

```
`moudles/index.ts`

```tsx
import { combineReducers, legacy_createStore as createStore } from 'redux'
import counter from './counter'

export const rootReducer = combineReducers({
    counter,
})

export type RootState = ReturnType<typeof rootReducer>

export const store = createStore(rootReducer)
```

RootState라는 타입을 만들어줘야 추후에 useSelector를 사용할 때 우리가 가져온 state가 정히 어떤 타입인지를 알려줄수 있다. 


`app.tsx`(Provider로 감사주기)
```tsx

import { Provider } from 'react-redux'
import Counter from './components/Counter'
import { store } from './modules'

function App() {
    return (
        <Provider store={store}>
            <Counter />
        </Provider>
    )
}

export default App
```

`components/Counter.tsx` (useSelector와 useDispatch 사용)

```tsx
import { useSelector, useDispatch } from 'react-redux'
import { RootState } from '../modules'
import { decrease, decreaseByDiff, increase, increaseByDiff } from '../modules/counter'

function Counter() {
    const count = useSelector((state: RootState) => state.counter.count)
    const dispatch = useDispatch()

    const onIncrease = () => {
        dispatch(increase())
    }
    const onDecrease = () => {
        dispatch(decrease())
    }
    const onIncreaseByDiff = (diff: number) => {
        dispatch(increaseByDiff(diff))
    }
    const onDecreaseByDiff = (diff: number) => {
        dispatch(decreaseByDiff(diff))
    }

    return (
        <div>
            <p>{count}</p>
            <button onClick={onDecrease}>-1</button>
            <button onClick={onIncrease}>+1</button>
            <button onClick={() => onDecreaseByDiff(2)}>-2</button>
            <button onClick={() => onIncreaseByDiff(2)}>+2</button>
        </div>
    )
}

export default Counter
```

useSelector 사용시에는 state의 타입을 RootState로 지정해주면 된다.

### 3) typesafe-actions 와 함께 사용해보기

`typesafe-actions`는 리덕스의 action, reducer 를 조금 더 간경한 코드로 작성할수 있도록 도와주는 라이브러리이다.

해당 라이브러리를 활용해서 우리는 우리가 작성한 코드를 조금 더 간경하게 바꿀 수 있다.

> yarn add typesafe-actions


수정할 파일은 
`modules/counter.ts`이다.

```tsx
import { createAction, ActionType, createReducer } from 'typesafe-actions'

// 액션 타입 (as const 필요없음)
const INCREASE = 'counter/INCREASE'
const DECREASE = 'counter/DECREASE'
const INCREASE_BY_DIFF = 'counter/INCREASE_BY_DIFF'
const DECREASE_BY_DIFF = 'counter/DECREASE_BY_DIFF'

// 액션 객체 생성 (createAction 활용)
export const increase = createAction(INCREASE)()
export const decrease = createAction(DECREASE)()
// createAction(액션 type, 액션 payload) 형태로 명시
// 액션 payload 부분은 (필요한 인자) => (payload로 쓸 값) 형태로 작성 
export const increaseByDiff = createAction(INCREASE_BY_DIFF, (diff: number) => ({ diff }))()
export const decreaseByDiff = createAction(DECREASE_BY_DIFF, (diff: number) => ({ diff }))()

// 액션 객체들을 하나의 객체로 만들고,
const actions = { increase, decrease, increaseByDiff, decreaseByDiff }
// ActionType 을 활용해서 action 타입을 한번에 선언
type CounterAction = ActionType<typeof actions>

type CounterState = {
    count: number
}

const initialState: CounterState = {
    count: 0,
}

// 리듀서 선언 (createReducer 활용)
// createReducer<state타입, action타입>(초기값, 리듀서 객체) 형태로 작성
const counter = createReducer<CounterState, CounterAction>(initialState, {
    [INCREASE]: (state) => ({ count: state.count + 1 }),
    [DECREASE]: (state) => ({ count: state.count - 1 }),
    [INCREASE_BY_DIFF]: (state, action) => ({ count: state.count + action.payload.diff }),
    [DECREASE_BY_DIFF]: (state, action) => ({ count: state.count - action.payload.diff }),
})

export default counter
```

훨씬 깔끔하다!


