## code review

23년 1월 4주차 코드리뷰 진행함

발표 내용
### 안전지대 문제 
https://school.programmers.co.kr/learn/courses/30/lessons/120866

```js
function solution(land) {
// 지뢰가 어디있는가?
let mineSpot = land.flat().indexOf(1)
// 지뢰가 있는곳을 모두 파악한다. 
let mineList = []
let obj ={}
let col = 0
let row = 0
while(mineSpot != -1){
  mineList.push(mineSpot)
  mineSpot = land.flat().indexOf(1, mineSpot+1);
}
// board가 1 * 1 구조일 경우
if (land[0].length===1) {
    return land[0][0] === 1? 0 : 1
} 
// 위험지역 선정하기. 지뢰지역의 주변 1칸씩이 모두 위험 지역이 된다.
for (let e of mineList){
    col = Math.floor(e / land.length )
   row = e % land[0].length

  // col이 첫번째인 경우
  if (col === 0){
    // row 가 2번째에서 4번째 사이에 있는 경우
   (1 <=row && row < land.length-2 ) ?  [
land[col].splice(row-1,3,1,1,1),
     land[col+1].splice(row-1,3,1,1,1),
   ]
    //row가 맨 끝에 있는 경우
  : row === land.length-1 ?
     [land[col].splice(row-1,2,1,1), land[col+1].splice(row-1,2,1,1)]
  // row가 맨 처음에 있는 경우
  :
   [land[col].splice(row,2,1,1),land[col+1].splice(row,2,1,1)]
  
  }
  // col 이 2번째와 끝에서 2번째사이에 있는 경우
  else if ( 1<= col && col <= land.length-2){
    // row 가 2번째에서 4번째 사이에 있는 경우
  (1 <= row && row < land[0].length -2 ) ? 
    [ land[col-1].splice(row-1,3,1,1,1),
  land[col].splice(row-1,3,1,1,1),
  land[col+1].splice(row-1,3,1,1,1)]
    //row가 맨 끝에 있는 경우
  : row === land[0].length-1 ?
    [  land[col-1].splice(row-1,2,1,1),
  land[col].splice(row-1,2,1,1),
  land[col+1].splice(row-1,2,1,1)]
      // row가 맨 처음에 있는 경우
  :   [land[col-1].splice(row,2,1,1),
  land[col].splice(row,2,1,1),
  land[col+1].splice(row,2,1,1)] 
   }
  // col이 맨 끝일 경우
  else{
    (1 <= row && row < land[0].length - 2) ? [ land[col-1].splice(row-1,3,1,1,1),
  land[col].splice(row-1,3,1,1,1)]
  : row === land[0].length - 1 ? [ land[col-1].splice(row-1,2,1,1),
  land[col].splice(row-1,2,1,1)]
  :    
    [land[col-1].splice(row,2,1,1),
  land[col].splice(row,2,1,1)]
  }  
}

    return  land.flat().filter((e) => e === 0).length
}
```
### 문제 풀이

- 지뢰가 있는 곳을 파악하기위해서 while문을 사용하여 land에서 가지고 있는 1이 어느 인덱스에 위치했는지 파악
- 해당 인덱스를 단위 요소(배열)의 길이만큼 나눈 몫을 행, 나머지를 열로 지정해서 정확한 위지를 지정한다
- 해당 열과 행의 위치에 따라서 splice메소드를 호출하여 주변 값을 1로 변경한다.
- 변경된 land에서 filter를 사용해서 요소의 값이 0인 것만 반환하여 길이를 리턴하면 안전지대의 갯수가 된다.

### 느낀 점

항상 예외를 잘 생각해서 코드를 작성해야한다는 사실을 다시한번 느끼게 되었다.
```js
if (land[0].length===1) {
    return land[0][0] === 1? 0 : 1
}
```
land가 1X1일 가능성을 생각하지 않아서 테스트의 오류가 발생했고 이를 해결하기 위해서 따로 1X1인 경우에
해당하는 코드를 작성했다.
