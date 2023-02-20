


# Typescript 

## 타입스크립트 구성
`tsconfig.json` 파일을 사용해서 타입스크립트 컴파일러가 프로젝트를 JS로 변환하는 방법을 지정한다.

```json
{
  "compilerOptions": {},
  "files": ["node_modules/my-library/index.ts"],
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"],
  "extends": "config/base.json"
}
```

옵션 | 우선순위 | 설명
---|---|---
compilerOptions | - | 컴파일러 옵션 지정
files | 1 | 컴파일할 개별 파일 목록(확장자 필수!)Configuration
exclude | 2 | 컴파일에서 제외할 파일 경로 목록
include | 3 | 컴파일할 파일 경로 목록<br/>(`.ts`, `.tsx`, `.d.ts` 확장자 생략 가능,<br/>`allowJS: true`인 경우 `.js`, `.jsx` 추가)
extends | - | 상속할 다른 TS 구성

 
 
## 컴파일러 옵션

```json
{
  "compilerOptions": {
    "target": "ES2015",
    "module": "ESNext",
    "moduleResolution": "Node",
    "jsx": "preserve",
    "strict": true,
    "esModuleInterop": true,
    "lib": ["ESNext", "DOM"]
  }
}
```

옵션 | 기본값 | 설명 | 허용 값 예시
---|---|---|---
target | `"ES3"` | 컴파일될 ES(JS) 버전 명시, `"ES2015"`(ES6)를 권장(대부분의 브라우저 지원) | `"ES3"`, `"ES5"`, `"ES2015"`, `"ESNext"` 
lib | - | 컴파일에서 사용할 라이브러리 지정 | `"ESNext"`, `"DOM"`
module | `"CommonJS"` | 모듈 시스템 지정 | `"CommonJS"`, `"AMD"`, `"ESNext"`  
moduleResolution | `"Node"` | 모듈 해석 방식 지정 | `"Classic"`, `"Node"`
esModuleInterop | `false` | ESM 모듈 방식 호환성 활성화 |
isolatedModules | `false` | 모든 파일을 모듈로 컴파일, `import` 혹은 `export`가 없는 경우 에러 |
allowSyntheticDefaultImports | `true` | 기본 내보내기(`export default`)가 없는 모듈에서 기본 가져오기 가능 |
baseUrl | - | 모듈 해석에 사용할 기준 경로 지정 | `"./"`, `"./project/"`
paths | - | 모듈 해석에 사용할 경로 별칭 지정 | `{"~/*": ["./src/*"]}`
types | - | 컴파일러가 참조할 @types 패키지의 목록을 지정, 명시되지 않으면 무시 | `["lodash", "node", "jest"]`
typeRoots | `["./node_modules/@types"]` | 컴파일러가 참조할 타입 선언(d.ts)의 경로 목록을 지정, 명시되지 않으면 무시 | `["./types", "./node_modules/@types"]`
allowJS | `false` | JS 파일 컴파일 허용 |
checkJs | `false` | `.js` 파일의 오류 보고 여부, `allowJS`가 `true`일 때만 유효 |
jsx | `"preserve"` | JSX 지정 | `"preserve"`, `"react"`
declaration | `true` | 모든 TS(JS) 파일의의 d.ts 파일 생성 여부 |
sourceMap | `false` | 소스맵 파일 생성 여부, 디버깅 도구 등에서 JS 파일의 원본 TS 파일 표시 가능 |
strict | `false` | 더 엄격한 타입 검사 활성화 |
noImplicitAny | `false` | 암시적 `any` 타입 검사 활성화 |
noImplicitThis | `false` | 암시적 `this` 타입 검사 활성화 |
strictNullChecks | `false` | 엄격한 `null`, `undefined` 타입 검사 활성화 |
strictFunctionTypes | `false` | 엄격한 함수의 매개변수 타입 검사 활성화 |
strictPropertyInitialization | `false` | 엄격한 클래스의 속성 초기화 검사 활성화 |
strictBindCallApply | `false` | 엄격한 Bind, Call, Apply 메소드의 인수 검사 활성화 |


## 타입 지정

```js
let num: number = 12

function sum(a: number, b: number): number {
  return a + b
}
const res: number = sum(1, 2)

console.log(num, res)
```

자바스크립트 변수 선언과 비슷하지만, 변수 이름 그 다음에 변수의 타입을 작성해야한다.

num의 데이터 타입이 숫자이므로 number이므로 num은 무조건 숫자 타입이어야 한다.

sum 함수에서 괄호 바로 앞의 number는 바로 리턴 되는 데이터의 타입을 나타낸 것이다. 

## 타입 에러

일치하지 않는 타입을 지정하면, 코드를 작성하는 시점에서 에러가 발생하게 된다.
```js
function sum(a: number, b: number): number {
  return a + b
}
const res: string = sum(1, 2) // TypeError!
```

숫자 2개를 더했는데 string이 나올수가 없으므로 TypeError!가 발생한다.

## 타입 선언

### String 
문자열을 나타낸다
```js
let red: string = "Red"
let green: string = 'Green'
let myColor: string = `My color is ${red}.`
let yourColor: string = 'Your color is' + green
```
### Number
모든 부동 소수점 값을 사용할수 있다.
```js
let integer: number = 6
let float: number = 3.14
let hex: number = 0xf00d // 61453
let binary: number = 0b1010 // 10
let octal: number = 0o744 // 484
let infinity: number = Infinity
let nan: number = NaN
```
### Boolean
`true`|`false` 값을 나타낸다.
```js
let isBoolean: boolean
let isDone: boolean = false
```
### null,Undefined
null 과 undefiend는 모든 타입의 하위 타입이지만, 엄격한 타입스크립트 문법에서 직접할당은 가능하지 않다. 그러나 void에는 undefined를 할당할수 있다.

```js
let str: string = null // Error!
let num: number = undefined // Error!
let boo: boolean = null // Error!
let arr: number[] = undefined // Error!
let obj: object = null // Error!
let voi1: void = null // Error!
let voi2: void = undefined // OK!
```

변수를 초기화하지 않으면, undefined가 되기 때문에 초기화 한 후에 사용해야한다.

### Array

일반 배열을 나타낸다.
두가지 방법으로 선언가능
```js
const fruits: string[] = ['Apple', 'Banana', 'Cherry'] // 문자 데이터로만 이루어진 배열이란다.
const numbers: number[] = [1, 2, 3, 4, 5, 6, 7] // 숫자 데이터로만 이루어진 배열이란다.
```

```js
// 문자열만 가지는 배열
const fruits: string[] = ['Apple', 'Banana', 'Mango']
// Or
const fruits: Array<string> = ['Apple', 'Banana', 'Mango']
```
가능하면 첫번째 방법으로 써라.

유니언 타입(다중 타입)의 '문자열과 숫자를 동시에 가지는 배열'도 선언할 수 있다.
```js
const array: (string | number)[] = ['Apple', 1, 2, 'Banana', 'Mango', 3]
// Or
const array: Array<string | number> = ['Apple', 1, 2, 'Banana', 'Mango', 3]
```
### Tuple
배열과 유사함
튜플은 정해진 타입의 고정된 길이 배열을 표현한다.

```js
let tuple: [string, number]
tuple = ['a', 1]
tuple = ['a', 1, 2] // Error!
tuple = [1, 'a'] // Error!
```
