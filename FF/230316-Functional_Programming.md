
# 프로그램이 왜 복잡해질까?
- 어떤 문제를 해결하기 위해 컴퓨터에게 주어지는 처리 방법과 순서를 기술한 일련의 명령문의 집합체가 바로 프로그램.

- 그럼 그 해결하려는 `어떤 문제`가 단순한 것이 아니라 여러가지 조건을 만족해야지 해결되는 문제라면, 그 조건을 만족하기 위한 처리 방법과 순서를 기술할테고,
- 설령 그 문제를 해결했다고 해도, 다른 문제가 발생하게 되면 또 그 문제를 해결하기 위한 코드가 추가될 것이다.

프로그램이 복잡해지는 이유는 `복잡한 문제를 해결하기 위해서`..?

# 함수란 무엇인가?

- 함수란 하나의 특별한 목적의 작업을 수행하기 위해 독립적으로 설계된 프로그램 코드의 집합
- 문제를 해결하기 위해 만든 프로그램 코드

# 함수를 합성한다는건?

- 함수를 합성한다 > 문제를 해결하기 위해 만든 프로그램 코드를 합성한다 -> 각각의 프로그램으로는 해결할수 없는 것을 합성하여 해결한다.



# 객체지향 vs 함수형

- 객체지향 : 움직이는 부분을 캠슐화하여 코드 이해를 돕는다
- 함수형 : 움직이는 부분을 최소화 하여 코드 이해를 돕는다.


# 부수효과

함수형 프로그래밍은 부수효과를 최소화 하고 순수 함수로 프로그래밍을 한다는 내용이 있는데, 여기서 부수효과는 무엇일까?

> 값을 반환하는 것 외에 부수적으로 일으키는 효과

변수나 상태를 바꾸거나 수정
화면이나 파일에 데이터를 쓰는 IO작업
다른 부수효과가 있는 함수나 상태 값에 의존

-> 기계와 저수준에 최적화된 명령형 방식


그럼 이런 것들을 최소화하게 구현한다고 치면 함수형으로 모든 프로그램을 짤수는 없겠네?

> 함수형 프로그램이라는 것은 이러한 io작업이나 전역 변수를 못쓰는 것이 아니라 이런 부분을 코드에서 분리하고 격리해서 우리가 기존에 부수효과로만 다뤘던 작업들을 순수한 것처럼 다룰수 있게 추상화 시켜서 작업한다는 뜻이다.


# 순수 함수?

아니 함수가 그냥 함수지 무슨 순수함이 있어?

> 똑같은 매개변수(입력)을 받으면 항상 같은 값을 반환하는 함수

- **~~부수효과~~가 없음**
- 수학의 함수 -> 증명가능, 안전
- 순서나 실행 횟수랑 상관없이 , 항상 예측이 가능한 결과

-> (비교적) 인간의 뇌로 생각하기 쉽고 합성하기도 쉽다.


![](https://velog.velcdn.com/images/jhs000123/post/b089b10c-685b-4821-80c5-a4bdd80d45fd/image.png)

그니까 왜 굳이 이렇게까지 만들어서 코드를 짜는거야?

>부수효과가 있는 코드를 합성하는 것보다 순수함수를 합성하는것이 훨씬 쉽고 다른 변수가 생기는 일이 적기 때문

# 합성하기 어려운 부수효과

- for문

![](https://velog.velcdn.com/images/jhs000123/post/a8aff4f7-e7ce-4fa7-8303-9c48c85b1d6a/image.png)


[Ofor 나 while 같은 구문은 불변성을 위반하기 때문에, 함수형 프로그래밍에서는 이를 사용할수 없다. 대신 재귀용법을 사용해서 반복을 구현하지만 

 ![](https://velog.velcdn.com/images/jhs000123/post/e64e6509-9ba1-49f3-ab34-b6b61b5720af/image.png)

이렇게 조건이 여러가지로 나뉘게 되면 장황하고 복잡한 코드가 생성된다.


조건을 검사해서 종료여부를 판단하는 작업과 종료가 되지않을때 실행하는 동작을 수행하는 부분을 뜯어서 이 둘을 따로 사용할수 있게 함수를 만들게 된다면...?
![](https://velog.velcdn.com/images/jhs000123/post/26849103-8746-48bd-9fd5-4f17b6deb4d8/image.png)


- loop
`loop` : 종료 조건이 아닐때 실행되는 함수 `fn`, 이전의 데이터값을 가지고 있는 `acc` loop라는 함수가 실행되는 데이터 배열이 담긴 `list`를 매개변수로 입력을 받는다. 

- `list`의 길이가 없으면 `acc`를 리턴

- 만약 길이가 존재한다면 `list`배열에서 맨 앞의 요소를 뜯어서 fn함수의 파라미터로 집어넣고 다시 fn 함수를 호출하게 된다.

- range 
배열의 범위를 지정하는것으로 start,end까지 Array.from을 사용하여 생성한다.

- plus 입력받은 데이터를 서로 더하는 메서드


배열이 실제로 재귀적인 구조는 아니지만 분석해서 사용할수는 있다.

> loop 와 range는 한번만들어두면 재사용 가능


## 효과를 안전하게 추상화하기

- 명확하고 순수한 Effect
- 효과와 계산이 직교하게 분리
- 재사용하기 쉬운 추상화
- 당순하고 일관된 인터페이스

# 계산과 부수효과가 함께 있으면? 

- 토마토 오렌지 사과 가격을 작성하고, 과일을 추가할때마다 총 금액`totalPrice`의 가격이 오르게 된다. 

```ts

 /*
 토마토 : 7000원
 오렌지: 15000원
 사과 : 10000원
 */

export let totalPrice = 0;

// 명령형 프로그래밍
// 1. 변수를 선언한다.
// 2. 변수에 값을 할당한다.

export function addTomato() {
  totalPrice += 7000;
}

export function addOrange() {
  totalPrice += 15000;
}

export function addApple() {
  totalPrice += 10000;
}


export function list1() {
  // 토마토, 오렌지 , 사과 한상자
  addTomato()
  addOrange()
  addApple()
}


export function list2() {
  // 토마토 2상자
  addTomato()
  addTomato()
}

export function list3() {
  // 오렌지 100상자
  for (let i = 0; i < 100; i++) {
    addOrange()
  }
}

export function main() {
  list3()
}
```

자. 이런 상태에서 만약 App.tsx를 아래와 같이작성하면
```tsx
import { useState } from 'react'
import { main, totalPrice } from './test';



function App() {
  const [count, setCount] = useState(0)
  main()
  return (
    <div className="App">
      Total price : {totalPrice}
      </div>
  )
}

export default App

```
현재 main()에는 list3()이 적혀있으니까 당현히 1500000 가 나올것이다. 

그럼 여기서 만약에 Total price라고 적혀있는 것을 Sum 이라는 텍스트로 바꾼다고 해보자.

![](https://velog.velcdn.com/images/jhs000123/post/539fb772-320f-4155-9a90-767535ce38d5/image.png)

![](https://velog.velcdn.com/images/jhs000123/post/42c11720-b542-4662-afb6-47cae9825309/image.png)

<div align=center>?</div>

분명 텍스트만 바꿨을 뿐인데 왜 가격이 늘어나는거지?

> App.tsx의 내용이 바뀌었을때, list3()이 실행은 되었으나 test.ts 전체가 실행된 것이 아니므로 초기화기 되지 않았기 때문!


# 그럼 어떻게 해야 이런 일이 안생길까?


토마토, 오렌지, 사과의 가격을 '총합'을 보관하는 변수에더하는 내용으로 함수를 작성하지 않고, 그냥 가격 그 자체만 return 시킨 다음 list에서 직접 더해주는 방식으로 해주면 어떻게 될까?
```ts
 /*
 토마토 : 7000원
 오렌지: 15000원
 사과 : 10000원
 */


export function priceOfTomato() {
  return 7000;
}
 
export function priceOfOrange() {
  return 15000;
}

export function priceOfApple() {
  return 10000;
}


export function list() {
  // 토마토, 오렌지 , 사과 한상자
  return priceOfTomato() + priceOfOrange() + priceOfApple();
}

export function list2() {
 // 토마토 2상자
  return priceOfTomato() * 2;
}

export function list3() {
  // 오렌지 100상자
  return priceOfOrange() * 100;
}

export function main() {
  return list()
}
```

자 이제 똑같이 텍스트를 변경하면 어떻게 될까?
![](https://velog.velcdn.com/images/jhs000123/post/be4dbe10-b432-4a3e-b516-f3c4c26f5de3/image.png)

텍스트를 변경해도 이제 가격이 변하지 않는다!

여기서 더 일반화를 해본다면 과일의 가격에 따른 함수를 만들지 않고, 해당 과일의 가격을 반환하는 `getPrice`함수를 만들어보면 어떨까?

``` tsx
function getPrice(name: string) {
  if (name === "tomato") {
    return 7000;
  } else if( name === "orange") {
    return 15000;
  } else if (name === "apple") {
    return 10000;
  }
}
```

어차피 과일에 대한 가격은 변하지 않으니까 하나의 단순한 레코드로 리팩토링이 되지 않을까?

```tsx
const priceOfFruit = {
  tomato: 7000,
  orange: 15000,
  apple: 10000,
}
```

이렇게만 줄이면 코드는?

```tsx
const priceOfFruit = {
  tomato: 7000,
  orange: 15000,
  apple: 10000,
}


export function list() {
  // 토마토, 오렌지 , 사과 한상자
 return priceOfFruit.tomato + priceOfFruit.orange + priceOfFruit.apple;
}

export function list2() {
 // 토마토 2상자
  return priceOfTomato() * 2;
}

export function list3() {
  // 오렌지 100상자
  return priceOfOrange() * 100;
}

export function main() {
  return list()
}
```
이렇게 줄어들었다. 


여담으로 만약 문자열의 길이가 홀수인지 아닌지를 파악하는 메소드를 만들때, 추상화를 통해 로직을 만들게 되면 훨씬 간단하고 직관적으로 만들수 있게 된다.


```tsx
const isEvenFn = (str: string) => str.length % 2 === 0
```

