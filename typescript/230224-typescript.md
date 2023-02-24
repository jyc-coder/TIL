js모듈을 TS에서 사용할 경우의 예시!

`src/common.js`

```js
const heropy = {
  name: 'Heropy',
  age: 85
}
module.exports = {
  heropy
}
```

`src/common.d.ts`
```js
interface User {
  name: string
  age: number
}
declare const heropy: User
export {
  heropy
}
```


`src/main.ts`

```js
// compilerOptions.esModuleInterop = false
import * as commonjs from './common'
// compilerOptions.esModuleInterop = true
import commonjs from './common'

console.log(commonjs.heropy)
```


## 유틸리티 타입

타입스크립트에서 제공하는 여러 전역 유틸리티 타입이 있다.

> 타입 변수 `T`는 타입, `U`는 또 다른 타입, `K`는 속성을 의미하는 약어이다.
이해를 돕기 위해 타입 변수를 `T`는 `TYPE` 또는 `TYPE1`, `U`는 `TYPE2`, `K`는 `KEY`로 명시한다.


유틸리티 이름 | 설명 (대표 타입) | 타입 변수
---|---|---
`Partial` | `TYPE`의 모든 속성을 선택적으로 변경한 새로운 타입 반환 (인터페이스) | `<TYPE>`
`Required` | `TYPE`의 모든 속성을 필수로 변경한 새로운 타입 반환 (인터페이스) | `<TYPE>`
`Readonly` | `TYPE`의 모든 속성을 읽기 전용으로 변경한 새로운 타입 반환 (인터페이스) | `<TYPE>`
`ReadonlyArray` |  | `<TYPE>`
`Record` | `KEY`를 속성으로, `TYPE`를 그 속성값의 타입으로 지정하는 새로운 타입 반환 (인터페이스) | `<KEY, TYPE>`
`Pick` | `TYPE`에서 `KEY`로 속성을 선택한 새로운 타입 반환 (인터페이스) | `<TYPE, KEY>`
`Omit` | `TYPE`에서 `KEY`로 속성을 생략하고 나머지를 선택한 새로운 타입 반환 (인터페이스) | `<TYPE, KEY>`
`Exclude` | `TYPE1`에서 `TYPE2`를 제외한 새로운 타입 반환 (유니언) | `<TYPE1, TYPE2>`
`Extract` | `TYPE1`에서 `TYPE2`를 추출한 새로운 타입 반환 (유니언) | `<TYPE1, TYPE2>`
`NonNullable` | `TYPE`에서 `null`과 `undefined`를 제외한 새로운 타입 반환 (유니언) | `<TYPE>`
`Parameters` | `TYPE`의 매개변수 타입을 새로운 튜플 타입으로 반환 (함수, 튜플) | `<TYPE>`
`ConstructorParameters` | `TYPE`의 매개변수 타입을 새로운 튜플 타입으로 반환 (클래스, 튜플) | `<TYPE>`
`ReturnType` | `TYPE`의 반환 타입을 새로운 타입으로 반환 (함수) | `<TYPE>`
`InstanceType` | `TYPE`의 인스턴스 타입을 반환 (클래스) | `<TYPE>`
`ThisParameterType` | `TYPE`의 명시적 `this` 매개변수 타입을 새로운 타입으로 반환 (함수) | `<TYPE>`
`OmitThisParameter` | `TYPE`의 명시적 `this` 매개변수를 제거한 새로운 타입을 반환 (함수) | `<TYPE>`
`ThisType` | `TYPE`의 `this` 컨텍스트(Context)를 명시, 별도 반환 없음! (인터페이스) | `<TYPE>`


