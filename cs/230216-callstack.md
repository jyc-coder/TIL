## 콜스택
 함수가 호출될때 그 함수들을 모아두는 자료구조
 
 
## 재귀 함수 VS 반복문

공통점 : 반복해서 값을 추출한다.


차이점

함수 | 접근방식 | 우위 | 예시
:-:|:--:|:--:|:--:|
재귀함수 | 상향식 / 하향식| 하향식 접근| 팩토리얼 구현
반복문 | 상향식| 상향식 접근| 일반적인 상황

상향식 / 하향식? 
- 상향식은 상위문제를 해결하기위해 하위문제를 사용하는 것
- 하향식은 하위문제를 해결하는데 상위문제를 사용하는 것 


## 재귀함수 한계점

- 호출될 때마다 메모리에 스택이 쌓이고, 한계치 이상으로 호출이 되어 스택이 넘쳐버리면 메모리 부족으로 에러가 발생한다.

- 반복문에 비해 시간이 오래 걸린다. 

- 함수 호출과 복귀를 하기 위한  context switching 비용이 발생하기 때문에, 속도가 상대적으로 느립니다. 즉, 오버헤드가 발생하여 속도가   느리게 됩니다.

## 꼬리 재귀
- 재귀호출이 끈난 후 현재 함수에서 추가연산을 요구하지 않도록 구현하는 재귀의 형태이다.

- 이를 이용하면 함수 호출이 반복되어 스택이 깊어지는 문제를 컴파일러가 선형으로 처리하도록 알고리즘을 바꿔 스택을 재사용할수 있게 된다.


## nullish 병합 연산자
nullish 병합 연산자(nullish coalescing operator) ??를 사용하면 짧은 문법으로 여러 피연산자 중 그 값이 ‘확정되어있는’ 변수를 찾을 수 있습다.

```js
let height = 0;

alert(height || 100); // 100
alert(height ?? 100); // 0
```

||는 `height` 에 0ㅇ을 할당했지만 0을 falsy 한 값으로 취급했기 때문에 `null`이나 `undefined`를 할당한 것과 동일하게 처리한다. 따라서 결과가 100이 나온다.

반면 ?? 는 height 가 정확하게 null 이나 undefined일 경우에만 100이 된다. 예시에서는 height에 0이라는 값을 할당했으므로 0이 출력된다.

