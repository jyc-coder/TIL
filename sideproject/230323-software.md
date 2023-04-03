# 왜 알아야 됨?

- 소프트웨어를 개발하고 관리하는 데에 있어 가장 근간이 되는 프로세스이자 프레인워크
- 실무에서도 소프트웨어 개발 주기를 기반으로 프로젝트를 진행
- 사이드 프로젝트도 이 개발 주기를 기반으로 진행해야 체계적으로 진행하고 더 좋은 품질의 제품을 개발할수 있다.

# SDLC (Software Development Life Cycle)
- 계획
- 분석
- 디자인
- 구현
- 테스팅 및 배포
- 유지보수 
- 다시 처음으로


## 계획

- 요구사항을 수집하고 프로젝트를 기획하는 단계
- 사용자 설문, 마케팅 요구사항 등 다양한 채널을 통해 데이터를 모으는 과정으로 가장 중요하면서도 기초가 되는 단계
- QA(Quality Assurance)를 위한 요구사항과 프로젝트가 가질수 있는 리스크 판단


## 분석

- 제품의 요구사항을 정의하는 단계
- SRS (Software Requirement Specification)에 기록
- SRS : 디자인/구현해야할 소프트웨어의 모든 요구사항을 기록해 둔 명세서
- IEEE 에서 제공하는 SRS 템플릿 : 
https://web.cs.dal.ca/~hawkey/3130/srs_template-ieee.doc


## 디자인

- 시스템을 디자인하고 설계하는 단계


## 구현

- 디자인을 기반으로 개발자가 코드로 기능을 구현하는 단계

## 테스팅 및 배포

- 구현 내용이 요구 사항을 충족하는지 검증하는 단계
- 프로젝트 / 비지니스 성격에 따라 베타오픈, 특정시간에 오픈하기도 함

## 유지보수 

- 제품을 마켓에 배포하고 서비스를 모니터링 하면서 유지보수 하는 단계


# 소프트웨어 개발 방법론




## Waterfall
- 전통적인 개발 모델로 프로젝트 기간 동안 SDLC의 주기를 한사이클만 돈다. 즉 , 모든 단계를 완벽하게 준비하고 진행해서 모든 기능의 배포를 한번에 하는 것
-개발 하기 앞서 완벽에 가까운 플래닝을 요구하기 때문에 요구사항이 변돌됐을 때 유동적으로 대처가 어려움

이 모델을 사용하는 회사는 `거의 없음`


## agile

### 등장 배경

- 소프트웨어의 요구사항은 개발 도중 자주 변경된다. 그리고 변경된 요구사항의 따른 작업량을 예측하기 힘들기 때문에, 계획과 형식에 지나치게 의존할 경우에는, 전체적인 개발의 흐름이 느려지게 된다.


- 아무계획이 없이 개발하는 것과 지나치게 많은 계획을 세우고 개발하는 방법들 사이에서 타협점을 찾고자 고안된 방법

- 제한된 시간과 비용안에서 정보는 불완전하고 예측은 불가능하다는전제를 가진다. 그 전 제안에서 합리적인 답을 내는 것이 애자일 개발 프로세스

### 특징

- 일정한 주기를 가지고 프로토타입을 만들어내며 그때 그때 필요한 요구를 더하고 수정하여 소프트웨어를 개발해 나가는 실용적인 스타일



### Waterfall vs Agile

![](https://velog.velcdn.com/images/jhs000123/post/e438e537-a039-447a-ad11-3237d30f3b34/image.png)


### 종류

Extreme Programming (a.k.a XP)
- 1~2주 단위로 개발 - 데모를 프로젝트가 끝날 때까지 반복
- 사용자의 모든 요구사항을 한번에 구현하는 방식 ❌
- 반복형 모델의 개발주기를 2주 단위 정도로 짧게함 -> 설계,구현,프로토타이핑을 전체 개발 기간에 걸쳐 조금씩 자주 실시하도록 하는 개발방법


Scrum

- 주기적인 데모 대신 Sprint 목표를 달성하는 것에 촛점을 둔 프로세스로, 2~4주 단위의 스플니트를 운영
- Grooming 미팅 : Backlog에 쌓인 task들에 대해 팀원들끼리 논의를 해서 ,해당 task의 size estimation과 우선 순위를 정한다.

- Planning 미팅: Product Backlog 에서 Sprint 기간 동안 할 수 있는 일을 팀원들끼리 결정하고 할당한다.

- Daily Scrum: 매일 팀원들끼리 모여 전날 어떤 일을 했는지, 오늘 어떤 일을 할 것인지,
blocker가 있는지에 대해 간단하게 공유, 보통 15분 내외

- 회고 미팅: 스프린트가 끝난 후 팀원들끼리 모여 스프린트에서 잘 된것들, 개선되면 좋을 것들을 노의해서 다음 스프린트에 반영할수 있도록, 협업을 더 잘하기 위한 시간, 보통 1시간 내외


