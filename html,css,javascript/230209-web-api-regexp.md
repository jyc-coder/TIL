
# javascript
## web api
### history
브라우저 히스토리(세션 기록) 정보를 반환하거나 제어한다.

#### 속성
- .length: 등록된 히스토리 개수
- .scrollRestoration: 스크롤 위치 복원 여부 - 'auto' / 'manual'
- .state: 현재 히스토리에 등록된 데이터(상태)

#### 메소드

- .back(): 뒤로 가기
- .forward(): 앞으로 가기
- .go(위치): 현재 페이지 기준 특정 히스토리 '위치'로 이동 - .go(-2)
- .pushState(상태, 제목, 주소): 히스토리에 상태 및 주소를 추가

- .replaceState(상태, 제목, 주소): 현재 히스토리의 상태 및 주소를 교체

## 정규표현식


정규표현식(RegExp, Regular Expression, 정규식)은 문자열을 확인, 대체, 추출하는 용도로 사용되는 패턴이다.

### 정규식 생성

```js
// 생성자
new RegExp('표현', '옵션')
new RegExp('[a-z]', 'gi')

// 리터럴
/표현/옵션
/[a-z]/gi
```

### 메소드

메소드 | 문법 | 설명
---|---|---
test | `정규식.test(문자열)` | 일치 여부(Boolean) 반환
match | `문자열.match(정규식)` | 일치하는 문자의 배열(Array) 반환
replace | `문자열.replace(정규식, 대체문자)` | 일치하는 문자를 대체

### 플래그(옵션)

- g : 모든 문자 일치
- i : 영어 대소문자를 구분않고 일치(ignore case)
- m : 여러줄 일치, 각각의 줄을 시작과 끝으로 인식한다.


### 패턴(표현)
패턴 | 설명
---|---
^ab | 줄(Line) 시작에 있는 ab와 일치
ab$ | 줄(Line) 끝에 있는 ab와 일치
.	| 임의의 한 문자와 일치
a&verbar;b | a 또는 b와 일치
ab?	| b가 없거나 b와 일치
{3} |	3개 연속 일치
{3,} |	3개 이상 연속 일치
{3,5} |	3개 이상 5개 이하(3~5개) 연속 일치
| +	| 1회 이상 연속 일치, `{1,}`
| *	| 0회 이상 연속 일치, `{0,}`
() |	그룹핑
[ab] | a 또는 b
[a-z] |	a부터 z 사이의 문자 구간에 일치(영어 소문자)
[A-Z] |	A부터 Z 사이의 문자 구간에 일치(영어 대문자)
[0-9] |	0부터 9 사이의 문자 구간에 일치(숫자)
[가-힣] | 가부터 힣 사이의 문자 구간에 일치(한글)
\w | 63개 문자(Word, 대소영문52개 + 숫자10개 + _)에 일치
\b | 63개 문자에 일치하지 않는 문자 경계(Boundary)
\d | 숫자(Digit)에 일치
\s | 공백(Space, Tab 등)에 일치
(?:) | 그룹 지정
(?=) | 앞쪽 일치(Lookahead)
(?<=) | 뒤쪽 일치(Lookbehind)