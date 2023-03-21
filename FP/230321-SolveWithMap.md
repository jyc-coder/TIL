
순수 함수 프로그래밍에서는 예외를 표현하는 데이터 타입을 Try, Either로 작성한다.

제네릭으로 구현한 map 
```
type map<A,B> = (Array<A>.A => B) => Array<B>
```

타입 파라미터 A가 number 타입이라면?
```
(Array<number>,number => B ) => Array<B>
```

함수의 타입 내에서 타입 패러미터와 같은  인수와 리턴의 타입은 일치해야한다

이말은 타입 파라미터가 서로 다를 때는 다른 타입을 써도 된다는 말이지 

> 무조건 일치해야하는 말은 아니라는 소리

예를들어 위에 작성한 A가 number 타입일 경우의 코드처럼 Array의 타입이 number라면 입력하는 인자의 타입이 number여야 한다는 소리이며, 이로인한 리턴이 꼭 number일 필요는 없다는 뜻이다


## 장바구니 map,filter,reduce 로 구현하기

이전에 사용한 for 문법으로 장바구니를 구현해봤으나, 일련의 문제점이 존재했다. 

그러므로, map을 사용하면 어떤 모습으로 구현되는지 실습해보자.

### 장바구니 특징 

- 아이템 목록 화면
-> 재고가 있는 아이템
-> 재고가 없는 아이템
- 전체 수량 표시
- 전체 가격 표시
- 장바구니 구현 방식
-> 장바구니를 순회
-> 화면에 상품 이름, 가격, 수량을 표시
전체 가격과 전체 수량도 화면에 그려야한다.
-> 순회 동작을 수행 할때 totalPrice, totalCount에 값을 누적한다.
- 재고 없는 상품의 처리
-> 순회와 누적 동작을 수행할때 재고 여부에 따라 다르게 동작시킨다. 


틀을 한번에 잘 만들면 안정적으로 만들수 있지만, 
틀이 완벽하지 않으면 보수해야할 작업이 늘어나게 된다. 

함수형 프로그래밍은 단순한 기능을 수행하는 함수들을 생성해서 그것들을 레고블럭처럼 조합해서 복합적인 기능을 구현한다.

### 장바구니 함수형 프로그래밍

1. 아이템을 입력 받고 string을 리턴
```tsx
// 아이템을 입력 받고 string 을 리턴
const stockItem = (item: Item): string => `
<li>
 <h2>${item.name}</h2>
<div>가격: ${item.price}원</div>
<div>수량: ${item.quantity}상자</div>
</li>
`
```

2. 품절된 아이템 입력을 받고 string을 리턴
```tsx
// 품절된 아이템 입력을 받고  string을 리턴
// 품절된 아이템 입력을 받고  string을 리턴
const outOfStockItem = (item: Item): string => `
  <li class="gray">
 <h2>${item.name}(품절)</h2>
<div class="strike">가격: ${item.price}</div>
<div class="strike">수량: ${item.quantity}</div>
</li>
`
```
3. outOfStock의 여부에 따라 다른 함수 호출
```tsx
// outOfStock의 여부에 따라 다른 함수를 호출
const item = (item: Item): string => {
  if (item.outOfStock) {
    return outOfStockItem(item)
  } else {
    return stockItem(item)
  }
}

```

4. 전체 수량 리턴
```tsx
// 전체 수량 리턴
const totalCount = (): string => {
  let totalCount = 0
  for (let i = 0; i < cart.length; i++) {
    if (cart[i].outOfStock === false) {
      totalCount += cart[i].quantity
    }
  }

  return `<h2>전체 수량: ${totalCount}상자</h2>`
}
```

5. 전체 가격 리턴
```tsx
// 전체 가격 리턴

const totalPrice = (): string => {
  let totalPrice = 0
  for (let i = 0; i < cart.length; i++) {
    if (cart[i].outOfStock === false) {
      totalPrice += cart[i].price * cart[i].quantity
    }
  }
  return `<h2>전체 가격: ${totalPrice}</h2>`
}
```



여기서 totalCount, totalPrice는 아직은 순수함수라고 보기 어렵다. 전역 변수인 cart를 묵시적으로 의존하고 있고, 입력받는 인수가 없다. cart라는 목록이 전역변수로 주어지지 않으면 임의의 목록에 대한 테스트도 힘들어진다. 함수들을 별도의 파일로 분리하는 것 또한 힘들어진다.

그래서 이 두개의 메소드의 인자에 list를 추가해본다.

```tsx
// 전체 수량 리턴
const totalCount = (list: Array<Item>): string => {
  let totalCount = 0
  for (let i = 0; i < list.length; i++) {
    if (list[i].outOfStock === false) {
      totalCount += list[i].quantity
    }
  }

  return `<h2>전체 수량: ${totalCount}상자</h2>`
}

// 전체 가격 리턴

const totalPrice = (list: Array<Item>): string => {
  let totalPrice = 0
  for (let i = 0; i < list.length; i++) {
    if (list[i].outOfStock === false) {
      totalPrice += list[i].price * list[i].quantity
    }
  }
  return `<h2>전체 가격: ${totalPrice}</h2>`
}

const list = (list: Array<Item>) => {
  let html = '<ul>'
  for (let i = 0; i < list.length; i++) {
    html += item(list[i])
  }

  html += '</ul>'
  return `
  ${html}
  `
}
```

받는 인자 항목을 추가했으니 당연히 호출하는 코드에 인자를 추가해줘야한다.


### 리팩토링 : totalCount , totalPrice 
totalCount 와 totalPrice 는 리턴하는 내용은 다를지 몰라도, for 문과 if 문은 완벽하게 일치한다. 이렇게 반복되는 구조는, 고차함수를 이용하면 쉽게 추상화가 가능하다.

```tsx
const totalCalculator = (
  list: Array<Item>,
  getValue: (item: Item) => number
) => {
  let total = 0
  for (let i = 0; i < list.length; i++) {
    if (list[i].outOfStock === false) {
      total += getValue(list[i])
    }
  }

  return total
}
```

입력하는 인자는  Item 타입을 가진 배열과, Item을 입력받아 number 타입을 리턴하는 함수 이렇게 2가지를 통해서 total을 리턴하는 메서드이다.


이 메소드를 사용해서 `totalPrice` 와 `totalCount`를 리팩토링 할수 있다.  

```tsx
// 전체 수량 리턴
const totalCount = (list: Array<Item>): string => {
  let totalCount = totalCalculator(list, (item) => item.quantity)
  return `<h2>전체 수량: ${totalCount}상자</h2>`
}

// 전체 가격 리턴

const totalPrice = (list: Array<Item>): string => {
  let totalPrice = totalCalculator(list, (item) => item.price * item.quantity)
  return `<h2>전체 가격: ${totalPrice}</h2>`
}

```

### 리팩토링 : totalCalculator

훨씬 더 깔끔해졌다. 2번째 getValue 함수를 화살표함수 형태로 익명함수로 작성해서 인자로 보내주면, 그에대한 값을 number로 변경하여 total에 더해지면서 그 값을 totalCalculator가 리턴하는 것이다. 

>  // 전체 목록중 재고가 있는 상품만 getValue를 실행하고 그 값을 모두 더한다.
  // 1. 재고가 있는 상품만 분류하기
  // 2. 분류된 상품들에 대해서 getValue 실행하기
  // 3. getValue의 결과를 모두 더하기
  
  
  이런 느낌을 구현하기 위해서 이번에는 map,filter,reduce을 사용해서 구현해보자.
  
  ```jsx
const totalCalculator = (
  list: Array<Item>,
  getValue: (item: Item) => number
) => {
  // 전체 목록중 재고가 있는 상품만 getValue를 실행하고 그 값을 모두 더한다.
  return (
    list
      // 1. 재고가 있는 상품만 분류하기
      .filter((item) => item.outOfStock === false)
      // 2. 분류된 상품들에 대해서 getValue 실행하기
      .map(getValue)
      // 3. getValue의 결과를 모두 더하기
      .reduce((total, value) => total + value, 0)
  )
}
  ```
  레전드...
  
  ### 리팩토링 : list
  
  현재 html의 태그들을 for문을 사용해서 반복적으로 더한 dom을 리턴하는 방식도 리팩토링이 가능하다.
  
> // 1. 목록에 있는 아이템을 태그로 변경
// 2. 태그의 목록을 모두 하나의 문자열로 연결
  
  
```tsx
const list = (list: Array<Item>) => {
  return `
  <ul>
  ${list
    // 1. 목록에 있는 아이템을 태그로 변경
    .map(item)
    // 2. 태그의 목록을 모두 하나의 문자열로 연결
    .reduce((tags, tag) => tags + tag, '')}
    </ul>
  `
}
```
이렇게 만들어주면 html변수를 따로 선언할 필요없이 깔끔하게 구현이 가능하다.

