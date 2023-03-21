지난번에는 map,filter,reduce를 사용해서 리팩토링을 진행해봤다. 

그렇다면 이런 for문을 사용하는 대신에 무조건 이 방법을 사용하는게 맞을까?

map을 사용해서 리팩토링을 한다고 해도 잘못된 방향으로 리팩토링을 진행하면 어떻게 되는지 보자.


## 접근 방식
> 세분화 했던 프로세스를 단순하게 만들수는 없을까?

totalCalculator 함수는 filter를 사용해서 outOfStock이 false인 것을 필터링하고 map을 호출하는데, 그러지말고, 필터링을 통해 나온 결과값들을 따로 배열로 만들어서 그걸 map메서드를 사용해 getValue를 호출하면 어떨까?

제고가 있는 제품들만 담은 배열을 따로 생성


```tsx
const totalCalculator = (
  list: Array<Item>,
  getValue: (item: Item) => number
) => {
  const result: Array<number> = []
  list.map(function (item) {
    if (item.outOfStock === false) {
      result.push(getValue(item))
    }
  })
  return result.reduce((total, value) => total + value)
}
```
function(item) 콜백함수가 리턴하는 값을 사용하는걸 원하지 않는다면 forEach로 사용해주라는 오류가 나타난다.

현재 map안에 있는 콜백은 void를 리턴한다. 만약 이런식으로 작성해버리면 map 함수를 통해 리턴된 값의 타입도 void로 추론해버리게 되며, void 타입으로는 더이상의 합성이 불가능하다.  

void 타입은 함수형 프로그래밍에서는 부수효과를 일으키는 함수는 void 타입을 리턴하는데, 이런 함수들은 입력받은 값으로 부수효과를 발생하는 일을 하며, 리턴값이 의미가 없거나, 그 값을 정할수 없기 때문이다. 

가장 큰예시로는 console.log 가 있다.

입력받은 문자열을 화면에 출력하기만 할뿐 리턴할 값이 아무것도 없다.


지금 사용한 forEach 함수는 result라는 외부의 값만 변경하기만 할뿐, 입력값에 대응되는 리턴값이 존재하지 않는다.

따라서 이런 상황에서는 map을 사용하는 것이 잘못된 것이다.



- 그럼 forEach 쓰는게 낫지 않음?

아까도 말했지만, forEach 같이 void 타입을 리턴하는 함수는 부수효과를 일으키며, 그곳에서 연산이 종료되므로, 더이상의 합성이 불가능하다. 그래서 화면의 값을 출력하는 '불가피한 경우'를 제외하고는 순수하게 동작하는 map의 사용을 선호하는 것이다.


함수형 프로그래밍의 목적은 '사람의 생각과 최대한 가까운 코드를 작성' 하는 것이다.


위에서 작성한 코드처럼 잘못된 상황에서 map,filter,reduce를 사용하면 이런 목적을 이룰수 없다.


- 아니... 3개 쓸바에 loop 하나만 쓰는게 성능상 좋지 않나요?

물론 성능. 중요하다. 하지만 협업과 유지보수를 위해서는 쉽게 이해할수 있는 코드가 더 중요하다고 생각한다. 또한 성능이 중요하다면, iterator, string 같은 lazy한 자료구조를 사용해서 표현력은 비슷하지만, 자료를 더 적게 순회하는 방법을 찾아볼수 있다.









