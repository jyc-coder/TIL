## Coding Test 

16일차 문제풀이 완료 

- 직사각형 넓이 구하기
- 캐릭터의 좌료
- 최댓값 만들기 (2)
- 다항식 더하기


### 다항식 더하기 문제
링크 : https://school.programmers.co.kr/learn/courses/30/lessons/120863
```js
function solution(polynomial) {
   let lst = polynomial.split(" + ")
   let bs = 0
  let num = 0
  for (let idx of lst){
    idx.includes('x') ? idx.length === 1 ? bs+=1 : bs+=Number(idx.substr(0,idx.length-1))  : num += Number(idx) 
  }
  
  //변수가 없을때
  if (bs === 0){
    return num === 0 ? "0" : String(num)
  }
   // 변수가 1일때
  else if( bs === 1){
    return num === 0  ? "x" : "x" + " "+  "+" + " " + String(num)
  }
  // 2이상일때 
  else{
    return num === 0 ? bs + "x" : bs + "x" + " " + "+" + " " + String(num)
  }
 
}
  
```

입력값이 문자열로된 다항식이기 때문에 이를 하나하나 뜯어서 계수에 해당하는 요소와 상수항에 해당하는 요소를 나누어서 경우에 따라 다른 값이 반환 되도록 작성함

- split()을 사용해서 문자열을 " + " 을 기준으로 분리함. + 양옆에 공백을 추가하는것이 중요!
- for of 문을 사용해서 각각의 요소가 계수인지 그냥 상수항인지를 확인하고 그에 따라 `bs`와 `num`값을 증가시킨다. 
- 최종적으로는 계수가 0일때, 계수가 1일때, 2 이상일때를 나누어서 반환되는 값을 따로 지정했다.

if, else if를 써서 조금 지저분한 느낌이었지만, 다른사람들은 어떤 방식으로 풀었는지 한번 공유해보고 싶다.

