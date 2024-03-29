### RDBMS vs NoSQL
구분| RDBMS| NoSQL
:--:|:--:|:--:|
형태 |Table| Key-value, Document, Column
데이터| 정형 데이터 |비정형 데이터
성능 |대용량 처리시 저하| 잦은 수정시 저하
스키마| 고정 |Schemeless
장점 |안정적| 확장성, 높은 성능
유명| Mysql, MariaDB, PostgreSQL| MongoDB, CouchDB, Redis, Cassandra


### noSQL

- 확장가능성, 스키마 없는 데이터 모델에 유리
- Row, Document, key-value 등 다양


### RDBMS와 다른점
- Schemaless
- join 불가능(reference 등으로 구현)
- No Transaction
- 수평확장 용이


## 데이터 베이스 디자인 하는 방법?

### Schema
- Database의 구조와 제약조건에 대한 정반적인 명세 기술
- Database 의 Blueprint
- 외부 스키마, 개념 스키마, 내부 스키마

### 외부 스키마
- 프로그램 사용자가 필요로 하는 데이터 베이스의 논리적인 구조를 정의

### 개념 스키마
- 조직 전체의 관점에서의 구조와 관계를 정의
- 외부 스키마의 합과 그 사이의 데이터의 관계 등등
- 일반적인 스키마의 정의

### 내부 스키마
- 저장장치의 입장에서 데이터베이스가 저장되는 방법을 기술


## Software Engineering

- 소프트웨어의 개발,운용, 유지보수 등의 생명주기 전반을 체계적이고 서술적이며 정략적으로 다루는 학문


### DevOps

- 기존의 개발과 운영 분리로 인해 발생하는 문제들
문제 발샐 -> 비방 -> 욕 -> 상처 -> 원인분석 -> 문제해결
- 좋은 소프트 웨어를 위한 필수조건
  - 기획팀과의 원활한 소통으로 요구사항을 충실히 반영
  - 운영팀과의 원활한 소통으로 소비자 불만과 의견을 반영
    
운영과 개발을 통합하여 커뮤니케이션 리소스를 줄이고, 개발 실패 확률을 줄임과 동시에 보
다 안정적인 서비스를 운영할 수 있음!!

## Software Development Life Cycle

- 소프트웨어를 계획, 개발, 시험, 배포하는과정
- 요구사항 분석 -> 설계 -> 구현 -> 테스트 -> 유지 및 보수


### 요구사항 분석

- 무엇이 구현되어야 하는가에대한 명세
- 시스템이 어떻게 동작해야하는지 혹은 시스템의 특징이나 속성에 대한 설명

- 시스템 공학과 소프트웨어 공학분야에서 수혜자 또는 사용자와 같은 다양한 이해관계자의 상충할수도 있는 요구사항을 고려하여 새로운 제품이나 변경된 제품에 부합하는 요구와 조건을 결정하는 것과 같은 업무

- 나(개발자)와 클라이언트(사장) 모두를 만족시키기 위한 연결고리

- 요구사항 유도(수집) : 대화를 통해 요구사항을 결정하는 작업
- 요구사항 분석 : 수집한 요구사항을 분석하여 모순되거나 불완전한 사항을 해결하는 것
- 요구사항 기록 : 요구사항의 문서화 작업

### 사업 요구사항

- "왜?" 프로젝트를 수행하는가?
- 고객이 제품을 개발함으로써 얻을 수 있는 이득
- 비전과 범위

### 사용자 요구사항

- 사용자가 이 제품을 통해 할수 있는 "무엇"


### 기능 요구사항

- 개발자가 이 제품의 "무엇"을 개발할 것인가
- '~해야한다'로 끝나 반드시 수행해야 하거나 사용자가 할수 있어야 하는 것들에 대해 작성


### Business Rules

- 비즈니스 스트럭쳐의 요구나 제약사항을 명세
- "유저 로그인을 위해서는 페이스북 계정이 있어야 한다."
- "유저 프로필 페이지에 접근하기 위해서는 로그인되어 있어야 한다"

### Quality Attribute
- 소프트웨어의 품질에 대해 명세
- "결제과정에서 100명의 사용자가 평균 1.5초의 지연시간 안에 요청을 처리해야 한다"

### External Interface

- 시스템과 외부를 연결하는 인터페이스
- 다른 소프트웨어, 하드웨어, 통신 인터페이스, 프로토콜

### Constraint

- 기술,표준,업무,법,제도 등의 제약조건 명세
- 개발자들의 선택사항에 제한을 두는 것 


### 지나치게 자세한 명세작성
- 명세서는 말 그대로 명세일 뿐, 실제 개발 단계에서 마주칠 모든 것을 담을 수 없음
- 개발을 언어로 모두 표현할 수 없음
- 명세서가 완벽하다고 해서 상품도 완벽하리란 보장은 없음
- 때로는 명세를 작성하기 보단 프로토타이핑이 더 간단할 수 있음.


## Software Development LifeCycle Process Model


### Waterfall Model

- 순차적인 개발 모델, 가장 많이 사용됨
- 정형화된 접근 가능, 체계적인 문서화 가능
- 직전 단계가 완료되어야 진행 가능


### Prototype Model

- 고객 요구사항을 적극적으로 반영하는 모델
- 빠른 개발과 고객 피드백을 빠르게 반영할수 있음
- 대규모 프로젝트에 적용하기 힘듦

### Spiral Model

- 대규모 or 고비용 프로젝트
- 프로젝트의 위험요인을 제거해 나갈수 있음
- 각 단계가 명확하지 않음

### Agile

- UP(Unified Process)
	- 도입(분석위주), 상세(설계위주), 구축(구현위주), 이행(최종 릴리즈)의 반복
- XP(eXtreme Process)
	- 스크럼 마스터가 주도적으로 프로세스를 주도하며, 고객과 개발자 사이의 소통을 중시함
    - Product Owner와 Development Team, Customer로 롤을 구분하고 각자의 역할에 충실
	- TDD 중시
    
### TDD: Test Driven Development

- 객체지향적
- 재설계 시간단축
- 디버깅 시간 단축
- 애자일과의 시너지(사용자 중심적)
- 테스트 문서 대체
- 추가 구현 용이


### Agile Software Development

- 프로젝트의 생명주기동안 반복적인 개발을 촉진하는 개발모델
- TMP(Too Much Plan)과 TLP(Too Less Plan)의 타협
- Code-oriented Methodology
- XP, Scrum 등의 상세 방법론 존재
    
### eXtreme Programming

- 고객 중심의 양질의 소프트웨어를 빠른 시간안에 전달한다!
- Business Requirements의 변동이 심한 경우 적합한 개발 방법.
- 테스트 그리고 테스트 - Test Driven Development

