# TIL 

## Branch 정의와 명령어

줄기로부터 삐져나온 가지!

분기점을 생성해서 내맘대로 코드를 변경할수 있도록 도와주는 모델

`git branch` - 현재 사용가능한 branch 목록 표시

`git switch 브랜치 이름` -  사용하는 branch를 변경 

원래 `checkout`이라는 명령어를 썼었지만 곧 사라지고 `switch`와 `restore`로 나뉜다.


`git merge 브랜치 이름` - 현재 브랜치에서 `브랜치 이름`에 해당하는 내용을 가져와서 합친다

`git branch -D 브랜치 이름` - `브랜치 이름`브랜치를 제거한다.


브랜치를 따서 작업해서 밀어넣기를 반복하는것이 실무의 일상


## git init 함부로 쓰지 않기
실무하면서 많이 사용하지 않는다.



## merge Conflict

main 브랜치에서 수정하고, 다른 브랜치에서도 같은 파일을 다르게 수정한 다음, main 에서 merge를 시도하면 이런 상황이 발생하는데,

### 에러가 아니다.

`나는 모르겠으니 니가 두개 알아서 해결해라` 라는 뜻

conflict 가 일어나는 파일을 `vi`로 열어보면 각각의 브랜치에서 작성한 내용들이 모두 적혀있는 모습이 보인다. 

이때 내가 원하는 대로 내용을 수정해서 저장하면 된다.

이때 중복되는 문구나 쓸데없는 특수문자는 지워야한다. 

원하는 대로 파일을 수정하고 `git add`,`git commit`을 진행하면 커밋 메시지가 자동으로 작성되어있는 모습을 볼수 있다.



![](https://velog.velcdn.com/images/jhs000123/post/3f6586e2-f379-4f60-9c76-13bfe8c02c61/image.png)



이래서 `git commit -m` 명령어를 습관적으로 쓰면 안된다는 것! 중요한 절차가 진행되는 것은 커밋 메시지가 자동으로 작성되어있다.

## 브랜칭 모델 

- git flow : 작은 단위의 사이클을 돌면서 개발 주기가 있는 경우 많이 사용하는 방법 (ex 게임 패치)

장점: 각 단계가 명확하게 구분할 수 있음
단점: 복잡함

![](https://velog.velcdn.com/images/jhs000123/post/8a0b1acf-53f6-4e9f-80d6-ab384ef06048/image.png)

출처 : https://velog.velcdn.com/images/jhs000123/post/8a0b1acf-53f6-4e9f-80d6-ab384ef06048/image.png


## git-flow cheatsheet

https://danielkummer.github.io/git-flow-cheatsheet/index.ko_KR.html

git flow 전략을 연습함

`git flow init` = git flow 시작

`git flow feature start 브랜치 이름` = `feature`내부에 branch 생성

원하는 파일을 수정하고 커밋한 다음

`git flow feature finish 브랜치 이름` = 

- 내가 만든 `브랜치 이름`이 `develop` 브랜치에 `merge`
- 이전에 만든 브랜치가 제거
- 현재 브랜치 위치를 `develop`으로 변경된다.


`git flow release start 버전이름` = 버전 이름으로 브랜치가 생성된다


`git flow release finish 버전이름` = 
- `merge commit` 
- `릴리즈 노트`를 커밋메시지로 한 커밋을 진행한다.
- develop으로 `merge commit` 진행


이렇게 해놔도 아직 remote로 `push`를 하지 않았기 때문에 github에서는 아무런 변화가 없음

따라서 `git push -u origin develop`을 해야 원격저장소에 나타난다

그리고 버전 네이밍을 했기 때문에 `git push --tags`를 통해 태그도 push 해줘야한다.


## 실수 대처 

- 이름 바꾸기

```
$ git mv 이름 바꿀이름
```

- 이전으로 복귀(Undoing)

```
$ git restore --파일이름
```
커밋하기 전의 내용으로 돌아오게 된다.


- 만약 `git add`까지 해버린 상황이어도 다시 되돌릴수 있다.

```
$ git reset HEAD 파일이름
```
이러면 스테이징된 파일을 불러올 수 있다.



- 가장 최근의 커밋을 수정하기
```
$ git commit --amend
```

- Reset, Revert

Worst case : Reset 
-> 있었던 과거 기록까지 한꺼번에 제거함 

Best case : Revert
-> 잘못하기 전 과거로 돌아가 최신유지하면서 되돌렸다는 이력을 commit

예를 들어서 최근으로부터 3개의 커밋을 수정하고싶다면

```
$ git revert -- no-commit HEAD~3..
```

이렇게 되면 커밋창으로 이동하게 되고 커밋 내용을 수정하면 된다.





## 버전 네이밍

v1.0.1

첫번째 자리 숫자 : 이전 버전과 호환되지 않을 정도로 큰 변화가 있을 경우 변경

두번째 자리 숫자 : 첫번째 숫자보다는 작은 변화가 일어난 경우에 숫자를 변경

세번째 자리 숫자 : 가장 기초적인 단위의 변화가 일어난 경우에 변경됨


## 협업 

팀장 - organization 만들기 - create repository (organization 이름을 owner로 사용함)

내 로컬에서 작업 -> 내 remote로 push -> 팀 remote 로 pull request

나만 하는 작업공간 feature
팀이 일하는 작업공간 develop

git으로 협업을 진행하는 과정을 봤지만 연습을 하지 않으면 힘들 것 같다. 
