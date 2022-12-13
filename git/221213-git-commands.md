# TIL

## git objects

- blob : 컨텐츠의 용량을 bytes로 표시하고 텍스트, 이미지, 음악 혹은 단순한 파일처럼 다양한 형식의 파일이 저장될 수 있음
- tree : 트리 오브젝츠의 용량을 bytes로 표시하고 하위 디렉토리의 트리객체를 재귀적으로 참조함
- commit : blob + tree

- tag : 저장소의 소스 버전을 간간히 표시


## git = github?
git -> 오픈 소스 버전 관리 시스템
github -> Git Repository를 위한 웹 기반 호스팅 서비스

git은 프로그램이고 github은 원격 저장소이다.


## git bash로 git config 설정하기

만약 설정내용을 취소하고 싶으면 `git config --global --unset *(이메일, 유저)`를 입력해서
해당 내용을 제거할수 있음

git log 가독성 높이는 설정
링크 https://gist.github.com/johanmeiring/3002458


## git bash를 사용해서 github 관련 작업 

`git clone 원격저장소 링크` -> 원격저장소에서 만든 리포지토리를 불러옴

`git status` git에 의해 관리되는 파일들의 가능한 상태(status) 확인
- `git status -uall` : 디렉토리 안의 세부 파일의 내용을 보여줌

`git add`를 통해서 변경사항을 스테이징 하기

`git commit` 를 입력하면 이전 설정의 editor를 vim으로 해놨기 때문에 vim이 나타나게 됨.
 
 - 첫번째 줄에는 제목을, 세번째 줄에 내용을 입력한다. 
 - 작성한거 다 작성하고 커맨드 모드에서 wq누르면 종료
 

 
`git push` -> 원격 저장소(remote repository)에 코드 변경내용을 업로드함



## READEME는 뭐하러 만들지?

프로젝트와 Repository의 개요를 설명하는 문서내용이 적혀있음

필수적으로 들어있어야 할 내용
- 프로젝트 이름
- 문서
- 설치 방법
- 기술 스택
- 상세 정보
- 라이센스 정보

## commit 하기 전에

- 최소한의 단위로 작성했는가?

- 해당 작업에서 변환된 파일이 모두 포함 되어있는가?

- 제목을 너무 장황하게 쓰지 않았는가?

- 내용이 해당 commit의 의도와 일치하는가?

> 시간 혹은 작업단위로 정해서 커밋해야 나중에 확인해도 파악이 쉬워진다
 

## git add. git commit -m 같은거 습관적으로 쓰지 말자

하나만 추가하거나 커밋을 해야할 상황이 있을때에도 습관적으로 써버리면 추가해선 안되는 파일까지 커밋되거나 추가될수 있음

## commit 내용 작성 요령

https://www.conventionalcommits.org/ko/v1.0.0/

- commit의 제목은 간결하게
- 옆에 태그를 붙여서 어떤 커밋인지 파악할수 있게 작성(feat, fix, docs, test, conf, build, ci)

## .gitignore

Git 버전 관리에서 제외할 파일 목록을 지정하는 파일

https://www.toptal.com/developers/gitignore/
해당 사이트에서 제외할 항목을 작성하고 `생성`을 누르면 .gitignore를 자동으로 생성해서 결과물을 보여준다.

## 라이센스

내가 만들때나, 배포할때 가장 신중하게 사용해야한다

MIT - 행동 제약 없음

Apache Licence 2.0 - Apache 재단이 만든 라이센스

GNU GPL v3.0 - 제일 많이 알려져있으며 의무사항이 존재함


## 실무때 저장소 사용
팀 프로젝트의 저장소를 복사해와서 개발자가 코딩을 진행한다. 이렇게 하면 내가 잘못한다고 해도 팀 프로젝트에 영향이 가지 않음

## git 은 습관이 정말 중요함

TIL을 만들고 매일 얻은 지식을 정리
커밋을 쌓아서 commit 하는 습관도 기를 것! 

hexo + {username}.github.io repository로 정적 블로그를 만들어 정리하는 습관을 들이는 것도 좋음

## TIL 작성법

- 학문, 프레임워크 관련 디렉토리를 생성
- 디렉토리안에 파일을 생성(오늘 날짜-배운 항목의 이름.md -> 꼭 md일 필요는 없음)

커밋 작성 우선순위 : 시간순서 




