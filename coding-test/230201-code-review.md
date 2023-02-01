# 코드리뷰 
2월 1주차 코드리뷰를 진행함

## 발표한 문제
이진수 더하기 문제:https://school.programmers.co.kr/learn/courses/30/lessons/120885

### 이진수 더하기

이진수를 의미하는 두 개의 문자열 bin1과 bin2가 매개변수로 주어질 때, 
두 이진수의 합을 return하도록 solution 함수를 완성해주세요.
#### 문제 풀이

```js
function changeDecimal(bin){
  return  [...bin].reverse().reduce((a,c,idx) => {
      return a + Number(c) * 2 ** idx
    },0)
}

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

#### changeDecimal메서드 구현
사용자가 입력한 2개의 2진수 문자데이터를 10진수로 변경하는 메서드
배열은 인덱스순서로 진행되지만 2진수는 오른쪽부터 차례로 숫자가 증가하는 방식이므로 입력한 문자데이터를 
revers()를 호출하여 뒤집은 다음에 reduce메서드를 통해 자릿수마다 값을 구해서 총합을 리턴한다.

#### changeBinary메서드 구현
사용자가 입력한 10진수 숫자데이터를 2진법 문자데이터로 리턴하는 메서드
2로 나누었을때의 나머지 값을 빈배열에 추가하고 dec가 1보다 같거나 작아질 때까지 반복문을 수행하여
배열을 join('')으로 합쳐서 리턴한다.


## 연속된 수의 합 
https://school.programmers.co.kr/learn/courses/30/lessons/120923

연속된 세 개의 정수를 더해 12가 되는 경우는 3, 4, 5입니다. 
두 정수 num과 total이 주어집니다. 연속된 수 num개를 더한 값이 total이 될 때, 
정수 배열을 오름차순으로 담아 return하도록 solution함수를 완성해보세요.
```js
function solution(num, total) {
 
  
  // 연속 되는 수의 시작점을 구하려면? 
  let firstNum = (total - Math.floor((num * (num - 1)) / 2)) / num
  let list = []
  for (i = 0; i < num ; i ++){
    list.push(firstNum)
    firstNum++
  }
 return list
  
}
```

등차수열 공식을 사용해서 첫번째 항이 0 이고 항의 갯수가 num 개이며 공차가 1인 등차수열의 합을 사용해서 처음으로 시작되는 숫자의 값을 구한다음 반복문을 사용해서 num의 숫자만큼 반복하여 빈배열에 숫자를 추가한다.

기능별로 메서드를 따로 나누어서 구현하니까 solution 메서드의 내용이 간결해지면서 깔끔한 코드가 되었다.
다른 코드를 구현할 때에도 이런 식으로 접근해가면서 클린 코딩을 하기위해 노력해야겠다.

수학에 대한 배경지식을 보유하고 있으면 코드가 좀더 간결한 방식으로 코드 작성이 가능해지므로 가끔씩 
이전에 배웠던 수학책을 한번 읽어봐야겠다.
