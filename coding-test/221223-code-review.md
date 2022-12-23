## 코드 리뷰 진행

프로그래머스 lv.0 최빈값 문제 풀이에 대해서 논의를 진행함.

https://school.programmers.co.kr/learn/courses/30/lessons/120812

## 나는 어떻게 풀었나?
빈 배열`answerList`을 하나 따로 생성하고
배열에 객체를 만들어서 반복적으로 추가하게 작성함(array의 숫자값 `number` ,세어진 횟수 `count`). 행동의 기준점을 `findIndex`를 사용함.

- `array`의 첫 항목의 경우에는 `answerList`에 아무것도 없으니 `findIndex`의 값이 -1이기 때문에 해당 경우에는 `count`값을 1로 하는 항목을 `push`함.

- 그 뒤로 들어온 항목에서 첫번째 조건이 `false`가 나온다면, 해당 값이 존재하는 `index`를 찾아서 해당 값의 `count`값만 더해준다.

- 반복 과정을 마친 다음 `sort`메서드를 사용해서 `count`를 기준으로 내림차순으로 정렬함

- 만약, 
	- 입력한 배열의 항목이 1개밖에 없다면, 첫번째 항목의 `number`값을 대입
    - `answerList`의 첫번째 와 두번째 항목의 count값이 동일하다면 최빈값이 존재하지 않는 것이기 때문에 -1로 대입
    - 위의 두가지 조건을 만족하지 않는다면 최빈값이 하나가 존재한다는 뜻이므로 가장 첫번째 인덱스의 `number`값을 대입한다.
    
```js
function solution(array) {
    if ((0 <= array.length) | (array.length < 1000)) {
        var answerList = [];
        for (let i = 0; i < array.length; i++) {
            //해당 number가 이미 추가되었는가?
            if (answerList.findIndex((k) => k.number === array[i]) === -1) {
                //없으면 새로 항목을 추가하고
                answerList.push({ number: array[i], count: 1 });
            } else {
                var someIndex = answerList.findIndex((k) => k.number === array[i]);
                // 만약 있으면 count값을 추가로 더한다
                answerList[someIndex].count++;
            }
        }
    }
    // 최빈값을 구하기 위해 count를 내림차순 기준으로 정렬
    answerList.sort((a,b) => b.count- a.count);
    // 만약 입력한 배열의 항목이 1개밖에 없다면
    if (answerList.length === 1) { 
        var answer = answerList[0].number
        }
    // 만약 answerList의 첫번째와 두번째 항목의 count 값이 동일하다면 가장 많이 존재하는 숫자가 하나가 아닌 것이므로 -1로 지정
    else if (answerList[0].count - answerList[1].count === 0){
        var answer = -1
        } else{
            // 가장 첫번째 인덱스의 number 값을 지정
            var answer = answerList[0].number
            }

    return answer;
}

```
    
    
## 다른 사람은 어떻게 풀었나?
빈 배열에 객체를 집어넣는 나의 방법과는 다르게 그냥, 빈 객체를 만들어서 그곳에 집어넣고 반복문을 만들어서 구현하니 코드가 좀더 단순해지고 이해하기가 편해졌다.
```js
function solution(array) {
  let numObj = {};
  let max = 0;
  let answer = -1;
  // numObj 객체에 배열의 각 요소별 개수 정보를 입력
  for (let num of array) {
    numObj[num] ? (numObj[num] += 1) : (numObj[num] = 1);
  }
  // 가장 높은 빈도수를 max에 할당
  for (let key in numObj) {
    max = Math.max(max, numObj[key]);
  }
  // max 빈도수를 가진 요소를 출력
  for (let key in numObj) {
    // max 빈도수가 중복이라면 -1 반환
    if (numObj[answer] === numObj[key]) {
      return -1;
    }
    // max 빈도수값을 가지는 요소를 반환
    if (numObj[key] === max) {
      answer = parseInt(key);
    }
  }
  return answer;
}
```
	


다른사람이 해결한 코드를 보니, 저런 방법으로도 해결할수 있다는 사실에 감탄하면서 아직 멀었다는 생각이 든다.
