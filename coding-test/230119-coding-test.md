## Coding Test

프로그래머스 코딩 테스트 문제풀이 20일차 완료
lv.0 문제풀이 100% 달성

- 치킨 쿠폰
- 이진수 더하기
- A로 B 만들기
- k의 개수
- 문자열 밀기
- 종이 자르기
- 연속된 수의 합
- 다음에 올 숫자

### 이진수 더하기

사용자가 이진수 문자열을 2개 입력하면 그 둘을 더한 값을 이진수 문자열로 반환하는 함수를 구현하는 것이다. 

```js
// 이진법으로 표기된 문자열을 십진법으로 변경해서 숫자로 변경하는 메서드
function changeDecimal(bin){
  return  [...bin].reverse().reduce((a,c,idx) => {
      return a + Number(c) * 2 ** idx
    },0)
}
// 십진법으로 표기된 숫자를 이진법으로 변경하는 메서드 
function changeBinary(dec) {
  let bin = []
  if(dec === 0) {
    bin.push(dec)
  }
  while(dec > 1){
    bin.push(dec % 2)
    dec = Math.floor(dec / 2 )
    if (dec === 1){
         bin.push(dec)
         break;
       } 
  }
  return bin.reverse().join("")
}

function solution(bin1, bin2) {
  let dec1 = changeDecimal(bin1)
  let dec2 = changeDecimal(bin2)
  return changeBinary(dec1 + dec2)                      
  
}
```
해결 방법은 입력한 2개의 이진법 문자열을 각각 십진법의 자연수로 변경하고, 그 둘을 더한 값을 다시 이진법으로 표기하는 메서드를 호출해서 이진수로 변경하는 과정이다. 

100문제의 코딩테스트 문제를 풀어가면서 확실하게 늘어난 것은 바로 자바스크립트 내장 객체 함수를 사용하는 능력이다. 이전 같았으면, for,if로 덕지덕지 되었을텐데, 지금은 그런 빈도가 많이 줄어들었다. 
사용할수 있는 메서드의 종류가 늘어나면서 문제 해결을 위해 접근하는 방식이 더욱 늘어나게 된 점도 아주 좋았다. 
비록 제일 낮은 난이도의 문제들로 구성되었지만, 더 난이도 있는 문제를 마주해도 맨 처음의 자신보다는 더 달려들수 있는 마음가짐이 생겨서 뿌듯했다. 

