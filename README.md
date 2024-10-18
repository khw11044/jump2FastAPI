# 점프 투 FastAPI 따라하기

## 개발 기초 공사 

### FastAPI 프로젝트 구조

앞으로 만들 파이보 프로젝트의 전체 구조는 다음과 같다. 지금은 구상하는 단계이므로 눈으로만 살펴보고 넘어가자. 

```
├── main.py
├── database.py
├── models.py
├── domain
│   ├── answer
│   ├── question
│   └── user
└── frontend
```

### main.py 파일

- 파이보 프로젝트를 설정하는 main.py 파일

- main.py 파일에 생성한 app 객체는 FastAPI의 핵심 객체이다.

- app 객체를 통해 FastAPI의 설정을 할 수 있다. 

- main.py는 FastAPI 프로젝트의 전체적인 환경을 설정하는 파일이다.

### database.py 파일
- 데이터베이스를 설정하는 database.py 파일
- database.py 파일은 데이터베이스와 관련된 설정을 하는 파일이다. 
- 이 파일에는 데이터베이스를 사용하기 위한 변수, 함수등을 정의하고 접속할 데이터베이스의 주소와 사용자, 비밀번호등을 관리한다.

### models.py 파일
- 모델을 관리하는 models.py 파일
- 파이보 프로젝트는 ORM(object relational mapping)을 지원하는 파이썬 데이터베이스 도구인 SQLAlchemy를 사용한다.
- SQLAlchemy는 모델 기반으로 데이터베이스를 처리한다.

### domain 디렉터리
- API를 구성하는 domain 디렉터리
- 파이보 프로젝트는 질문과 답변을 작성하는 게시판을 만드는 것을 최종 목표로 하고 있다.
- 이에 "질문", "답변", "사용자" 라는 총 3개의 도메인을 두어 그 하위에 관련된 파일을 작성하고자 한다.

> 도메인은 "질문", "답변", "사용자" 처럼 굵직한 요구사항 또는 문제 영역을 대표하는 말이다.


- 질문 (question)
- 답변 (answer)
- 사용자 (user)

그리고 각 도메인은 API를 생성하기 위해서 다음과 같은 파일들이 필요하다.

- **라우터 파일** : URL과 API의 전체적인 동작을 관리
- **데이터베이스 처리 파일** : 데이터의 생성(Create), 조회(Read), 수정(Update), 삭제(Delete)를 처리 (CRUD)
- **입출력 관리 파일** : 입력 데이터와 출력 데이터의 스펙 정의 및 검증

예를 들어 질문(domain/question) 도메인이라면 다음의 3개 파일이 필요하다.

- question_router.py : 라우터 파일
- question_crud.py : 데이터베이스 처리 파일
- question_schema.py : 입출력 관리 파일

## frontend

Svelte 프레임워크를 설치한 디렉터리이다. Svelte의 소스 및 빌드 파일들을 저장할 프론트엔드의 루트 디렉터리이다. 최종적으로 frontend/dist 디렉터리에 생성된 빌드 파일들을 배포시에 사용할 것이다.


## SQLAlchemy ORM 라이브러리 사용하기

### ORM 라이브러리 설치하기
> pip install sqlalchemy


### database.py 생성 

[파일명: ./database.py]

```python
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

SQLALCHEMY_DATABASE_URL = "sqlite:///./myapi.db"

engine = create_engine(
    SQLALCHEMY_DATABASE_URL, connect_args={"check_same_thread": False}
)
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

Base = declarative_base()
```

SQLALCHEMY_DATABASE_URL: 데이터베이스 접속 주소 

> sqlite:///./myapi.db는 sqlite3 데이터베이스의 파일을 의미하며 프로젝트 루트 디렉터리에 저장한다는 의미이다.

SessionLocal: 데이터베이스에 접속하기 위해 필요한 클래스

create_engine, sessionmaker 등을 사용하는것은 SQLAlchemy 데이터베이스를 사용하기 위해 따라야 할 규칙이다. 

autocommit=False: 데이터를 변경했을때 commit 이라는 사인을 주어야만 실제 저장이 된다. 

autocommit=True:  commit이라는 사인이 없어도 즉시 데이터베이스에 변경사항이 적용된다. 

create_engine: 컨넥션 풀을 생성

컨넥션 풀이란 데이터베이스에 접속하는 객체를 일정 갯수만큼 만들어 놓고 돌려가며 사용하는 것을 말한다.

## 모델 만들기

이제 파이보에서 사용할 모델을 만들어 보자. 파이보는 질문 답변 게시판이므로 질문과 답변에 해당하는 모델이 있어야 한다.


## ** 모델 속성 구상하기


### models.py 파일

질문 모델인 Question 클래스 작성 

[파일명: ./models.py]

```python

from sqlalchemy import Column, Integer, String, Text, DateTime, ForeignKey
from sqlalchemy.orm import relationship

from database import Base


class Question(Base):
    __tablename__ = "question"

    id = Column(Integer, primary_key=True)
    subject = Column(String, nullable=False)
    content = Column(Text, nullable=False)
    create_date = Column(DateTime, nullable=False)


class Answer(Base):
    __tablename__ = "answer"

    id = Column(Integer, primary_key=True)
    content = Column(Text, nullable=False)
    create_date = Column(DateTime, nullable=False)
    question_id = Column(Integer, ForeignKey("question.id"))
    question = relationship("Question", backref="answers")
```


### 모델을 이용해 테이블 자동으로 생성하기

모델을 구상하고 생성했으므로 SQLAlchemy의 alembic을 이용해 데이터베이스 테이블을 생성해 보자.

alembic은 SQLAlchemy로 작성한 모델을 기반으로 데이터베이스를 쉽게 관리할 수 있게 도와주는 도구이다. 예를들어 models.py 파일에 작성한 모델을 이용하여 테이블을 생성하고 변경할수 있다.

### alembic 설치

> (myapi) c:/projects/myapi> pip install alembic

### alembic 초기화

> (myapi) c:/projects/myapi> alembic init migrations

그러면 myapi 디렉터리 하위에 migrations라는 디렉터리와 alembic.ini 파일이 생성된다. migrations 디렉터리는 alembic 도구를 사용할 때 생성되는 리비전 파일들을 저장하는 용도로 사용되고 alembic.ini 파일은 alembic의 환경설정 파일이다.

> alembic을 이용하여 테이블을 생성 또는 변경할 때마다 작업 파일이 생성되는데 이 작업 파일을 리비전 파일이라고 한다. 그리고 이 리비전 파일은 migrations 디렉터리에 저장된다.

이어서 alembic.ini 파일을 파이참으로 열어서 다음과 같이 수정하자.

[파일명: ./alembic.ini]

```
(... 생략 ...)
sqlalchemy.url = sqlite:///./myapi.db
(... 생략 ...)
```

> alembic이 사용할 데이터베이스의 접속주소를 설정했다.

migrations 디렉터리의 env.py도 다음과 같이 수정

[파일명: ./migrations/env.py]

```
(... 생략 ...)
import models
(... 생략 ...)
# add your model's MetaData object here
# for 'autogenerate' support
# from myapp import mymodel
# target_metadata = mymodel.Base.metadata
target_metadata = models.Base.metadata
(... 생략 ...)
```

### 리비전 파일 생성하기

그리고 파이참 터미널에서 alembic revision --autogenerate 명령을 수행하자.

[파이참 터미널]

> (myapi) c:/projects/myapi> alembic revision --autogenerate

### 리비전 파일 실행하기

이어서 alembic revision --autogenerate 명령으로 만들어진 리비전 파일을 alembic upgrade head 명령으로 실행하자.

> (myapi) c:/projects/myapi> alembic upgrade head
