---
title: "[Clairy]Postgresql과 FastAPI"
excerpt: "AI 문서 정리 웹앱 Clairy 초기 세팅과 기술 스택 정리"
name: AI-chemist97
writer: AI-chemist97
categories: [Portfolio,Clairy]
tags:
  - [Clairy, FastAPI, NextJS, 개발환경, AI, 문서정리]
published: false
toc: true
toc_sticky: true
date: 2025-07-09 03:00:00 +0900
last_modified_at: 2025-07-09 04:00:00 +0900
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


