## Code Review

2023년 1월 2째주 코드리뷰 진행함

링크 : https://www.notion.so/23-1-2-e96841b1c2f9435097b32b09ae5ab6dc

### 발표한 문제
다항식 문제: https://school.programmers.co.kr/learn/courses/30/lessons/120863
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
  // 1 이상일때 
  else{
    return num === 0 ? bs + "x" : bs + "x" + " " + "+" + " " + String(num)
  }
}
```
- `polynomial` 을 “ + “ 기준으로 split해서 `lst` 배열을 만들고 계수와 상수항값을 저장하는 `bs` `num` 값을 선언
- `lst` 의 요소만큼 반복을 실행하며 요소에 `x` 가 존재한다면, 숫자를 bs에 더하고, 존재하지 않을 경우에는 `num` 에 더한다
- 변수가 없을때, 계수가 1일때, 혹은 1이상일때로 경우를 나눠서 그에 맞는 값을 출력함
