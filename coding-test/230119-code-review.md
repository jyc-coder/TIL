## Code Review
23년 1월 3주차 코드리뷰 진행함

### 발표 내용


#### 특이한 정렬문제 : https://school.programmers.co.kr/learn/courses/30/lessons/120880

```js
function solution(numlist, n) {
  let sortedList = numlist.sort((a,b) => {
    const [absA, absB] = [Math.abs(a-n), Math.abs(b-n)]
    if(absA === absB) return b-a
    
    return absA-absB
  })
  

return sortedList
 }
 ```
 - sort 메소드에 디테일한 조건을 부여함으로써 원하는 상황에서 내림,오름 차순 정렬을 결정할수 있음
- 만약 해당 요소에서 n을 뺀 절대값이 일치한 경우는 내림차순으로, 그 외의 경우에는 오름 차순으로 정렬했다.  절댓값이 작은 순서대로 정렬을 해야하기 때문!

#### 로그인 성공? : https://school.programmers.co.kr/learn/courses/30/lessons/120883

```js
//1번 테스트 실패한 풀이
function solution(id_pw, db) {
   const id = id_pw[0]
   const pw = id_pw[1]
   
   // const idRegex = new RegExp(id,"g")
   // const pwRegex = new RegExp(pw,"g")
   // console.log(idRegex.test("meosseugi"))  
 const result =  db.map((e,idx) => {
   // return  idRegex.test(e[0]) ? (pwRegex.test(e[1]) ? "login" : "wrong pw") : "fail" 
  
   
   const testId = e[0];
   const testPw = e[1];
   const idRegex = new RegExp(testId,"g")
   const pwRegex = new RegExp(testPw,"g")
   
   if( e[0] === "" || e[1] === "") {
     return "fail"
   }
    else if (idRegex.test(id) === true)  {
     return pwRegex.test(pw) === true ?  "login" : "wrong pw"
   }
   else if(idRegex.test(id) === false ){
     return "fail"
   }
   
  })
 

 console.log(result)
 if (result.indexOf("login") >= 0){
    return "login"
 }
 else if(result.indexOf("login") < 0 ) {
   return result.indexOf("wrong pw") >= 0 ? "wrong pw" : "fail"
 }
  
}
// 두번째 접근 방식

function solution(id_pw, db) {
  let result = ''
 for (const element of db){
   if(id_pw[0] === element[0] && id_pw[1] === element[1]){
     result = "login"
     break;
   }
   else if(id_pw[0] === element[0] && id_pw[1] !== element[1]){
     result ="wrong pw"
     break;
   }
   else {
     result="fail"
   }
 }
  
  return result 
}

```
##### 처음 풀이 방법

- 정규표현식을 사용해서 db를 map 메소드를 사용해서 사용자가 입력한 id와 pw를 따로 선언한 정규표현식을 통해 test 메소드를 호출함
- 해당 결과에 따라서 if문을 통해 문자 데이터를 리턴함
    - id와 pw 모두 일치 : “login”
    - id일치, pw 불일치 : “wrong pw”
    - id와 pw 모두 불일치 / pw만 일치 : “fail”
- `result` 배열에 특정 요소가 존재하는지의 여부를 통해 최종 리턴값을 결정하기로함
- 요소가  `login` 인 인덱스가 존재한다면 `login` 으로,  없다면, `wrong pw` 가 존재할때 `wrong pw` , 둘다 없는 경우에는 `fail` 을 리턴하게 함

이와 같은 방식으로 접근을 했지만, 첫번째 테스트케이스에서 실패가 나왔음. 조사 결과 map대신에 for break를 사용해보기로함. map은 도중에 멈출수가 없고 for break같은 경우는 특정 상황에 멈출수 있기 때문에 변수를 최소화 시킬수 있다고 판단하여 다른 방식으로 접근했다. 

##### 두번 째 접근 방식

- 빈 문자열 `result`  선언 후 db의 요소를 for of 를 통해서 `id_pw`  와 비교 작업을 실시함
- 만약 id_pw의 id와 db의 id 가 일치한 경우 pw도 일치하다면 `login`  의 값을 `result` 에 넣어준다.
- id는 같아도 pw 가 틀린 경우에는 `worng pw` 를 넣어주고 그 외에는 전부 fail을 넣어준다.
- 이런 방식으로 result에 값을 넣어준뒤 리턴하는 방식으로 구현했다.

### 느낀점

무작정 for문을 배척할 것이 아니라 경우에 맞게 반복 메서드를 사용하는것이 바람직 하다고 생각했다.
