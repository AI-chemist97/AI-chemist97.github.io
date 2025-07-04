---
title: "[Clairy] 환경 세팅2"
excerpt: "AI 문서 정리 웹앱 Clairy 초기 세팅과 기술 스택 정리"
name: AI-chemist97
writer: AI-chemist97
categories: [Portfolio,Clairy]
tags:
  - [Clairy, FastAPI, NextJS, 개발환경, AI, 문서정리]
published: false
toc: true
toc_sticky: true
date: 2025-07-02 03:00:00 +0900
last_modified_at: 2025-07-02 04:00:00 +0900
---

# PostgreSQL + FastAPI 연결하기 – Clairy 프로젝트에서 ORM은 어떻게 썼을까?

Clairy는 문서를 자동 정리해주는 FastAPI 기반 웹앱인데,
문서를 분석하고 태깅한 정보를 DB에 저장하려면 **DB 연결**이 필수임.
여기서 우리는 **PostgreSQL**을 쓸 거고,
FastAPI에서는 이걸 연결할 때 보통 **SQLAlchemy**라는 도구를 씀.
이전 OCR확인할 때는 임시로 SQLite를 사용했었음.-> 설치가 필요 없고 .db 파일 하나로 작동해서 빠르게 테스트 하기 좋음
근데 이런 생각이 들 수도 있음:

> Django나 Spring에서는 SQLAlchemy 안 쓰나
> ORM이 뭔데 꼭 써야 하지

그래서 이번 글에서는 \*\*Clairy에서 PostgreSQL 연결할 때 쓰는 ORM(SQLAlchemy)\*\*가
Django나 Spring에서 쓰는 ORM이랑 뭐가 다른지도 같이 정리해봤음.

---

## 🔌 FastAPI에서는 왜 SQLAlchemy를 쓰는가?

FastAPI는 기본적으로 ORM이 따로 없음.
그래서 Python 생태계에서 가장 많이 쓰는 ORM인 **SQLAlchemy**를 가져다 씀.

```python
# 예시: 문서의 ID가 1인 데이터 가져오기
db.query(Document).filter(Document.id == 1).first()
```
sql로 쓴다면
```sql
SELECT * FROM document WHERE id = 1 LIMIT 1;
```

SQL 쿼리 안 쓰고 Python 코드로 데이터를 다룰 수 있어서 간편하고,
보안 문제도 줄어듬 (SQL Injection 위험 감소 등).

(사실 아직 뭐가 다른지 크게 모르겠음. 문법은 다른게 당연한데 굳이 sql 쿼리를 알 필요없는게 장점일까? 썩 코드가 짧지도 않고)
---

## 🧩 Django는 자체 ORM이 있다

Django는 아예 프레임워크 자체에 ORM이 내장돼있어서,
별도로 SQLAlchemy 같은 걸 쓸 필요가 없음.

```python
# 예시
from myapp.models import Document

doc = Document.objects.get(id=1)
```

* 테이블은 `models.Model` 상속받아서 만들고,
* 데이터는 `.objects.get()` 같은 API로 가져옴.

---

## ☕ Spring은 JPA + Hibernate

Spring에서는 **JPA(Java Persistence API)** 라는 표준 ORM을 쓰고,
그 구현체로 **Hibernate**가 가장 많이 쓰임.

```java
// 예시
Optional<Document> doc = documentRepository.findById(1L);
```

* `@Entity` 어노테이션으로 테이블 정의
* `JpaRepository`로 기본적인 CRUD 다 가능

---

## ✅ 세 가지 ORM 방식 비교

| 프레임워크 | ORM 방식             | ORM 이름        | SQLAlchemy 사용? |
| ---------- | -------------------- | --------------- | ---------------- |
| FastAPI    | 외부 라이브러리 사용 | SQLAlchemy      | ✅ Yes            |
| Django     | 내장 ORM             | Django ORM      | ❌ No             |
| Spring     | Java ORM + JPA       | Hibernate (JPA) | ❌ No             |

---

## 🧠 요약

* Clairy는 FastAPI 기반이기 때문에 ORM으로 **SQLAlchemy**를 씀.
* Django나 Spring은 자체 ORM 시스템이 있어서 **SQLAlchemy를 따로 쓸 필요 없음**.
* ORM을 쓰면 SQL을 덜 직접 쓰고, 더 안전하고 유지보수 쉬운 코드 작성 가능!

---

# 📦 1단계 – PostgreSQL 연결 (FastAPI + SQLAlchemy)

## ✅ 사용할 패키지 설치

```bash
pip install sqlalchemy psycopg2-binary
```

이거하고 `pip freeze`도 잘 해주기

> `sqlalchemy`: ORM (Object-Relational Mapping)
> `psycopg2-binary`: PostgreSQL에 연결하는 드라이버

---

## ✅ `database.py` 만들기 (FastAPI에서 DB 연결 설정)

```python
# backend/database.py

from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# PostgreSQL 연결 정보 (변경해줘야 함!)
SQLALCHEMY_DATABASE_URL = "postgresql://username:password@localhost:5432/clairy_db"

engine = create_engine(SQLALCHEMY_DATABASE_URL)

SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)
Base = declarative_base()
```

> `clairy_db`는 네가 만들 PostgreSQL 데이터베이스 이름
> 이거는 나중에 `.env`로 분리할 수도 있음

---

## ✅ 모델 정의 (문서 저장용 테이블 만들기)

```python
# backend/models.py

from sqlalchemy import Column, Integer, String, Text, DateTime
from datetime import datetime
from database import Base

class Document(Base):
    __tablename__ = "documents"

    id = Column(Integer, primary_key=True, index=True)
    filename = Column(String, nullable=False)
    text = Column(Text)
    tags = Column(Text)  # JSON처럼 저장할 예정 (리스트 형태 문자열)
    created_at = Column(DateTime, default=datetime.utcnow)
```

---

## ✅ DB 초기화 코드 작성

```python
# backend/init_db.py

from database import engine
from models import Base

def init_db():
    Base.metadata.create_all(bind=engine)

if __name__ == "__main__":
    init_db()
```

```bash
python backend/init_db.py
```

> 이걸 실행하면 DB에 `documents` 테이블이 생성됨!

---

## ✅ FastAPI에서 DB 연결 주입하기

```python
# backend/deps.py

from database import SessionLocal
from sqlalchemy.orm import Session

def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

---

## ✅ 문서 저장 API 만들기

`upload_file()` 함수에서 추출한 데이터를 DB에 저장하려면:

```python
# backend/main.py

from fastapi import Depends
from sqlalchemy.orm import Session
from models import Document
from deps import get_db
import json

@app.post("/upload")
async def upload_file(
    file: UploadFile = File(...),
    db: Session = Depends(get_db)
):
    # ... 기존 파일 처리 코드 유지 ...

    tags = extract_tags(text)

    # DB 저장
    document = Document(
        filename=file.filename,
        text=text,
        tags=json.dumps(tags)
    )
    db.add(document)
    db.commit()
    db.refresh(document)

    return {
        "id": document.id,
        "filename": file.filename,
        "text": text,
        "tags": tags
    }
```

> `tags`는 딕셔너리니까 `json.dumps()`로 문자열 변환해서 저장!

---

# ⏭️ 다음 할 일

1. PostgreSQL 설치 및 `clairy_db` 데이터베이스 생성
2. 위 코드를 직접 작성해서 데이터 저장 확인
3. 나중엔 `GET /documents`로 리스트 불러오기 기능도 만들 수 있음







---

## 🔐 Supabase 준비 (예정)

* DB, 인증, 스토리지까지 무료로 지원
* PostgreSQL 기반이라 SQL 쓰기도 편함
* FastAPI에서 asyncpg나 SQLAlchemy로 연결 예정
* (추후 연동 내용도 따로 포스트할 예정)



==> 미감이 없어서 항상 블로그를 쓸 때 고민인데 ai가 틀을 잡아주니 정리할 때 훨씬 편하다. 다시 읽으면서 틀리거나 이해안되는 부분을 다시 질문해서 정리하고 하다보면 뭔가 머리에 남는 기분...!
전에는 공부하다가 지치는 경우가 다반사였는데 ai 발전 이런식이면 좋을지도.
근데 이녀석 자꾸 틀린정보를 맞다고 우겨서 팩트 체크가 중요할 것 같다.