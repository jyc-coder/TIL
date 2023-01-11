## Coding Test 
프로그래머스 코딩테스트 17일차 문제풀이 완료

- 숨어있는 숫자의 덧셈 (2)
- 안전지대
- 삼각형의 완성조건 (2)
- 외계어 사전


### 안전지대 문제 

링크: https://school.programmers.co.kr/learn/courses/30/lessons/120866

2차원 배열의 board에 지뢰가 존재하는 곳에 1이, 없으면 0이라는 값이 존재한다고 했을때, 해당 board를 입력하면 안전지대의 수를 출력하는 함수를 완성해야한다. 이때 지뢰가 있으면 주변 1곳씩은 위험지대로 간주한다.

아래처럼 문제를 풀긴 했지만 테스트가 정말 딱 하나가 실패했다.
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

### 뭐가 문제였는가?
처음에 입출력 예시를 자세히 안보고 그냥 5x5 형태인 board라고 생각했는데 nxn 형태였다. 그말은 즉슨 1x1의 board 일 경우에도 생각을 해야하는데 코드 후반부의 splice는 보다시피 지뢰가 board 가운데에 위치한 경우에는 3개 적게는 2개의 값을 교체하는 방식으로 이루어져있다.

만약 1x1 형태의 경우라면 이런 splice의 값들이 전부 null로 나오기 때문에 런타임 에러가 발생한것!

### 어떻게 해결했는가?

결국 1x1 형태의 경우를 따로 작성했음
```js
if (land[0].length===1) {
    return land[0][0] === 1? 0 : 1
} 
```
만약 2차원 배열의 첫번째 인덱스의 `length`가 1이라면(해당 board는 정사각형 배열이므로 하나의 인덱스 길이만 비교해도 문제 없다고 생각함) 단 하나의 값을 보고 안전지대의 수를 경우에 따라 리턴하게 설정함

코딩 테스트를 하면서 예외의 경우를 항상 고려해야 한다고 느꼈다. 정말 이 하나의 예외때문에 오답이 되어버리다니..
