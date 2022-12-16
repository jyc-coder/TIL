# TIL

## The requested URL returned error: 403
협업 관련 연습을 하기위해서 `github`에 `organization`을 만들고 내부에 `repository`를 생성하여 git flow 전략을 사용하여 협업을 하는 연습을 하던 도중에 `fit flow feature finish`를 실행하여 `develop`브랜치로 이동한 다음 `push`를 하는데 위의 부제와 같은 오류가 나타났다.

  ### 해결법
여러 블로그를 뒤져보고 해결한 방법은 바로 윈도우 제어판에서 자격증명 관리목록중에서 git이 들어있는 목록을 제거하고 난 다음

```
git remote add origin "원격저장소주소"
```
를 입력 한 다음 push를 다시 실행해보니 성공했다.

출처: https://data-jj.tistory.com/49

## 팀장의 업무 지시

아무튼 이로써 성공적으로 push가 완료 됐고 develop 브랜치에 파일이 업로드 되었다. 
이제 팀원이 진행해야하는 업무를 작성하는 것은 `issue`에서 하면 된다.

![](https://velog.velcdn.com/images/jhs000123/post/7ca939eb-f40e-4f11-a4fb-b066312963df/image.png)


이 organiztion에 팀원들을 초대하면 팀원들이 이슈를 올리기 시작하면,

팀장은 이것을 보고 업무를 지시한다.

![](https://velog.velcdn.com/images/jhs000123/post/21b6bda4-e8ff-4075-8d96-22acc47691d9/image.png)



## 팀원 입장에서 fork 후 업무 시작

- `organiztion repository` 가 생성되고 팀원이 이슈를 올려서 승인을 받으면 팀원은 해당 `repository`를 `fortk`하여 복사한다.

이때 `Copy the branch only`항목 체크는 해제 하고 `fork`할 것! 
develop에서 일을 할 것이기 때문이다.

- `fork`한 repo를 clone 해서 `git flow init`을 통해 develop branch로 이동한다.

이때 git remote -v를 보면 origin이 `fork`로 가져온 팀원의 repo 주소로 되어있다.


> 팀원 로컬에서 일을 한다 -> 팀원 원격 저장소로 `push`한다 -> `push`한것을 기반으로 팀 repo에 `pull request`를 날려준다

근데 나말고 다른 팀원이 한것도 있을텐데 그것은 어떻게 하느냐? 바로 팀 repo에서 직접 받아오면 된다.

그래서 팀 repo의 주소가 필요한 것!

```bash
$ git remote add 'remote 이름' '원격 저장소 주소'(둘다 따옴표는 작성안해도 됨)
```
위와 같이 remote 를 추가하고, 해당 저장소에서 fetch를 통해 변동사항을 가져와서 적용시키면 된다.

이제 팀원이 올린 이슈의 내용을 전부 작성을 하고 `git flow feature finish` 를 사용하면 다시 develop으로 돌아온다.


develop 으로 push를 하는데 오류가 생겼는데

https://stackoverflow.com/questions/40069344/remote-rejected-master-master-permission-denied

여기를 참고하여 해결했음.

