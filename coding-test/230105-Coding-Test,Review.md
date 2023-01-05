## 코딩 테스트 13일차 문제풀이, 코드리뷰 완료

### 코드리뷰 발표 내용
영어는 싫어요 문제 
링크 : https://school.programmers.co.kr/learn/courses/30/lessons/120894
```js


// 문자열 numbers가 매개변수로 주어질 때, numbers를 정수로 바꿔 return 하도록 solution 함수를 완성해 주세요.

function mreplace( replacements, str) {
  let result = str;
  for( let [x,y] of replacements)
    result =result.replace(x,y); //replace 메서드를 사용해서 정규식을 통해 찾은 값 'x'를 'y' 값으로 교체하고 result에 추가한다.
  return result;
}

function solution(numbers) {
   return parseInt(mreplace([[/one/g, "1"], // 이때 변경된 값이 문자데이터이기 때문에 parseInt를 사용하여 숫자로 변경함!
  [/two/g, "2"],
  [/three/g, "3"],
  [/four/g,"4"],
  [/five/g,"5"],
  [/six/g,"6"],
  [/seven/g,"7"],
  [/eight/g,"8"],
  [/nine/g,"9"],
  [/zero/g,"0"]],numbers));
}

// [/단어/g]
문자열에서 단어를 찾아라. 단, 한번만 찾고 끝내지 말고 문자열 전체를 탐색할것. 플래그 문자 g가 있으면 문자열 전체를 탐색한다는 뜻이다. 

relpace(x,y) -> x 값을 y로 변경하는 메서드
```
