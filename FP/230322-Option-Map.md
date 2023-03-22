이전 글에서 If 문을 리팩토링하기 위해서 Option 타입으로 변환하여 값을 결정하는 방식으로 리팩토링을 진행했으나, 할인가격을 화면에 표시하기위해 생성한 saleText는 getOrElse를 사용하지 못하고 if문으로 남겨놨다.

그 이유는 Option안에 존재하는 값을 사용하는게 아니라 해당 값으로부터 파생된 값이 필요했기 때문이다.

그렇다면 이런 문제를 어떻게 해결하면 될까?

# Array map

배열에서 사용하는 map 은 0개 이상의 값을 처리할때 N개의 값이 입력되면 N개의 값이 출력된다.

몇번을 실행할지 알수없고, 심지어 한번도 실행을 안할수도 있기 때문에 부수효과를 동반하지 않는 순수함수여야 안전하게 사용할수 있다.


이런 관점을 Option에 적용할수 있을까?

# Option map

![](https://velog.velcdn.com/images/jhs000123/post/46b4a55b-26b2-43bc-afbd-8c3557146389/image.png)

- Option은 값이 있을수도 없을수도 있는 타입이기 때문에 만약 값이 있다면, 즉 Some이라면 그에대한 결과를 다시 Some으로 리턴한다

- 반대로 값이 없다면, 즉 None이라면 그에 대한 결과를 다시 None으로 리턴한다.


- Option에서의 map은 인자로 주어진 함수를, 옵션의 상태에 따라서 0번 또는 1번만 실행하는 모습이 되어야 한다. 

- 그리고 None 이든 Some 이든 그 타입 그대로 결과 타입에 돌려주어야한다.


# 그럼 한번 해보자

```tsx
export const map = <A, B>(oa: Option<A>, f: (a: A) => B): Option<B> => {
  // 값이 없으면 값이 없는 상태를 유지한다.
  if (isNone(oa)) return oa
  // 값이 있으면 값을 함수에 적용한다.
  return f(oa.value) // 빨간줄!!
}
```

이렇게만 작성하면 `Option<B>`에 빨간줄이 그어진다. f의 리턴 타입은 `Option<B>`타입이 아니라 순수한 B타입이기 때문이다. 따라서 f의 리턴 타입을 값이 있는 Option 타입인 Some으로 바꿔야한다.

```tsx
 return some(f(oa.value))
```

이제 이 map을 사용해서 상품별 할인가격을 표시하는 코드를 리팩토링해보자

## 리팩토링 : 상품별 할인가격

- 상품별 `optionDiscountPrice`와 해당 값이 존재할 경우 동작하는 함수를 넣은 map을 생성한다.

- 그리고 map을 통해 만들어진 optionSaleText를 saleText에 집어넣는다. 이때 getOrElse를 사용해서 값의 존재 유무에 따라 다른 값이 되도록 한다.

```tsx
  const optionSaleText = O.map(
    optionDiscountPrice,
    (discountPrice) => `${discountPrice}원 할인`
  )
  const saleText = O.getOrElse(optionSaleText, '')

```
`이전 코드`
```tsx
  /* let saleText = ''
  if (O.isSome(optionDiscountPrice)) {
    saleText = `(${discountPrice}원 할인)`
  } */
```
  
이전 코드와 결과값도 값고 하는일도 같다.
이전 코드는 saleText를 빈문자로 선언해둔후 OptionDiscountPrice에 값이 있으면 saleText를 특정 문자열로 지정하는 방식으로 구현했다. 

> saleText를 선언한 후 Option상태에 따라서 값을 변경하는 방식


새롭게 작성한 코드는 `optionDiscountPrice`의 숫자를 특정 문자열로 변환해서 새로운 Option을 얻는다. Option 숫자타입에서 Option 문자열 타입으로 리턴되는 방식으로 만약 꺼내올 값이 존재하지 않을 경우에는 ''만 출력된다.

> Option 타입의 값의 상태라고 할수 있는 None 이나 Some은 유지하면서 그 안의 값이 존재하면 주어진 함수를 사용하여 변환을 하고 최종적으로 getOrElse를 통해 saleText를 결정하는 방식


명령적 방식의 코드를 함수형 프로그래밍을 통해 선언적 방식으로 바꾼것이라고 할수 있겠다.

여기서 saleText 를 하나로 표현하고 싶다면 
함수 합성을 사용하면 된다.

```tsx
const saleText = O.getOrElse(
    O.map(optionDiscountPrice, (discountPrice) => `${discountPrice}원 할인`),
    ''
  )
```
이런식으로 중간 변수 없이 바로 getOrElse와 map을 같이 사용할수 있다. 이런 함성함수를 미리 선언해두면 나중에 다시 사용할수 있다.

```tsx
export const mapOrElse = <A, B>(
  oa: Option<A>,
  f: (a: A) => B,
  defaultValue: B
): B => {
  return getOrElse(map(oa, f), defaultValue)
}

```

map 을 먼저 진행한 다음 리턴된 값과, defaultValue로 입력한 값을 인자로 삼아서 getOrElse로 진행한다. 

- (oa,f를 사용해서 mapdmf 통해  리턴한 B 타입 ), (defaultValue로 입력한 B타입) 을 getOrElse를 호출해서 B 타입 리턴!


이걸 이제 saleText에 적용하면?
```tsx
  const saleText = O.mapOrElse(
    optionDiscountPrice,
    (discountPrice) => `${discountPrice}원 할인`,
    ''
  )
```
더 단순해졌다.


