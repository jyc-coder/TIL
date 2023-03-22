# 부분함수 

정의역의 일부분에만 정의되는 함수를 뜻한다.

하지만 함수형 프로그래밍에서는 순수함수의 개념은 어떤 정의역이든 그에 해당하는 함수값이 존재하므로 부분함수로 존재할수는 없다. 

아래와 같은 상황에서도 함수는 그에대한 답을 찾지 못한다면 다른 값으로 대신 반환하게 된다.


- 주어진 인자에 대응되는 변환 값이 없으면  -> 타입에 포함된 임의의 값을 대신 반환하기

ex) -1,0,null,undefined, infinity, NaN,


- 입력에 대응되는 값을 찾기 못했을 때


```js
const arr =["apple","banana","strawberry"]

arr.indexOf("banana")// 배열에서 위치 인덱스 찾기
// 1

arr.indexOf("Onion")// 배열에 찾는 값이 없을 때
// -1
```

- 입력에 대응되는 계산이 불가능할때

```js
const div = (a,b) => a/b;

div(10,0) // 0으로 나눌 수 있나?
// infinity

div(0,0) // 0을 0으로??
// NaN
```

## If 문 살펴보기

장바구니에 할인가격 필드를 추가하면서 undefined를 다루는 상황을 만들어보자.

이전에 사용했던 `cart.ts`에 할인가격에 해당하는 `discountPrice`를 Item 인터페이스에 추가했다.

그리고 cart 객체에서 토마토와 오렌지에만 할인 가격을 추가하고, 사과에는 넣지 않았다.

```tsx
export interface Item {
  code: string
  outOfStock: boolean
  name: string
  price: number
  quantity: number
  discountPrice?: number
}

export const cart: Array<Item> = [
  {
    code: 'tomato',
    outOfStock: false,
    name: '토마토',
    price: 7000,
    quantity: 2,
    discountPrice: 1000,
  },
  {
    code: 'orange',
    outOfStock: true,
    name: '오렌지',
    price: 15000,
    quantity: 3,
    discountPrice: 2000,
  },
  {
    code: 'apple',
    outOfStock: false,
    name: '사과',
    price: 10000,
    quantity: 1,
  },
]

```

배열을 다룰때 for문으로 먼저 해결했던 것처럼,
이번에는 undefined 여부를 검사하는 if문을 사용해서 해결해보자.

## 할인 가격 표시

`discountPrice` 가 존재하는 과일의 경우 가격 옆에 할인 가격을 표시해주는 기능을 구현하려고 한다.

### 1. 가격 표시 하드코딩

텍스트를 추가해서 할인 표시를 보여주기만 한다.

![](https://velog.velcdn.com/images/jhs000123/post/d504d1e3-736c-4730-9d88-d83c5f655e47/image.png)

- 할인 가격을 담는 변수를 선언
- item.discountPrice 가 undefined가 아니라면 할인 가격을 표시하는 텍스트를 추가한다

```tsx
const stockItem = (item: Item): string => {
  let saleText = ''
  if (item.discountPrice !== undefined) {
    saleText = `(${item.discountPrice}원 할인)`
  }
  return `
<li>
 <h2>${item.name}</h2>
<div>가격: ${item.price}원 ${saleText}</div>
<div>수량: ${item.quantity}상자</div>
</li>
`
}
```
![](https://velog.velcdn.com/images/jhs000123/post/018fcca7-3948-4ccb-9ec3-1c843b75d2da/image.png)

### 2. 할인된 가격 표시

음. 할인 가격은 표시해주고 있지만, 할인가격이 적용되지 않고 원래 가격을 보여주고 있다. 따라서 할인된 가격을 표시해주는 기능을 추가해보자

- item.price = item.price - 할인된 가격
> <div>가격: ${item.price - item.discountPrice}원 ${saleText}</div>

![](https://velog.velcdn.com/images/jhs000123/post/a647be7c-2d3a-43c4-aac3-983dc1779f9b/image.png)

### 3. 할인가격 동작 옵션 설정

음. 사과에는 할인 가격이 정의되어있지 않았으므로 price가 숫자 - undefiend로 계산되어 NaN이 나와버리는 현상이 발생했다.

따라서 할인가격이 정의된 상황에서만 동작해야한다.

- 할인 가격을 담고있는 변수를 따로 선언한다.
- `item.discountPrice` 가 정의되어있다면 discountPrice에 해당 값을 집어넣는다

- 가격 표시 코드에서 `item.discountPrice`대신에 `discountPrice`를 추가한다

```tsx
const stockItem = (item: Item): string => {
  let saleText = ''
  let discountPrice = 0
  if (item.discountPrice !== undefined) {
    saleText = `(${item.discountPrice}원 할인)`
    discountPrice = item.discountPrice
  }
  return `
<li>
 <h2>${item.name}</h2>
<div>가격: ${item.price - discountPrice}원 ${saleText}</div>
<div>수량: ${item.quantity}상자</div>
</li>
`
}
```
![](https://velog.velcdn.com/images/jhs000123/post/c94b4ad2-0b88-40db-bf86-129761682875/image.png)

이제 사과의 가격이 제대로 나타난다.

### 4. 전체 가격에도 할인 가격 적용

- totalPrice 코드 내부에 총 할인 가격을 리턴하는 `totalDiscountPrice` 작성
- totalPrice 의 리턴 내용을 totalPrice - totalDiscountPrice 로 변경

```tsx
const totalPrice = (list: Array<Item>): string => {
  let totalPrice = totalCalculator(list, (item) => item.price * item.quantity)

  const totalDiscountPrice = totalCalculator(list, (item) => {
    let discountPrice = 0
    if (item.discountPrice !== undefined) {
      discountPrice = item.discountPrice
    }
    return discountPrice * item.quantity
  })
  return `<h2>전체 가격: ${
    totalPrice - totalDiscountPrice
  }원 (총 ${totalDiscountPrice}원 할인)</h2>`
}
[O
```
![](https://velog.velcdn.com/images/jhs000123/post/c2090398-1cf4-4165-8589-e530cee94ce3/image.png)

오케이 이제 할인가격만큼 깎인 상태의 전체가격을 볼수 있다.

## Option 

방금 구현한 내용을 좀더 쉽게 작성할순 없을까?

- discountPrice 의 타입 : number | undefined

이런 타입을 제네릭 으로 작성하면?

```tsx
type Option<A> = A | undefined
```
그러나 이런 방법은 undefined가 여러 맥락에서 사용되기 때문에 혼란이 생길수 있다.

그러면 값이 있을때와 없을 때의 타입을 따로 써보자.

  
 ```tsx
// 값이 있을 때
export type Some<A> = {
  readonly _tag: 'Some'
  readonly value: A
}
// 값이 없을 때
export type None = {
  readonly _tag: 'None'
}
 ```
 
 이렇게 2개로 나누고, Some 과 None의 유니온 타입으로 Option 타입을 제작한다. 
 
 ```tsx
export type Option<A> = Some<A> | None

 ```
 
 Option의 값을 만들려면 태그를 하나하나 다 적어줘야해서 불편하다.
 
 ```
 const n1 : Option<number> = {_tag: "Some" , value : 1}
 ```
 
 값을 조금더 쉽게 만들기 위해 Some과 None 타입의 값을 만들어주는 함수를 작성한다.
 
 ```tsx
export const some = <A>(value: A): Option<A> => ({ _tag: 'Some', value })

export const none = (): Option<never> => ({ _tag: 'None' })

 ```

주어진 옵션 타입의 값이 있는지 없는지, 즉, some 타입인지 none 타입인지 알수 있는 방법도 필요하다.

```tsx
export const isSome = <A>(oa: Option<A>): oa is Some<A> => oa._tag === 'Some'
export const isNone = <A>(oa: Option<A>): oa is None => oa._tag === 'None'

```

자 그럼이제 이것들을 활용해서 if문으로 작성했던 내용들을 리팩토링 해보자.

if 문은 사용하는 맥락에 따라서 다용하게 사용된다. 값의 부재를 확인하기 위해서 뿐만 아니라, 참 거짓을 비교할수 있는 계산식이라면 어떤 것이든 검사하고, 검사 결과에 따라서 특정 동작을 수행할수 있다.

쓰임이 매우 다양하기 때문에 if문이 어떤 조건으로 어떤 동작을 하는지 파악하기 위해서는 이것이 사용되는 맥락도 같이 고려해봐야한다.


## 리팩토링

- undefined를 사용해서 값의 부재를 나타내는 타입을 Option으로 변환해주는 함수 작성

```tsx
export const fromUndefined = <A>(a: A | undefined): Option<A> => {
  if (a === undefined) return none()
  return some(a)
}
```
만약 입력한 타입이 undefined이면 none()을 리턴하고, 아니면 some()을 리턴한다


값의 타입을 검사하는 횟수는 2번, 상품별 할인가격 표시, 전체 할인가격 계산 구간이다. 
만약 `item.discountPrice` 의 값이 없으면 0으로 사용하고, 값이 있으면 그 값을 사용한다.

이 과정을 정리해보면, 
- 값이 없으면 지정된 값을 사용하고, 값이 있으면 해당 값을 사용하게 만들어준다

```tsx
export const getOrElse = <A>(oa: Option<A>, defaultValue: A): A => {
  // 값이 없으면 지정된 값을 사용
  if (isNone(oa)) return defaultValue
  // 값이 있으면 해당 값을 사용한다.
  return oa.value
}
```

### 1. 상품 할인 가격 리팩토링

- fromUndefined를 사용해서 `iteam.discountPrice` 를 Option으로 변환
- getOrElse를 사용해서 다시 구현한다. (값이 있으면 해당 값을 쓰고, 없으면 default 값을 0으로 지정)

```tsx
 const optionDiscountPrice = O.fromUndefined(item.discountPrice)
  const discountPrice = O.getOrElse(optionDiscountPrice, 0)
```

- optionDiscountPrice 가 값이 존재한다면 saleText의 내용을 수정한다.

```tsx
 let saleText = ''

  if (O.isSome(optionDiscountPrice)) {
    saleText = `(${discountPrice}원 할인)`
  }
```

### 2. 전체 가격 할인가 적용

- totalCalculator를 사용해서 getValue의 함수 discountPrice를 구하고 item.quentity만큼 곱한 값으로 지정한다.


```tsx
const totalDiscountPrice = totalCalculator(list, (item) => {
    const discountPrice = O.getOrElse(O.fromUndefined(item.discountPrice), 0)
    return discountPrice * item.quantity
  })
```
![](https://velog.velcdn.com/images/jhs000123/post/e5e01387-3f8f-42ea-b5de-f402b85edc4f/image.png)

