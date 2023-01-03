## 프로그래머스 코딩 테스트 11일차 문제 풀이 완료!

### 풀이 진행하면서 조사한 내용

- 절댓값
Math 객체의 abs() 메서드를 사용하면 절댓값을 구할수 있다.

```js
var num1 = Math.abs(-1);      //음수
var num2 = Math.abs('-1');    //문자형 숫자
var num3 = Math.abs('ABC');   //문자
var num4 = Math.abs(null);    //null


console.log(num1, num2, num3, num4) // 1 1 NaN 0
```

- reduce()
오늘차 가까운 수 문제를 풀고 난다음 다른사람의 답을 보니 아래와 같이 적었다.
```js
function solution(array, n) {
    return array.reduce((a,c)=> Math.abs(a-n) < Math.abs(c-n) ? a : Math.abs(a-n) === Math.abs(c-n) ? Math.min(a, c) : c);
}

```
나는 10줄이 넘어가는데 이렇게 풀수있다니...

reduce 메서드의 파라미터에 초기값을 작성하지 않으면  c는 첫번째값부터 시작한다. 따라서 0부터 시작해서 a의 값을 변경하는 것이다. 
삼항연산자를 사용해서 a와 c에서 각각 n을 뺀값의 절대값을 비교하고 c의 경우가 더 크다면 이전에 구한 두가지의 값이 같은지 확인(절댓값이 같은 수가 있는지를 확인하는 작업)하고 있다면 그 둘중에 작슨수를 리턴하고, 아니라면 a값을 c로 변경한다.

이렇게 반복을 거치면 최종적으로 c를 리턴하게 되는 것이다. 

#### 나는 어떻게 풀었는가
```js
function solution(array, n) {
    const absList = array.map(data => Math.abs(data)-n)
    // 항목을 절댓값으로 변경하고 n만큼 뺀값으로 교체한 배열 
    const dabList = absList.map(data => Math.abs(data))
    // 정확한 비교를 위해서 한번더 모든 항목을 절댓값을 교체한 배열 

    
    //// 절댓값이 같은 항목이 있는지 체크하는 과정 
    const minIndex = dabList.indexOf((Math.min.apply(null,dabList)))
    // n과 가장 가까운 조건을 만족하는 숫자가 위치한 첫번째 인덱스
    const maxIndex = dabList.lastIndexOf((Math.min.apply(null,dabList)))
    // n과 가장 가까운 조건을 만족하는 숫자가 위치한 마지막 인덱스
    
    
    // 만약 그 둘이 다르다면 값이 2개가 존재한다는 뜻
    if (minIndex !== maxIndex){
        // 그 둘을 비교해서 작은 것을 리턴
      return Math.min(array[minIndex],array[maxIndex])
    }
  else{
     // 두개의 값이 같다면 그냥 minIndex에 위치한 배열의 값을 리턴한다.
    return array[minIndex]
  }
}
```
`indexOf()`와 `lastIndexOf()`를 활용해서 절댓값이 같은 숫자가 존재하는지 여부를 파악하고 그에따라 리턴값을 다르게 설정하는 방식으로 풀었다.

이 긴 코드가 한문장으로도 같은 기능을 수행한다는 사실을 알게되면 허탈함이 배가된다.


- filter()

주어진 함수의 테스트를 통과하는 요소만 모아서 새로운 배열로 반환하는 메서드.
원하지 않는 값을 조건을 통해 제거할수 있다.

```js
const words = ['spray', 'limit', 'elite', 'exuberant', 'destruction', 'present'];

const result = words.filter(word => word.length > 6);

console.log(result);
// expected output: Array ["exuberant", "destruction", "present"]
```

- toUpperCase() , toLowerCase() 문자열을 대문자, 소문자로 변환해서 반환하는 메서드

```js
const sentence = 'The quick brown fox jumps over the lazy dog.';

console.log(sentence.toUpperCase());
// expected output: "THE QUICK BROWN FOX JUMPS OVER THE LAZY DOG."

```

```js
let str = 'HELLO WORLD'


console.log(str.toLowerCase()) 
// hello world
```

