
## 19일차 코딩테스트 풀이 완료

- 특이한 정렬
- 등수 매기기
- 옹알이
- 로그인 성공


### 로그인 성공... 왜 안돼지?

```js
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

```

map 메소드를 간단하게 작성하면 풀수 있다는건 알수 있지만, 정규 표현식을 사용해서 문제를 해결해보고 싶어서 위와 같이 작성했는데, 첫번째 테스트 케이스에서 실패가 되어버린다. map 메서드는 도중에 멈출수 없다는 특징을 알고 있었기 때문에 result 배열을 구한다음에 "login" 값이 존재하는 메서드가 있다면 "login"을 리턴하고 없는 경우에 "wrong pw"가 존재하는 인덱스가 있다면 "wrong pw"를 아니면 "fail"을 리턴하게 설정했지만 그래도 실패하고 말았다.

그래서 그냥 도중에 멈출수 있는 for of 문을 사용해서 구현했더니 나를 비웃기라도 하는듯이 단번에 성공했다.

```js
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
다른 방식으로 접근하면서 해결해보고 싶었는데, 생각처럼 잘 되지 않아서 정말 답답했다..
그냥 하던대로 하는게 맞는건가..


