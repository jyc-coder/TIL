
## 2중 for문

카드 문양 4개 배열 `suits` 와 숫자 13개 배열 `nubmers` 를 2중 배열로 만들어서 총 52장의 카드를 나타내는 새로운 배열을 만들어본다.


```tsx
const suits = ['♠', '♥', '♣', '♦']
const numbers = [
  '2',
  '3',
  '4',
  '5',
  '6',
  '7',
  '8',
  '9',
  '10',
  'J',
  'Q',
  'K',
  'A',
]
const cards: Array<string> = []
for (const suit of suits) {
  for (const number of numbers) {
    cards.push(suit + number)
  }
}

const main = () => {
  const app = document.getElementById('app')
  if (app === null) {
    return
  }

  app.innerHTML = `
    <h1>cards</h1>
    <pre>
    ${JSON.stringify(cards, null, 2)}
    </pre>
  `
}

main()
```

이제 화면을 보면 

![](https://velog.velcdn.com/images/jhs000123/post/03c8d92b-1ca4-4077-9bc3-98313830d938/image.png)

52장의 카드가 들어있는 배열이 나타난다.

이렇게 2중으로 for문을 통해 만드는 코드는 어떻게 map을 사용하여 리팩토링할수 있을까?

## 작업 분석

- 모든 카드 목록은 아래의 작업을 모두 완료된 것이다.
- 아래의 작업을 모든 무늬에 적용한다
- 아래의 작업을 모든 숫자에 적용한다.
	- 카드는 무늬와 숫자를 연결한 문자열이다.
  

```tsx
// 모든 카드 목록은 아래의 작업이 완료된 것이다..
const cards2 =
  //  아래의 작업을 모든 무늬에 적용한다.
  suits.map((suit) =>
    //    아래의 작업을 모든 숫자에 적용한다.
    numbers.map(
      (number) =>
        //      카드는 무늬와 숫자를 연결한 문자열이다.
        suit + number
    )
  )
```
이런식으로 구현하면 어떻게 될까?

![](https://velog.velcdn.com/images/jhs000123/post/4616816e-f537-496c-af4f-3bc9a99308cc/image.png)

이렇게 문양별로 배열이 나눠져서 구현된다.

왜 이렇게 될까?

numbers.map 을 통해서 string 배열이 리턴되는데 해당 배열을 다시 suits.map에 적용되었기 때문에 이중 배열이 리던된 것이다!

![](https://velog.velcdn.com/images/jhs000123/post/1a5f1462-7083-48e3-8de5-de21f3bdfa57/image.png)

우리가 원하는 것은 전체 카드 배열이므로, 중첩된 배열을 하나로 합치는 작업이 필요하다.
> array<Array<T>> => Array<T>
  
이런 기능을하는 함수는 flat()이라는 함수가 존재한다. 
  
  ```tsx
  // 모든 카드 목록은 아래의 작업이 완료된 것이다..
const cards2 =
  //  아래의 작업을 모든 무늬에 적용한다.
  suits
    .map((suit) =>
      //    아래의 작업을 모든 숫자에 적용한다.
      numbers.map(
        (number) =>
          //      카드는 무늬와 숫자를 연결한 문자열이다.
          suit + number
      )
    )
    // 무늬별로 나누어진 카드를 하나로 합친다.
    // Array<Array<T>> => Array<T>
    .flat()
  ```
 
따라서 이렇게 마지막에 flat()을 추가해주면?
  
  ![](https://velog.velcdn.com/images/jhs000123/post/cd4921d5-0e9a-425e-9021-0ea951aaa544/image.png)

  이렇게 하나의 배열이 완성된다.

  물론 flatMap()이라는 함수도 있어서 map대신 사용해도 같은 결과가 나온다!
